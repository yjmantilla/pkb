---
title: "Automated Interpretability Metrics Do Not Distinguish Trained and Random Transformers"
source: "https://arxiv.org/html/2501.17727v2"
author:

created: 2026-04-27
description:
tags:
  - "clippings"
  - "citable"
  - "mechinterp"
  - "interpretability"
---
Thomas Heap  
University of Bristol  
Bristol, UK  
thomas.heap@bristol.ac.uk  
&Tim Lawson  
University of Bristol  
Bristol, UK  
&Lucy Farnik  
University of Bristol  
Bristol, UK  
&Laurence Aitchison  
University of Bristol  
Bristol, UK

###### Abstract

Sparse autoencoders (SAEs) are widely used to extract sparse, interpretable latents from transformer activations. We test whether commonly used SAE quality metrics and automatic explanation pipelines can distinguish trained transformers from randomly initialized ones (e.g., where parameters are sampled i.i.d. from a Gaussian). Over a wide range of Pythia model sizes and multiple randomization schemes, we find that, in many settings, SAEs trained on randomly initialized transformers produce auto-interpretability scores and reconstruction metrics that are similar to those from trained models. These results show that high aggregate auto-interpretability scores do not, by themselves, guarantee that learned, computationally relevant features have been recovered. We therefore recommend treating common SAE metrics as useful but insufficient proxies for mechanistic interpretability and argue for routine randomized baselines and targeted measures of feature ‘abstractness.’

## 1 Introduction

Sparse autoencoders (SAEs) are a popular tool in mechanistic interpretability research, with the aim of disentangling the internal representations of neural networks by learning sparse, interpretable features from network activations [^14] [^53] [^12] [^7]. An autoencoder with a high-dimensional hidden layer is trained to reconstruct activations while enforcing sparsity [^18] [^54] [^32], with the aim of discovering the underlying concepts or ‘features’ learned by the network [^46] [^55]. Developing better SAEs relies on quantitative evaluation metrics like auto-interpretability scores that measure agreement between generated explanations and activation patterns [^5] [^48] [^25].

For an interpretability method to be considered robust, its evaluation metrics should distinguish features learned through training from artifacts arising from the data or model architecture. A key sanity check is therefore to compare the method’s output on a trained model against a strong null model, such as one with randomly initialized weights [^1]. We apply this sanity check to SAEs and find that several common quantitative metrics do not always clearly distinguish between the trained and randomized settings. In particular, we found that SAEs trained on transformers with random parameters can yield latents with auto-interpretability scores [^5] [^48] that are surprisingly similar to those from a fully trained model.

This result raises important questions about what we can glean from applying these metrics of SAE quality. High auto-interpretability scores alone do not guarantee that an SAE has identified complex, learned computations. Instead, such scores may sometimes reflect simpler statistical properties of the training data [^13] or architectural inductive biases that are present even without training. Indeed, one could argue that a randomly initialized network still performs a basic form of computation, such as preserving or amplifying the sparse structure of its inputs (Section 4). From this perspective, SAEs might faithfully interpret this simple, inherent computation.

While some SAE features from trained models clearly arise from learned computation, the commonly used aggregate metrics are often insufficient for determining whether a given SAE has learned these more complex features. These results have important implications for mechanistic interpretability research. In particular, we suggest that more rigorous methods to distinguish between artifacts and genuinely learned computations are needed, and that interpretability techniques should be carefully validated against appropriate null models.

Finally, we speculate about why these patterns might emerge. At a high level, there are two hypotheses: (1) the input data already exhibits superposition, and randomly initialized neural networks largely preserve this superposition; and (2) randomly initialized neural networks amplify or even introduce superposed structure to the input data (e.g., given dense input generated i.i.d. from a Gaussian). We present toy models to demonstrate the plausibility of these hypotheses in Section 4 but defer conclusions as to the mechanism responsible to future work.

## 2 Related Work

##### Sparse dictionary learning

Under a different name, ‘superposition’ in visual data is one of the foundational observations of computational neuroscience. [^44] [^45] showed that the receptive fields of simple cells in the mammalian visual cortex can be explained as a result of sparse coding, i.e., representing a relatively large number of signals (sensory information) by simultaneously activating a small number of elements (neurons). Coding theory offers a perspective on efforts to extract the ‘underlying signals’ responsible for neural network activations [^38].

Sparse dictionary learning (SDL) approximates a set of input vectors by linear combinations of a relatively small number of learned basis vectors. The learned basis is usually overcomplete: it has a greater dimension than the inputs. SDL algorithms include Independent Component Analysis (ICA), which finds a linear representation of the data such that the components are maximally statistically independent [^2] [^23]. Sparse autoencoders (SAEs) are a simple neural network approach [^31] [^42] [^37]. Typically, an autoencoder with a single hidden layer that is many times larger than the input activation vectors is trained with an objective that imposes or incentivizes sparsity in its hidden layer activations to try to find this structure. A latent is a single neuron (dimension) in the autoencoder’s hidden layer.

##### Mechanistic interpretability

Recently, it has become common to understand ‘features’ or concepts in language models as low-dimensional subspaces of internal model activations [^46] [^55] [^15]. If such sparse or ‘superposed’ structure exists, we expect to be able to ‘intervene on’ or ‘steer’ the activations, i.e., to modify or replace them to express different concepts and so influence model behavior [^39] [^58] [^21] [^36] [^43].

SAEs are a popular approach for discovering features, where one typically trains a single autoencoder to reconstruct the activations of a single neural network layer, e.g., the transformer residual stream [^53] [^12] [^7]. Many SAE architectures have been suggested, which commonly vary the activation function applied after the linear encoder [^37] [^18] [^51] [^32]. SAEs have also been trained with different objectives [^6] [^16] and applied to multiple layers simultaneously [^57] [^29] [^34].

Besides reconstruction errors and preservation of the underlying model’s performance, SAEs have been evaluated according to whether they capture specific concepts [^20] [^18] or factual knowledge [^22] [^10], and whether these can be used to ‘unlearn’ concepts [^26].

##### Automatic neuron description

SAEs often learn tens of thousands of latents, which are infeasible to describe by hand. [^57] find the tokens that maximally activate a dictionary element from a text dataset and manually inspect activation patterns. Instead, researchers typically collect latent activation patterns over a text dataset and prompt a large language model to explain them [^5] [^17]. These methods have been widely adopted [^12] [^7] [^18] [^54] [^32].

[^5] generate an explanation for the activation patterns of a language-model neuron over examples from a dataset, simulate the patterns based on the explanation, and score the explanation by comparing the observed and simulated activations. This method is commonly known as auto-interpretability (as in self-interpreting). [^48] introduce classification-based measures of the fidelity of automatic descriptions that are inexpensive to compute relative to simulating activation patterns and an open-source pipeline to compute these measures. [^11] use best-of- $k$ sampling to generate multiple explanations based on different subsets of the examples that maximally activate a neuron. Importantly, they fine-tune Llama-3.1-8B-Instruct on the top-scoring explanations to obtain inexpensive ‘explainer’ and ‘simulator’ models.

##### Polysemanticity

[^30] noted that neurons may become polysemantic incidentally. A polysemantic neuron (basis dimension) of a network layer represents multiple interpretable concepts [^14] [^52]; unsurprisingly, individual neurons in a randomly initialized network may be polysemantic. By contrast, our work studies *superposition* [^14] [^9], which pertains to the representations learned across a whole network layer as opposed to any individual neuron. In particular, superposition allows a network layer as a whole to represent a larger number of (sparse) features than the layer has (dense) neurons by sparse coding (only a few concepts are active at a time, i.e., a given token position).

##### Training only the embeddings

[^59] showed that transformers learn surprising algorithmic capabilities when only the embeddings are trained and no other parameters. These results demonstrate that the behavior of a randomly initialized transformer can be shaped to a surprising extent by training only a few parameters. However, our setting is very different: besides considering SAEs, we randomize *all* the parameters, including the embeddings, in our ‘Step-0’ and ‘Re-randomized incl. embeddings’ variants. Our ‘Re-randomized excl. embeddings’ variant uses pre-trained embeddings, but we do not train those embeddings with fixed, randomized weights. Instead, we freeze the pre-trained embeddings and randomize the other weights (Section 3).

##### Random transformers for board games

[^27] found that SAEs were considerably better at extracting meaningful structure from chess games using pre-trained transformers, as opposed to those with random weights. However, the data from board games is wildly different from language data. In particular, there is reason to expect that language is sparse (e.g., a particular concept such as ‘serendipitous’ appears only rarely), and that this sparse structure is ‘aligned’ with conceptual meaning. In contrast, in board games, this is not necessarily true: a useful concept such as a knight fork does not necessarily turn up sparsely in board games.

##### Random one-layer transformers

[^7] found that auto-interpretability scores discriminated effectively between random and trained one-layer transformers. Similarly, we found that auto-interpretability scores for randomized models were relatively low for smaller models (e.g., Pythia-70m) but that the gap was narrowed for larger models (e.g., Pythia-6.9b).

