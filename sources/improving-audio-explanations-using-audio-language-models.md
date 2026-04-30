---
title: "Improving Audio Explanations Using Audio Language Models"
source: "https://ieeexplore.ieee.org/abstract/document/10847866/references#references"
author:
  - "[[Alican Akman]]"
  - "[[Qiyang Sun]]"
  - "[[Björn W. Schuller]]"

created: 2026-04-30
description: "Foundation models are widely utilised for their strong representational capabilities, driven by training on extensive datasets with self-supervised learning. Th"
tags:
  - "clippings"
  - "speech"
  - "ai-model"
  - "speech-representations"
  - "citable"
---
**References is not available for this document.**

---

Foundation models have become a prerequisite for designing deep models for various applications. By leveraging the principles of self-learning and large scale training data, they have established a new state-of-the-art across various tasks. While they show outstanding performance on automatic speech recognition \[1\], \[2\], \[3\], \[4\], \[5\], \[6\] and emotion recognition \[7\], \[8\], \[9\], in the audio domain, they are also primarily used for language tasks such as text classification \[10\], \[11\], \[12\]. In addition to their predictive capabilities, foundation models, such as (large) language and audio models, also exhibit strong generative abilities, producing both text and sound under various conditions, including text-to-sound generation \[13\], \[14\].

The growing popularity of foundation models, along with their complex and large-scale architectures, has made interpretability an essential concern. Most existing explainable artificial intelligence (XAI) methods focus on identifying important features in the input space that contribute to a model's final decision. These feature attribution methods can be categorised into perturbation-based \[15\], \[16\] and backpropagation-based \[17\], \[18\], \[19\] techniques \[20\]. They aim to identify relevant input features, such as tokens for natural language processing tasks or time-frequency points in spectrograms for audio processing tasks. However, extracting high-level insights from these features is often challenging, particularly in tasks like audio processing, where interpreting spectrogram features typically requires domain expertise. In addition, focusing on low-level input features when generating explanations limits the ability to capture and convey a model's high-level behaviour. In \[21\], \[22\], the authors exploit non-negative matrix factorisation (NMF) \[23\] to decompose audio into meaningful components, aiming to generate listenable and interpretable audio explanations. However, these methods primarily focus on computing relevance in the input space without delving into the intermediate representations within a deep model.

Certain methods target to improve interpretability by leveraging the intermediate representations of deep models. Testing with Concept Activation Vectors (TCAV) \[24\] and Network Dissection \[25\] aim to explain a model's behaviour by utilising its internal representations with user-provided concepts. However, these approaches rely on predefined concepts and do not account for discovering the learnt concepts already embedded within the latent space of a foundation model. In \[26\], generative foundation models are used to produce explanations from relevant embedding vectors computed by applying a feature attribution method in the latent space. However, due to the simplistic feature selection strategy, which replaces less relevant parts of an embedding vector with a base value, this approach has not explored the impact of less relevant dimensions of the latent space on explanation generation.

To address these challenges, we propose a framework that leverages the generative capabilities of audio language models (ALMs), combined with feature attribution in the latent space of foundation models, to explain model behaviour in audio processing tasks. Our approach first utilises an audio foundation model as an encoder, and then trains an additional classification model on this backbone, tailored to the specific downstream task. To interpret the behaviour of the final model, we identify important features in the latent space using a prominent feature attribution method from the literature. Once the key features are selected, ALMs are employed to fill in the less relevant feature dimensions, generating a complete explanation that reflects the model's decision-making process. In the final step, the decoder of the foundation model is used to reconstruct the relevant audio in the input space. We demonstrate that our method can generate high-fidelity explanations through experiments that assess the model's performance on relevant features. Additionally, we evaluate the interpretability of the generated explanations by analysing speech recognition performance in relation to the speech emotion recognition task. The main contributions of this study are as follows:

- We propose a novel audio explanation framework that integrates ALMs with foundation model-based audio classifiers to provide a comprehensive understanding of important audio features. Our method leverages the high-level latent space of foundation models to identify meaningful audio features relevant to the model's decisions.
- Our approach utilises ALMs to refine and complete the attributed features within the latent space of foundation models, generating listenable and interpretable audio explanations. This offers an alternative to traditional feature selection strategies in the XAI literature, enabling richer and more coherent explanations.
- We evaluate our framework on keyword spotting and speech emotion recognition tasks, showing that, in addition to delivering high-fidelity explanations, our method enhances the understandability of these explanations.

