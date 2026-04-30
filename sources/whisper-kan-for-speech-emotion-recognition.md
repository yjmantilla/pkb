---
title: "Whisper-KAN for Speech Emotion Recognition"
source: "https://link.springer.com/chapter/10.1007/978-3-032-01536-5_23"
author:
  - "[[Adil Chakhtouna]]"
  - "[[Sara Sekkate]]"
  - "[[Abdellah Adib]]"
published: 2026-01-01
created: 2026-04-30
description: "Speech Emotion Recognition (SER) is a critical component of human-computer interaction, with speech serving as a primary modality for understanding emotional states. In this study, we propose Whisper-KAN, a novel framework that combines the Whisper pretrained..."
tags:
  - "clippings"
  - "speech"
  - "ai-model"
  - "speech-representations"
  - "citable"
---
## Abstract

Speech Emotion Recognition (SER) is a critical component of human-computer interaction, with speech serving as a primary modality for understanding emotional states. In this study, we propose Whisper-KAN, a novel framework that combines the Whisper pretrained model for extracting log-Mel spectrogram-based embeddings with the Kolmogorov-Arnold Network (KAN) for interpretable and efficient emotion classification. Evaluated on the IEMOCAP dataset, our approach achieves a state-of-the-art results, surpassing conventional weight-based deep learning methods. The model demonstrates strong performance, particularly for nuanced emotions like anger and sad, as evidenced by Receiver Operating Characteristic (ROC) curve analysis.