## 3 Results

![Refer to caption](https://arxiv.org/html/2501.17727v2/x1.png)

Figure 1: ‘Fuzzing’ ROC curve vs. layer for Pythia-6.9b (100 latents sampled per SAE). The trained model (gray line) and randomized variants (colored) overlap, whereas the control (black) is near chance (dotted). This suggests aggregate AUROC alone is insufficient to attribute latents to learned computation. See Figure 2 for other metrics/model sizes and Appendix E for multiple random seeds.

We trained per-layer SAEs on the residual stream activation vectors of transformer language models from the Pythia suite, with between 70M and 7B parameters [^4]. We compared SAEs trained on different variants of the underlying transformers:

- Trained: The usual, trained model.
- Re-randomized incl. embeddings: All the model parameters, including the embeddings, are re-initialized by sampling Gaussian noise with mean and variance equal to the values for each of the original, trained weight matrices.
- Re-randomized excl. embeddings: As above, except the embedding and unembedding weight matrices are not re-initialized, i.e., are the same as the original, trained model.
- Step-0: For Pythia models, the step0 revisions are available, which are the original model weights at initialization, i.e., before any learning [^4].
- Control: The original, trained model, except where the input token embeddings are replaced at inference time by sampling i.i.d. standard Gaussian noise for each token, such that a given token does not have a consistent embedding vector. For this variant, we expect auto-interpretability to perform at the level of chance.

For our primary experiments, we trained SAEs on 100M tokens from the RedPajama dataset [^56] using an activation buffer size of 10M tokens (see Appendix C for a subset of experiments that demonstrate similar results with SAEs trained on one billion tokens). For models with fewer than 410M parameters, we trained an SAE at every layer; for Pythia-1b, we trained SAEs at every second layer; and for Pythia-6.9b, we trained SAEs at every fourth layer.

Unless otherwise stated, we trained $k$ -sparse autoencoders (also known as TopK SAEs; [^37] [^18]), with an expansion factor of $R=64$ and sparsity $k=32$. We confirm that our results are robust with respect to these hyperparameters by training SAEs on Pythia-160m with expansion factors equal to powers of 2 between 16 and 128, and sparsities of 16 and 32 (Figure 18). The training implementation is based on [^3]; our evaluations are based on [^8] and [^25].

##### Auto-interpretability

Feature explanations that identify a concept can be input to a classifier that predicts whether the concept appears in the text inputs. Such a classifier may be evaluted by traditional metrics, like the area under the receiver operating characteristic (ROC) curve (AUROC). [^48] proposed ‘fuzzing’ and ‘detection’ classification tasks to evaluate feature explanations. For ‘fuzzing’ scoring, both positive and negative examples of tokens (i.e., with non-zero and zero activation values, respectively) for a given latent are delimited with special characters, and a language model is prompted to identify which examples have been correctly delimited for the latent given its explanation. For ‘detection’, a language model is asked to identify which examples contain activating tokens for each feature. [^5] originally proposed ‘simulation’ scoring, based on the correlation between predicted and observed activations, but this method is expensive to compute.

Except where noted, we report ‘fuzzing’ scores as a measure of auto-interpretability, because this measure has been demonstrated to correlate with simulation scoring [^48]. We include similar AUROC curves for the ‘detection’ scoring method in Appendix B. For each trained SAE (i.e., underlying model, variant, and layer), we randomly sampled 100 features to obtain auto-interpretability scores. The implementation is based on [^48]. We use the Meta-Llama-3.1-70B-Instruct-AWQ-INT4 model to generate explanations and make predictions (larger than the 8B models used by [^11] and open-source, unlike [^5]).

We found that the auto-interpretability scores were far more similar between the trained and randomized models than with the control (Figures 1 and 2). The similarity between the ROC scores for trained and randomized transformers demonstrates that ‘fuzzing’ auto-interpretability alone, applied to SAE latent explanations, may not meaningfully distinguish between these underlying models.

##### Evaluation

![Refer to caption](https://arxiv.org/html/2501.17727v2/x2.png)

Figure 2: Comparison of sparse autoencoder performance across Pythia models (70M to 6.9B parameters). The different SAE variants show remarkably similar trends across model scales, with larger models exhibiting more consistent behavior across layers. All variants save for control achieve comparable performance despite fundamentally different initialization approaches.

We considered standard SAE evaluation metrics alongside the auto-interpretability AUROC for Pythia models with between 70M and 6.9B parameters. As above, we broadly found that the randomly initialized and re-randomized models (Figure 2; blue, green, orange lines) were more similar to the trained model (Figure 2; gray lines) than to our control (Figure 2; black lines).

Notably, the cosine similarity between the original activation and the SAE reconstruction and explained variance are often far lower for the random control than the other models, and its reconstruction errors tend to increase across layers while the remaining variants decrease. For the random control, this can perhaps be explained by the fact that a Gaussian is the highest entropy distribution with fixed mean and variance [^24]; we speculate that Gaussian vectors are the ‘least structured’, in some sense, and thus hardest for SAEs to reconstruct. As Gaussian-distributed activations are propagated through successive layers, we would expect the activations to become less Gaussian and perhaps more ‘sparse’, i.e., easier to reconstruct (Section 4).

Interestingly, the randomized variants (blue and orange lines) are more similar to the trained model than the variant at initialization (green line). This is especially evident if we look at the $L^1$ norm values in larger models. We speculate that this pattern arises because parameter norms may differ greatly between a trained model and its state at initialization. In contrast, our randomization procedure was specifically designed to preserve parameter norms with respect to the trained model. The scale of parameters at different layers may be important, e.g., to control the growth of activations as they progress through the residual stream [^35]. In the AUROC plots, we find that for all but the control variant, AUROC increases with model size. We speculate that features become more specific as SAE size increases: in smaller SAEs, each latent must explain more of the input, making classification tasks easier for larger SAEs.

Figure 2 (row five) shows the cross-entropy (CE) loss score, or loss recovered, against model layer. This is the increase in the loss when the original model activations are replaced by their SAE reconstructions, divided by the increase when the activations are replaced by zeros (‘ablated’). The results show that the ‘trained’ variant SAEs perform similarly to others from the literature [^28] [^50] [^40]. Importantly, the CE loss score only makes sense for the trained variant: for any of the randomized variants, the loss is very poor, regardless of whether the original or reconstructed activations are used.

##### Latent explanation complexity

Despite sometimes similar auto-interpretability scores and evaluation metrics, we had expected that SAEs applied to trained vs. randomized transformers would discover qualitatively different features. In particular, we expected SAEs trained on the randomized variants to learn relatively simple features based on characteristics of the input text, but not more complex, abstract features as with trained transformers [^54]. For qualitative examples, we provide a random sample of features and the corresponding maximally activating dataset examples for each variant of Pythia-6.9b in Appendix J, and more detailed information in Appendix L.

Anecdotally, we have observed that a significant proportion of SAE latents have non-zero activations only on a single token or a small number of distinct tokens within a text dataset [^33] [^13]. Hence, a simple measure of the complexity of an SAE latent given a set of maximally activating examples is the degree to which the latent activates on a single token ID or multiple distinct IDs. Specifically, we quantify the number of token IDs in terms of the entropy of the observed distribution of latent activations over tokens: the greater the entropy, the more ‘spread out’ the latent activations, and the less token-specific the latent. We take this distribution to be the total latent activation per token across the set of maximally activating examples used to generate explanations for auto-interpretability. We show the relationship between entropy and ‘fuzzing’ AUROC score for individual latents in Appendix H.

We include the entropy of the observed distributions of latent activations over token IDs in the last row of Figure 2. The negative control variant displays a consistently high entropy, which is to be expected given that the embedding for a given token ID is sampled i.i.d. from a Gaussian on each occurrence of the token, i.e., a token does not have a consistent embedding vector (Section 3). For the trained variant, the entropy increases across layers, i.e., the further into the model, the less likely the maximally activating examples for each latent contain activations concentrated on a single token. This is also expected: at later layers, we expect more abstract features that are less similar to token embeddings. Finally, the entropy for randomized models tends to be lower than for either the trained or control variants, indicating that latents are activated specifically at one or a few IDs.

In combination with the preceding results, this suggests that standard SAE quality and auto-interpretability metrics are missing an important aspect of SAE features: their ‘abstractness’. While the token distribution entropy is not a direct measure of ‘abstractness’, it suggests that the randomized variants, viewed in the context of their similar auto-interpretability scores to the trained variant, remain able to learn simple, single-token features. However, unlike the trained variant, the features of the randomized variants do not become more complex as the layer index increases.

## 4 A toy model of superposition in random networks

We speculated in Section 1 that the apparently high degree of sparsity and interpretability in the activations of randomized transformers might be because the input data exhibits superposition, which neural networks preserve, or neural networks somehow amplify or even introduce superposition into the input data. In this section, we examine both possibilities through the lens of toy models. We find some evidence to support each potential cause, but we leave the question of which predominates in the case of randomized transformers and the results detailed in the main text to future work.

![Refer to caption](https://arxiv.org/html/2501.17727v2/figures/sparse_inputs.png)

(a) Superposed inputs

### 4.1 Matrix multiplications preserve superposition

First, we consider a simplified model to demonstrate that multiplication by a weight matrix $W$ preserves superposition. Imagine that we generate superposed input data $x$ by first generating $n_s$ i.i.d. ‘sparse’ features $z$ from a heavy-tailed Lomax distribution $z\sim\operatornameLomax(\alpha,\lambda)$. We can project the higher-dimensional, sparse $z$ down to lower-dimensional, dense $x$ with a matrix $D$, then add Gaussian noise with a small variance $\Sigma$, $x\sim\mathcalN(x;Dz,\Sigma)$. Importantly, if we multiply $x$ by some matrix $W$, then $x^\prime=Wx$ is *also* superposed: it is generated by the same model as $x$, except with different noise covariances and mappings from $z$ to $x$, namely $x^\prime\sim\mathcalN(z;WDz,W\Sigma W^T)$.

We can see that the same intuition might extend to neural networks with nonlinearities by visualizing the results of passing the dense activations through a simple feed-forward network (MLP). Figure 3 shows an example where $n_s=3$ sparse features are projected down to $n_d=2$ dense features, and the MLP outputs appear superposed despite the non-linearity. Moreover, it suggests that NNs might amplify superposition rather than only preserving it: comparing the inputs (Figures 3(a) and 3(b)) to the outputs (Figures 3(d) and 3(c)), there are fewer points between the ‘arms’ of the outputs.

### 4.2 Do random NNs preserve or amplify superposition?

We investigated this suggestion by generating toy data with the same procedure as [^53], i.e., sampling ground-truth features on a hypersphere and generating correlated feature coefficients such that only a small number are active (Appendix I.1). We then passed these inputs to a two-layer MLP at initialization and trained SAEs on both the inputs and outputs individually. As a control, we used Gaussian-distributed inputs with a mean and standard deviation equal to the superposed toy data. We used standard SAEs with an $L^1$ sparsity penalty (Appendix I.2).

![Refer to caption](https://arxiv.org/html/2501.17727v2/x3.png)

Figure 4: The mean max cosine similarity (MMCS) between the features learned by a standard SAE (decoder weight vectors) and the data-generating features against the L 1 L^1 penalty coefficient in the training loss, following 53. There is a ‘Goldilocks zone’ where SAEs near-perfectly recover the data-generating features, given enough latents to represent them.

Following [^53], we confirmed that SAEs can recover the ground-truth features that generated the data (Figure 4). In particular, we measured the mean max cosine similarity (MMCS): for every data-generating feature, we found its maximum cosine similarity with the features learned by the SAE (its decoder weight vectors) and took the average over data-generating features. However, the MMCS only applies to the MLP inputs, where we have access to the data-generating features – a different approach is required to analyze the MLP outputs. To this end, we took the ability of SAEs to achieve low reconstruction error with high sparsity as a proxy for the degree to which the training data exhibits superposition. Specifically, we vary the $L^1$ penalty coefficient to obtain Pareto frontiers of the explained variance against sparsity measures (Figure 5(a)).

As expected, we found that SAEs achieved much greater sparsity at a given level of explained variance for the superposed inputs relative to the Gaussian control (Figure 5(a); orange and blue-green). Interestingly, the difference between the superposed outputs, i.e., the outputs of the MLP given the superposed inputs, and the Gaussian outputs is much smaller, with only slightly greater sparsity at a given level of explained variance. This suggests that the outputs of randomly initialized MLPs have a relatively high level of sparsity insensitive to the input distribution. We consider other sparsity measures and hyperparameters in Appendix I.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x4.png)

(a) Toy datasets following 53.

### 4.3 Do token embeddings exhibit superposition?

To the extent that randomly initialized neural networks preserve or amplify superposition, our results (Section 3) could be explained by the degree to which the inputs to transformer language models exhibit superposition. We study this question by applying the procedure described in Section 4.2 to language data. In particular, we train SAEs on pre-trained GloVe word vectors, the embedding matrices of Pythia models, the results of passing these inputs to a randomly initialized two-layer MLP, and Gaussian controls. The setup is unchanged from Section 4.2, except that the number of data points is fixed by the number of word embeddings or tokens, and we use a single random seed.

We find that the gap between the Pareto frontiers of the GloVe word vectors and the corresponding Gaussian controls (Figure 5(b)) is smaller than that observed for the toy superposed datasets described in Section 4.2 (Figure 5(a)). More interestingly, we again see that the Pareto frontiers for both inputs improve when they are passed to a randomly initialized two-layer MLP, emphasizing the possibility that random NNs ‘sparsify’ their inputs (i.e., increase the degree of apparent superposition).

## 5 Limitations

In this work, we demonstrate that auto-interpretability measures can produce apparently meaningful, interpretable results for SAEs trained on randomly initialized models, which are unlikely to exhibit computationally interesting features. Given the impossibility of testing across all datasets and model architectures, we strategically focused on the Pythia family of models, widely adopted in mechanistic interpretability research [^47] [^19] [^41], and the RedPajama-V2 dataset, representing typical pre-training data for language models and SAEs.

While we used the default model for generating explanations in the EleutherAI auto-interpretability framework [^8], exploring alternative models could yield valuable insights into aggregate behaviors and the quality of generated explanations. Importantly, we do not claim that SAEs fail to capture information from trained Transformers above and beyond randomly initialized transformers; only that aggregate auto-interpretability measures do not necessarily indicate the existence of interesting underlying features.

## 6 Conclusion

In this work, we applied sparse autoencoders to both trained and randomly initialized transformers and evaluated them with a suite of common quantitative metrics. Our central empirical finding is that, under certain conditions, these metrics – particularly aggregate auto-interpretability scores – can be surprisingly similar in both settings. While we observe that features derived from trained transformers are qualitatively more complex and abstract, especially in later layers, these aggregate metrics often fail to capture this distinction.

This result does not imply that SAEs trained on real models fail to learn meaningful computational features. Rather, it reveals a limitation in our current evaluation methods. High aggregate auto-interpretability scores are insufficient proof for the discovery of complex, learned computations: they may instead reflect simpler structure inherent in the data or model architecture that is preserved even by random weights. Our analysis of token distribution entropy, while preliminary, serves as a proof-of-concept: it successfully revealed differences in feature ‘abstractness’ that aggregate auto-interpretability scores missed. Future work should focus on developing more robust metrics that can quantify the computational significance of the features SAEs discover. Our work reaffirms the importance of benchmarking interpretability techniques against strong, appropriately constructed null models, such as the randomly initialized transformers used here. Without such baselines, it is difficult to confidently attribute discovered features to the process of learning.

## References

## Appendix A Broader Impact

This work investigates a method currently used for mechanistic interpretability of LLMs, yielding results that challenge certain assumptions about sparse autoencoders. By demonstrating that SAEs can produce similar aggregate auto-interpretability scores for both random and trained transformers, our findings raise important questions about what these SAE evaluation methods are actually capturing.

By better understanding the metrics of SAE quality, we hope that this work will contribute to a more informed search of better SAE-like methods and thus help to make these models more interpretable and to mitigate the potential harm these models could cause. Since our work is an empirical study of the capabilities of a presently used method, and it shows that the method provides interpretation of both random and trained transformers, we think the risk that this work could lead to negative social impact is minimal.

## Appendix B Auto-interpretability ROC curves

Figures 6, 8, 12 show the similarity between ‘fuzzing’ AUROC for the trained and randomized SAEs for the 70M, 160M, and 1B models. Figures 7, 9, 13, show the similarity between ‘detection’ AUROC for the trained and randomized SAEs for the 70M, 160M, and 1B models.

### B.1 Pythia 70m

![Refer to caption](https://arxiv.org/html/2501.17727v2/x5.png)

Figure 6: ROC curves for ‘fuzzing’ auto-interpretability for Pythia-70m over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, as well as the overall degradation in performance as the layer index increases.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x6.png)

Figure 7: ROC curves for ‘detection’ auto-interpretability for Pythia-70m over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, as well as the overall degradation in performance as the layer index increases.

### B.2 Pythia 160m

![Refer to caption](https://arxiv.org/html/2501.17727v2/x7.png)

Figure 8: ROC curves for ‘fuzzing’ auto-interpretability for Pythia-160m over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, as well as the overall degradation in performance as the layer index increases.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x8.png)

Figure 9: ROC curves for ‘detection’ auto-interpretability for Pythia-160m over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, as well as the overall degradation in performance as the layer index increases.

### B.3 Pythia 410m

![Refer to caption](https://arxiv.org/html/2501.17727v2/x9.png)

Figure 10: ROC curves for ‘fuzzing’ auto-interpretability for Pythia-410m over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, as well as the overall degradation in performance as the layer index increases. The auto-interpretability scores here fail to distinguish between trained and randomized models.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x10.png)

Figure 11: ROC curves for ‘detection’ auto-interpretability for Pythia-410m over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, as well as the overall degradation in performance as the layer index increases.

### B.4 Pythia-1b

![Refer to caption](https://arxiv.org/html/2501.17727v2/x11.png)

Figure 12: ROC curves for ‘fuzzing’ auto-interpretability for Pythia-1b over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, although here we do not observe an overall degradation in quality.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x12.png)

Figure 13: ROC curves for ‘detection’ auto-interpretability for Pythia-1b over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants, although here we do not observe an overall degradation in quality.

### B.5 Pythia 6.9b

![Refer to caption](https://arxiv.org/html/2501.17727v2/x13.png)

Figure 14: ROC curves for ‘detection’ auto-interpretability for Pythia-6.9b over 100 SAE latents. These results demonstrate the similarity in performance between the SAE variants.

## Appendix C Effect of increased training data

For our primary experiments, we trained SAEs on 100M tokens (Section 3). We verified that our results were not explained by a lack of sufficient training data by repeating a subset of these experiments with SAEs trained on 1B tokens from the RedPajama dataset (Figure 15).

![Refer to caption](https://arxiv.org/html/2501.17727v2/x14.png)

Figure 15: Evaluation metrics for SAEs trained with one billion tokens on the Pythia-70m and 410m models. These results correspond to columns of Figure 2, which show the same evaluation metrics for SAEs trained on 100M tokens, and qualitatively similar behavior.

## Appendix D Effect of decreased training data for Pythia-1B

![Refer to caption](https://arxiv.org/html/2501.17727v2/x15.png)

Figure 16: Evaluation metrics for SAEs trained with 1M and 1B tokens on Pythia-1b. The explained variance and CE loss score are significantly lower for the 1M model, showing that the SAEs are under-trained. Average auto-interpretability scores are slightly lower for the earliest layers, but decline sharply with increasing layer. The trends in auto-interpretability and token distribution entropy with layer index are consistent with other SAEs.

## Appendix E Uncertainty plots for Pythia-70m

We computed uncertainty for our evaluation metrics on Pythia-70m using five random seeds.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x16.png)

Figure 17: Uncertainty for Pythia-70m metrics computed using five random seeds.

## Appendix F Effect of SAE hyperparameters for Pythia-160m

![Refer to caption](https://arxiv.org/html/2501.17727v2/x17.png)

Figure 18: Robustness of SAE performance to hyperparameter selection. Standard evaluation metrics remain stable across a wide range of expansion factors R (16 to 128) and sparsities k (16 to 32), with all initialization strategies maintaining their relative performance ordering. This stability suggests that moderate hyperparameter values (e.g., expansion factor = 64 R=64, sparsity 32 k=32 ) suffice.

## Appendix G Effect of SAE hyperparameters for Pythia-1b

![Refer to caption](https://arxiv.org/html/2501.17727v2/x18.png)

Figure 19: Evaluation metrics for SAEs trained on the Pythia-1b model with different hyperparameters, including the main results from Figure 2. SAEs with a very small expansion factor R = R=2 and sparsity k 4 k=4 are clearly distinguished from our default hyperparameters by the explained variance and CE loss score. Importantly, the auto-interpretability scores of these SAEs remain similar to those trained with default hyperparameters on either trained or randomised models.

## Appendix H Token distribution entropy vs. auto-interpretability

![Refer to caption](https://arxiv.org/html/2501.17727v2/x19.png)

Figure 20: Scatter plots of the per-latent token distribution entropy against ‘fuzzing’ AUROC (auto-interpretability score) for SAEs trained on multiple layers of the Pythia-6.9b model. Each point corresponds to a single latent, taken from the sample of latents used to compute the aggregate metrics displayed in Figures 1 and 2.

Figure 20 clearly distinguishes the negative control, randomized variants, and the trained variant:

- Control: Latents have a consistently high entropy (i.e., max activating examples with activation patterns spread across many tokens) and low auto-interpretability score (i.e., generated explanations that fail to adequately explain these activation patterns). No correlation between the two variables is evident.
- Randomized: For each of the randomization schemes described in Section 3, we see a negative correlation between entropy and auto-interpretability: in general, the wider variety of tokens for which a latent is activated, the less well the latent’s activation patterns are explained by its generated explanation.
- Trained: There is a weaker correlation between the two variables. Crucially, in addition to the broad trend observed for the randomized variants, we also see latents with high entropy *and* auto-interpretability. Some latents have activation patterns that are spread across multiple tokens, which are nevertheless consistent with the latent’s generated explanation.

These results are consistent with the view that aggregate auto-interpretability scores obscure the differences between SAEs based on trained and randomized models. While randomized models with consistent token embeddings can produce ‘single-token’ features, whose activation patterns are easy to explain, only Transformers trained on natural language produce more complex semantic features.

## Appendix I A toy model of superposition

In Section 4, we trained SAEs on toy data designed to exhibit superposition [^53] and GloVe word vectors [^49]. In this section, we detail the data-generation procedure and training setup.

### I.1 Data generation

First, we construct ground-truth features by sampling $n_s$ points on an $n_d$ -dimensional hypersphere.

For each sample, we determine the feature coefficients by generating $A\in\mathbbR^n_s\timesn_s$ where $A_ij\sim\mathcalN(0,1)$, defining a covariance matrix $\Sigma=AA^\mathsfT$, sampling $\vec\alpha\in\mathbbR^n_s$ where $\alpha_i\sim\mathcalN(\vec0,\Sigma)$, projecting $\alpha_i$ onto the c.d.f. of $\mathcalN(0,1)$, decaying $\alpha_i\to\alpha_i^\lambda i$ where $\lambda\in\mathbbR$, normalizing $\alpha_i\to m\alpha_i\big/n_s\sum_j\alpha_j$ where $m\in\mathbbR$, and performing $n_s$ independent Bernoulli trials with $p=\alpha_i$. Finally, we multiply the trial outcomes by $n_s$ independent samples from a continuous uniform distribution $\mathcalU_[0,1)$.

The parameter $\lambda$ determines how sharply the frequency of nonzero ground-truth feature coefficients decays with the feature index $i$. The parameter $m$ is the expected value of the number of nonzero feature coefficients for each sample.

Like [^53], we choose $n_s=512$, $n_d=256$, $\lambda=0.99$, and $m=5$. We include a Python implementation of this procedure in Figure 21.

### I.2 Training

The SAEs described in Section 4 comprise a linear encoder with a bias term, a ReLU activation function, and a linear decoder without a bias term. We use orthogonal initialization for the decoder weights and normalize the decoder weight vectors before each training step.

The training loss is the mean squared error (MSE) between the input and decoded vectors, plus the mean $L^1$ norm of the encoded vectors multiplied by a coefficient, which we vary between 1e-3 and 100.

For the toy data, we train for 100 epochs on 10K data points with 10 random seeds. For the word vectors, we train for 100 epochs on 400K data points with 1 random seed. In both experiments, we reserve 10% of the data points as a validation set, which we use to compute evaluation metrics.

The MLPs described in Section 4 comprise two layers (i.e., one hidden layer) and a ReLU activation function. The input and output sizes are both equal to $n_d$, and the hidden size is $4n_d$. We loosely based these choices on the feed-forward network components of transformer language models.

[⬇](data:text/plain;base64,ZGVmIGdlbmVyYXRlX3NoYXJrZXkoCiAgICBudW1fc2FtcGxlczogaW50LAogICAgbnVtX2lucHV0czogaW50LAogICAgbnVtX2ZlYXR1cmVzOiBpbnQsCiAgICBhdmdfYWN0aXZlX2ZlYXR1cmVzOiBmbG9hdCwKICAgIGxhbWJkYV9kZWNheTogZmxvYXQsCikgLT4gdHVwbGVbVGVuc29yLCBUZW5zb3JdOgogICAgIiIiCiAgICBBcmdzOgogICAgICAgIG51bV9zYW1wbGVzIChpbnQpOiBUaGUgbnVtYmVyIG9mIHNhbXBsZXMgdG8gZ2VuZXJhdGUuCiAgICAgICAgbnVtX2lucHV0cyAoaW50KTogVGhlIG51bWJlciBvZiBpbnB1dCBkaW1lbnNpb25zLgogICAgICAgIG51bV9mZWF0dXJlcyAoaW50KTogVGhlIG51bWJlciBvZiBncm91bmQgdHJ1dGggZmVhdHVyZXMuCiAgICAgICAgYXZnX2FjdGl2ZV9mZWF0dXJlcyAoZmxvYXQpOiBUaGUgYXZlcmFnZSBudW1iZXIgb2YKICAgICAgICAgICAgZ3JvdW5kIHRydXRoIGZlYXR1cmVzIGFjdGl2ZSBhdCBhIHRpbWUuCiAgICAgICAgbGFtYmRhX2RlY2F5IChmbG9hdCk6IFRoZSBleHBvbmVudGlhbCBkZWNheSBmYWN0b3IgZm9yCiAgICAgICAgICAgIGZlYXR1cmUgcHJvYmFiaWxpdGllcy4KICAgICIiIgogICAgZmVhdHVyZXMgPSB0b3JjaC5yYW5kbihudW1faW5wdXRzLCBudW1fZmVhdHVyZXMpCiAgICBmZWF0dXJlcyAvPSB0b3JjaC5ub3JtKGZlYXR1cmVzLCBkaW09MCwga2VlcGRpbT1UcnVlKQoKICAgIGNvdmFyaWFuY2UgPSB0b3JjaC5yYW5kbihudW1fZmVhdHVyZXMsIG51bV9mZWF0dXJlcykKICAgIGNvdmFyaWFuY2UgPSBjb3ZhcmlhbmNlIEAgY292YXJpYW5jZS5UCiAgICBjb3JyZWxhdGVkX25vcm1hbCA9IE11bHRpdmFyaWF0ZU5vcm1hbCgKICAgICAgICB0b3JjaC56ZXJvcyhudW1fZmVhdHVyZXMpLCBjb3ZhcmlhbmNlX21hdHJpeD1jb3ZhcmlhbmNlCiAgICApCgogICAgc2FtcGxlcyA9IFtdCiAgICBmb3IgXyBpbiByYW5nZShudW1fc2FtcGxlcyk6CiAgICAgICAgcCA9IFNUQU5EQVJEX05PUk1BTC5jZGYoY29ycmVsYXRlZF9ub3JtYWwuc2FtcGxlKCkpCiAgICAgICAgcCA9IHAgKiogKGxhbWJkYV9kZWNheSAqIHRvcmNoLmFyYW5nZShudW1fZmVhdHVyZXMpKQogICAgICAgIHAgPSBwICogKGF2Z19hY3RpdmVfZmVhdHVyZXMgLyAobnVtX2ZlYXR1cmVzICogcC5tZWFuKCkpKQogICAgICAgIHAgPSB0b3JjaC5iZXJub3VsbGkocC5jbGFtcCgwLCAxKSkKICAgICAgICBjb2VmID0gcCAqIHRvcmNoLnJhbmQobnVtX2ZlYXR1cmVzKQoKICAgICAgICBzYW1wbGUgPSBjb2VmIEAgZmVhdHVyZXMuVAogICAgICAgIHNhbXBsZXMuYXBwZW5kKHNhbXBsZSkKCiAgICByZXR1cm4gdG9yY2guc3RhY2soc2FtcGxlcyksIGZlYXR1cmVz)

def generate\_sharkey(

num\_samples: int,

num\_inputs: int,

num\_features: int,

avg\_active\_features: float,

lambda\_decay: float,

) -> tuple\[Tensor, Tensor\]:

"""

Args:

num\_samples (int): The number of samples to generate.

num\_inputs (int): The number of input dimensions.

num\_features (int): The number of ground truth features.

avg\_active\_features (float): The average number of

ground truth features active at a time.

lambda\_decay (float): The exponential decay factor for

feature probabilities.

"""

features = torch.randn(num\_inputs, num\_features)

features /= torch.norm(features, dim=0, keepdim=True)

covariance = torch.randn(num\_features, num\_features)

covariance = covariance @ covariance.T

correlated\_normal = MultivariateNormal(

torch.zeros(num\_features), covariance\_matrix=covariance

)

samples = \[\]

for \_ in range(num\_samples):

p = STANDARD\_NORMAL.cdf(correlated\_normal.sample())

p = p \*\* (lambda\_decay \* torch.arange(num\_features))

p = p \* (avg\_active\_features / (num\_features \* p.mean()))

p = torch.bernoulli(p.clamp(0, 1))

coef = p \* torch.rand(num\_features)

sample = coef @ features.T

samples.append(sample)

return torch.stack(samples), features

Figure 21: A Python implementation of the data-generation procedure introduced by [^53] and used in Section 4.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x20.png)

Figure 22: Pareto frontiers of the explained variance against the L 0 L^0 norm (sparsity) for toy datasets generated to exhibit superposition, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x21.png)

Figure 23: Pareto frontiers of the explained variance against the L 1 L^1 norm (sparsity) for toy datasets generated to exhibit superposition, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x22.png)

Figure 24: Pareto frontiers of explained variance against the L 1 L^1 norm over the square root of the 2 L^2 norm (sparsity) for toy datasets generated to exhibit superposition, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x23.png)

Figure 25: Pareto frontiers of explained variance against the Hoyer sparseness (sparsity) for toy datasets generated to exhibit superposition, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x24.png)

Figure 26: Pareto frontiers of explained variance against sparsity measures for 50-dimensional GloVe word vectors, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x25.png)

Figure 27: Pareto frontiers of explained variance against sparsity measures for 100-dimensional GloVe word embeddings, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x26.png)

Figure 28: Pareto frontiers of explained variance against sparsity measures for 200-dimensional GloVe word embeddings, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

![Refer to caption](https://arxiv.org/html/2501.17727v2/x27.png)

Figure 29: Pareto frontiers of explained variance against sparsity measures for 300-dimensional GloVe word embeddings, Gaussian controls with the same mean and variance, and the corresponding outputs when these are passed to a randomly initialized two-layer MLP.

## Appendix J Example features for Pythia-6.9b

### Variant: Trained

#### Feature 180935 (Layer 0)

Interpretation: The term ”security” is predominantly used to refer to protection, safety, and measures to prevent harm, while ”oz” is likely referring to ounces, possibly in a context of measurement or quantification, although ”oz” appears less frequently and often in a different context.

Top Examples:

1. Text: `<` endoftext—¿—. If for any reason you are unhappy with our service please contact us directly so we can make it right for you.Journal of Cyber Security, Vol. Activation: 4.3750 Active tokens: Security
2. Text: `<` endoftext—¿— Security Practitioner. Tremendously passing CompTIA Advanced Security Practitioner (casp) cert has never been as easy as it Activation: 4.3125 Active tokens: Security Security
3. Text: in trusted hands for your Cyber Security career or staffing needs. Call 0203 643 0248 to find out more. Technically proficient using Activation: 4.3125 Active tokens: Security

#### Feature 93790 (Layer 8)

Interpretation: Nouns and phrases related to economic concepts, development, and business, often referring to growth, progress, and improvement.

Top Examples:

1. Text: training requirements. See “Workforce” section for additional information. The Economic Development Transportation Fund, commonly referred to as the “Road Fund,” is an Activation: 21.3750 Active tokens: Development
2. Text: Montréal.The Williamsburg Economic Development Authority offers a 33% matching grant up to $7,500 for exterior improvements to existing businesses in the City of Activation: 20.2500 Active tokens: Development Authority
3. Text: Correction: In a July 16 web story The Real Deal incorrectly stated that the Economic Development Corporation was “circumventing” laws with its restructuring. In Activation: 20.0000 Active tokens: Development

#### Feature 128309 (Layer 12)

Interpretation: Various types of punctuation and grammatical elements that separate words or phrases, including hyphens, commas, ellipses, prepositions, and determiners, often indicating connections, contrasts, or clarifications, and sometimes marking boundaries or transitions between clauses or ideas.

Top Examples:

1. Text: Run it in JDK6, and it will print ”\[axons, bandrils, chumblies\]”. If you are having trouble switching from Activation: 8.0000 Active tokens: in JD K
2. Text: Here, we introduce the coordinate systems for three-dimensional space $\square$ $\square$ $\square$ 2. The study of 3-dimensional spaces lead us to the setting for our study Activation: 7.8125 Active tokens: $\square$
3. Text:.path.expanduser(”~/malwarehouse/”) because this server doesn’t have X-Windows running. If you are looking for a simple and Activation: 7.7188 Active tokens:. path expand user

### Variant: Step 0

#### Feature 126848 (Layer 12)

Interpretation: Nouns denoting people who train others, units or marks of measurement, and abbreviations or acronyms representing specific standards or technologies.

Top Examples:

1. Text: What are the various lessons a member can access at a tennis club? Whether you are a beginner or advanced player, trainers help you to choose the right gaming Activation: 13.1250 Active tokens: trainers
2. Text: report include various simulation platforms and Serious Games. The report also analyzes some major allied products such as patient simulators and task trainers. The technologies analyzed Activation: 13.0625 Active tokens: trainers
3. Text: a stylish spring in your step when you buy from our fantastic range of men’s and women’s Asics trainers. We’ve got numerous styles from Activation: 13.0000 Active tokens: trainers

#### Feature 2125 (Layer 4)

Interpretation: The word ”papers” is often used in contexts referring to written documents, such as academic papers, court documents, or printed materials, and is frequently mentioned in relation to tasks like writing, research, and education.

Top Examples:

1. Text: caustic solution. As an abrasive, alumina is coated into abrasive papers and.. Pakistan. Sierra leone. Taiwan. Turkey. Venezuela. Activation: 5.7188 Active tokens: papers
2. Text: that can be associated with interaction with other individuals. For everybody who is uncertain regardless of whether your papers is misstep no cost, buy inexpensive experienced proofreading services Activation: 5.6875 Active tokens: papers
3. Text: who RV, often traveling in groups, often alone. You just want to have all the papers like RC, licence and insurance coverage as effectively as PUC ( Activation: 5.6562 Active tokens: papers

#### Feature 9944 (Layer 16)

Interpretation: Words or parts of words that are usually the beginning or end of a proper noun, surname, or a word of foreign origin.

Top Examples:

1. Text: astic gestures.Home Music World News Music the artform Where Do Music Festivals Go Now? Where Do Music Festivals Go Now? Are you ready Activation: 10.4375 Active tokens: Fest Fest
2. Text: client streams, and encouraging existing customers to become more involved.Welcome to Fil Fest USA!! Do you love lumpia? Can you eat a handful of them Activation: 10.3750 Active tokens: Fest
3. Text: Powers talking about his early inspirations. In response to San Diego Comic Fest 2012!: Off to Comic Fest 2012. Should be interesting if nothing else! Activation: 10.3750 Active tokens: Fest Fest

### Variant: Randomized excluding embeddings

#### Feature 151030 (Layer 28)

Interpretation: Common nouns, proper nouns, or adjectives found in various contexts, including but not limited to geographical locations, people, organizations, time, and concepts, often possessing relevance to the surrounding text.

Top Examples:

1. Text: ion Patch Kills Owner, Son,”, Los Angeles Times, June 12, 1994, http://articles.latimes.com/1994–06-12 Activation: 58.0000 Active tokens: lat
2. Text: hard-headed coin of the realm their look. Firstly you’ve got to inventory on incident unique a distinct blunt in a retailer you superlativeness be Activation: 56.5000 Active tokens: lat
3. Text: Jr’s new film Dovlatov follows the life of the now celebrated writer Sergei Dovlatov over six days in 1971, as he struggles Activation: 55.7500 Active tokens: lat lat

#### Feature 98924 (Layer 12)

Interpretation: Adjectives describing size, or nouns representing concepts or objects that are being described in terms of their size.

Top Examples:

1. Text:. The area to the right is for large dogs (small dogs also welcomed) however the area to the left is for small dogs only. Troup 69 Activation: 46.0000 Active tokens: small
2. Text:,I would not mind, but has to pretty less expensive. Can it use any windows aplication???? Or I am really need a cool,small Activation: 45.5000 Active tokens: small
3. Text: 3/4” – 8 1/2” rather than strictly 8 1/4”) I chose the “small” version, though I should be a Activation: 44.7500 Active tokens: small

#### Feature 180589 (Layer 24)

Interpretation: Nouns mostly referring to tasks, responsibilities or jobs to be accomplished, often in a professional or organizational context, sometimes accompanied by proper nouns and a few instances with words having suffixes or prefixes.

Top Examples:

1. Text: completing tasks or a captcha, users are awarded by GRSfractions. Why These Groestlcoin Faucets provide rewards? Many people Activation: 130.0000 Active tokens: tasks
2. Text: from Groestlcoin Faucets is In the exchange of completing tasks or a captcha, users are awarded by Free GRS. To Earn Activation: 129.0000 Active tokens: tasks
3. Text: and still contain a small remnant circular genome, known as mitochondrial DNA. Of the varied tasks undertaken by mitochondria, the most important is the generation of the chemical energy Activation: 128.0000 Active tokens: tasks

### Variant: Randomized including embeddings

#### Feature 39748 (Layer 0)

Interpretation: Words contain ”Pul” are often used in the context of Pulitzer, a prestigious journalism award, while ”Looking” typically precedes a phrase expressing anticipation, expectation, or searching for something.

Top Examples:

1. Text: `<` endoftext—¿— to the 21st Century. Rhodes won the Pulitzer prize for The Making of the Atomic Bomb (01987) his first of four books chronicling the Activation: 3.3750 Active tokens: Pul
2. Text: `<` endoftext—¿—, James Coburn, movies we love, Pulp Consumption, Steve McQueen, Western, Yul Brynner. Bookmark the permal Activation: 3.3438 Active tokens: Pul
3. Text: Crusher, Coal Mill and Coal Pulverizer for sale Coal crusher and coal mill is the major mining equipment in. sbm ceramic machinary - Activation: 3.2969 Active tokens: Pul

#### Feature 15633 (Layer 20)

Interpretation: Nouns representing individuals or entities possessing or having authority over something, often in a possessive or authoritative relationship with that thing.

Top Examples:

1. Text: poetry. The Starkville/Mississippi State University Symphony Orchestra kicks off 2012 with a Jan. 21 concert dedicated to parents of the performing musicians. The free Activation: 80.0000 Active tokens: Stark
2. Text: Baptist. Kris Kirkwood, Stark Raving Solutions’ lighting designer, says the architectural system used, ETC’s Paradigm architectural control, is Activation: 77.5000 Active tokens: Stark
3. Text: so you can be as cool as Tony Stark. The Marvel Training Academy will be taking place throughout May, just check with your local shop to guarantee your place Activation: 77.5000 Active tokens: Stark

#### Feature 6069 (Layer 4)

Interpretation: Proper nouns, nouns referring to objects or places, and nouns with strong semantic connotations often related to religion or technology.

Top Examples:

1. Text: in production include Bullfinch’s Mythology: Age of Fable, The Story of Dr. Doolittle, and a collection of Hans Christian Anderson fairy Activation: 22.1250 Active tokens: Christian
2. Text: reaction of the remaining flock remains the same: ostracism, shunning, even retaliation. So yeah, Christian leaders won’t make any big Activation: 22.0000 Active tokens: Christian
3. Text: was one of the best loved characters in the film. Walt Disney attempted as far back as 1937 to adapt the Hans Christian Anderson fairy tale, The Snow Queen into Activation: 22.0000 Active tokens: Christian

### Variant: Control

#### Feature 290 (Layer 4)

Interpretation: Function words and occasionally nouns or proper nouns that seem to be emphasized as part of a larger phrase or topic, often indicating transition or conjunction.

Top Examples:

1. Text: `<` endoftext—¿—ance - Chapters: 1 - Words:. Fruits Basket - Rated: T - English - Romance/Angst - Chapters: Activation: 5.0938 Active tokens: -
2. Text: ococcus neoformans-reactive and total immunoglobulin profiles of human immunodeficiency virus-infected and uninfected Ugandans’. Clinical and Diagnostic Laboratory Immunology, Vol 12 Activation: 5.0938 Active tokens: un
3. Text: and in fact any correspondence that the social club had in the run-up to the sit-in was from the social club’s own solicitors. Activation: 5.0625 Active tokens: the

#### Feature 176433 (Layer 24)

Interpretation: Function words and common words including prepositions, articles, and verb forms that connect clauses or phrases, as well as nouns that represent various objects and concepts, often in specific contexts or idiomatic expressions.

Top Examples:

1. Text: forgo insurance. Ultimately, that choice is up to you. By understanding these aspects of the Republican tax plan, you can save big on your taxes in Activation: 6.9062 Active tokens: taxes
2. Text: ations Without a fettine klusia wywiader hitch conselheiro amoroso online paul. In France, Germany, Belgium, Luxem Activation: 6.8438 Active tokens: wi
3. Text: K-ras oncogene and also via mutations in BRAF. Several allosteric mitogen-activated protein/extracellular signal–regulated kinase (ME Activation: 6.5000 Active tokens: rac

#### Feature 203901 (Layer 20)

Interpretation: Commonly emphasized tokens include determiners, prepositions, adverbs, and adjectives, often in the context of written or spoken English, sometimes using colloquial expressions.

Top Examples:

1. Text: was an avid reader and a fantastic cook. Susan was a brave and courageous woman who battled MS for over 40 years. Even given the limitations of her Activation: 9.1875 Active tokens: given
2. Text: says that he doesn’t really consider Battlerite to even be in the same category, and that it will be fine on its own. Well I Activation: 9.1250 Active tokens: to
3. Text: to see a dime of the funds. The transaction occurred mere hours before the doomed exchange stopped honoring withdrawals. Tsao sold nearly 20 bit Activation: 9.1250 Active tokens:.

## Appendix K Compute Details

We performed all experiments with a single NVIDIA A100 80GB GPU in a private cluster. Table 1 lists the approximate duration of the final experiments for each model size and transformer variant. We estimate that the total cost of preliminary and failed experiments is roughly equal to the cost of the final experiments.

<table><tbody><tr><td>Model</td><td>Variants</td><td>Approx. time per variant (hours)</td><td>Total time (hours)</td></tr><tr><td>Pythia-6.9b</td><td>5</td><td>70</td><td>350</td></tr><tr><td>Pythia-1b</td><td>5</td><td>10</td><td>50</td></tr><tr><td>Pythia-410m</td><td>5</td><td>5</td><td>25</td></tr><tr><td>Pythia-160m</td><td>5</td><td>1</td><td>5</td></tr><tr><td>Pythia-70m</td><td>5</td><td>1</td><td>5</td></tr><tr><td colspan="3">Overall time:</td><td>435</td></tr></tbody></table>

Table 1: Approximate time required for our experiments.

## Appendix L Example feature dashboards for Pythia-6.9b

Here we provide more detailed ‘feature dashboards,’ including per-feature activation patterns, token distribution entropy, and auto-interpretability (‘fuzz’ ROC) scores. We include two randomly sampled features for the control, randomized, and trained variants described in Section 3, trained on every fourth layer of Pythia-6.9b.

### L.1 Trained

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x28.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x29.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x30.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x31.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x32.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x33.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x34.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x35.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x36.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x37.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x38.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x39.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x40.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x41.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x42.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x43.png)

\[Uncaptioned image\]

### L.2 Control

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x44.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x45.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x46.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x47.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x48.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x49.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x50.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x51.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x52.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x53.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x54.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x55.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x56.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x57.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x58.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x59.png)

\[Uncaptioned image\]

### L.3 Randomized excluding embeddings

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x60.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x61.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x62.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x63.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x64.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x65.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x66.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x67.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x68.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x69.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x70.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x71.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x72.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x73.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x74.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x75.png)