This section details the design of our framework, which harnesses ALMs to generate comprehensive and meaningful audio explanations. We first outline the overall architecture of ALMs and their application in generative tasks. Next, we elaborate on our explainer system, which assigns importance to features within the embedding space of a foundation model-based classifier, while augmenting these features with ALMs. Finally, we describe the process for producing full audio explanations using our method. An overview of our system is illustrated in Fig. 1.

[![Fig. 1. - An overview of our method: The process starts by computing feature attribution for an input audio sample in the embedding space of a foundation model-based audio classifier. In the second stage, Audio Language Models (ALMs) are employed to augment the important features, generating a complete and coherent audio explanation.](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman1-3532218-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman1-3532218-large.gif)

**Fig. 1.**

An overview of our method: The process starts by computing feature attribution for an input audio sample in the embedding space of a foundation model-based audio classifier. In the second stage, Audio Language Models (ALMs) are employed to augment the important features, generating a complete and coherent audio explanation.

### A. Audio Language Models

Similar to large language models which are built on large textual datasets, ALMs try to utilise the expressive power of the audio modality including emotions and speaker timbre \[27\]. They exploit neural audio codecs \[28\], \[29\], \[30\] as suitable tokenisers for converting continuous audio representation into discrete codes. Using the audio modality as their primary input type, certain studies also incorporate textual conditioning as a secondary input type for their ALMs \[13\], \[14\], \[31\].

A typical ALM consists of three components in order to obtain the generative capacity. First, the tokeniser module, $Enc$, transforms a single channel audio signal into discrete audio codecs, as described in [(1)](#deqn1-deqn3). Next, the decoder-only transformer module predicts the subsequent audio token by training on a likelihood objective function, outlined in [(2)](#deqn1-deqn3). Finally, the detokeniser module acts as a lookup table, mapping the generated token sequence back into the audio waveform, as shown in [(3)](#deqn1-deqn3). The overall process of ALMs can be formulated as follows:

$$
\begin$align*$
Z_$u$ &= Enc(X) \in $\mathbb $R$^$T$$ \tag$1$\\
Objective &= \prod _$t=1$^$T$p(Z_$u_$t$$ | Z_$u_$< t$$, [Z_$v_$< t$$]) \tag$2$\\
\hat$X$ &= Dec(\hat$Z_$u$$), \tag$3$
\end$align*$
$$
 View Source

where $X\in $[-1,1]^$D \times F_$s$$$$ represents an audio signal input of duration $D$ with sample rate $F_$s$$, $Z_$u$$ represents the audio token sequence with $T$ denoting the number of audio frames after down-sampling in the tokeniser, and $Z_$v$$ represents the text tokens that is optionally used for conditional generation from text input.

### B. Explainer Design

We aim to provide complete and listenable audio explanations using ALMs based on the extracted important features by standard feature attribution methods. To achieve this, we focus on explaining audio models that utilise the same tokeniser backbone as ALMs and are trained on specific audio tasks, such as audio classification. Our system begins by training an audio classifier on top of the EnCodec neural audio codec \[28\], which serves as our tokeniser. To preserve the learnt representation space during fine-tuning, we freeze the weights of the tokeniser and update only the additional task-specific components, such as the classification head. Subsequently, our framework employs feature attribution methods to identify the most relevant features in the latent space represented by audio codecs that contribute to model decisions. We backpropagate the feature attribution computations only to this latent space, allowing our method to learn the important high-level components. We formulate the process of feature attribution in the latent space as follows, where $\Theta$ represents a standard feature attribution method which produces the relevant subpart of the latent vector, $Z_\theta$, by selecting most important features by a ratio of $\alpha \in $[0,1]$$. Note that a higher value of $\alpha$ corresponds to selecting a larger proportion of latent features from the input for the explanation.

$$
\begin$equation*$
Z_\theta = \Theta (Classifier(Z), \alpha) \in $\mathbb $R$^$T$$, \tag$4$
\end$equation*$
$$
 View Source

By operating in the embedding space, our method aims to enhance important latent features with complementary features using ALMs, which can generate audio from this tokenised representation. To achieve this, we designate the remaining dimensions of the resulting latent vector, $Z_\theta$, as $unknown$ tokens and feed it into the ALM to fill in the complementary features: $\hat$Z_\theta $ = ALM(Z_\theta)$. We do not utilise any text conditions in this process, even though the ALM supports this option. This approach allows our method to prioritise high-level important features for the final prediction while providing a comprehensive understanding of the model's behaviour through the use of an ALM with the same tokeniser backbone. Finally, we leverage the decoder component of the EnCodec model to transform the relevant latent vector into audio explanations. Our explanation generation framework to produce audio explanations in the input space, $\hat$X_\theta $$, can be written as:

$$
\begin$equation*$
\hat$X_\theta $ = Dec(\hat$Z_\theta $). \tag$5$
\end$equation*$
$$
 View Source

We evaluated our method using two datasets: Speech Commands \[32\] and the Toronto Emotional Speech Set (TESS) \[33\], to measure its performance in keyword spotting and speech emotion recognition tasks. The Speech Commands dataset contains over 100,000 utterances of 35 distinct English words spoken by 2,618 individuals. TESS consists of 2,800 recordings where 200 target words were spoken within the phrase “Say the word \*” by two actresses, each portraying one of seven emotions: anger, disgust, fear, happiness, pleasant surprise, sadness, and neutral. In this section, we detail the implementation of our approach and present a thorough quantitative and qualitative evaluation. The implementation code and sample audio explanations can be found on our project page: [https://github.com/glam-imperial/AudioXLM](https://github.com/glam-imperial/AudioXLM).

### A. Implementation Details

We choose AudioGen \[13\] as our ALM to generate complementary audio features in the embedding space. AudioGen is trained on a diverse set of datasets that include various types of audio and text annotations. For our experiments, we use the second version of AudioGen,<sup>1</sup> which is trained on the same data but with longer audio samples, up to 10 seconds in duration, using a retrained EnCodec model on environmental sound data, following the standard EnCodec setup \[28\]. This reimplementation of AudioGen is based on the ALM architecture from MusicGen \[34\] and employs a single-stage, auto-regressive Transformer. It uses a 16 kHz EnCodec tokeniser with four codebooks sampled at 50 Hz, having an embedding space of dimension 128. This version maintains comparable audio quality to the original AudioGen while achieving faster generation speeds due to its lower frame rate.

We train classification models using embeddings from EnCodec, omitting the quantization step to improve accuracy with higher-dimensional representations. For each task, we train two models with different architectures to capture both average and strong classification performance. For the average classifier, used in keyword spotting on the Speech Commands dataset and speech emotion recognition on TESS, we employ a convolutional neural network (CNN) with 4 convolutional layers and 2 linear layers. For the strong classifier on Speech Commands, we implement a transformer model with 3 layers, each containing an 8-head multi-head attention mechanism, a 0.1 dropout rate, and a 512-dimensional feed-forward network. On TESS, the strong classifier is a recurrent neural network based on gated recurrent units (GRU), with 2 layers, 128 hidden units, and a 0.2 dropout rate. While our models achieve test accuracy rates of 85.7%–89.7% on Speech Commands and 87.7%–99.6% on TESS, these results may not reach state-of-the-art performance because our method is constrained to use the same encoder as the ALM. For TESS, we create a test set by randomly selecting emotions for each spoken word, with a 0.2 split ratio, and we provide the test indices for reproducibility. To compute feature attribution in the latent space, we apply Integrated Gradients (IG) \[18\], chosen for its theoretical rigor and suitability for neural networks, though our framework is compatible with any feature attribution method.

We conducted our experiments on a single NVIDIA TITAN Xp GPU. For CNN-based classifiers, explanation generation times for the SC-TESS datasets were approximately 1.5–1.5 ms, 20–24 ms, and 3.62–7.48 s, respectively, showing similar trends for both classifiers.

### B. Quantitative Evaluation

We conduct fidelity experiments to evaluate how closely the predictions of the underlying model align with the generated explanations. Fidelity is measured as the fraction of instances where the predicted class for the generated explanations matches the classifier's original prediction in the latent space. We validate our approach by comparing it to two approaches: 1) a random selection baseline, which randomly selects latent dimensions based on the $\alpha$ ratio to create explanation embeddings, and 2) the approach from \[26\], which selects the most important features using IG and zeros out the remaining dimensions. As shown in Tables I and II, our method consistently outperforms both approaches, producing higher-fidelity explanations across both datasets. However, at low $\alpha$ values, the performance improvement of our method is less pronounced, particularly for the simpler convolutional model. This may be expected, as ALMs tend to generate irrelevant tokens when given more flexibility under weaker constraints. Note that the fidelity scores indicate alignment with the original model, not accuracy, explaining cases like the stronger transformer model showing lower alignment with a random selection strategy.

**TABLE I** Fidelity Results by Measuring Explanation Classification Agreement on the Speech Commands Dataset, With Mean and Standard Deviation Over Five Runs

[![Table I- Fidelity Results by Measuring Explanation Classification Agreement on the Speech Commands Dataset, With Mean and Standard Deviation Over Five Runs](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman.t1-3532218-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman.t1-3532218-large.gif)

**TABLE II** Fidelity Results by Measuring Explanation Classification Agreement on the TESS Dataset, With Mean and Standard Deviation Over Five Runs

[![Table II- Fidelity Results by Measuring Explanation Classification Agreement on the TESS Dataset, With Mean and Standard Deviation Over Five Runs](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman.t2-3532218-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman.t2-3532218-large.gif)

Additionally, we evaluate the audio quality of explanations generated by our framework using quantitative metrics from an automatic speech recognition model. Since the keyword spotting task is closely related to speech recognition, we focus our analysis on the speech emotion recognition model trained on the TESS dataset to avoid redundant experiments. Our hypothesis is that incorporating language context into emotion-related features improves interpretability compared to generating explanation audios solely from abstract emotional concepts. Thus, we measure the speech recognition performance of the explanations generated by our method and the two baseline methods (1) and (2), using word error rate (WER). Table III indicates that our method yields more understandable explanations, evidenced by lower WER scores, enhancing clarity and interpretability for end-users.

**TABLE III** Automatic Speech Recognition Results by Measuring WER on the TESS Dataset, With Mean and Standard Deviation Over Five Runs

[![Table III- Automatic Speech Recognition Results by Measuring WER on the TESS Dataset, With Mean and Standard Deviation Over Five Runs](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman.t3-3532218-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman.t3-3532218-large.gif)

### C. Qualitative Evaluation

We evaluate the quality of the explanations generated by our method for the speech emotion recognition task using the TESS dataset. In this experiment, we set $\alpha = 0.4$ to generate audio explanations. To evaluate the quality of the generated explanations, we compare them against both the original audio and the explanations generated using the method from \[26\]. In addition, we generate audio explanations with our method using input text conditions to examine their effect on the emotion recognition task. Initially, we tested using the full script of the original audio sentence, e.g., “Say the word time” but observed no significant changes in the output. Given the known limitations of the AudioGen model in generating realistic vocals, as noted by its authors, we instead used a simpler text condition “Human speech” for the generation process. As shown in Fig. 2, while our method produces clearer audio explanations compared to the noisier outputs from \[26\], adding text conditioning helps improve the quality in certain cases.

[![Fig. 2. - Sample audio waveform visualisations from the qualitative audio experiments. Each row represents different spoken words and corresponding emotions, while the columns compare various methods against the original audio input: (a) Method from [26], (b) Our method, and (c) Our method conditioned with the audio script.](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman2-3532218-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/97/10802935/10847866/akman2-3532218-large.gif)

**Fig. 2.**

Sample audio waveform visualisations from the qualitative audio experiments. Each row represents different spoken words and corresponding emotions, while the columns compare various methods against the original audio input: (a) Method from \[26\], (b) Our method, and (c) Our method conditioned with the audio script.

In this letter, we introduced a novel XAI method that enhances the quality of audio explanations using ALMs. Our approach not only generates complete explanations but also redefines existing feature attribution methods by modifying their feature selection strategy, incorporating generated features via embedding space supervision from ALMs. The experimental results demonstrated that our method produces high-fidelity explanations, accurately identifying important audio components and meaningfully completing them for audio classification tasks. Furthermore, we showed that our method leverages the generative capabilities of ALMs to create listenable and more interpretable audio explanations for end-users. Our work opens new avenues for research into using language models to interpret state-of-the-art audio models. Future extensions of this work could focus on adapting the method to explain classifiers not necessarily based on specific foundation models, broadening its applicability across various domains.