Access provided by Canadian Research Knowledge Network (CRKN). [Download conference paper PDF](https://link.springer.com/content/pdf/10.1007/978-3-032-01536-5_23.pdf?pdf=inline%20link)

## 1 Introduction & Related Works

As human beings, each individual experiences a unique sensation during an emotional event, influenced by the context and the passage of time within that experience. The emotions one feels and expresses in a specific situation, or toward particular individuals, are shaped by various factors, including social norms, language, accents, and gender, among others \[[^1]\]. Historically, emotions have been explored primarily within the domains of humanities and psychology \[[^2], [^3]\]. However, with the rapid advancement of modern technologies, particularly the emergence of Deep Learning (DL), which has demonstrated its effectiveness in fields such as disease detection, economic forecasting, and electric vehicles, among others \[[^4],[^5],[^6],[^7],[^8],[^9],[^10]\], the study of emotions has transitioned toward data-driven and objective methodologies. These emotional states can be recognized either unimodally, using a single modality such as image, text, or speech, or through a multimodal approach by combining two or more modalities \[[^11]\]. In the current study, we focused on Speech Emotion Recognition (SER), which examines emotional states through speech signals. Speech is one of the primary forms of communication among humans worldwide, making it a vital medium for emotion analysis.

Numerous studies in the field of SER have been conducted, each approaching the problem from a unique perspective. Some studies have focused on extracting conventional features from speech signals and using them as input to various Machine Learning (ML) and DL models. These features are typically categorized into several groups: prosodic, voice quality, spectral, and cepstral features \[[^12]\]. Additionally, higher-level representations, such as Mel spectrograms \[[^13]\], have also been utilized to capture more complex patterns in speech data. On the other hand, recent studies have increasingly transitioned toward Transfer Learning (TL) and end-to-end approaches. In the TL paradigm, speech signals are processed using pretrained DL architectures, such as those developed for speech recognition tasks \[[^14]\]. Once meaningful features are extracted from speech signals, the subsequent step in SER is to classify these signals into distinct emotional categories. Traditional ML algorithms, have commonly been employed for this purpose \[[^15], [^16]\]. Additionally, DL models, have also been utilized \[[^17]\]. By leveraging the capabilities of these diverse ML and DL architectures, SER systems have achieved significant improvements in detecting subtle emotional nuances. Despite the potential of DL classifiers in SER, interpretability remains a key challenge. To address this limitation, Kolmogorov-Arnold Network (KAN) were introduced \[[^18]\]. Unlike traditional weight updates, KAN relies on updating activation functions, leveraging the Kolmogorov-Arnold representation theorem to enhance interpretability during the training process. In this study, we proposed Whisper-KAN framework based on speech representations derived from the Whisper pretrained model \[[^19]\]. The resulting embedding vectors were then used to train the KAN model. The structure of this paper is organized as follows: Sect. [2](#Sec2) presents the proposed methodology. Section [3](#Sec5) presents experiments and results analysis. Finally, Sect. [4](#Sec6) concludes and suggests future directions in the field.

## 2 Methodology

This section presents the Whisper-KAN system for SER, the proposed approach encompasses the extraction of log-Mel spectrograms, their embedding into the Whisper encoder, and the final emotion classification using KAN model. The overall workflow of these steps is depicted in Fig. [1](#Fig1), and each component is detailed in the subsequent subsections of this section.

**Fig. 1.**

[![Flow chart illustrating the process of analyzing emotional speech signals. The chart is divided into three main sections: "Emotional speech signals," "Whisper," and "KAN." The process begins with emotional speech signals, represented by a waveform icon. These signals are processed into log-Mel spectrograms, shown as colorful spectrogram images. The spectrograms are then encoded into embeddings. The final section, KAN, depicts a network diagram leading to various emoticons representing different emotions, such as happiness, disgust, anger, and neutrality. The flow chart highlights the transformation from speech signals to emotion classification.](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-01536-5_23/MediaObjects/649497_1_En_23_Fig1_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-01536-5_23/figures/1)

Overview of the proposed Whisper-KAN system for SER.

### 2.1 Whisper

To extract meaningful information from speech signals, this study leverages the capabilities of the Whisper pretrained model \[[^19]\]. It is a sequence-to-sequence transformer model, with two key components: the processor block and the model one.

**Log-Mel Spectrograms:** The processor block is responsible with preparing the input audio data. This involves several steps, each audio file is processed by converting it into a log-Mel spectrogram with 128 Mel bands, utilizing 25-ms windows and a 10-ms stride. The final step involves normalizing the data to a range of $-1$ to 1, ensuring a mean value close to zero. The extraction of log-Mel spectrograms captures the spectral properties of speech signals, and plays a crucial role in analyzing the emotional characteristics of speech.

**Embeddings:** Following the extraction of log-Mel spectrograms, they are fed into the model block, which consists of an encoder and a decoder. In this study, the focus is solely on the encoder part, which processes the log-Mel spectrogram to generate rich audio representations. The output of this process is a compact speech representation of size 1280 for each speech file.

### 2.2 KAN

KAN utilize the Kolmogorov-Arnold theorem \[[^18]\], which approximates complex multivariate functions as a superposition of univariate continuous functions and addition operations. Unlike standard Multilayer Perceptron (MLP) networks, which rely on learnable weights on edges and fixed activation functions on nodes, KAN explicitly incorporates learnable activation functions on edges and employs summation operations at the nodes. The mathematical formulation of KAN is as follows:

 $K A N \left(\right. x \left.\right) = \left(\right. \phi_$0$ \cdot \phi_$1$ \hdots \phi_$n$ \left.\right) \cdot x$ 
$$
\begin$aligned$ KAN(x) = \big (\phi _0 \cdot \phi _1 \cdots \phi _n\big ) \cdot x \end$aligned$
$$

(1)

where *x* represents the input features, and $\phi _n$ is the learnable activation function in the *n* -th layer.

For comparison, the formulation of a standard MLP network is given by:

 $M L P \left(\right. x \left.\right) = \sigma \left(\right. W_$0$ \cdot W_$1$ \hdots W_$n$ \left.\right) \cdot x$ 
$$
\begin$aligned$ MLP(x) = \sigma \big (W_0 \cdot W_1 \cdots W_n\big ) \cdot x \end$aligned$
$$

(2)

where $W_n$ are the learnable weights for layer *n*, and $\sigma$ represents the fixed activation function.

## 3 Experimental Setups and Results

This section outlines the experimental protocols, including the emotional database utilized, the configuration of the KAN model, and the performance metrics. The IEMOCAP \[[^20]\] dataset was employed in this study, featuring four baseline emotional states: anger, happy, neutral, and sad. A total of 5531 speech signals were used, with $80\%$ allocated for training the Whisper-KAN model and the remaining $20\%$ reserved for testing. Regarding the KAN configuration, the Limited-memory Broyden–Fletcher–Goldfarb–Shanno (L-BFGS) optimizer was utilized, along with the cross-entropy loss function, to optimize the model’s performance. Additionally, the grid size and spline order were set as hyperparameters, with values of 5 and 3, respectively. The Whisper-KAN model was trained over 30 epochs, employing the early stopping regularization technique to prevent overfitting. Several metrics were employed, including accuracy, Cohen’s kappa ($\kappa$), and the Receiver Operating Characteristic (ROC) curve.

Table [1](#Tab1) provides a detailed evaluation of the proposed Whisper-KAN model. The results demonstrate that the model achieves an overall accuracy of $71.36\%$, indicating its ability to correctly classify emotions in the majority of cases. Furthermore, the $\kappa$ score is reported at $61.26\%$. This score reflects moderate agreement and highlights the challenges associated with distinguishing between emotions such as neutral and sad or happy and anger. The reported precision, recall, and F1-score demonstrate that the model achieves a well-balanced performance in recognizing the majority of instances for each emotion, with particularly strong results for the sad category and slightly lower, yet competitive, performance for the neutral category. An additional method for assessing the performance of the proposed Whisper-KAN model is illustrated in Fig. [2](#Fig2), which visualizes the ROC curves for each emotion separately. The highest Area Under the Curve (AUC) of 0.93 was achieved by sad, indicating that the model excels in distinguishing this emotion from the others with high reliability. Anger performs well with an AUC of 0.92, followed by happy with an AUC of 0.86, though slightly lower than sad and anger. Finally, the emotion of neutral which has the lowest AUC of $81.55\%$, indicating that the model struggles more due to overlaps in characteristics with other emotional states.

**Table 1. Whisper-KAN overall performance results for SER (%).**

**Fig. 2.**

[![Receiver Operating Characteristic (ROC) curves comparing emotion detection performance. The x-axis represents the False Positive Rate, and the y-axis represents the True Positive Rate. Curves are shown for Anger (AUC = 0.92), Happy (AUC = 0.86), Neutral (AUC = 0.82), Sad (AUC = 0.94), and a Micro-average (AUC = 0.89). A diagonal line indicates random performance.](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-3-032-01536-5_23/MediaObjects/649497_1_En_23_Fig2_HTML.png?as=webp)](https://link.springer.com/chapter/10.1007/978-3-032-01536-5_23/figures/2)

ROC curve of the Whisper-KAN model across the four emotional states.

In addition, we conducted a comparative study to evaluate the proposed method against state-of-the-art using the same IEMOCAP dataset. \[[^21]\] introduced a multi-scale Time-Frequency CNN (TFCNN) and a densely connected Capsule Network (DenseCap) to process spectrogram. Extreme Learning Machine (ELM) achieved an accuracy of $70.78\%$ for classification. \[[^22]\] proposed a model combining Vector Quantized Variational Autoencoder (VQ-VAE) and a Temporal Attention Convolutional Network (TACN). They achieved an accuracy of $70.16\%$. \[[^23]\] developed a 1D Convolutional Neural Network (1D CNN). Mel-spectrogram, Log-Mel-spectrogram, and MFCC were extracted and they obtained an accuracy of $65.35\%$. Unlike these methods, which rely on the conventional approach of weight-based learning, The proposed Whisper-KAN model with function activation-based learning, achieved an accuracy of $71.36\%$, outperforming the aforementioned methods.

## 4 Conclusion and Prospects

In this study, we proposed the Whisper-KAN framework for SER, leveraging the Whisper pretrained model for feature extraction and the KAN for emotion classification. Experimental results on the IEMOCAP dataset demonstrated that our method achieves competitive performance, with an accuracy of $71.36\%$ and a $\kappa$ of $61.26\%$, surpassing several state-of-the-art SER approaches. Future work will focus on extending this approach to multimodal emotion recognition by integrating complementary modalities, such as facial expressions and textual cues.

## References

## Acknowledgments

This work was supported by the Ministry of Higher Education, Scientific Research and Innovation, the Digital Development Agency (DDA) and the CNRST of Morocco (Alkhawarizmi/2020/01).

## Editor information

### Editors and Affiliations

## Rights and permissions

## About this paper

### Cite this paper

Chakhtouna, A., Sekkate, S., Adib, A. (2026). Whisper-KAN for Speech Emotion Recognition. In: Mejdoub, Y., Elamri, A., Kardouchi, M. (eds) Connected Objects, Artificial Intelligence, Telecommunications and Electronics Engineering. COCIA 2025. Lecture Notes in Networks and Systems, vol 1584. Springer, Cham. https://doi.org/10.1007/978-3-032-01536-5\_23

- [.RIS](https://citation-needed.springer.com/v2/references/10.1007/978-3-032-01536-5_23?format=refman&flavour=citation "Download this article's citation as a .RIS file")
- [.ENW](https://citation-needed.springer.com/v2/references/10.1007/978-3-032-01536-5_23?format=endnote&flavour=citation "Download this article's citation as a .ENW file")
- [.BIB](https://citation-needed.springer.com/v2/references/10.1007/978-3-032-01536-5_23?format=bibtex&flavour=citation "Download this article's citation as a .BIB file")
- DOI https://doi.org/10.1007/978-3-032-01536-5\_23
- Published 08 October 2025
- Publisher NameSpringer, Cham
- Print ISBN978-3-032-01535-8
- Online ISBN978-3-032-01536-5
- eBook Packages [Intelligent Technologies and Robotics](https://link.springer.com/search?facet-content-type=%22Book%22&package=42732&facet-start-year=2026&facet-end-year=2026) [Intelligent Technologies and Robotics (R0)](https://link.springer.com/search?facet-content-type=%22Book%22&package=43728&facet-start-year=2026&facet-end-year=2026) [Springer Nature Proceedings excluding Computer Science](https://link.springer.com/search?facet-content-type=%22Book%22&package=85345&facet-start-year=2026&facet-end-year=2026)

### Keywords

## Publish with us

[Policies and ethics](https://www.springernature.com/gp/policies/book-publishing-policies)

[^1]: Mehrabian, A.: Basic Dimensions for a General Psychological Theory: Implications for Personality, Social, Environmental, and Developmental Studies. Oelgeschlager, Gunn & Hain, Cambridge (1980)

[Google Scholar](https://scholar.google.com/scholar?&q=Mehrabian%2C%20A.%3A%20Basic%20Dimensions%20for%20a%20General%20Psychological%20Theory%3A%20Implications%20for%20Personality%2C%20Social%2C%20Environmental%2C%20and%20Developmental%20Studies.%20Oelgeschlager%2C%20Gunn%20%26%20Hain%2C%20Cambridge%20%281980%29)

[^2]: Frijda, N.H.: The emotions. Studies in Emotion and Social Interaction (1986)

[Google Scholar](https://scholar.google.com/scholar?&q=Frijda%2C%20N.H.%3A%20The%20emotions.%20Studies%20in%20Emotion%20and%20Social%20Interaction%20%281986%29)

[^3]: Cannon, W.B.: The James-Lange theory of emotions: a critical examination and an alternative theory. Am. J. Psychol. **100**, 567–586 (1987)

[Article](https://doi.org/10.2307%2F1422695) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=The%20James-Lange%20theory%20of%20emotions%3A%20a%20critical%20examination%20and%20an%20alternative%20theory&journal=Am.%20J.%20Psychol.&volume=100&pages=567-586&publication_year=1987&author=Cannon%2CWB)

[^4]: Riyad, M., Adib, A.,: P300 Classification with ConvNets for Brain Invader. In: Congress on Smart Computing Technologies, pp. 205–214. Springer, Singapore (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Riyad%2C%20M.%2C%20Adib%2C%20A.%2C%3A%20P300%20Classification%20with%20ConvNets%20for%20Brain%20Invader.%20In%3A%20Congress%20on%20Smart%20Computing%20Technologies%2C%20pp.%20205%E2%80%93214.%20Springer%2C%20Singapore%20%282024%29)

[^5]: El Bouny, L., Zouidine, M., Fakhar, K., Khalil M.: Wavelet-based denoising diffusion models for ECG signal enhancement. In: 2024 IEEE 12th International Symposium on Signal, Image, Video and Communications (ISIVC), pp. 1–5 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=El%20Bouny%2C%20L.%2C%20Zouidine%2C%20M.%2C%20Fakhar%2C%20K.%2C%20Khalil%20M.%3A%20Wavelet-based%20denoising%20diffusion%20models%20for%20ECG%20signal%20enhancement.%20In%3A%202024%20IEEE%2012th%20International%20Symposium%20on%20Signal%2C%20Image%2C%20Video%20and%20Communications%20%28ISIVC%29%2C%20pp.%201%E2%80%935%20%282024%29)

[^6]: Akil, S., Sekkate, S., Adib, A.: Impact of economic income on mental health post-quarantine. In: 2024 IEEE Thirteenth International Conference on Image Processing Theory, Tools and Applications (IPTA), pp. 1–7 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Akil%2C%20S.%2C%20Sekkate%2C%20S.%2C%20Adib%2C%20A.%3A%20Impact%20of%20economic%20income%20on%20mental%20health%20post-quarantine.%20In%3A%202024%20IEEE%20Thirteenth%20International%20Conference%20on%20Image%20Processing%20Theory%2C%20Tools%20and%20Applications%20%28IPTA%29%2C%20pp.%201%E2%80%937%20%282024%29)

[^7]: Elachhab, A., Laadissi, E.M., Tabine, A., Hajjaji, A.: Deep learning and data augmentation for robust battery state of charge estimation in electric vehicles. Electrical Engineering (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Elachhab%2C%20A.%2C%20Laadissi%2C%20E.M.%2C%20Tabine%2C%20A.%2C%20Hajjaji%2C%20A.%3A%20Deep%20learning%20and%20data%20augmentation%20for%20robust%20battery%20state%20of%20charge%20estimation%20in%20electric%20vehicles.%20Electrical%20Engineering%20%282024%29)

[^8]: Chakhtouna, A., Sekkate, S., Adib, A.: Multi-features learning via attention-BLSTM for speech emotion recognition. In: 2024 IEEE 12th International Symposium on Signal, Image, Video and Communications (ISIVC), pp. 1–6 (2024)

[Google Scholar](https://scholar.google.com/scholar?&q=Chakhtouna%2C%20A.%2C%20Sekkate%2C%20S.%2C%20Adib%2C%20A.%3A%20Multi-features%20learning%20via%20attention-BLSTM%20for%20speech%20emotion%20recognition.%20In%3A%202024%20IEEE%2012th%20International%20Symposium%20on%20Signal%2C%20Image%2C%20Video%20and%20Communications%20%28ISIVC%29%2C%20pp.%201%E2%80%936%20%282024%29)

[^9]: El Hallani, A., Chakhtouna, A., Adib, A.: Advanced speech biomarker integration for robust Alzheimer’s disease diagnosis. Ann. Telecommun. (2025)

[Google Scholar](https://scholar.google.com/scholar?&q=El%20Hallani%2C%20A.%2C%20Chakhtouna%2C%20A.%2C%20Adib%2C%20A.%3A%20Advanced%20speech%20biomarker%20integration%20for%20robust%20Alzheimer%E2%80%99s%20disease%20diagnosis.%20Ann.%20Telecommun.%20%282025%29)

[^10]: Chakhtouna, A., Sekkate, S., Adib, A.: Speaker and gender dependencies in within/cross linguistic Speech Emotion Recognition. Int. J. Speech Technol. **26**, 609–625 (2023)

[Article](https://link.springer.com/doi/10.1007/s10772-023-10038-9) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Speaker%20and%20gender%20dependencies%20in%20within%2Fcross%20linguistic%20Speech%20Emotion%20Recognition&journal=Int.%20J.%20Speech%20Technol.&volume=26&pages=609-625&publication_year=2023&author=Chakhtouna%2CA&author=Sekkate%2CS&author=Adib%2CA)

[^11]: Ezzameli, K., Mahersia, H.: Emotion recognition from unimodal to multimodal analysis: a review. Inf. Fusion **99** (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Ezzameli%2C%20K.%2C%20Mahersia%2C%20H.%3A%20Emotion%20recognition%20from%20unimodal%20to%20multimodal%20analysis%3A%20a%20review.%20Inf.%20Fusion%2099%20%282023%29)

[^12]: Chakhtouna, A., Sekkate, S., Adib, A.: Modeling speech emotion recognition via imagebind representations. Procedia Comput. Sci. **236**, 428–435 (2024)

[Article](https://doi.org/10.1016%2Fj.procs.2024.05.050) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Modeling%20speech%20emotion%20recognition%20via%20imagebind%20representations&journal=Procedia%20Comput.%20Sci.&volume=236&pages=428-435&publication_year=2024&author=Chakhtouna%2CA&author=Sekkate%2CS&author=Adib%2CA)

[^13]: Chakhtouna, A., Sekkate, S., Adib, A.: Speech emotion recognition using pre-trained and fine-tuned transfer learning approaches. In: The Proceedings of the International Conference on Smart City Applications, pp. 365–374 (2022)

[Google Scholar](https://scholar.google.com/scholar?&q=Chakhtouna%2C%20A.%2C%20Sekkate%2C%20S.%2C%20Adib%2C%20A.%3A%20Speech%20emotion%20recognition%20using%20pre-trained%20and%20fine-tuned%20transfer%20learning%20approaches.%20In%3A%20The%20Proceedings%20of%20the%20International%20Conference%20on%20Smart%20City%20Applications%2C%20pp.%20365%E2%80%93374%20%282022%29)

[^14]: Chakhtouna, A., Sekkate, S., Adib, A.: Unveiling embedded features in Wav2vec2 and HuBERT msodels for Speech Emotion Recognition. Procedia Comput. Sci. **232**, 2560–2569 (2024)

[Article](https://doi.org/10.1016%2Fj.procs.2024.02.074) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Unveiling%20embedded%20features%20in%20Wav2vec2%20and%20HuBERT%20msodels%20for%20Speech%20Emotion%20Recognition&journal=Procedia%20Comput.%20Sci.&volume=232&pages=2560-2569&publication_year=2024&author=Chakhtouna%2CA&author=Sekkate%2CS&author=Adib%2CA)

[^15]: Mao, S., Ching, P.C., Lee, T.: Deep learning of segment-level feature representation with multiple instance learning for utterance-level speech emotion recognition. In: Interspeech, pp. 1686–1690 (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Mao%2C%20S.%2C%20Ching%2C%20P.C.%2C%20Lee%2C%20T.%3A%20Deep%20learning%20of%20segment-level%20feature%20representation%20with%20multiple%20instance%20learning%20for%20utterance-level%20speech%20emotion%20recognition.%20In%3A%20Interspeech%2C%20pp.%201686%E2%80%931690%20%282019%29)

[^16]: Jo, A.H., Kwak, K.C.: Speech emotion recognition based on two-stream deep learning model using Korean audio information. Appl. Sci. **13** (4), 2167 (2023)

[Article](https://doi.org/10.3390%2Fapp13042167) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Speech%20emotion%20recognition%20based%20on%20two-stream%20deep%20learning%20model%20using%20Korean%20audio%20information&journal=Appl.%20Sci.&volume=13&issue=4&publication_year=2023&author=Jo%2CAH&author=Kwak%2CKC)

[^17]: Latif, S., Rana, R., Younis, S., Qadir, J., et al.: Transfer learning for improving speech emotion classification accuracy. arXiv preprint [arXiv:1801.06353](http://arxiv.org/abs/1801.06353) (2018)

[^18]: Liu, Z., Wang, Y., Vaidya, S., Ruehle, F., et al.: Kan: Kolmogorov-arnold networks. arXiv preprint [arXiv:2404.19756](http://arxiv.org/abs/2404.19756) (2024)

[^19]: Radford, A., Kim, J. W., Xu, T., Brockman, G., et al.: Robust speech recognition via large-scale weak supervision. In: International Conference on Machine Learning, pp. 28492–28518 (2023)

[Google Scholar](https://scholar.google.com/scholar?&q=Radford%2C%20A.%2C%20Kim%2C%20J.%20W.%2C%20Xu%2C%20T.%2C%20Brockman%2C%20G.%2C%20et%20al.%3A%20Robust%20speech%20recognition%20via%20large-scale%20weak%20supervision.%20In%3A%20International%20Conference%20on%20Machine%20Learning%2C%20pp.%2028492%E2%80%9328518%20%282023%29)

[^20]: Busso, C., Bulut, M., Lee, C.-C., Kazemzadeh, A., et al.: Iemocap: interactive emotional dyadic motion capture database. Lang. Res. Eval. **42**, 335–359 (2008)

[Article](https://link.springer.com/doi/10.1007/s10579-008-9076-6) [Google Scholar](https://scholar.google.com/scholar_lookup?&title=Iemocap%3A%20interactive%20emotional%20dyadic%20motion%20capture%20database&journal=Lang.%20Res.%20Eval.&volume=42&pages=335-359&publication_year=2008&author=Busso%2CC&author=Bulut%2CM&author=Lee%2CC-C&author=Kazemzadeh%2CA)

[^21]: Liu, J., Liu, Z., Wang, L., Guo, L., Dang, J.: Speech emotion recognition with local-global aware deep representation learning. In: ICASSP 2020-2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), pp. 7174–7178 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Liu%2C%20J.%2C%20Liu%2C%20Z.%2C%20Wang%2C%20L.%2C%20Guo%2C%20L.%2C%20Dang%2C%20J.%3A%20Speech%20emotion%20recognition%20with%20local-global%20aware%20deep%20representation%20learning.%20In%3A%20ICASSP%202020-2020%20IEEE%20International%20Conference%20on%20Acoustics%2C%20Speech%20and%20Signal%20Processing%20%28ICASSP%29%2C%20pp.%207174%E2%80%937178%20%282020%29)

[^22]: Liu, J., Liu, Z., Wang, L., Guo, L., Dang, J.: Temporal attention convolutional network for speech emotion recognition with latent representation. In: INTERSPEECH, pp. 2337–2341 (2020)

[Google Scholar](https://scholar.google.com/scholar?&q=Liu%2C%20J.%2C%20Liu%2C%20Z.%2C%20Wang%2C%20L.%2C%20Guo%2C%20L.%2C%20Dang%2C%20J.%3A%20Temporal%20attention%20convolutional%20network%20for%20speech%20emotion%20recognition%20with%20latent%20representation.%20In%3A%20INTERSPEECH%2C%20pp.%202337%E2%80%932341%20%282020%29)

[^23]: Li, Y., Baidoo, C., Cai, T., Kusi, G.A.: Speech emotion recognition using 1d cnn with no attention. In: 2019 23rd International Computer Science and Engineering Conference (ICSEC), pp. 351–356 (2019)

[Google Scholar](https://scholar.google.com/scholar?&q=Li%2C%20Y.%2C%20Baidoo%2C%20C.%2C%20Cai%2C%20T.%2C%20Kusi%2C%20G.A.%3A%20Speech%20emotion%20recognition%20using%201d%20cnn%20with%20no%20attention.%20In%3A%202019%2023rd%20International%20Computer%20Science%20and%20Engineering%20Conference%20%28ICSEC%29%2C%20pp.%20351%E2%80%93356%20%282019%29)