\[Uncaptioned image\]

### L.4 Randomized including embeddings

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x76.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x77.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x78.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x79.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x80.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x81.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x82.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x83.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x84.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x85.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x86.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x87.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x88.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x89.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x90.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x91.png)

\[Uncaptioned image\]

### L.5 Step 0

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x92.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x93.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x94.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x95.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x96.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x97.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x98.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x99.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x100.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x101.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x102.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x103.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x104.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x105.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x106.png)

\[Uncaptioned image\]

![[Uncaptioned image]](https://arxiv.org/html/2501.17727v2/x107.png)

\[Uncaptioned image\]

[^1]: Sanity Checks for Saliency Maps. External Links: 1810.03292, [Link](https://arxiv.org/abs/1810.03292) Cited by: §1.

[^2]: An Information-Maximization Approach to Blind Separation and Blind Deconvolution. Neural Computation 7 (6), pp. 1129–1159. Note: Conference Name: Neural Computation External Links: ISSN 0899-7667, [Link](https://ieeexplore.ieee.org/abstract/document/6796129), [Document](https://dx.doi.org/10.1162/neco.1995.7.6.1129) Cited by: §2.

[^3]: EleutherAI/sparsify. Note: https://github.com/EleutherAI/sparsify External Links: [Link](https://github.com/EleutherAI/sparsify) Cited by: §3.

[^4]: Pythia: A Suite for Analyzing Large Language Models Across Training and Scaling. In Proceedings of the 40th International Conference on Machine Learning, pp. 2397–2430 (en). Note: ISSN: 2640-3498 External Links: [Link](https://proceedings.mlr.press/v202/biderman23a.html) Cited by: 4th item, §3.

[^5]: Language models can explain neurons in language models. External Links: [Link](https://openaipublic.blob.core.windows.net/neuron-explainer/paper/index.html) Cited by: §1, §1, §2, §2, §3, §3.

[^6]: Identifying Functionally Important Features with End-to-End Sparse Dictionary Learning. arXiv. Note: arXiv:2405.12241 \[cs\] External Links: [Link](http://arxiv.org/abs/2405.12241), [Document](https://dx.doi.org/10.48550/arXiv.2405.12241) Cited by: §2.

[^7]: Towards Monosemanticity: Decomposing Language Models With Dictionary Learning. External Links: [Link](https://transformer-circuits.pub/2023/monosemantic-features) Cited by: §1, §2, §2, §2.

[^8]: EleutherAI/delphi. Note: https://github.com/EleutherAI/delphi External Links: [Link](https://github.com/EleutherAI/delphi) Cited by: §3, §5.

[^9]: Superposition is not “just”’ neuron polysemanticity. (en). External Links: [Link](https://www.alignmentforum.org/posts/8EyCQKuWo6swZpagS/superposition-is-not-just-neuron-polysemanticity) Cited by: §2.

[^10]: Evaluating Open-Source Sparse Autoencoders on Disentangling Factual Knowledge in GPT-2 Small. arXiv. Note: arXiv:2409.04478 \[cs\] External Links: [Link](http://arxiv.org/abs/2409.04478), [Document](https://dx.doi.org/10.48550/arXiv.2409.04478) Cited by: §2.

[^11]: Scaling Automatic Neuron Description. External Links: [Link](https://transluce.org/neuron-descriptions) Cited by: §2, §3.

[^12]: Sparse Autoencoders Find Highly Interpretable Features in Language Models. arXiv. Note: arXiv:2309.08600 \[cs\] External Links: [Link](http://arxiv.org/abs/2309.08600), [Document](https://dx.doi.org/10.48550/arXiv.2309.08600) Cited by: §1, §2, §2.

[^13]: Tokenized SAEs: Disentangling SAE Reconstructions. (en). External Links: [Link](https://openreview.net/forum?id=5Eas7HCe38) Cited by: §1, §3.

[^14]: Toy Models of Superposition. arXiv. Note: arXiv:2209.10652 \[cs\] External Links: [Link](http://arxiv.org/abs/2209.10652), [Document](https://dx.doi.org/10.48550/arXiv.2209.10652) Cited by: §1, §2.

[^15]: Not All Language Model Features Are Linear. arXiv. Note: arXiv:2405.14860 \[cs\] External Links: [Link](http://arxiv.org/abs/2405.14860), [Document](https://dx.doi.org/10.48550/arXiv.2405.14860) Cited by: §2.

[^16]: Jacobian Sparse Autoencoders: Sparsify Computations, Not Just Activations. In Forty-second International Conference on Machine Learning, External Links: [Link](https://openreview.net/forum?id=TPuFRuNano) Cited by: §2.

[^17]: N2G: A Scalable Approach for Quantifying Interpretable Neuron Representations in Large Language Models. arXiv. Note: arXiv:2304.12918 \[cs\] External Links: [Link](http://arxiv.org/abs/2304.12918), [Document](https://dx.doi.org/10.48550/arXiv.2304.12918) Cited by: §2.

[^18]: Scaling and evaluating sparse autoencoders. arXiv. Note: arXiv:2406.04093 \[cs\] External Links: [Link](http://arxiv.org/abs/2406.04093), [Document](https://dx.doi.org/10.48550/arXiv.2406.04093) Cited by: §1, §2, §2, §2, §3.

[^19]: Group-SAE: Efficient Training of Sparse Autoencoders for Large Language Models via Layer Groups. arXiv. External Links: 2410.21508, [Document](https://dx.doi.org/10.48550/arXiv.2410.21508) Cited by: §5.

[^20]: Finding Neurons in a Haystack: Case Studies with Sparse Probing. arXiv. Note: arXiv:2305.01610 \[cs\] External Links: [Link](http://arxiv.org/abs/2305.01610), [Document](https://dx.doi.org/10.48550/arXiv.2305.01610) Cited by: §2.

[^21]: How to use and interpret activation patching. arXiv. Note: arXiv:2404.15255 \[cs\] External Links: [Link](http://arxiv.org/abs/2404.15255), [Document](https://dx.doi.org/10.48550/arXiv.2404.15255) Cited by: §2.

[^22]: RAVEL: Evaluating Interpretability Methods on Disentangling Language Model Representations. arXiv. Note: arXiv:2402.17700 \[cs\] External Links: [Link](http://arxiv.org/abs/2402.17700), [Document](https://dx.doi.org/10.48550/arXiv.2402.17700) Cited by: §2.

[^23]: Independent component analysis: algorithms and applications. Neural Networks 13 (4), pp. 411–430. External Links: ISSN 0893-6080, [Link](https://www.sciencedirect.com/science/article/pii/S0893608000000265), [Document](https://dx.doi.org/10.1016/S0893-6080%2800%2900026-5) Cited by: §2.

[^24]: Probability theory: the logic of science. Cambridge University Press, Cambridge, UK. Note: The maximum entropy property of the Gaussian distribution is discussed on pages 208 and 365, in chapters 7 and 11 External Links: ISBN 0521592712 Cited by: §3.

[^25]: SAEBench: A Comprehensive Benchmark for Sparse Autoencoders. External Links: [Link](https://www.neuronpedia.org/sae-bench/info) Cited by: §1, §3.

[^26]: Evaluating Sparse Autoencoders on Targeted Concept Erasure Tasks. arXiv. External Links: 2411.18895, [Document](https://dx.doi.org/10.48550/arXiv.2411.18895), [Link](https://arxiv.org/abs/2411.18895) Cited by: §2.

[^27]: Measuring Progress in Dictionary Learning for Language Model Interpretability with Board Game Models. arXiv preprint arXiv:2408.00113. External Links: [Link](https://arxiv.org/abs/2408.00113) Cited by: §2.

[^28]: Interpreting Attention Layer Outputs with Sparse Autoencoders. (en). External Links: [Link](https://openreview.net/forum?id=fewUBDwjji) Cited by: §3.

[^29]: Residual Stream Analysis with Multi-Layer SAEs. In The Thirteenth International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=XAjfjizaKs) Cited by: §2.

[^30]: What Causes Polysemanticity? An Alternative Origin Story of Mixed Selectivity from Incidental Causes. arXiv. Note: arXiv:2312.03096 \[cs\] External Links: [Link](http://arxiv.org/abs/2312.03096), [Document](https://dx.doi.org/10.48550/arXiv.2312.03096) Cited by: §2.

[^31]: Efficient sparse coding algorithms. In Advances in Neural Information Processing Systems, Vol. 19. External Links: [Link](https://proceedings.neurips.cc/paper_files/paper/2006/hash/2d71b2ae158c7c5912cc0bbde2bb9d95-Abstract.html) Cited by: §2.

[^32]: Gemma Scope: Open Sparse Autoencoders Everywhere All At Once on Gemma 2. arXiv. Note: arXiv:2408.05147 \[cs\] External Links: [Link](http://arxiv.org/abs/2408.05147), [Document](https://dx.doi.org/10.48550/arXiv.2408.05147) Cited by: §1, §2, §2.

[^33]: Announcing Neuronpedia: Platform for accelerating research into Sparse Autoencoders. External Links: [Link](https://www.alignmentforum.org/posts/BaEQoxHhWPrkinmxd/announcing-neuronpedia-platform-for-accelerating-research) Cited by: §3.

[^34]: Sparse Crosscoders for Cross-Layer Features and Model Diffing. External Links: [Link](https://transformer-circuits.pub/2024/crosscoders/index.html) Cited by: §2.

[^35]: Understanding the difficulty of training transformers. In Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing (EMNLP), B. Webber, T. Cohn, Y. He, and Y. Liu (Eds.), Online, pp. 5747–5763. External Links: [Link](https://aclanthology.org/2020.emnlp-main.463/), [Document](https://dx.doi.org/10.18653/v1/2020.emnlp-main.463) Cited by: §3.

[^36]: Sparse Autoencoders Match Supervised Features for Model Steering on the IOI Task. (en). External Links: [Link](https://openreview.net/forum?id=JdrVuEQih5) Cited by: §2.

[^37]: K-Sparse Autoencoders. arXiv. Note: arXiv:1312.5663 \[cs\] External Links: [Link](http://arxiv.org/abs/1312.5663), [Document](https://dx.doi.org/10.48550/arXiv.1312.5663) Cited by: §2, §2, §3.

[^38]: Understanding polysemanticity in neural networks through coding theory. arXiv. Note: arXiv:2401.17975 \[cs\] External Links: [Link](http://arxiv.org/abs/2401.17975), [Document](https://dx.doi.org/10.48550/arXiv.2401.17975) Cited by: §2.

[^39]: Locating and Editing Factual Associations in GPT. Advances in Neural Information Processing Systems 35, pp. 17359–17372. External Links: [Link](https://proceedings.neurips.cc/paper_files/paper/2022/hash/6f1d43d5a82a37e89b0665b33bf3a182-Abstract-Conference.html) Cited by: §2.

[^40]: Efficient Dictionary Learning with Switch Sparse Autoencoders. arXiv preprint arXiv:2410.08201. External Links: [Link](https://arxiv.org/abs/2410.08201) Cited by: §3.

[^41]: Missed Causes and Ambiguous Effects: Counterfactuals Pose Challenges for Interpreting Neural Networks. External Links: 2407.04690, [Link](https://arxiv.org/abs/2407.04690) Cited by: §5.

[^42]: Sparse autoencoder. External Links: [Link](https://graphics.stanford.edu/courses/cs233-21-spring/ReferencedPapers/SAE.pdf) Cited by: §2.

[^43]: Steering Language Model Refusal with Sparse Autoencoders. arXiv. External Links: 2411.11296, [Document](https://dx.doi.org/10.48550/arXiv.2411.11296), [Link](https://arxiv.org/abs/2411.11296) Cited by: §2.

[^44]: Emergence of simple-cell receptive field properties by learning a sparse code for natural images. Nature 381 (6583), pp. 607–609 (en). Note: Publisher: Nature Publishing Group External Links: ISSN 1476-4687, [Link](https://www.nature.com/articles/381607a0), [Document](https://dx.doi.org/10.1038/381607a0) Cited by: §2.

[^45]: Sparse coding with an overcomplete basis set: A strategy employed by V1?. Vision Research 37 (23), pp. 3311–3325. External Links: ISSN 0042-6989, [Link](https://www.sciencedirect.com/science/article/pii/S0042698997001697), [Document](https://dx.doi.org/10.1016/S0042-6989%2897%2900169-7) Cited by: §2.

[^46]: The Linear Representation Hypothesis and the Geometry of Large Language Models. arXiv (en). Note: arXiv:2311.03658 \[cs, stat\] External Links: [Link](http://arxiv.org/abs/2311.03658), [Document](https://dx.doi.org/10.48550/arxiv.2311.03658) Cited by: §1, §2.

[^47]: Sparse Autoencoders Trained on the Same Data Learn Different Features. External Links: 2501.16615, [Link](https://arxiv.org/abs/2501.16615) Cited by: §5.

[^48]: Automatically Interpreting Millions of Features in Large Language Models. arXiv. External Links: 2410.13928, [Document](https://dx.doi.org/10.48550/arXiv.2410.13928) Cited by: §1, §1, §2, §3, §3.

[^49]: GloVe: Global Vectors for Word Representation. In Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP), A. Moschitti, B. Pang, and W. Daelemans (Eds.), Doha, Qatar, pp. 1532–1543. External Links: [Document](https://dx.doi.org/10.3115/v1/D14-1162) Cited by: Appendix I, 5(b).

[^50]: Improving Sparse Decomposition of Language Model Activations with Gated Sparse Autoencoders. (en). External Links: [Link](https://openreview.net/forum?id=Ppj5KvzU8Q) Cited by: §3.

[^51]: Jumping Ahead: Improving Reconstruction Fidelity with JumpReLU Sparse Autoencoders. arXiv. Note: arXiv:2407.14435 \[cs\] External Links: [Link](http://arxiv.org/abs/2407.14435), [Document](https://dx.doi.org/10.48550/arXiv.2407.14435) Cited by: §2.

[^52]: Polysemanticity and Capacity in Neural Networks. arXiv. Note: arXiv:2210.01892 \[cs\] External Links: [Link](http://arxiv.org/abs/2210.01892), [Document](https://dx.doi.org/10.48550/arXiv.2210.01892) Cited by: §2.

[^53]: Taking features out of superposition with sparse autoencoders. (en). External Links: [Link](https://www.alignmentforum.org/posts/z6QQJbtpkEAX3Aojj/interim-research-report-taking-features-out-of-superposition) Cited by: Figure 21, §I.1, Appendix I, §1, §2, Figure 4, 5(a), §4.2, §4.2.

[^54]: Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet. External Links: [Link](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html) Cited by: §1, §2, §3.

[^55]: Relational Composition in Neural Networks: A Survey and Call to Action. (en). External Links: [Link](https://openreview.net/forum?id=zzCEiUIPk9) Cited by: §1, §2.

[^56]: RedPajama: an Open Dataset for Training Large Language Models. External Links: 2411.12372, [Link](https://arxiv.org/abs/2411.12372) Cited by: §3.

[^57]: Transformer visualization via dictionary learning: contextualized embedding as a linear superposition of transformer factors. In Proceedings of Deep Learning Inside Out (DeeLIO): The 2nd Workshop on Knowledge Extraction and Integration for Deep Learning Architectures, E. Agirre, M. Apidianaki, and I. Vulić (Eds.), Online, pp. 1–10. External Links: [Link](https://aclanthology.org/2021.deelio-1.1), [Document](https://dx.doi.org/10.18653/v1/2021.deelio-1.1) Cited by: §2, §2.

[^58]: Towards Best Practices of Activation Patching in Language Models: Metrics and Methods. In The Twelfth International Conference on Learning Representations, External Links: [Link](https://openreview.net/forum?id=Hf17y6u9BC) Cited by: §2.

[^59]: Algorithmic Capabilities of Random Transformers. External Links: 2410.04368, [Link](https://arxiv.org/abs/2410.04368) Cited by: §2.


[//begin]: # "Autogenerated link references for markdown compatibility"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[//end]: # "Autogenerated link references"
