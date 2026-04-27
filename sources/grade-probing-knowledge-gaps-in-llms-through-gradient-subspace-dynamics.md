---
title: "GRADE: Probing Knowledge Gaps in LLMs through Gradient Subspace Dynamics"
source: "https://arxiv.org/html/2604.02830v2"
author:

created: 2026-04-21
description:
tags:
  - "clippings"
  - "citable"
---
Yujing Wang <sup>1</sup>  Yuanbang Liang <sup>2</sup>  Yukun Lai <sup>2</sup>  Hainan Zhang <sup>1</sup>  Hanqi Yan <sup>3</sup>  
<sup>1</sup> Beihang University   <sup>2</sup> Cardiff University   <sup>3</sup> King’s College London  
eugenia@buaa.edu.cn  LiangY32, LaiY4@cardiff.ac.uk  hanqi.1.yan@kcl.ac.uk

###### Abstract

Detecting whether a model’s internal knowledge is sufficient to correctly answer a given question is a fundamental challenge in deploying responsible LLMs. In addition to verbalising the confidence by LLM self-report, more recent methods explore the model internals, such as the hidden states of the response tokens to capture how much knowledge is activated. We argue that such activated knowledge may not align with what the query requires, e.g., capturing the stylistic and length-related features that are uninformative for answering the query. To fill the gap, we propose GRADE (GRAdient Dynamics for knowlEdge gap detection), which quantifies the knowledge gap via the cross-layer rank ratio of the gradient to that of the corresponding hidden state subspace. This is motivated by the property of gradients as estimators of the required knowledge updates for a given target. We validate GRADE on six benchmarks, demonstrating its effectiveness and robustness to input perturbations. In addition, we present a case study showing how the gradient chain can generate interpretable explanations of knowledge gaps for long-form answers.<sup>1</sup>

## 1 Introduction

Large Language Models (LLMs) have demonstrated remarkable capabilities across diverse reasoning tasks by eliciting internal knowledge in response to various queries [^44] [^1]. However, before taking the generated response for granted, we need to check whether the model is capable to solve the problem [^22] [^26], which is different from uncertainty quantification as a model can be highly confident yet still be wrong. Detecting such knowledge gaps is essential to mitigating failure modes such as hallucination [^20] [^16] [^48]. While LLM verbalization [^28] [^41] [^37] offers the most direct route to eliciting self-reported knowledge gap, like “Yes, I’m able to”, recent work has shifted toward exploiting intrinsic model signals. Among these, hidden states of generated answer tokens [^30] [^6] have emerged as a particularly informative source for measuring a model’s internal knowledge.

Inspired by recent advances in mechanistic interpretability [^4] [^3] [^39], hidden states have been used to classify correctly and incorrectly answered reasoning samples. These representations — ranging from the hidden state of the final response token [^4] [^3] to a geometric Chain-of-Embedding (CoE) [^43] over reasoning chains — consistently outperform predominant metrics such as perplexity and entropy, with further gains achieved by supervised variants [^19] [^30]. The success of leveraging the encoded internal representations extends beyond knowledge gap detection to applications such as fact verification [^46], underscoring the powerful expressivity of LLM representations more broadly.

However, hidden states capture activated knowledge broadly, including those irrelevant to the query, such as input lengths and style variations, which do not principally affect the knowledge required to answer correctly. In other words, existing hidden-states based methods can be violated by unexpected trivial perturbations in the input.

