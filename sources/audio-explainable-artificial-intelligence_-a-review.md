---
title: "Audio Explainable Artificial Intelligence: A Review"
source: "https://spj.science.org/doi/full/10.34133/icomputing.0074"
author:
  - "[[Alican Akman]]"
  - "[[Björn W. Schuller]]"
published: 2024-01-23
created: 2026-04-30
description: "Artificial intelligence (AI) capabilities have grown rapidly with the introduction of cutting-edge deep-model architectures and learning strategies. Explainable AI (XAI) methods aim to make the cap..."
tags:
  - "clippings"
  - "interpretability"
  - "speech"
  - "ai-model"
  - "speech-representations"
---
## Abstract

Artificial intelligence (AI) capabilities have grown rapidly with the introduction of cutting-edge deep-model architectures and learning strategies. Explainable AI (XAI) methods aim to make the capabilities of AI models beyond accuracy interpretable by providing explanations. The explanations are mainly used to increase model transparency, debug the model, and justify the model predictions to the end user. Most current XAI methods focus on providing visual and textual explanations that are prone to being present in visual media. However, audio explanations are crucial because of their intuitiveness in audio-based tasks and higher expressiveness than other modalities in specific scenarios, such as when understanding visual explanations requires expertise. In this review, we provide an overview of XAI methods for audio in 2 categories: exploiting generic XAI methods to explain audio models, and XAI methods specialised for the interpretability of audio models. Additionally, we discuss certain open problems and highlight future directions for the development of XAI techniques for audio modeling.

## Introduction

Recent advancements in artificial intelligence (AI) technology have revolutionised numerous domains and have been widely used in various industries such as healthcare, finance, transportation, and education. Current AI methods include a range of techniques, from standard machine learning models such as decision trees and linear regression to more complex deep learning models such as multilayer perceptrons (MLPs), convolutional neural networks (CNNs), recurrent neural networks (RNNs), and transformers. Nowadays, the transformer-based models have demonstrated their versatility across various domains, ranging from natural language processing tasks \[[^1] – [^2]\] such as machine translation, sentiment analysis, and question answering, to computer vision tasks \[[^3] – [^4]\] such as image generation, object detection, and image segmentation. With advancements in both model architectures and hardware technologies, state-of-the-art deep models, such as GPT-3 \[[^5]\] and ViT \[[^6]\], evolved to large sizes and complexities, pushing the boundaries with hundreds of billions of parameters. Although an increase in complexity has improved the accuracy of these models, the challenge of interpretability has intensified, making it difficult to understand the inner workings and decision-making processes of these black box models.

Explainable AI (XAI) methods play a crucial role in the decision-making processes of AI systems by providing transparency and trust. These XAI properties justify the AI model prediction, which builds trust among human users and debugs the model when necessary. Exploiting XAI methods is inevitable, particularly in sectors such as healthcare \[[^7],[^8]\], law \[[^9]\], transportation \[[^10]\], and finance \[[^11]\] where the decisions made have critical consequences. For example, XAI helps doctors and medical professionals understand and trust the recommendations made by AI models, thereby facilitating better patient care and treatment planning. In finance, XAI explains credit scoring decisions, aiding banks in understanding factors that influence the credibility of individuals or companies and mitigating potential bad debts. XAI aids in interpreting legal documents and precedents, assisting lawyers in building stronger cases, and ensuring fair judgments. Furthermore, XAI empowers autonomous vehicles by explaining their decision-making processes and ensuring safety and reliability while building public acceptance of this cutting-edge technology.

Existing XAI methods cover different types of techniques, including rule-based explanations, feature importance analysis, local surrogate models, prototype/example-based explanations, counterfactuals, model distillation, and attention mechanisms. XAI methods are typically classified according to their stage (ante hoc versus post hoc), scope (local versus global), input data (numerical/categorical, pictorial, textual, and time series), and output format (numerical, rules, textual, visual, audio, and mixed) \[[^12]\]. The stage of providing explanations refers to the stage at which explanations are generated, where ante hoc methods make models naturally explainable by considering interpretability before and during the training process, and post hoc methods exploit external explainer models to ensure explainability without changing the original model after training. While global explanations target the entire model, local explanations explicitly explain the inference of a specific input. In addition to methodological differences, the input data representation and output format used by XAI models differ with respect to the domain of the AI task and the demographics and expertise level of human users.

The output format of explanations generated by an XAI model plays an important role in increasing the level of interpretability and helping human users better understand the model, depending on use-case scenarios. Numerous XAI methods use numerical, rule-based, textual, and visual explanations, all presented visually, to enhance communication with human users. Examples of numerical explanations include providing feature importance scores, class probabilities, or weights such as coefficients in linear models or importance values in tree-based models to quantify the impact of different variables on the model’s inference. Rule explanations include presenting logical rules or decision paths that the model follows to arrive at a decision, making the logic behind the model’s output clear, and making its decision-making process transparent. Visual explanations leverage the communication power of images, heat maps, or saliency maps to highlight important features or parts of an input data point, enabling users to understand the factors influencing the model’s decision-making process. Textual explanations involve the clarification of natural language statements. Recent surveys \[[^12],[^13]\] summarise the field of XAI regarding methods that provide explanations using these visually appealing or human-readable formats. In addition to these formats, audio explanations offer a unique way of presenting explanations that appeal to auditory perception. This includes providing speech, a sound signal, or parts of the input audio that are derived from the original input of audio tasks, or sonification of nonaudio formats for other modalities.

Audio models can be made interpretable and transparent either by applying XAI methods from other domains to these models or using audio-specific XAI methods. The main idea behind developing XAI audio methods is to provide more meaningful and comprehensible explanations by leveraging the expressive power of audio, which serves as an intuitive means of explaining audio tasks \[[^14]\]. Although most XAI methods have been proposed to provide visual explanations or tested only on visual tasks and datasets, certain methods have the potential to explain audio models with simple modifications. When using these methods in audio tasks, similar properties between the data representations used in audio-processing tasks and the original data representation of the XAI method (e.g., audio spectrograms and standard images) are considered to provide meaningful explanations. Conversely, there are also a substantial number of works that propose XAI methods tailored toward explaining audio models. Whereas some methods are built on existing XAI methods with substantial improvements, others propose completely new methods built from scratch to make audio models interpretable. In this study, we provide an overview of the existing XAI techniques, either generic or audio-specific, that are used to explain audio models. We also aim to highlight the potential of developing XAI methods that generate audio explanations, leading to promising research in this field.

The remainder of the paper is organised as follows: We first provide an overview of existing XAI methods applicable to audio models in the “Explaining audio models using generic XAI methods” section by categorising them with respect to the model’s input data representation. In the “Audio-specific XAI methods” section, we focus on the current audio-specific XAI methods to unleash the potential for specialisation in audio explanations. Finally, we discuss several open problems and highlight potential research directions, including testing XAI methods on audio data, generating listenable explanations, and providing sonified explanations from nonaudio modalities before drawing a conclusion.

## Explaining Audio Models Using Generic XAI Methods