![Refer to caption](https://arxiv.org/html/2604.02830v2/x1.png)

Figure 1: t-SNE visualizations of hidden states for answerable and unanswerable queries.

We illustrate this in Figure 1 using a knowledge-intensive task, where external documents can be retrieved as additional context. We prompt the LLM to generate answers both with and without retrieved documents, then project the last-layer’s hidden states into 2D space via t-SNE. Red and green denote correctly and incorrectly answered responses, respectively, while squares and triangles denote responses generated with and without retrieved documents. We observe that hidden states are clearly separated by the presence or absence of retrieved documents, but not by whether the answer is correct. It demonstrates that adding additional information, even when it doesn’t help close the knowledge gap, can greatly affect the hidden states distribution.

Building on these insights, we propose GRADE, a gradient-based dynamic analysis to directly measure the amounts of required knowledge updates to reach the given query. Inspired by pioneering works that utilize gradient-based Fisher Information to localize factual associations [^23] [^5], we quantify the required knowledge updates for a given query via the gradient chain-rule based on a query-related learning objective. Based on the observation that gradients lie within the subspace spanned by hidden states, we project the gradient into this shared subspace and apply a rank ratio to directly measure the effective proportion of required knowledge updates relative to all activated knowledge. To alleviate the hyperparameter reliance, we introduce a spectrum-aware measurement that adaptively selects the most informative components for stable rank analysis, and train a probe to capture cross-layer rank ratio dynamics.

Our main contributions can be summarized below:

- We identify a systematic problem of existing hidden state methods in knowledge gap detection, i.e., their vulnerability to irrelevant input perturbations.
- We propose a gradient-based dynamic analysis method GRADE by measuring the stable rank ratio of the projected knowledge updates, for LLM knowledge gap detection. This method also enables the interpretation generation for long-form answers.
- Empirically, we demonstrate that GRADE consistently outperforms baselines across six datasets, including question answering, multiple-choice, and math reasoning in predicting question answerability and cross-dataset transferability.

## 2 Related Work

Knowledge gaps detection can be achieved by either merely evaluating the model’s outputs (black-box) or accessing the model’s internal states.  
Black-box. Verbalization [^41] [^2] [^37] directly prompts LLMs to self-report their confidence or acknowledge their inability to answer a given question in the generated text. Despite the further enhancements, such as self-reflection capabilities of RLHF-tuned models, [^49] found these methods are still largely hindered by overconfidence and miscalibration. Sampling-based methods [^24] [^29] [^9] [^45] evaluate the semantic consistency or agreement across multiple sampled responses to the same query. For instance, semantic uncertainty [^24] estimates confidence by sampling multiple responses, clustering them by semantic equivalence, and computing entropy over the resulting clusters. However, such methods are often computationally expensive, requiring multiple sampling passes and external models for semantic consistency evaluation.  
White-box methods allow access to model internals, such as generated token probability and hidden states (activations). Entropy [^22] or perplexity [^36] from the model’s logits are the most-widely adopted baseline metrics for both knowledge gap [^13] or uncertainty [^31]. More recent work also extends the token-level measurement to the atomic fact-level of the predictions [^8] and query-level [^6], by computing a margin over sequence token probabilities [^42].

Moving deeper into the model’s architecture, i.e., internal activations, such as MLP outputs [^3] [^30] and attention [^27]. For example, [^27] identifies semantically crucial tokens by back-tracking through an attention chain to explain answer derivation. [^3] and [^30] train a knowledge gap detector with the hidden as inputs and factual accuracy as output label. Our GRADE also leverages the hidden states, but with emphasis on leveraging gradient information to alleviate the reliance on spurious features in the inputs.

## 3 GRADE for Knowledge Gap Probing

![Refer to caption](https://arxiv.org/html/2604.02830v2/x2.png)

Figure 2: GRADE for knowledge gap probing. Given a input q, (i) Forward pass: compute hidden states h, o h,o in the MLP block and loss ℒ \\mathcalL, either before response generation or after; Backward pass: derive gradient g; (ii) Rank ratio calculation: project gradients onto the subspace spanned by and compute rank ratios; (iii) Probe training: aggregate rank ratios across L layers to predict the knowledge gap.

The goal of knowledge gap detection is to determine whether the model $\mathcalM$ possesses sufficient latent knowledge to answer a given question $q$. Formally, it is to learn a mapping function $F$ from the model’s internal states after feeding with $q$ to a continuous score $s$ representing the gap:

$$
F:\mathcalI_\mathcalM(q)\mapsto[0,1],
$$

where $\mathcalI_\mathcalM(q)$ represents the updated model’s internal states after feeding with the given $q$.  
An effective detector should capture how much it contains knowledge specifically required to answer $q$. Many hidden state-based methods [^3] directly use the output representations from the Multi-Layer Perceptron (MLP) blocks as the detector input, i.e., $\mathcalI_M\leftarrow MLP(q)$. However, hidden states could encode information irrelevant to the required knowledge (see Figure 1), making them susceptible to spurious input variations. GRADE is proposed to extract a set of features from $\mathcalI_M(q)$ that isolates the components of the internal computation specifically relevant to knowledge sufficiency.

##### Method overview.

We propose using gradients as a proxy for the parameter updates required to answer a given query. The method overview is shown in Figure 2. Given an input query $q$, a forward pass derives hidden states $h$ in the MLP blocks and loss $\mathcalL$, conditioned on whether a sampled response $y$ is available ($\mathcalL_\textpost$) or not ($\mathcalL_\textpre$). A backward pass then yields the gradients (§3.1). To measure what proportion of the required knowledge is present in the activated parameters, we project the gradients onto the subspace spanned by $h$ and compute stable rank ratios (§3.2). Finally, rank ratios across all $L$ layers are aggregated as input features to train a knowledge gap detection probe (§3.3).

### 3.1 Bi-Directional Information Flow

MLP feedforward. Given a Large Language Model $M$ processing a sequence of length $n$, we focus on the MLP blocks because they are widely recognized as the primary storage for factual information [^10]. Let $\bmx^q\in\mathbbR^n\times d_\textmodel$ denote the MLP input hidden states, where $d_\textmodel$ is the model dimension. The information flow through a gated MLP block in Llama [^11] can be formalized as:

$$
\displaystyle\bmh=\sigma(\bmx^qW_\textgate^\top)\odot(\bmx^qW_\textup^\top),\quad\bmo=\bmhW_\textdown^\top.
$$

During the MLP feedforward process, we first obtain the intermediate hidden states $\bmh\in\mathbbR^n\times d_\textff$ via the two weight matrices $W_\textgate$ and $W_\textup$, where $\sigma$ is the non-linear activation function, $\odot$ denotes the element-wise product, and $d_\textff$ is the intermediate feed-forward dimension. The MLP output hidden states $\bmo\in\mathbbR^n\times d_\textmodel$ are then computed via the projection matrix $W_\textdown$ (hereafter denoted simply as $W$).

Generation and loss function. From the LLM generation head, we obtain the logit $z\in\mathbbR^V$, after the projection from model dimension $d_\textmodel$ to vocabulary $V$, from which tokens are sampled autoregressively to form the response $y=[y_1,y_2,\dots,y_\textout]$. We then define two loss objectives $\mathcalL$ depending on whether the response $y$ has been generated, formally defined in Eq (3):

$$
\displaystyle\mathcalL_pre=-\sum_j=1^Vp(z_j)\log p(z_j),\quad\mathcalL_pos=-\sum_t=1^|y_\textout|\log p(y_t\mid\bmy_<t,q),
$$

where $z_j$ is the logit for the $j$ -th vocabulary token, and $p(\cdot)$ denotes the softmax function.

- For $\mathcalL_pre$, we calculate the entropy of $z$ to measure how spread out the distribution is over the vocabulary. A low entropy implies that the model knows well what to generate next given the query.
- For $\mathcalL_pos$, we take the model’s own generated response $y$ as a pseudo-label for supervision, and then use the standard cross-entropy loss to measure how the model confidently explains what it just said. Specifically, in long-form reasoning scenarios, the response is segmented into discrete steps based on punctuation markers. A step-wise cross-entropy loss is then applied, restricting the loss calculation to the tokens within each step. We thus generate the interpretation for the detected gap based on this mechanism, and the results are shown in §5.

Backward pass. We now apply the chain rule to derive the gradient $\bmg\in\mathbbR^d_\textmodel\times d_\textff$ of the loss with respect to $W$. The gradient derivation can be expanded as:

$$
\bmg=\frac\partial\mathcalL\partial W=\left(\frac\partial\mathcalL\partial\bmo\right)^\top\frac\partial\bmo\partial W=\Delta^\top\bmh,
$$

where $\Delta=\frac\partial\mathcalL\partial\bmo\in\mathbbR^n\times d_\textmodel$ is the intermediate signal at the pre-activation layer. Consequently, the $i$ -th row of $\bmg$, $\bmg_i^\top\in\mathbbR^d_\textff$, can be written as:

$$
\bmg_i^\top=\sum_t=1^n\Delta_t,i\,\bmh_t^\top,
$$

a weighted sum of the input hidden states $\\bmh_t\_t=1^n$. Since every row of $\bmg$ is a linear combination of these vectors, $\bmg$ lies entirely within the subspace spanned by $\bmh$.

##### Gradient subspace illustration.

Based on Eq. (5), the gradient lies within the subspace spanned by the hidden states, as illustrated in Figure 2. Specifically, the gradient (white triangle) resides within the subspace (bounded by the red dashed boundary) of the 3D space ($x_h-y_h-z_h$) spanned by the hidden states. This motivates projecting both the gradient and hidden states onto their shared subspace, where the relative volume of the gradient subspace to the hidden state subspace serves as a natural proxy for the proportion of required knowledge updates within the activated knowledge. In what follows, we describe how the stable rank ratio operationalises this proxy.

### 3.2 Gradient Projection and Rank Ratio

To compute the Rank Ratio as a proxy for the proportion of required knowledge updates relative to activated knowledge, we first project both gradients and hidden states into a shared subspace (the region within the red dashed boundary in Figure 2), followed by the rank ratio computation, which also serves as an implicit normalisation against input length.

#### 3.2.1 Projected Rank Calcualtion

To facilitate a direct comparison of ranks, we first project the parameter gradient into a unified representation space shared with the hidden states. Then, we employ the stable rank to robustly compute the effective dimensionalities of the required knowledge updates.

Projection to a shared representation space. We project the gradient $\bmg$ onto the sample-specific representation space via $\bmh\bmg^\top\in\mathbbR^n\times d_\textmodel$ representing the first-order variation of token representations induced by the gradient, then compute the projected covariance $\bmh\bmg^\top\bmg\bmh^\top\in\mathbbR^n\times n$. We further normalise the covariance as follows:

$$
\mathcalC_g=\mathcalC_h^\dagger\left(\bmh\bmg^\top\bmg\bmh^\top\right)\mathcalC_h^\dagger,
$$

where multiplying on both sides with the Moore-Penrose pseudoinverse [^33] of the Gram matrix $\mathcalC_h=\bmh\bmh^\top$ is to ensure the projected gradient covariance $\mathcalC_g$ is not being distorted by the anisotropic geometry of $\bmh$.

To quantify the effective dimensionality of the projected gradient space, we compute the rank of $\mathcalC_g$ using Singular Value Decomposition (SVD). The singular value distributions are long-tailed, and recent studies suggest the core semantics only lie in a low-dimensional manifold, so we retain only singular values above $10^-6$ for rank calculation. Figure 8 demonstrates the effectiveness of gradient rank as a discriminative signal for answerable and unanswerable questions.

Stable rank for robust rank estimation. Despite the effectiveness of naive rank, the hard threshold makes the naive rank estimate sensitive to numerical noise near the cutoff. As illustrated in Figure 9, truncating at $1e-4$ yields a rank of 269, whereas a naive threshold at $1e-5$ results in a rank of 299, leading to a substantial difference in rank calculation. Therefore, we employ the stable rank [^38] [^17] to ensure a robust and stable calculation of effective dimensionality, as it offers a smooth, spectrum-aware measure that accounts for all singular values. Formally, $\lambda^g_1\geq\lambda^g_2\geq\dots\geq\lambda^g_n\geq 0$ be the sorted singular values of $\mathcalC_g$ The stable rank applied to the two objectives:

$$
srank_pre(\bmg)=\sum_i=1^n\frac\lambda^g_i\lambda^g_1,\;srank_pos(\bmg)=\sum_i=1^n\frac(\lambda^g_i)^2(\lambda^g_1)^2.
$$

The stable rank provides a continuous measure that naturally concentrates on high-energy singular values while discounting the long tail, as illustrated in Figure 9.

#### 3.2.2 Rank Ratio Calculation

Rank ratio as implicit normalisation. To quantify the knowledge gap, we introduce the Gradient Subspace Rank Ratio, which evaluates the proportion of effective required updates against the activated knowledge. The gradient subspace rank ratio is computed as $\fracsrank(\bmg)srank(\bmh),$ where $srank(\bmh)$ is computed analogously to Eq. (6). This ratio reflects how much new knowledge the model attempts to update within the representational space that is already being used to process the input and support the output label. More importantly, since longer inputs simultaneously expand both the gradient and hidden state subspaces, their ratio remains stable, acting as a natural normalization against input length variability. To verify the justification, we calculate the correlation between metrics and the different input sequence lengths (results in Figure 3). Specifically, we select the queries from the HotpotQA [^47] dataset that can’t be correctly answered even when the model is fed with retrieved external documents (longer inputs shown in the x-axis). We observe that the rank of hidden states (a) is sensitive to the changes of the input length, with 0.59 Pearson correlation. The rank ratio (c) is the most robust (0.18), with a significant improvement over the stable rank (0.36) of the gradient (b).

![Refer to caption](https://arxiv.org/html/2604.02830v2/figs/hs_rank_token_length_sensitivity_noanswer.png)

(a) Hidden State Rank

### 3.3 Supervised Probe based on Rank Ratio

To capture the dynamics of gradients across different layers, we gather an input feature consisting of $[\textRankRatio_1,\textRankRatio_2,\dots,\textRankRatio_L]$. This feature vector characterizes how ratio of required to activated knowledge evolves across layers, providing a richer propagation trajectory than scalar value-based thresholding. Empirical evidence in Table 6 validates that layer-wise features are more indicative of knowledge sufficiency than such threshold-based alternatives.

To train the supervised probe, we construct a dataset of paired inputs consisting of layer-wise feature vectors and binary labels, where each label indicates whether the model can correctly answer the given query. In practice, for each query, we sample multiple responses and assign a positive label (answerable) if the accuracy across samples exceeds an upper threshold, and a negative label (unanswerable) otherwise. Further details are in § A.1.4.

## 4 Experiments

### 4.1 Experimental Settings

Models and dataset. We use three backbone models, i.e., Llama3.1-8b [^11], Qwen2.5-7b [^34] and Gemma2-9b [^40]. Evaluation are conducted across six datasets, including math (GSM8K [^7], MATH [^15]), MMLU [^14], and free-from QA: NQ [^25], TQA [^21] and HotpotQA [^47].

Baselines. We compare with both verbalisation, i.e., Judge [^37] and white-box, including token-probability and hidden states. P-Entropy [^22] is to calculate the cumulative token-level entropy over the generated sequence. Internal Confidence (IC) [^6] is unsupervised, mapping the last layer hidden state of the last input token to a confidence score $P(\textYES)$, and Align-P [^30] is a supervised method by utilising the intermediate hidden states before response generation.

Metrics. Following [^42] and [^6] [^24], we utilize Accuracy (Acc) to calculate the proportion of samples where the predicted answerability aligns with the actual task performance; Area Under the Receiver Operating Characteristic (AUROC) [^12] as a robust evaluation sensitive to the class imbalance. <sup>2</sup>

### 4.2 Evaluation results of knowledge gap detection

Results for original queries. The knowledge gap detection for original queries in the evaluation benchmarks is in Table 1. Overall, our proposed $\textttGRADE_\textpre$ and $\textttGRADE_\textpos$ consistently outperform all baselines. Notably, $\textttGRADE_\textpre$ that requires no response generation achieves the best performance in 19 out of 36 settings, underscoring the strength of gradient-based signals as a lightweight yet effective indicator of knowledge sufficiency. Among the baselines, Align-P achieves the most competitive accuracy as a supervised probe, but suffers from severe AUROC degradation on complex datasets such as MMLU and HQA. Notably, the effectiveness of our two methods aligns with task complexity. For single-hop QA tasks, i.e., NQ and TQA, $\textttGRADE_\textpre$ achieves the best with only the query-related information. For HQA and difficult reasoning datasets (MMLU, GSM8K and MATH), those requiring Chain-of-thought reasoning capabilities, $\textttGRADE_\textpos$ can leverage its token-level information in the generated response to capture the procedure knowledge confidence.

<table><tbody><tr><th rowspan="2"></th><th rowspan="2">Method</th><td colspan="2">NQ</td><td colspan="2">TQA</td><td colspan="2">HQA</td><td colspan="2">MMLU</td><td colspan="2">GSM8K</td><td colspan="2">MATH</td></tr><tr><td>Acc</td><td>AUROC</td><td>Acc</td><td>AUROC</td><td>Acc</td><td>AUROC</td><td>Acc</td><td>AUROC</td><td>Acc</td><td>AUROC</td><td>Acc</td><td>AUROC</td></tr><tr><th rowspan="6">Llam3.1-8b</th><th>Judge</th><td>0.523</td><td>-</td><td>0.652</td><td>-</td><td>0.536</td><td>-</td><td>0.519</td><td>-</td><td>0.639</td><td>-</td><td>0.501</td><td>-</td></tr><tr><th>P-Entropy</th><td>0.616</td><td>0.643</td><td>0.645</td><td>0.691</td><td>0.596</td><td>0.607</td><td>0.701</td><td>0.728</td><td>0.643</td><td>0.496</td><td>0.661</td><td>0.704</td></tr><tr><th>IC</th><td>0.640</td><td>0.669</td><td>0.661</td><td>0.715</td><td>0.747</td><td>0.651</td><td>0.504</td><td>0.651</td><td>0.518</td><td>0.639</td><td>0.575</td><td>0.627</td></tr><tr><th>Align-P</th><td>0.771</td><td>0.793</td><td><math><semantics><munder><mn>0.784</mn> <mo>¯</mo></munder> <annotation>\underline0.784</annotation></semantics></math></td><td><math><semantics><munder><mn>0.855</mn> <mo>¯</mo></munder> <annotation>\underline0.855</annotation></semantics></math></td><td><math><semantics><mn>0.737</mn> <annotation>\color[rgb]0,0,00.737</annotation></semantics></math></td><td><math><semantics><mn>0.806</mn> <annotation>\color[rgb]0,0,00.806</annotation></semantics></math></td><td><math><semantics><mn>0.759</mn> <annotation>\bm0.759</annotation></semantics></math></td><td>0.506</td><td>0.692</td><td>0.766</td><td>0.753</td><td>0.607</td></tr><tr><th><math><semantics><msub><mtext>GRADE</mtext> <mtext>pos</mtext></msub> <annotation>\textttGRADE_\textpos</annotation></semantics></math></th><td><math><semantics><munder><mn>0.794</mn> <mo>¯</mo></munder> <annotation>\underline0.794</annotation></semantics></math></td><td><math><semantics><munder><mn>0.841</mn> <mo>¯</mo></munder> <annotation>\underline0.841</annotation></semantics></math></td><td>0.713</td><td>0.774</td><td><math><semantics><mn>0.778</mn> <annotation>\bm0.778</annotation></semantics></math></td><td><math><semantics><mn>0.857</mn> <annotation>\bm0.857</annotation></semantics></math></td><td><math><semantics><munder><mn>0.727</mn> <mo>¯</mo></munder> <annotation>\underline0.727</annotation></semantics></math></td><td><math><semantics><mn>0.778</mn> <annotation>\bm0.778</annotation></semantics></math></td><td><math><semantics><mn>0.836</mn> <annotation>\bm0.836</annotation></semantics></math></td><td><math><semantics><mn>0.893</mn> <annotation>\bm0.893</annotation></semantics></math></td><td><math><semantics><mn>0.891</mn> <annotation>\bm0.891</annotation></semantics></math></td><td><math><semantics><mn>0.962</mn> <annotation>\bm0.962</annotation></semantics></math></td></tr><tr><th><math><semantics><msub><mtext>GRADE</mtext> <mtext>pre</mtext></msub> <annotation>\textttGRADE_\textpre</annotation></semantics></math></th><td><math><semantics><mn>0.830</mn> <annotation>\bm0.830</annotation></semantics></math></td><td><math><semantics><mn>0.888</mn> <annotation>\bm0.888</annotation></semantics></math></td><td><math><semantics><mn>0.813</mn> <annotation>\bm0.813</annotation></semantics></math></td><td><math><semantics><mn>0.895</mn> <annotation>\bm0.895</annotation></semantics></math></td><td><math><semantics><munder><mn>0.754</mn> <mo>¯</mo></munder> <annotation>\underline0.754</annotation></semantics></math></td><td><math><semantics><munder><mn>0.810</mn> <mo>¯</mo></munder> <annotation>\underline0.810</annotation></semantics></math></td><td>0.703</td><td><math><semantics><munder><mn>0.751</mn> <mo>¯</mo></munder> <annotation>\underline0.751</annotation></semantics></math></td><td><math><semantics><munder><mn>0.714</mn> <mo>¯</mo></munder> <annotation>\underline0.714</annotation></semantics></math></td><td><math><semantics><munder><mn>0.767</mn> <mo>¯</mo></munder> <annotation>\underline0.767</annotation></semantics></math></td><td><math><semantics><munder><mn>0.770</mn> <mo>¯</mo></munder> <annotation>\underline0.770</annotation></semantics></math></td><td><math><semantics><munder><mn>0.841</mn> <mo>¯</mo></munder> <annotation>\underline0.841</annotation></semantics></math></td></tr><tr><th rowspan="6">Qwen2.5-7b</th><th>Judge</th><td>0.671</td><td>-</td><td>0.601</td><td>-</td><td>0.536</td><td>-</td><td>0.490</td><td>-</td><td>0.471</td><td>-</td><td>0.662</td><td>-</td></tr><tr><th>P-Entropy</th><td>0.596</td><td>0.616</td><td>0.611</td><td>0.649</td><td>0.543</td><td>0.541</td><td>0.713</td><td>0.678</td><td>0.627</td><td>0.541</td><td>0.702</td><td>0.542</td></tr><tr><th>IC</th><td>0.625</td><td>0.670</td><td>0.607</td><td>0.693</td><td>0.612</td><td>0.602</td><td>0.502</td><td>0.536</td><td>0.603</td><td>0.637</td><td>0.676</td><td>0.722</td></tr><tr><th>Align-P</th><td><math><semantics><mn>0.680</mn> <annotation>\color[rgb]0,0,00.680</annotation></semantics></math></td><td><math><semantics><mn>0.704</mn> <annotation>\color[rgb]0,0,00.704</annotation></semantics></math></td><td><math><semantics><mn>0.705</mn> <annotation>\color[rgb]0,0,00.705</annotation></semantics></math></td><td><math><semantics><munder><mn>0.833</mn> <mo>¯</mo></munder> <annotation>\underline\color[rgb]0,0,00.833</annotation></semantics></math></td><td><math><semantics><munder><mn>0.752</mn> <mo>¯</mo></munder> <annotation>\underline\color[rgb]0,0,00.752</annotation></semantics></math></td><td><math><semantics><mn>0.745</mn> <annotation>\color[rgb]0,0,00.745</annotation></semantics></math></td><td><math><semantics><mn>0.755</mn> <annotation>\bm0.755</annotation></semantics></math></td><td>0.476</td><td>0.645</td><td>0.669</td><td>0.747</td><td>0.797</td></tr><tr><th><math><semantics><msub><mtext>GRADE</mtext> <mtext>pos</mtext></msub> <annotation>\textttGRADE_\textpos</annotation></semantics></math></th><td>0.745</td><td><math><semantics><munder><mn>0.802</mn> <mo>¯</mo></munder> <annotation>\underline0.802</annotation></semantics></math></td><td>0.719</td><td>0.776</td><td><math><semantics><mn>0.786</mn> <annotation>\bm0.786</annotation></semantics></math></td><td><math><semantics><mn>0.866</mn> <annotation>\bm0.866</annotation></semantics></math></td><td>0.734</td><td><math><semantics><mn>0.758</mn> <annotation>\bm0.758</annotation></semantics></math></td><td><math><semantics><mn>0.719</mn> <annotation>\bm0.719</annotation></semantics></math></td><td><math><semantics><munder><mn>0.772</mn> <mo>¯</mo></munder> <annotation>\underline0.772</annotation></semantics></math></td><td><math><semantics><mn>0.879</mn> <annotation>\bm0.879</annotation></semantics></math></td><td><math><semantics><mn>0.921</mn> <annotation>\bm0.921</annotation></semantics></math></td></tr><tr><th><math><semantics><msub><mtext>GRADE</mtext> <mtext>pre</mtext></msub> <annotation>\textttGRADE_\textpre</annotation></semantics></math></th><td><math><semantics><mn>0.781</mn> <annotation>\bm0.781</annotation></semantics></math></td><td><math><semantics><mn>0.814</mn> <annotation>\bm0.814</annotation></semantics></math></td><td><math><semantics><mn>0.795</mn> <annotation>\bm0.795</annotation></semantics></math></td><td><math><semantics><mn>0.862</mn> <annotation>\bm0.862</annotation></semantics></math></td><td>0.715</td><td><math><semantics><munder><mn>0.778</mn> <mo>¯</mo></munder> <annotation>\underline0.778</annotation></semantics></math></td><td><math><semantics><munder><mn>0.735</mn> <mo>¯</mo></munder> <annotation>\underline0.735</annotation></semantics></math></td><td><math><semantics><munder><mn>0.688</mn> <mo>¯</mo></munder> <annotation>\underline0.688</annotation></semantics></math></td><td><math><semantics><munder><mn>0.699</mn> <mo>¯</mo></munder> <annotation>\underline0.699</annotation></semantics></math></td><td><math><semantics><mn>0.828</mn> <annotation>\bm0.828</annotation></semantics></math></td><td><math><semantics><munder><mn>0.804</mn> <mo>¯</mo></munder> <annotation>\underline0.804</annotation></semantics></math></td><td><math><semantics><munder><mn>0.879</mn> <mo>¯</mo></munder> <annotation>\underline0.879</annotation></semantics></math></td></tr><tr><th rowspan="6">Gemma2-9b</th><th>Judge</th><td>0.574</td><td>-</td><td>0.692</td><td>-</td><td>0.573</td><td>-</td><td>0.472</td><td>-</td><td>0.513</td><td>-</td><td>0.400</td><td>-</td></tr><tr><th>P-Entropy</th><td>0.588</td><td>0.607</td><td>0.631</td><td>0.655</td><td>0.547</td><td>0.538</td><td>0.702</td><td>0.765</td><td>0.739</td><td><math><semantics><munder><mn>0.794</mn> <mo>¯</mo></munder> <annotation>\underline0.794</annotation></semantics></math></td><td>0.691</td><td>0.751</td></tr><tr><th>IC</th><td>0.588</td><td>0.650</td><td>0.649</td><td>0.698</td><td>0.562</td><td>0.655</td><td>0.505</td><td>0.567</td><td>0.507</td><td>0.541</td><td>0.715</td><td>0.757</td></tr><tr><th>Align-P</th><td>0.734</td><td>0.730</td><td>0.725</td><td>0.799</td><td><math><semantics><mn>0.764</mn> <annotation>\color[rgb]0,0,00.764</annotation></semantics></math></td><td><math><semantics><mn>0.855</mn> <annotation>\color[rgb]0,0,00.855</annotation></semantics></math></td><td><math><semantics><mn>0.782</mn> <annotation>\bm0.782</annotation></semantics></math></td><td>0.534</td><td>0.727</td><td>0.776</td><td>0.613</td><td>0.636</td></tr><tr><th><math><semantics><msub><mtext>GRADE</mtext> <mtext>pos</mtext></msub> <annotation>\textttGRADE_\textpos</annotation></semantics></math></th><td><math><semantics><munder><mn>0.759</mn> <mo>¯</mo></munder> <annotation>\underline0.759</annotation></semantics></math></td><td><math><semantics><munder><mn>0.823</mn> <mo>¯</mo></munder> <annotation>\underline0.823</annotation></semantics></math></td><td><math><semantics><munder><mn>0.781</mn> <mo>¯</mo></munder> <annotation>\underline0.781</annotation></semantics></math></td><td><math><semantics><munder><mn>0.818</mn> <mo>¯</mo></munder> <annotation>\underline0.818</annotation></semantics></math></td><td><math><semantics><munder><mn>0.805</mn> <mo>¯</mo></munder> <annotation>\underline0.805</annotation></semantics></math></td><td><math><semantics><mn>0.882</mn> <annotation>\bm0.882</annotation></semantics></math></td><td><math><semantics><munder><mn>0.768</mn> <mo>¯</mo></munder> <annotation>\underline0.768</annotation></semantics></math></td><td><math><semantics><mn>0.838</mn> <annotation>\bm0.838</annotation></semantics></math></td><td><math><semantics><mn>0.776</mn> <annotation>\bm0.776</annotation></semantics></math></td><td><math><semantics><mn>0.821</mn> <annotation>\bm0.821</annotation></semantics></math></td><td><math><semantics><mn>0.871</mn> <annotation>\bm0.871</annotation></semantics></math></td><td><math><semantics><mn>0.934</mn> <annotation>\bm0.934</annotation></semantics></math></td></tr><tr><th><math><semantics><msub><mtext>GRADE</mtext> <mtext>pre</mtext></msub> <annotation>\textttGRADE_\textpre</annotation></semantics></math></th><td><math><semantics><mn>0.814</mn> <annotation>\bm0.814</annotation></semantics></math></td><td><math><semantics><mn>0.907</mn> <annotation>\bm0.907</annotation></semantics></math></td><td><math><semantics><mn>0.832</mn> <annotation>\bm0.832</annotation></semantics></math></td><td><math><semantics><mn>0.932</mn> <annotation>\bm0.932</annotation></semantics></math></td><td><math><semantics><mn>0.815</mn> <annotation>\bm0.815</annotation></semantics></math></td><td><math><semantics><munder><mn>0.868</mn> <mo>¯</mo></munder> <annotation>\underline0.868</annotation></semantics></math></td><td>0.750</td><td><math><semantics><munder><mn>0.809</mn> <mo>¯</mo></munder> <annotation>\underline0.809</annotation></semantics></math></td><td><math><semantics><munder><mn>0.745</mn> <mo>¯</mo></munder> <annotation>\underline0.745</annotation></semantics></math></td><td>0.783</td><td><math><semantics><munder><mn>0.794</mn> <mo>¯</mo></munder> <annotation>\underline0.794</annotation></semantics></math></td><td><math><semantics><munder><mn>0.813</mn> <mo>¯</mo></munder> <annotation>\underline0.813</annotation></semantics></math></td></tr></tbody></table>

Table 1: Main results on various datasets.

##### Robust to input perturbation.

![Refer to caption](https://arxiv.org/html/2604.02830v2/x3.png)

Figure 4: Relative change in detection accuracy ( Δ A c \\Delta Acc ) before and after input paraphrase. Smaller changes imply that the method is more robust to the perturbation.

To evaluate the robustness of the methods to the input perturbation, we use Qwen3-30b-instruct as the backbone model to rephrase the original queries as new inputs. We prompt the model to rephrase queries while preserving semantics. To maintain consistent task difficulty, we consider only query pairs for which the model’s answer success remains unchanged. Finally, we evaluate probe robustness by measuring the relative performance change ($\Delta$ Acc) on these rephrased inputs.

The evaluation results on the perturbed inputs are presented in Figure 4. The results clearly demonstrate that our proposed metrics, $\textttGRADE_\textpos$ and $\textttGRADE_\textpre$, are more robust to input perturbations compared to baselines. In contrast, IC and Align-P exhibit notable performance fluctuations when input phrasing changes. This confirms that GRADE ignores superficial linguistic variations and reliably isolates intrinsic representational signals associated with actual knowledge gaps.

![Refer to caption](https://arxiv.org/html/2604.02830v2/x4.png)

Figure 5: Cross-dataset generalization accuracy heatmaps. The results within the red box are transferred among similar complex questions (single-hop), i.e., between TQA and NQ.

### 4.3 Cross-Dataset Transferability

To assess the transferability, we evaluated GRADE by training on dataset A and testing on dataset B (See in Figure 5). By looking at the results at non-diagonal areas, we observe that $\textttGRADE_\textpos$ demonstrates the most robust generalization, followed by $\textttGRADE_\textpre$ and Align-P. Moreover, $\textttGRADE_\textpos$ also demonstrates the best transfer patterns across different complexity datasets. Specifically, the transfer between similar complex datasets (TQA and NQ are single-hop) is shown within the red frame, and $\textttGRADE_\textpre$ and Align-P show a significant degradation within this area. Noted that both $\textttGRADE_\textpre$ and Align-P are methods using the generation logit before response sampling. In contrast, the superiority of $\textttGRADE_\textpos$ suggests that sequence-level gradient dynamics along the generated response capture how required knowledge accumulates and interacts across tokens, making it more robust to distribution shifts between tasks of different complexity.

## 5 Further Analysis

We further analyze the effects of key designs of the proposed probes, such as modeling the gradient dynamics across different layers and the layer selections as probe inputs. We also show the generated fine-grained interpretation for the long-form response.

Effects of modeling the cross-layer dynamics. Some existing work suggests that layer-wise signals [^30] [^32] are reliable indicators of model behaviour, motivating a simple thresholding approach: take the rank ratio from a specific layer, determine an answerable/unanswerable threshold on the training set, and apply it to the test set. We compare several such variants in Figure 6, where Mean, Last, and Mid denote using the average, last-layer, and middle-layer rank ratio as the thresholding signal, respectively. In contrast, $\textttGRADE_\textpos$ takes the full layer-wise rank <sup>3</sup> ratio vector as probe input, enabling it to learn cross-layer propagation patterns rather than relying on any single layer’s signal. The results confirm the effectiveness of this design, with a clear and consistent performance gap over all thresholding baselines (Acc results are shown in Figure 12). We also show the rank ratios across different layers for both answerable and unanswerable queries in Figure 10: it shows that their absolute values for the two categories are not clearly distinguishable, but the dynamics across different layers differ.

![Refer to caption](https://arxiv.org/html/2604.02830v2/x5.png)

Figure 6: Comparison AUROC among detection threshold (the mean, last-layer, and middle-layer rank ratio, respectively) versus GRADE (rank ratio patterns cross different layers).

##### Interpretation.

As $\textttGRADE_\textpos$ considers the information across all the generated tokens, we can thus leverage it to generate token-level interpretation about the knowledge sufficiency. Specifically, we use the $t$ -th row-sum of $\mathcalC_g$ to identify the knowledge gap of token $t$. A high score indicates an epistemic shock, signifying a knowledge gap where the model requires large updates; a low score suggests that the token resides within the model’s knowledge boundary, requiring minimal adjustment. To ensure a fair and consistent comparison, the scores are globally normalized across all samples.

We show the generated uncertainty maps for both correctly and incorrectly answered queries from the GSM8k dataset in Figure 7(a). We observe that the high scores are predominantly concentrated on critical logical anchors and mathematical operators, such as ‘multiply’ and the subsequent numeric result. This suggests that the model experiences momentary cognitive pressure only at pivotal decision points or computational junctions. Once these key steps are resolved, the scores rapidly decay, indicating that the rest of the reasoning chain resides safely within the internal knowledge. For the incorrect reasoning, we observe a rather diffuse pattern across the whole inputs, such as a high score is observed during the unit conversion process (e.g., ‘convert’, ‘ounces’), which persists throughout the following tokens. This pattern reflects a global epistemic collapse.

![Refer to caption](https://arxiv.org/html/2604.02830v2/x6.png)

(a) Correct reasoning outcome.

## 6 Conclusion

We propose GRADE (GRAdient Dynamics for knowlEdge gap detection), a gradient-based approach that addresses the fundamental challenge of detecting knowledge gaps for a given query. Unlike hidden-state methods that capture activated knowledge richness, GRADE recognizes that activated knowledge may not align with query requirements. Specifically, GRADE quantifies the rank ratio between the gradient and hidden state subspaces, and uses the cross-layer ratio tendency as input to train a gap detector. Validation across six benchmarks demonstrates superior robustness and accuracy over verbalised, probabilistic, and hidden-state baselines, and a case study further illustrates how the gradient chain yields interpretable explanations of knowledge gaps in long-form answers.

## References

## Appendix A Appendix

### A.1 Implementation details

#### A.1.1 Data Setup

For open-domain QA datasets (NQ, TriviaQA, HotpotQA), we evaluate both no-context and with-doc settings. In the with-doc setting, we employ the Contriever [^18] dense retriever to retrieve relevant evidence passages. For each query, the top-1 retrieved document is selected and prepended to the question within the prompt. This document is inserted as an additional context block before the question to provide external evidence for answer generation.

#### A.1.2 Knowledg Gap Labeling

To determine answerability, we categorize queries based on their empirical accuracy across 10 samples for QA and multiple-choice tasks, and 5 samples for CoT reasoning. For the GSM8K dataset, we define the knowledge boundary using thresholds of 0.6 (answerable) and 0.4 (unanswerable), while other datasets utilize 0.8 and 0.2, respectively. Evaluation is conducted using Exact Match (EM) for QA and multiple-choice tasks, whereas CoT tasks are evaluated based on the correctness of the final result extracted from the reasoning chain.

#### A.1.3 GRADEpos\_\\textpos for Long-form Generation

To evaluate $\textttGRADE_pos$ in extended reasoning scenarios, we apply our gradient-based analysis to segmented sequences. Following the generation of a complete reasoning chain, we segment the response into discrete steps $\\mathcalS_1,\mathcalS_2,\dots,\mathcalS_K\$ based on punctuation markers (e.g., ”.”). For a given reasoning step $\mathcalS_k$, let $n_k=n_\textq+\sum_j=1^k|\mathcalS_j|$ be the cumulative length of the sequence up to the end of this step. The loss function of the current step is:

$$
\mathcalL_pos^(k)=-\sum_t\in\mathcalS_k\log p(y_t\mid\bmx_<t),
$$

where only the tokens within the current step $\mathcalS_k$ contribute. Let $\bmh^(k)\in\mathbbR^n_k\times d_\textff$ denote the intermediate hidden states,with $\bmg^(k)$ representing the corresponding step-specific gradient. Finally, to derive the metric $\textttGRADE_c$ for the complete sequence, we aggregate the step-specific stable rank ratios by averaging them across all $K$ steps:

$$
\textttGRADE_pos=\frac1K\sum_k=1^K\fracsrank(\bmg^(k))srank(\bmh^(k)).
$$

While $r_G$ captures the continuous dimensionality of the knowledge flux, it suffers from sensitivity to sequence length, since longer contexts inherently possess higher rank ceilings and introduce more baseline degrees of freedom. Let $\sigma_1\geq\sigma_2\geq\dots\geq\sigma_s>0$ be the sorted eigenvalues of the hidden Gram matrix $HH^\top$. We define the effective rank of the hidden-state $r_H$ similarly:

$$
r_H=\sum_i=1^s\frac\sigma_i\sigma_1
$$

Finally, we propose the Gradient Subspace Rank Ratio (GSRR) to quantify the relative geometric alignment between the parameter update and the representation subspace:

$$
\mathrmGSRR=\fracr_Gr_H.
$$

#### A.1.4 Probe Architecture

The alignment probe is implemented as a 5-layer feed-forward neural network designed to progressively compress input features into a scalar probability ($num\_layers\to 256\to 128\to 64\to 32\to 1$). Each hidden block consists of a linear transformation followed by Batch Normalization, a Leaky ReLU activation (negative slope $=0.01$), and a Dropout layer ($p=0.5$) to enhance generalization and prevent overfitting. We employ Kaiming uniform initialization for all linear weights and initialize batch normalization parameters to unit scale and zero bias. The final layer utilizes a Sigmoid activation to map the output to $[0,1]$.

#### A.1.5 Probe Training

We implement the training pipeline for the diagnostic probe using PyTorch. To ensure strict reproducibility across all experiments, the global random seed is fixed to 42. The probe is trained for a maximum of 100 epochs with a batch size of 64. For optimization, we initialize the learning rate at $5\times 10^-3$ and apply a weight decay of $1\times 10^-5$ to regularize the network and mitigate overfitting. To facilitate stable convergence and avoid local minima, we incorporate a learning rate scheduler that dynamically reduces the learning rate by a factor of 0.75 if no improvement is observed over a patience window of 5 epochs. Finally, during the evaluation phase, we adopt a standard decision threshold of 0.5 to binarize the probe’s continuous output into a definitive prediction

#### A.1.6 Paraphrase Generation

For each query in datasets, we generate a semantically equivalent paraphrase using Qwen3-30B-A3B-Instruct-2507 [^35]. To ensure the perturbations only affect the linguistic form without altering the core factual or logical requirements, we employ a zero-shot prompting strategy. The system is provided with the following instruction:

System Prompt: ”You are a helpful assistant. Please paraphrase the following question accurately without changing its original meaning or entity names. Output ONLY the paraphrased question.”

#### A.1.7 Calculation of AUROC

The Area Under the Receiver Operating Characteristic curve (AUROC) is a threshold-independent metric utilized to evaluate the discriminative capability of our gradient-based probing classifier. It is particularly advantageous in our setting as it remains robust against class imbalance—a common scenario when evaluating the proportion of answerable versus unanswerable queries in LLMs.

Let $\mathcalD^+$ represent the set of positive samples (e.g., queries the model answers correctly) and $\mathcalD^-$ represent the set of negative samples (e.g., queries exposing a knowledge gap). Let $s(x)\in[0,1]$ denote the predicted confidence score output by our probe for a given query $x$.For any decision threshold $\tau\in[0,1]$, the True Positive Rate (TPR) and False Positive Rate (FPR) are formally defined as:

$$
\textTPR(\tau)=\frac|\x\in\mathcalD^+\mid s(x)\geq\tau\||\mathcalD^+|
$$
 
$$
\textFPR(\tau)=\frac|\x\in\mathcalD^-\mid s(x)\geq\tau\||\mathcalD^-|.
$$

The ROC curve is constructed by plotting $\textTPR(\tau)$ against $\textFPR(\tau)$ as the threshold $\tau$ varies from 1 down to 0. The AUROC is geometrically defined as the integral of this curve:

$$
\textAUROC=\int_0^1\textTPR(\tau)\,d(\textFPR(\tau)).
$$

### A.2 Dataset Descriptions

GSM8K [^7] is a collection of 8,500 high-quality grade-school math word problems designed to evaluate models’ abilities in multi-step arithmetic reasoning. The dataset focuses on interpretable numerical reasoning rather than symbolic manipulation, making it a standard benchmark for assessing mathematical problem-solving in natural language.

MATH [^15] contains more than 12,000 challenging competition-level mathematics problems covering algebra, geometry, number theory, combinatorics, and calculus. Each problem includes step-by-step solutions, enabling rigorous evaluation of advanced mathematical reasoning and multi-step derivation abilities.

The Massive Multitask Language Understanding (MMLU) benchmark [^14] consists of 57 multiple-choice subjects spanning STEM, humanities, social sciences, and professional domains. It assesses models’ world knowledge and problem-solving skills across a broad range of disciplines, making it one of the most comprehensive evaluations of general knowledge reasoning.

Natural Questions (NQ) [^25] comprises real anonymized queries issued to the Google search engine, paired with evidence passages from Wikipedia. It evaluates open-domain question answering, requiring systems to locate and extract relevant information from long documents under realistic information-seeking scenarios.

TriviaQA (TQA) [^21] is a large-scale open-domain question answering dataset consisting of over 95,000 question–answer pairs authored by trivia enthusiasts. Each question is paired with evidence documents from the web or Wikipedia, enabling evaluation of retrieval-based QA, reasoning over long contexts, and robust answer extraction.

HotpotQA (HQA) [^47] is an open-domain QA dataset featuring multi-hop reasoning questions that require retrieving and integrating information from multiple Wikipedia articles. It supports both extractive and abstractive answers and provides supporting facts, enabling evaluation of interpretable multi-step reasoning.

### A.3 Baselines Implementation

Judge: The Prior Judge baseline evaluates epistemic uncertainty by directly prompting the Large Language Model (LLM) to explicitly state whether it possesses the knowledge to answer a question.

(1) In the closed-book setting, we use the prompt: ”Are you sure to accurately answer the following question based on your internal knowledge? If yes, you should give a short answer with one or a few words; if no, you should answer ’Unknown’. Question: question Answer: ”.

(2) For the Retrieval-Augmented Generation (RAG) setting, the prompt is adapted to: ”Given the following information: context Can you answer the following question based on the given information or your internal knowledge, if yes, you should give a short answer with one or few words, if no, you should answer ’Unknown’. Question: question Answer:”.

During inference, we enforce greedy decoding (temperature 0.0) and restrict the maximum generation length to 16 tokens to ensure deterministic and concise self-assessment. The generated text is then mapped to a binary confidence indicator $\hatc\in\0,1\$. If the response contains the keyword ”Unknown” (case-insensitive), we assign a confidence score of $\hatc=0$, indicating a lack of knowledge. Conversely, any other generated short answer yields $\hatc=1$.

P-entropy: The Predictive Entropy baseline quantifies epistemic uncertainty by aggregating the token-level Shannon entropy across the entire generated response. For a given instruction of length $L_instr$ and a generated response of length $L_resp$, we extract the probability distribution $P_M(x_t|x_<t)$ over the model’s full vocabulary $V$ at each generation step $t$. The total predictive entropy $H$ is then calculated as the sum of the step-wise entropies:

$$
H=-\sum_t=L_instr+1^L_instr+L_resp\sum_x_t\in VP_M(x_t|x_<t)\log P_M(x_t|x_<t).
$$

IC: The Internal Confidence baseline evaluates epistemic uncertainty by probing the model’s internal hidden states during a forced self-assessment. We instruct the model using the system prompt: ”You are a helpful assistant that assesses whether you can provide an accurate response to a question. Respond only with ’Yes’ or ’No’ to indicate whether you are capable of answering the following question.” The user input is then formatted as `"\<Question\>: \question\ \</Question\>"`. In the retrieval-augmented setting, external documents are prepended to the question using the format: `"Given the following information: context"`.

For a prompt of length $N$ and a model with $L$ layers, let the hidden state at layer $l$ and token position $n$ be $h_n^(l)$. Instead of relying solely on the final output token, these intermediate representations are extracted. Each hidden state is individually passed through the model’s language modeling head to isolate the probability of predicting the ”Yes” target token, denoted as $P(\textYES|h_n^(l))$.To compute the final continuous confidence score, these intermediate probabilities are aggregated using a weighted average. This aggregation is built around an optimal ”decision center”—the specific layer and token position where the model most effectively separates answerable from non-answerable queries. The Internal Confidence ($IC$) is formally defined as:

$$
IC(h)=\sum_n=1^N\sum_l=1^Lw_n^(l)P(\textYES|h_n^(l))
$$

where $w_n^(l).$ denotes the weight assigned to the hidden representation $h_n^(l)$. These weights are determined by a positional attention mechanism that applies a distance-based decay originating from the decision center, placing the highest emphasis on the final $k$ tokens and the deepest layers of the model.

### A.4 Additional experiment results

##### Rank Calculation is sensitive to threshold selection.

As illustrated in Figure 9, truncating at $1e-4$ yields a rank of 269, whereas a naive threshold at $1e-5$ results in a rank of 299, leading to a substantial difference in rank calculation. Both including substantial long-tail components. Therefore, we introduce the stable rank as a consistent and robust measure of effective dimensionality.

![Refer to caption](https://arxiv.org/html/2604.02830v2/figs/overall_with_doc_gen_hidden_state_rank.png)

(a) Hidden State Rank

![Refer to caption](https://arxiv.org/html/2604.02830v2/figs/model_layers_15_mlp_down_proj_weight_metrics.png)

Figure 9: Average eigenvalue spectrum on HotpotQA using Llama-3-8B-Instruct model. It shows that a trivial change in the threshold leads to larger rank fluctuation, motivating the proposal of the stable rank.

![Refer to caption](https://arxiv.org/html/2604.02830v2/x8.png)

Figure 10: Rank ratio across different layers for correctly and incorrectly answered samples.

##### Effects of layer selections.

- (a) Dynamics of the layer-wise rank ratio. To elucidate why a holistic, cross-layer dynamic modelling is necessary, we show the trends for both answerable and unanswerable examples in Figure 10. The distinguishability between answerable and unanswerable queries varies inconsistently across layers — no single layer dominates, and the absolute differences remain marginal throughout. This motivates a cross-layer approach that leverages all layer-wise representations to capture the nuanced dynamics underlying answerability.
- (b) Ablation of using rank ratios from different layers as probe input. We conduct an ablation study on the choice of Transformer layers used as input features for the probe. Figure 11 illustrates the classification performance when utilizing metrics from different layer subsets. The results demonstrate that aggregating signals from all layers yields the optimal performance, while utilizing later layers generally outperforms relying on earlier ones.

##### Acc results of comparison between GRADE and the threshold based methods.

In addition to the AUROC results in Figure 6, we show the Acc in Figure 12. The trends are consistent that our GRADE performs better across all the datasets and models compared to the layer-wise threshold baselines (mean, last and mid shown here).

![Refer to caption](https://arxiv.org/html/2604.02830v2/figs/layer_ablation_bar_chart.png)

Figure 11: Performance comparison across different selected layers.

![Refer to caption](https://arxiv.org/html/2604.02830v2/x9.png)

Figure 12: Comparison results (Acc) among detection threshold (the mean, last-layer, and middle-layer rank ratio, respectively) versus GRADE (rank ratio patterns cross different layers).

[^1]: Large language models for mathematical reasoning: progresses and challenges. In Proceedings of the 18th Conference of the European Chapter of the Association for Computational Linguistics: Student Research Workshop, pp. 225–237. Cited by: §1.

[^2]: Knowledge of knowledge: exploring known-unknowns uncertainty with large language models. In Findings of the Association for Computational Linguistics: ACL 2024, L. Ku, A. Martins, and V. Srikumar (Eds.), Bangkok, Thailand, pp. 6416–6432. External Links: [Link](https://aclanthology.org/2024.findings-acl.383/), [Document](https://dx.doi.org/10.18653/v1/2024.findings-acl.383) Cited by: §2.

[^3]: The internal state of an LLM knows when it’s lying. In The 2023 Conference on Empirical Methods in Natural Language Processing, External Links: [Link](https://openreview.net/forum?id=y2V6YgLaW7) Cited by: §1, §2, §3.

[^4]: Discovering latent knowledge in language models without supervision. In The Eleventh International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=ETKGuby0hcs) Cited by: §1.

[^5]: Towards robust and parameter-efficient knowledge unlearning for LLMs. In The Thirteenth International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=1ExfUpmIW4) Cited by: §1.

[^6]: Query-level uncertainty in large language models. In The Fourteenth International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=11QZITAMUO) Cited by: §1, §2, §4.1, §4.1.

[^7]: Training verifiers to solve math word problems. arXiv preprint arXiv:2110.14168. Cited by: §A.2, §4.1.

[^8]: Fact-checking the output of large language models via token-level uncertainty quantification. In Findings of the Association for Computational Linguistics: ACL 2024, L. Ku, A. Martins, and V. Srikumar (Eds.), Bangkok, Thailand, pp. 9367–9385. External Links: [Link](https://aclanthology.org/2024.findings-acl.558/), [Document](https://dx.doi.org/10.18653/v1/2024.findings-acl.558) Cited by: §2.

[^9]: SPUQ: perturbation-based uncertainty quantification for large language models. In Proceedings of the 18th Conference of the European Chapter of the Association for Computational Linguistics (Volume 1: Long Papers), Y. Graham and M. Purver (Eds.), St. Julian’s, Malta, pp. 2336–2346. External Links: [Link](https://aclanthology.org/2024.eacl-long.143/), [Document](https://dx.doi.org/10.18653/v1/2024.eacl-long.143) Cited by: §2.

[^10]: Dissecting recall of factual associations in auto-regressive language models. In Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing, H. Bouamor, J. Pino, and K. Bali (Eds.), Singapore, pp. 12216–12235. External Links: [Link](https://aclanthology.org/2023.emnlp-main.751/), [Document](https://dx.doi.org/10.18653/v1/2023.emnlp-main.751) Cited by: §3.1.

[^11]: The llama 3 herd of models. arXiv preprint arXiv:2407.21783. Cited by: §3.1, §4.1.

[^12]: The meaning and use of the area under a receiver operating characteristic (roc) curve.. Radiology 143 (1), pp. 29–36. Cited by: §4.1.

[^13]: Mmboundary: advancing mllm knowledge boundary awareness through reasoning step confidence calibration. In Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pp. 16427–16444. Cited by: §2.

[^14]: Measuring massive multitask language understanding. In International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=d7KBjmI3GmQ) Cited by: §A.2, §4.1.

[^15]: Measuring mathematical problem solving with the MATH dataset. In Thirty-fifth Conference on Neural Information Processing Systems Datasets and Benchmarks Track (Round 2), External Links: [Link](https://openreview.net/forum?id=7Bywt2mQsCe) Cited by: §A.2, §4.1.

[^16]: A survey on hallucination in large language models: principles, taxonomy, challenges, and open questions. ACM Transactions on Information Systems 43 (2), pp. 1–55. Cited by: §1.

[^17]: Stable rank and intrinsic dimension of real and complex matrices. SIAM Journal on Matrix Analysis and Applications 46 (3), pp. 1988–2007. Cited by: §3.2.1.

[^18]: Unsupervised dense information retrieval with contrastive learning. Transactions on Machine Learning Research. Note: External Links: ISSN 2835-8856, [Link](https://openreview.net/forum?id=jKN1pXi7b0) Cited by: §A.1.1.

[^19]: LLM internal states reveal hallucination risk faced with a query. In Proceedings of the 7th BlackboxNLP Workshop: Analyzing and Interpreting Neural Networks for NLP, Y. Belinkov, N. Kim, J. Jumelet, H. Mohebbi, A. Mueller, and H. Chen (Eds.), Miami, Florida, US, pp. 88–104. External Links: [Link](https://aclanthology.org/2024.blackboxnlp-1.6/), [Document](https://dx.doi.org/10.18653/v1/2024.blackboxnlp-1.6) Cited by: §1.

[^20]: Survey of hallucination in natural language generation. ACM computing surveys 55 (12), pp. 1–38. Cited by: §1.

[^21]: TriviaQA: a large scale distantly supervised challenge dataset for reading comprehension. In Proceedings of the 55th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), R. Barzilay and M. Kan (Eds.), Vancouver, Canada, pp. 1601–1611. External Links: [Link](https://aclanthology.org/P17-1147/), [Document](https://dx.doi.org/10.18653/v1/P17-1147) Cited by: §A.2, §4.1.

[^22]: Language models (mostly) know what they know. arXiv preprint arXiv:2207.05221. Cited by: §1, §2, §4.1.

[^23]: Overcoming catastrophic forgetting in neural networks. Proceedings of the national academy of sciences 114 (13), pp. 3521–3526. Cited by: §1.

[^24]: Semantic uncertainty: linguistic invariances for uncertainty estimation in natural language generation. In The Eleventh International Conference on Learning Representations, Cited by: §2, §4.1.

[^25]: Natural questions: a benchmark for question answering research. Transactions of the Association for Computational Linguistics 7, pp. 452–466. External Links: [Link](https://aclanthology.org/Q19-1026/), [Document](https://dx.doi.org/10.1162/tacl%5Fa%5F00276) Cited by: §A.2, §4.1.

[^26]: Knowledge boundary of large language models: a survey. In Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pp. 5131–5157. Cited by: §1.

[^27]: Language model uncertainty quantification with attention chain. In Second Conference on Language Modeling, External Links: [Link](https://openreview.net/forum?id=QTrW2HWNXe) Cited by: §2.

[^28]: Teaching models to express their uncertainty in words. Transactions on Machine Learning Research. Note: External Links: ISSN 2835-8856, [Link](https://openreview.net/forum?id=8s8K2UZGTZ) Cited by: §1.

[^29]: Generating with confidence: uncertainty quantification for black-box large language models. Transactions on Machine Learning Research. Note: External Links: ISSN 2835-8856, [Link](https://openreview.net/forum?id=DWkJCSxKU5) Cited by: §2.

[^30]: Towards fully exploiting LLM internal states to enhance knowledge boundary perception. In Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), W. Che, J. Nabende, E. Shutova, and M. T. Pilehvar (Eds.), Vienna, Austria, pp. 24315–24329. External Links: [Link](https://aclanthology.org/2025.acl-long.1184/), [Document](https://dx.doi.org/10.18653/v1/2025.acl-long.1184), ISBN 979-8-89176-251-0 Cited by: §1, §1, §2, §4.1, §5.

[^31]: Kernel language entropy: fine-grained uncertainty quantification for llms from semantic similarities. Advances in Neural Information Processing Systems 37, pp. 8901–8929. Cited by: §2.

[^32]: Steer LLM latents for hallucination detection. In Forty-second International Conference on Machine Learning, External Links: [Link](https://openreview.net/forum?id=UMqNQEPNT3) Cited by: §5.

[^33]: A generalized inverse for matrices. Mathematical Proceedings of the Cambridge Philosophical Society 51 (3), pp. 406–413. External Links: [Document](https://dx.doi.org/10.1017/S0305004100030401) Cited by: §3.2.1.

[^34]: Qwen2.5 technical report. External Links: 2412.15115, [Link](https://arxiv.org/abs/2412.15115) Cited by: §4.1.

[^35]: Qwen3 technical report. External Links: 2505.09388, [Link](https://arxiv.org/abs/2505.09388) Cited by: §A.1.6.

[^36]: Out-of-distribution detection and selective generation for conditional language models. In The Eleventh International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=kJUS5nD0vPB) Cited by: §2.

[^37]: Investigating the factual knowledge boundary of large language models with retrieval augmentation. In Proceedings of the 31st International Conference on Computational Linguistics, pp. 3697–3715. Cited by: §1, §2, §4.1.

[^38]: Stable rank normalization for improved generalization in neural networks and gans. In International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=H1enKkrFDB) Cited by: §3.2.1.

[^39]: Unsupervised real-time hallucination detection based on the internal states of large language models. In Findings of the Association for Computational Linguistics: ACL 2024, pp. 14379–14391. Cited by: §1.

[^40]: Gemma 2: improving open language models at a practical size. arXiv preprint arXiv:2408.00118. Cited by: §4.1.

[^41]: Just ask for calibration: strategies for eliciting calibrated confidence scores from language models fine-tuned with human feedback. In Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing, H. Bouamor, J. Pino, and K. Bali (Eds.), Singapore, pp. 5433–5442. External Links: [Link](https://aclanthology.org/2023.emnlp-main.330/), [Document](https://dx.doi.org/10.18653/v1/2023.emnlp-main.330) Cited by: §1, §2.

[^42]: Self-consistency improves chain of thought reasoning in language models. In The Eleventh International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=1PL1NIMMrw) Cited by: §2, §4.1.

[^43]: Latent space chain-of-embedding enables output-free LLM self-evaluation. In The Thirteenth International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=jxo70B9fQo) Cited by: §1.

[^44]: Chain-of-thought prompting elicits reasoning in large language models. Advances in neural information processing systems 35, pp. 24824–24837. Cited by: §1.

[^45]: Can LLMs express their uncertainty? an empirical evaluation of confidence elicitation in LLMs. In The Twelfth International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=gjeQKFxFpZ) Cited by: §2.

[^46]: Ualign: leveraging uncertainty estimations for factuality alignment on large language models. In Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pp. 6002–6024. Cited by: §1.

[^47]: HotpotQA: a dataset for diverse, explainable multi-hop question answering. In Proceedings of the 2018 Conference on Empirical Methods in Natural Language Processing, E. Riloff, D. Chiang, J. Hockenmaier, and J. Tsujii (Eds.), Brussels, Belgium, pp. 2369–2380. External Links: [Link](https://aclanthology.org/D18-1259/), [Document](https://dx.doi.org/10.18653/v1/D18-1259) Cited by: §A.2, §3.2.2, §4.1.

[^48]: Stable-rag: mitigating retrieval-permutation-induced hallucinations in retrieval-augmented generation. arXiv preprint arXiv:2601.02993. Cited by: §1.

[^49]: Relying on the unreliable: the impact of language models’ reluctance to express uncertainty. In Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), L. Ku, A. Martins, and V. Srikumar (Eds.), Bangkok, Thailand, pp. 3623–3643. External Links: [Link](https://aclanthology.org/2024.acl-long.198/), [Document](https://dx.doi.org/10.18653/v1/2024.acl-long.198) Cited by: §2.