Audio representations, such as waveforms, spectrograms, and mel-frequency cepstral coefficients (MFCCs), provide different ways of transforming and analysing audio data, enabling tasks such as automatic speech recognition (ASR), audio classification, speaker recognition, and signal processing. Depending on the selection of audio representations and the characteristics of the task, various deep model architectures are used, including MLPs, CNNs, RNNs, and transformers. In addition to these design choices for AI systems, the selection of an appropriate XAI method is vital to enabling users to understand and trust the decisions made by these systems. In this section, we provide an overview of the generic XAI methods applicable to audio-processing models, considering the input data representation of these models. We also present a summary of the generic XAI methods applicable to audio-processing models in Table [1](#).

| XAI method | Category | Input representation | Output format | Scope | Model agnostic |
| --- | --- | --- | --- | --- | --- |
| Guided backpropagation \[[^19]\] | Feature attributions | Spectrogram, waveform | Feature attributions | Local | No |
| LRP \[[^20]\] | Feature attributions | Spectrogram, waveform | Feature attributions | Local | No |
| Integrated gradients \[[^21]\] | Feature attributions | Spectrogram, waveform | Feature attributions | Local | No |
| Grad-CAM \[[^22]\] | Feature attributions | Spectrogram | Feature attributions | Local | No |
| LIME \[[^23]\] | Feature attributions | Spectrogram, waveform | Feature attributions | Local | No |
| SHAP \[[^24]\] | Feature attributions | Spectrogram, waveform | Feature attributions | Local | No |
| \[[^29]\] | Prototype-based | Spectrogram, waveform | Synthetic/natural input examples | Local | No |
| \[[^48]\] | Prototype-based | Spectrogram, waveform | Natural input examples | Local | No |
| \[[^30]\] | Prototype-based | Spectrogram, waveform | Synthetic/natural input examples | Local | No |
| Network dissection \[[^31]\] | Concept-based | Spectrogram, waveform | Concepts | Local | No |
| TCAV \[[^32]\] | Concept-based | Spectrogram, waveform | Concepts | Local | No |
| Attention mapping \[[^26]\] | Attention-based | Spectrogram | Feature attributions | Local | No |

Table 1. Overview of generic XAI methods applicable to audio-processing models

### Traditional feature extractors

Common feature extraction toolkits, such as openSMILE \[[^15]\], openXBOW \[[^16]\], DeepSpectrum \[[^17]\], and auDeep \[[^18]\], exploit various audio-processing methods to extract diverse acoustic features from audio data. The numerical tabular structure of these extracted features enables users to train self-explainable AI models, such as linear regression or decision trees, directly. Additionally, meaningful descriptors such as pitch, loudness, and the number of segments produced in openSMILE provide textual explanations. For example, in an emotion recognition task, an angry audio can be explained as “the emotion in the audio is angry because the loudness is high.”

### Spectrogram representation

Spectrograms and their derivatives, such as MFCCs, are commonly used to represent audio. They provide a visual and quantitative depiction of the frequency content and temporal variations of an audio signal, enabling a detailed analysis of audio signal characteristics and identifying patterns.

Given their image-like nature, CNNs are primarily used to process spectrograms for a wide range of audio tasks. A popular XAI method applicable to these models uses guided backpropagation \[[^19]\], which creates feature relevance by modifying the backpropagation by computing the imputed version of a gradient. It augments standard backpropagation with an additional guidance signal from the higher layers; that is, it backpropagates only positive gradients during gradient computation, which increases the activation of the higher-layer unit to be observed. By backpropagating the model prediction in a neural network, layer-wise relevance propagation (LRP) \[[^20]\], an alternative technique, awards relevance scores to specific pixels or neurons. It uses specially crafted local backpropagation rules subject to the conservation property, which requires that an identical quantity of what a neuron receives be disseminated to the bottom layer again. Integrated Gradients \[[^21]\] are a potent axiomatic attribution technique that can be easily implemented with only a few calls to the regular gradient operator and require no changes to the original network. It calculates the integral of the gradients along the direct line connecting the input and baseline images, offering a thorough assessment of feature attribution. The authors of \[[^22]\] proposed gradient-weighted class activation mapping (Grad-CAM), which is a feature attribution method designed for CNNs and is typically applied to the last convolutional layer. It creates a coarse localisation map that highlights key areas in the image for any predicted class using the gradients of the relevant classification score with respect to the features in the final convolutional layer. Examples of these methods are presented in Fig. [1](#).

![](https://spj.science.org/cms/10.34133/icomputing.0074/asset/5b063f90-c9af-40ba-883f-f924dcadd97d/assets/graphic/icomputing.0074.fig.001.jpg)

Fig. 1. Examples of feature attribution-based XAI methods on audio models. The positively relevant features toward the predicted class are marked in green. © 2022 IEEE. Reprinted, with permission, from Wullenweber A, Akman A, Schuller BW. CoughLIME: Sonified Explanations for the Predictions of COVID-19 Cough Classifiers. Annu Int Conf IEEE Eng Med Biol Soc. 2022 Jul;2022:1342-1345. doi: 10.1109/EMBC48229.2022.9871291. PMID: 36086189.

Model-agnostic XAI methods such as local interpretable model-agnostic explanations (LIME) \[[^23]\] and SHapley Additive exPlanations (SHAP) \[[^24]\] offer a unique method for explaining audio models, regardless of the model architecture. LIME approximates the original complex model using an interpretable surrogate model around the local region of a given prediction to provide faithful explanations for each prediction. It locally perturbs sample points around a particular input example to identify feature importance based on changes in predictions. SHAP, which incorporates cooperative game theory, presents a unified framework for computing the additive feature importance for each specific prediction. This framework has several desirable properties (local accuracy, missingness, and consistency) that are novel to other additive feature attribution approaches. Both methods also offer a method to directly assign feature importance to predefined larger data parts, such as simple spectrogram patches or sources of audio, rather than combining the additive scores of individual time–frequency points. The extraction of these important higher-level components makes it possible to provide more meaningful and comprehensible audio explanations. The sample feature importance assigned to the predefined spectrogram patches using LIME and SHAP is shown in Fig. [1](#).

Transformer models are a strong class of neural network architectures that have recently revolutionised various fields of deep learning. Although they were initially introduced for natural language processing (NLP) tasks, their impact has extended beyond NLP to computer vision and audio tasks. Examples of transformer models in vision include the vision transformer (ViT) \[[^6]\] and data-efficient image transformers, which have demonstrated remarkable performance in image classification, object detection, and image segmentation by leveraging self-attention mechanisms and hierarchical representations. Recently, there has been increasing interest in whether the attention maps offered by these modules are useful in explaining the reasoning for a model’s decision—Wiegreffe and Pinter \[[^25]\] proposed possibilities for explaining attention. Therefore, in transformer-vision models, attention mechanisms can be used to provide explanations or visual interpretations. The authors of \[[^26]\] demonstrated that attention maps from different heads could attend to different semantic regions of an image. Thus, providing attention maps can be highly useful, as it allows users to visualise the model’s focus on specific components of the audio spectrogram. To interpret transformers beyond attention visualisation, Chefer et al. \[[^27]\] proposed a new method for computing relevancy. This method assigns local relevance based on the deep Taylor decomposition principle \[[^28]\] and then propagates the relevancy scores through the layers. Propagation involves attention layers and skip connections, which pose challenges to the existing feature attribution methods.

In addition to feature importance methods, prototype/example-based explanations offer an effective method for explaining audio models with synthetic or natural input “examples” by addressing questions such as “what kind of input is the model most likely to misclassify?,” “which training samples are possibly mislabeled?,” or “which input activates an intermediate neuron most?” While Koh and Liang \[[^29]\] and Pruthi et al. \[24\] exploit influence functions to the training data points having the most “influence” on the test loss, Olah et al. \[[^30]\] searches for natural examples within a specified set or synthesises examples using gradient descent that maximally activates a neuron of interest for both. The latter also uses an optimisation approach to isolate the causes of model behavior from mere correlations, aiming to understand what a model is really looking for. Providing natural examples using these methods has a considerable advantage for audio models because the original audio corresponding to an example spectrogram can also be provided to increase model interpretability by humans. Although the original waveform audio is already included in the dataset, it is reconstructed for synthetic examples when phase information is available.

Representation-based explanations serve as a global explanation method for deriving model understanding by analysing the intermediate representations of a deep model. The network dissection method in \[[^31]\] was the first to identify a broad set of human-labeled visual semantic concepts, including low-level concepts such as colors and higher-level concepts such as objects. It then gathers the responses of the hidden variables from the CNNs to these known concepts to quantify the alignment of the hidden variable–concept pairs. Although this has proven successful in computer vision, new concepts that target the audio domain, such as frequencies, pitch, phonemes, and other audio parts or concepts, have been identified. Quantitative testing with concept activation vectors (TCAV) \[[^32]\] measures the sensitivity of a model’s prediction to a user-provided concept by exploiting its internal representation. Whereas network dissection requires concept labels at the level of segmented and annotated samples, TCAV only requires a set of samples where the concept is present and a set where it is not present. This also provides an advantage for concepts that are not localisable, such as frequency-related features in the waveform of a sample. Therefore, the explanations provided by TCAV are not restricted to preexisting features that provide flexibility for nonexpert users to explore the audio concepts they define.

### Waveform representation

Although MFCCs and spectrograms provide spectral representations of audio, they lose some temporal information when the phase information is discarded. The waveform preserves all temporal characteristics, allowing the model to capture subtle variations and intricate patterns in the audio signal. With advancements in deep model architectures, such as transformers and self-supervised learning (SSL) strategies, waveform representation has become favorable for deep audio processing to capture fine-grained temporal details without the need for any feature extraction method \[[^33]\]. In the context of XAI, waveform representation has certain advantages over spectral representation. This offers a more understandable explanation without the need for additional reconstruction. Spectral representations often experience information loss during audio reconstruction to the original domain, primarily because of the discarded phase information.

Wav2Vec 2.0 \[[^34]\], VQ-Wav2Vec \[[^35]\], and HuBERT \[[^36]\] are well-known transformer-based models that use SSL to learn audio representations of large-scale data. They all use raw waveforms as their input representation and are commonly used in various audio and speech processing tasks, as analysed in \[[^33]\]. Providing attention maps for audio transformers trained on raw waveform data is an effective method for enhancing the interpretability of these large models \[[^37]\].

The raw waveform representation is typically higher in dimension than the spectrogram representation because of the discarded content, such as phase information. Sharing time-point importance in high-dimensional waveform-represented audio using feature attribution methods may be difficult for users to understand. Thus, SHAP and LIME have an advantage in explaining waveform-fed models by directly assigning importance to decomposed audio patches rather than to single time points. Similarly, this could be achieved using additive attribution methods, such as LRP, by aggregating the additive scores of individual time–frequency points. Thus, users are provided with more meaningful audio segments, which enables them to listen to and enhance their understanding.

## Audio-Specific XAI Methods

Audio-processing models have distinctive characteristics compared with models in other domains, such as computer vision and NLP. These characteristics are sourced from both the properties of audio data and the deep model architectures specifically designed to process them. Thus, the development of XAI methods specialised in audio models is critical for enhancing the interpretability of these models beyond a superficial understanding. Audio-specific XAI methods aim to explain complex audio signals, leveraging humans’ adeptness at interpreting harmonies, rhythms, and other high-level concepts through listening. Table [2](#) presents a summary of audio-specific XAI methods.

| XAI method | Built on | Usage | Tested on | Category | Input representation | Output format | Scope |
| --- | --- | --- | --- | --- | --- | --- | --- |
| \[[^38]\] | LRP | Investigating feature relevance scores’ connections to concepts such as phonemes and certain frequency ranges | UrbanSound8k \[[^49]\] | Feature attributions | Spectrogram, waveform | Attribution map | Local |
| \[[^39]\] | DFT-LRP | Determining the most suitable input representation for audio event detection models, understanding the model reasoning if it aligns with human requirements | AudioMNIST \[[^38]\] | Feature attributions | Spectrogram, waveform | Attribution map | Local |
| CoughLIME \[[^41]\] | LIME, loudness/NMF decomposition | Generating faithful audio explanations for COVID-19 detection specifically tailored toward cough data | DiCOVA challenge \[[^50]\] | Feature attributions | Spectrogram, waveform | Attribution map | Local |
| audioLIME \[[^42]\] | LIME, Spleeter | Explaining music-tagging models with respect to the sources generated by a source separation system | MillionSongDatatset \[[^51]\] | Feature attributions | Spectrogram, waveform | Attribution map | Local |
| \[[^44]\] | Surrogate self-explainable model NMF decomposition | Explaining audio-processing models with meaningful audio objects, keeping them listenable to the users | ESC-50 \[[^52]\], SONYC-UST \[[^53]\] | Feature attributions | Spectrogram, waveform | Attribution map | Local |
| X-ASR \[[^45]\] | LIME, SFL, Causal | Providing a minimal and sufficient cause for an ASR transcription as a subset of audio frames | Commonvoice \[[^54]\] | Feature attributions | Spectrogram, waveform | Attribution map | Local |

Table 2. Overview of XAI methods specialised on audio-processing models

Exploiting existing feature attribution methods to understand the predictions of audio models is a naive approach. Becker et al. \[[^38]\] explored the interpretability of deep models in the audio domain by using LRP. They aimed to explain multiple deep models trained with raw waveforms and spectrograms for the classification tasks of spoken digits and speaker gender. For both tasks, the authors investigated the connections between feature relevance scores and concepts, such as phonemes or certain frequency ranges. In \[[^39]\], the authors used DFT-LRP \[[^40]\], which is a recently modified version of LRP, to explain audio event detection models with different architectures. They scored the importance of each time–frequency component for the predicted class. The insights are then used to determine the most suitable input representation for audio event detection models and to understand the model’s reasoning if it aligns with human requirements.

Decomposing audio input into meaningful components offers an effective method for disclosing the audio parts to be considered by XAI methods. CoughLIME \[[^41]\] extended the LIME method to explain audio-processing models specifically tailored to cough data. The crucial part of CoughLIME, which distinguishes it from directly using standard LIME on audio spectrograms, decomposes the input audio into components that can be interpreted by humans. Two decomposition methods are emphasised: loudness and nonnegative matrix factorisation (NMF). While the former targets the extraction of individual cough sounds by computing audio power and thresholding the raw waveform, the latter leverages NMF to decompose the spectrogram matrix into a spectral pattern and a time activation matrix. Following audio decomposition, CoughLIME assigns importance to each decomposed part. Given that the NMF decomposition is applied at the spectrogram level, an inverse short-time Fourier transformation is used to generate listenable explanations from the NMF components that are considered important. The authors showed that CoughLIME can generate faithful audio explanations for coronavirus disease 2019 (COVID-19) detection (see Fig. [2](#)). Using a parallel approach, audioLIME \[[^42]\] extends LIME by creating a perturbation process based on the components extracted through source separation. It aimed to explain music-tagging models with respect to the sources generated by Spleeter \[[^43]\] as a source separation system. For example, audioLIME explains a predicted tag “female vocalist” with separated vocals with a female singer or “rock” with components including a driving drum set and a distorted guitar.

![](https://spj.science.org/cms/10.34133/icomputing.0074/asset/c89d88c3-bda2-417f-a4a8-60a1acd44610/assets/graphic/icomputing.0074.fig.002.jpg)

Fig. 2. Example explanations from CoughLIME \[ 41 \] © 2022 IEEE. Reprinted, with permission, from Wullenweber A, Akman A, Schuller BW. CoughLIME: Sonified Explanations for the Predictions of COVID-19 Cough Classifiers. Annu Int Conf IEEE Eng Med Biol Soc. 2022 Jul;2022:1342-1345. doi: 10.1109/EMBC48229.2022.9871291. PMID: 36086189. using loudness decomposition. While both (A) and (B) are COVID-19-positive samples, components highlighted in red (green) account for a COVID-19-negative (positive) prediction.

In addition to using the existing XAI methods to create audio-specific methods, the authors of \[[^44]\] proposed an interpreter network from scratch. They aimed to explain audio-processing models with meaningful audio objects and keep them listenable to users. Their interpreter network incorporated NMF as the audio decomposition methodology. They designed 2 surrogate models that form the interpreter module: (a) a regularised interpreter model that takes hidden layer representations of the targeted network as input and produces time activations of pre-learnt NMF components as intermediate outputs and (b) a simple model that pools the time activations of the NMF components and uses a linear layer to mimic the output of the original classifier. It also learns a separate NMF decoder to ensure that the intermediate representation corresponds to the time activations of a pre-learnt spectral pattern dictionary. For audio interpretation generation, the important components are selected by estimating their relevance using the pooled time activations and weights of the linear layer in (b). A listenable waveform explanation was obtained by performing NMF inversion using only the relevant components. The authors validated their method qualitatively by mixing audio with a sample of different classes. They also illustrated that the interpretations emphasised the audio object of interest. Figure [3](#) presents the examples.

![](https://spj.science.org/cms/10.34133/icomputing.0074/asset/de3cab2d-76da-483a-9791-87e3278dbe6a/assets/graphic/icomputing.0074.fig.003.jpg)

Fig. 3. Examples of interpretation of audios generated from \[ 44 \]. (A) Original signal from the class “dog.” (B) Mixed signal with a signal from the class “crying-baby.” (C) Interpretation audio for the predicted class “dog.”

ASR is a primary task in audio processing, which outputs transcriptions for a given audio input. The length of the transcriptions varies depending on the length of the audio input, which makes explaining them more challenging than using simple classification labels. X-ASR \[[^45]\] provides an explanation for an ASR transcription as a subset of audio frames that is both a minimal and sufficient cause of the transcription. It modifies the existing XAI methods, statistical fault localisation (SFL) \[[^46]\], causality \[[^47]\], and LIME to build the framework X-ASR. To generate explanations using these adapted models, it also exploits a classification step, in which correct or incorrect labels are attached based on a similarity metric.

## Discussion and Future Directions

Owing to the distinctive properties of audio data and the need for a model to process these data, explaining audio models differs from explaining models in other domains such as computer vision and NLP. Although this review covers substantial research on audio model interpretability, there remains room for improvement in addressing the necessity of explaining these models. The current issues and potential future directions for these issues are itemised as follows:

• While the performance of common XAI techniques is proven on the tasks in computer vision and NLP, almost none of the authors test their method and present results on audio datasets. Considering the expressive power of audio and wide range of AI models in this domain, we emphasise the importance of testing future XAI research on audio datasets, which would enhance audio explainability.

• Independently selecting raw waveforms or spectrograms to represent the audio data and providing listenable explanations serve as effective and intuitive methods for enhancing the interpretability of audio models. Therefore, generating listenable explanations for end users by reconstructing spectrogram-level explanations or complex waveform features could be a potential research area.

• Although it is relatively easy to define higher-lever concepts in pictorial data by using superpixels to assign feature importance to these image parts, there is a need to define similar concepts in audio data. For example, in COVID-19 detection from cough sounds, a model might look for whether the cough is wet or dry when making the decision. Defining such concepts in the spectrogram or waveform level may be helpful to understand the more meaningful parts of the audio toward the decision as well, as they provide human-like reasoning like medical experts in this case.

• Providing audio explanations should not be restricted to providing explainability for audio models. As proposed in \[[^14]\], sonification has also high potential to provide audio explanations on nonaudio models owing to the expressive power of audio and its complementary communication channel for vision-based user interactions.

## Conclusion

This overview summarised XAI methods that focus on audio-processing models to increase their explainability and interpretability. The existing methods were shared from 2 perspectives: (a) explaining audio models using generic XAI methods and (b) audio-specific XAI methods. In the first category, we investigated common XAI methods that have high potential for application to audio models to enhance model understandability and trust. XAI methods specialised in audio models were shared in the second category, highlighting the advantages of these methods over common XAI methods. We systematically reviewed the recent literature in both categories by organising studies on the selection of audio input representation, audio model architecture, and the format of providing explanations for these models. In conclusion, we aim to support researchers and experts interested in explaining audio models by providing a comprehensive review of existing methods and promoting future research in this field.

## Acknowledgments

**Author contributions:** A.A. conceived the study, conducted the experiments, and wrote the manuscript. B.W.S. consulted on the work and reviewed the manuscript.

**Competing interests:** The authors declare that they have no competing interests.

|

[^1]: 

[^2]: 

[^3]: 

[^4]: 

[^5]: 

[^6]: 

[^7]: 

[^8]: 

[^9]: 

[^10]: 

[^11]: 

[^12]: 

[^13]: 

[^14]: 

[^15]: 

[^16]: 

[^17]: 

[^18]: 

[^19]: 

[^20]: 

[^21]: 

[^22]: 

[^23]: 

[^24]: 

[^25]: 

[^26]: 

[^27]: 

[^28]: 

[^29]: 

[^30]: 

[^31]: 

[^32]: 

[^33]: 

[^34]: 

[^35]: 

[^36]: 

[^37]: 

[^38]: 

[^39]: 

[^40]: 

[^41]: 

[^42]: 

[^43]: 

[^44]: 

[^45]: 

[^46]: 

[^47]: 

[^48]: 

[^49]: 

[^50]: 

[^51]: 

[^52]: 

[^53]: 

[^54]: