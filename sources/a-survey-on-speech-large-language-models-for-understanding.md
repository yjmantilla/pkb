---
title: "A Survey on Speech Large Language Models for Understanding"
source: "https://ieeexplore.ieee.org/abstract/document/11278041"
author:

created: 2026-04-30
description: "Speech understanding is essential for interpreting the diverse forms of information embedded in spoken language, including linguistic, paralinguistic, and non-l"
tags:
  - "clippings"
  - "speech-representations"
  - "speech"
  - "citable"
  - "ai-model"
---
## Abstract:

Speech understanding is essential for interpreting the diverse forms of information embedded in spoken language, including linguistic, paralinguistic, and non-linguistic...

---

Speech contains a wealth of useful information, ranging from linguistic content, such as lexical meaning and syntax, to paralinguistic cues like emotion and speaking style, as well as non-linguistic context, such as background noise and spatial cues. Understanding speech is therefore an essential task, enabling effective human-machine communication and driving advancements in diverse fields such as virtual assistants, automatic transcription, sentiment analysis, and conversational AI \[1\], \[2\]. Research on speech understanding has a long and evolving history, deeply rooted in the intersection of speech processing and natural language processing (NLP). Its development has spanned decades, transitioning from early rule-based and statistical approaches to modern deep learning and large language model (LLM) paradigms \[3\].

Although the term “speech understanding” is widely used, there is currently no universally accepted definition. In this survey, we adopt a broad, task-agnostic definition: speech understanding can be defined as the integrated process that involves the perception and cognition of spoken language, ultimately producing a textual interpretation. Unlike traditional natural language understanding (NLU), which operates solely on symbolic textual input, speech understanding involves the multimodal interpretation of acoustic signals—including lexical content, prosody, speaker traits, and environmental context. It requires not only recognizing what is said but also perceiving how it is said, who is speaking, and under what conditions. This process entails extracting diverse information types from speech, aligning them with cognitive goals, and producing structured or generative outputs, thereby bridging low-level signal processing with high-level language understanding.

This contrasts with spoken language understanding (SLU), which focuses more narrowly on extracting structured semantics from transcribed speech \[3\]. In comparison, our definition reflects a more holistic view that treats speech as a multimodal signal for broad understanding and reasoning tasks. In this introduction, we will start with the historical evolution of speech understanding approaches, and subsequently introduce the primary focus of this survey: Speech Large Language Models (Speech LLMs) for understanding.

Speech understanding was historically approached through a cascaded architecture, where individual subtasks—such as transcribing speech into text via automatic speech recognition (ASR) and extracting semantic information like intent or entities—were handled by separate modules in a sequential pipeline. This design became the standard paradigm from the 1990s through the 2010s, forming the backbone of early research and commercial systems alike \[3\], \[4\]. Over time, ASR and downstream semantic tasks evolved largely independently. ASR research focused on reducing word error rates through advances in acoustic modeling, language modeling, and, later, deep learning-based end-to-end transcription models \[5\], \[6\], \[7\]. Meanwhile, semantic understanding tasks—often grouped under the term SLU—progressed from rule-based systems to statistical and neural approaches for intent classification and slot filling, frequently borrowing methods from text-based NLP \[8\]. Although researchers recognized the limitations of this modular setup, efforts to optimize the subtasks jointly remained limited. Early approaches included passing ASR confidence scores or N-best hypotheses to the semantic module \[9\], \[10\], \[11\], while later work explored joint modeling via feature sharing and multi-task learning \[12\], \[13\], \[14\]. However, these integrations were generally shallow and constrained by the rigid pipeline structure, which remained dominant throughout this period.

While cascaded architectures have been widely adopted due to their modularity and flexibility, researchers have long identified several fundamental drawbacks. These include the propagation of recognition errors to the understanding component \[4\], increased latency from sequential processing \[15\], loss of valuable acoustic and prosodic information during transcription \[16\], and the lack of joint optimization across modules \[4\]. These limitations have motivated a shift toward more unified modeling approaches. In recent years, this has led to the emergence of end-to-end speech language models that integrate recognition and understanding into a single system.

The first major stage in the development of unified speech language models emerged in the late 2010 s, characterized by end-to-end systems that map speech directly to high-level semantic representations, such as textual labels, summaries, or task-specific outputs \[4\], \[14\], \[17\], \[18\]. These models typically employed either encoder architectures trained from scratch using ASR modeling frameworks, such as CTC, RNN-T, or attention-based encoder-decoder models \[7\], \[19\], \[20\], \[21\], or self-supervised pretrained encoders such as wav2vec 2.0, HuBERT, and WavLM \[22\], \[23\], \[24\]. In these cases, encoders are paired with lightweight decoders to produce task-specific semantic outputs. Compared to cascaded pipelines, these models offer reduced error propagation, improved robustness, and greater architectural simplicity. **However, although task-specific models have achieved relatively mature performance in individual speech understanding tasks, they remain inadequate for the broader challenge of general speech understanding, which requires the ability to process and integrate diverse types of information from speech**. The capacity for complex reasoning and task adaptation of models at this stage remains limited, primarily due to the simple decoders and the lack of general linguistic knowledge. These limitations motivate the next-stage LLM-driven approaches.

Speech LLMs transition speech understanding from the encoder-decoder architectures to language-model-centric frameworks. Leveraging powerful pretrained text-based LLMs, Speech LLMs map speech inputs, which are typically processed by self-supervised encoders, into the textual embedding space. This enables language models to perform reasoning, text generation, and understanding directly from speech. This architecture enables a wide range of downstream tasks, including summarization, question answering, and emotion detection through flexible prompting and instruction tuning. Representative models such as SALMONN \[25\], SEED \[26\], Listen-Think-Understand \[27\], and DESTA-2 \[28\] exemplify this paradigm \[29\], \[30\], \[31\], \[32\]. More importantly, Speech LLMs integrate transcription, semantic interpretation, and response generation into a unified framework, eliminating the barrier between ASR and SLU and enabling holistic speech understanding. A detailed discussion of general speech understanding is provided in Section III.

Despite the rapid progress of LLM-centric frameworks, the definition of Speech LLMs remains non-standardized in current research. Although interpretations and research scopes vary, this survey defines Speech LLMs as understanding-oriented spoken language processing. Specifically, we focus on Speech LLMs that generate textual outputs, such as transcriptions, intent labels, summaries, or answers. Alternative definitions may include speech-to-speech generation tasks, which are beyond the scope of this work and are reviewed in other surveys \[33\], \[34\]. While several recent surveys have explored the related concepts \[33\], \[35\], none have provided a focused and systematic review from the perspective of speech understanding. **Beyond serving as a core module in most mainstream Speech LLMs, speech understanding itself has emerged as a distinct research direction, with dedicated Speech LLMs now being developed solely for understanding tasks—a trend gaining significant traction in both academia and industry \[25\], \[27\], \[31\], \[36\], \[37\], \[38\].** Existing surveys primarily focus on speech generation or broad multimodal frameworks, offering limited context for linking Speech LLMs to traditional speech understanding tasks. This survey fills this gap by providing a focused, structured overview of the design, training, and evaluation of Speech LLMs for understanding-oriented tasks.

Our contributions are as follows:

- We present the first comprehensive survey of Speech Large Language Models (Speech LLMs) from the perspective of speech understanding, providing the first systematic conceptualization and task-oriented definition of speech understanding itself, along with a taxonomy spanning informational, functional, and format dimensions.
- We review the evolution of modeling approaches—ranging from cascaded pipelines to LLM-centric architectures—and provide a structured synthesis of current model designs, training strategies, and datasets, along with an innovative evaluation of their generalization performance across speech understanding tasks.
- Based on our own experimental analysis, we identify key challenges facing current Speech LLMs—including instruction sensitivity and the first articulation of limitations in semantic reasoning—and outline directions for future research toward more generalizable and robust speech understanding.

By centering the discussion on understanding-oriented tasks and offering both conceptual clarity and technical depth, this work serves as a foundational reference that helps researchers in speech processing, spoken language understanding, multimodal modeling, and conversational AI better understand the current landscape and situate new models within it.

The remainder of this survey is organized as follows. Section II introduces a taxonomy of speech understanding tasks, defining the scope of speech understanding and categorizing the major task types. Section III traces the evolution of Speech LLMs, covering the shift from cascaded pipelines to unified models and contrasting architectural paradigms based on discrete versus continuous speech modeling. Section IV examines the model structure of Speech LLMs, breaking it down into three stages: modality feature extraction, modality information fusion, and LLM inference. Section V reviews the the training strategies of current Speech LLMs, while Section VI summarizes the key datasets used for pretraining and evaluation. Section VII discusses performance benchmarks and empirical results across various understanding tasks. Finally, Section VIII outlines open challenges and future directions for advancing deep speech understanding with LLMs. The overall organization is illustrated in Fig. 1.

[![Fig. 1. - 
          The organization of this paper is illustrated here. For ease of navigation, this figure offers a concise outline of the paper structure. Readers can refer to the respective sections for full details.
        ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu1-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu1-3640535-large.gif)

**Fig. 1.**

The organization of this paper is illustrated here. For ease of navigation, this figure offers a concise outline of the paper structure. Readers can refer to the respective sections for full details.

Speech understanding refers to the multimodal cognitive process of extracting and interpreting meaningful information from spoken input. It differs from Natural Language Understanding (NLU), which primarily focuses on linguistic cognition by inferring semantic, logical, and affective meaning from symbolic text. In contrast, speech understanding integrates a critical perceptual component. The spoken modality carries not only linguistic content but also paralinguistic and non-linguistic information. As such, speech understanding requires models to perceive and interpret rich, temporally dynamic acoustic signals before any semantic reasoning can occur. This makes speech understanding a joint endeavor of perception and cognition. Perception provides the raw, multi-dimensional signal basis, while cognition formulates the goals and strategies for interpretation. Crucially, the two are interdependent: effective cognition hinges on accurate, fine-grained perception, and meaningful perception must be guided by cognitive priors and task objectives. This relationship underpins the design of speech understanding systems.

To further structure the concept of speech understanding, we categorize speech understanding tasks from a system-oriented perspective, comprising three complementary dimensions, as illustrated in Fig. 2. The *informational* dimension focuses on the types of information embedded in speech input, forming the perceptual front end. The *functional* dimension considers the understanding objectives, defining what cognition tasks the system is expected to perform. Finally, the *format* dimension addresses how these tasks are instantiated in terms of input–output structure, capturing the overall formulation and operationalization of speech understanding systems.

### A. Informational Dimension: Categorization by Speech Information Types

Speech signals convey a rich set of information types, which can be grouped into three broad categories. Based on this dimension, speech understanding tasks can be further categorized according to the primary type of information they aim to interpret.

- *Linguistic Information:* Focuses on transcribing and understanding lexical and syntactic content from speech. Key tasks include Automatic Speech Recognition (ASR) and Spoken Language Translation (SLT), where challenges arise from homophones, dialectal variations, and transcription errors.
- *Paralinguistic Information:* Concerned with extracting cues beyond words, such as emotion, prosody, and speaker intent. Related tasks include Emotion Recognition, Speaker Diarization, and Dialogue Act Classification, which rely on features like intonation and speaking style.
- *Non-Linguistic Information:* Encompasses contextual acoustic cues supporting interaction analysis. This includes tasks like Voice Activity Detection (VAD), Sound Event Classification, and Acoustic Scene Understanding, which depend on background noise, speaker turns, and spatial cues.

This dimension informs the design of front-end speech encoders by highlighting the diverse information types embedded in speech. By distinguishing between linguistic, paralinguistic, and non-linguistic content, it enables targeted modeling strategies for feature extraction, representation learning, and input conditioning in speech understanding systems.

### B. Functional Dimension: Categorization by Cognitive Objectives

Speech understanding tasks can be broadly categorized according to their cognitive goals into three progressive levels:

- *Perception Tasks:* Focus on extracting and manipulating audio signals, requiring minimal semantic reasoning. Representative tasks include automatic speech recognition (ASR), keyword spotting (KWS), speaker diarization (SD), and voice activity detection (VAD).
- *Shallow Cognition Tasks:* Rely on intuitive, pattern-based understanding, corresponding to fast and heuristic reasoning. Examples include speech translation (ST), emotion classification, intent detection, and sentiment recognition.
- *Deep Cognition Tasks:* Require complex, context-aware reasoning, often involving discourse modeling and inference. Typical tasks include complex spoken question answering, conversational summarization, speaker intent explanation in ambiguous contexts, and multimodal reasoning grounded in acoustic and textual context.

Our definition of the distinction between shallow and deep cognition tasks is grounded in the *Dual Process Theory of cognition*, which posits two complementary modes of reasoning: System 1, which operates quickly, automatically, and intuitively, and System 2, which engages in slower, more deliberate, and analytical thinking \[39\], \[40\]. In this framework, shallow cognition tasks in speech understanding reflect System 1 processes, as they often rely on heuristic or pattern-based interpretation without the need for explicit multi-step reasoning. In contrast, deep cognition tasks correspond to System 2, requiring models to perform structured inference, integrate complex contextual cues, and resolve ambiguity. Framing speech understanding in this dual-process perspective helps provide a principled foundation for stratifying task complexity and guiding model design. By clarifying the cognitive depth required by each task type—from perceptual to deep inference—this dimension facilitates strategic planning of training objectives, curriculum learning stages, and evaluation protocols across varying levels of semantic complexity.

### C. Format Dimension: Categorization by Input–Output Structures

Unlike the information and functional dimensions, which describe what knowledge is used and why a task is performed, the format dimension specifies how inputs are packaged and constrained. Because structure is observable and easy to label, this dimension offers a crisp taxonomy without diluting the main theme. It also aligns with our evaluation suite in Section VII-A.

#### 1) Input Formulation

We can distinguish speech understanding tasks based on the structure of the input conditions. While the core inputs always include the speech signal and natural language instruction, models may also be conditioned on *external information sources* that enhance performance or enable new capabilities. Based on the degree of structure and prior knowledge encoded in these inputs, we categorize them into:

- *Structured Input Sources:* Provide explicitly formatted signals that provide direct guidance to the model. Typical examples include:
	- *Hotword lists* (e.g., user-defined keywords for biasing ASR),
		- *Speaker embeddings* (e.g., fixed-dimensional vectors encoding speaker identity),
		- *Lexicons or domain constraints* (e.g., task-specific vocabularies or grammar rules),
		- *Knowledge structures* (e.g., ontologies, documents, or database schemas that encode external domain knowledge in structured form)
- *Unstructured Input Sources:* Provide open-form, contextually derived signals that are not explicitly constrained in format. These may include:
	- *Dialogue history* (e.g., preceding conversational turns),
		- *Background descriptions* (e.g., task instructions, user profiles, or scene summaries), and
		- *Long-form contextual summaries* (e.g., prior utterance content or semantic memory).

#### 2) Output Formulation

We can also categorize tasks based on the structure of their output, distinguishing among the following types:

- *Structured Tasks:* Defined by structured outputs that can be rigorously evaluated using formal methods, enabling the computation of precise and objective performance metrics. Common examples include:
	- *Automatic speech recognition (ASR)* (e.g., transcribing speech into verbatim or normalized text),
		- *Speaker analysis* (e.g., speaker identification, verification, or diarization with predefined labels),
		- *Emotion classification* (e.g., assigning categorical emotional labels),
		- *Keyword spotting (KWS)* (e.g., detecting predefined terms),
		- *Voice activity detection (VAD)* (e.g., binary speech/non-speech labeling), and
		- *Task-oriented structured tagging* (e.g., predicting dialogue acts, user intents, or domain codes).
- *Unstructured Tasks:* Produce open-ended outputs without standardized answers, resisting formal evaluation and depending on contextual or subjective judgment. Representative tasks include:
	- *Speaker description* (e.g., generating natural language descriptions of speaker traits such as age or accent),
		- *Emotion description* (e.g., expressing nuanced emotional states in free-form text),
		- *Speech quality assessment* (e.g., evaluating fluency or clarity),
		- *Conversational style analysis* (e.g., inferring politeness or assertiveness),
		- *User intent explanation* (e.g., interpreting ambiguous or informat utterances), and
		- *Free-form speech interpretation* (e.g., summarizing or describing content in open language).
- *Weakly-Structured Tasks:* Lie between structured and unstructured outputs, allowing multiple acceptable answers under soft correctness constraints. Such tasks support approximate objective metrics that enable partial formal evaluation. Typical cases include:
	- *Speech translation (ST)* (e.g., converting speech from one language to another with diverse valid outputs, evaluated by BLEU \[41\], \[42\]),
		- *Speech summarization* (e.g., generating condensed content where factual coverage and coherence are evaluated using ROUGE \[43\]).

This framework not only enables a comprehensive categorization of existing tasks but also provides guidance for the design of future speech understanding systems. Structured outputs benefit from precise metrics and strong supervision, while unstructured outputs better reflect real-world complexity and the need for generative reasoning. In particular, structured outputs are directly interpretable by downstream computational systems and allow reliable, objective evaluation, whereas unstructured outputs require generative understanding and are not straightforwardly machine-actionable. Similarly, integrating structured and unstructured input sources enhances model controllability, personalization, and adaptability in diverse environments.

Taken together, when situated in the context of Speech LLMs, this taxonomy not only provides a principled framework for organizing current capabilities, but also highlights the evolving demands of future systems. With increasing emphasis on multi-task generalization, instruction following, and context-aware reasoning, Speech LLMs must flexibly integrate diverse input types and generate outputs of varying structure and specificity. The taxonomy thus informs the development of more adaptive, interpretable, and goal-aligned architectures. It supports a shift from narrow ASR-centric modeling to a broader paradigm of holistic spoken language understanding.

To provide a clearer exposition of the development trajectory of Speech LLMs, this section unfolds the discussion along two complementary dimensions. Section III-A presents a longitudinal view that traces the historical evolution of speech understanding systems, highlighting the emergence and significance of Speech LLMs in this context. In parallel, Section III-B offers a structural perspective, characterizing the progression and iterations of Speech LLMs across two different architectural paradigms.

### A. From Cascade to Unified: The Emergence of Speech LLMs for Speech Understanding

Following the definition of speech understanding in Section II and the task taxonomy proposed in Section II-B, we can categorize speech understanding into two broad perspectives: one centered on perceptual processing (e.g., ASR and related tasks), and the other focused on cognitive reasoning over spoken input, encompassing both shallow and deep understanding objectives (e.g., SLU and beyond). Given the absence of a consistent historical term, this section traces the evolution of Speech LLMs from the representative perspectives of ASR and SLU, which jointly illuminate the broader trajectory of speech understanding. While not exhaustive, these two lines of development offer illustrative anchors for understanding the structural and functional progress in the field.

We broadly divide the development of models for speech understanding into two main stages. The first is the era of *modular architectures*, typically characterized by cascaded structures with separately optimized components. The second is the emergence of *end-to-end architectures*, where the system is trained holistically to optimize performance across the entire pipeline. Within the end-to-end paradigm, a further transition has occurred. The field has moved from *E2E Specialized Speech Understanding Systems*, which are designed for a single or a small set of well-defined tasks, to *E2E General Speech Understanding Systems*. In these general systems, language instructions can specify any task, and the model supports outputs that can capture any form of understanding. Each stage not only represents a milestone in model capacity and architecture but also demonstrates the increasing convergence between tasks, ultimately culminating in a unified modeling capability aimed at general speech understanding. An overview of this development process is illustrated in Fig. 3.

[![Fig. 2. - 
            A Three-Dimensional Taxonomy of Speech Understanding Tasks.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu2-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu2-3640535-large.gif)

**Fig. 2.**

A Three-Dimensional Taxonomy of Speech Understanding Tasks.

[![Fig. 3. - 
            The structural evolution of Speech LLMs, illustrating the transition from modular architectures to end-to-end frameworks. Early modular systems are exemplified by traditional ASR pipelines. The end-to-end paradigm subsequently diverges into two branches: E2E Specialized Speech Understanding System and E2E General Speech Understanding System. E2E General Speech Understanding systems, also regarded as LLM-Centric systems, are further categorized into discrete sequence modeling (top) and continuous sequence modeling (bottom), reflecting their strategies of representing features.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu3-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu3-3640535-large.gif)

**Fig. 3.**

The structural evolution of Speech LLMs, illustrating the transition from modular architectures to end-to-end frameworks. Early modular systems are exemplified by traditional ASR pipelines. The end-to-end paradigm subsequently diverges into two branches: E2E Specialized Speech Understanding System and E2E General Speech Understanding System. E2E General Speech Understanding systems, also regarded as *LLM-Centric* systems, are further categorized into discrete sequence modeling (top) and continuous sequence modeling (bottom), reflecting their strategies of representing features.

#### 1) Modular Architecture

Early efforts in speech processing largely followed a modular architecture. For example, in ASR, the pipeline typically involved a separately trained acoustic model (AM), language model (LM), and decoder, integrated via weighted finite-state transducers (WFSTs). These components were optimized independently and lacked task adaptability. Typical implementations include Gaussian Mixture Model - Hidden Markov Model (GMM-HMM) frameworks and later Deep Neural Network (DNN-HMM) hybrids \[5\], \[44\], which formed the foundation of many large-scale ASR toolkits such as Kaldi \[45\] and CMU Sphinx \[46\]. This design required extensive engineering effort, heavily relied on handcrafted configurations, and lacked the capacity to integrate knowledge holistically or support multitask generalization.

Moreover, in the SLU domain, spoken language understanding was conventionally built on top of ASR outputs. Early SLU systems adopted the *ASR $\rightarrow$ NLU* pipeline \[3\], \[47\], where natural language understanding models operated on 1-best or N-best ASR hypotheses to perform tasks such as intent classification, slot filling, and dialogue state tracking \[48\]. However, such cascaded systems suffer from error propagation and degraded performance in noisy or ambiguous speech scenarios. In addition, rich prosodic and acoustic cues were often lost during transcription, limiting deeper semantic interpretation.

Other models for specific speech understanding tasks, such as emotion recognition, have similarly undergone a modular development phase. Although these approaches provide strong interpretability, they still exhibit substantial potential for improvement in performance.

#### 2) E2E Specialized Speech Understanding System

With the development of deep neural networks, there has been a shift toward end-to-end architectures in models designed for specific speech understanding tasks: from modular pipelines to end-to-end architectures, and from cascaded processing to unified modeling. We refer to this class of models, which focus on solving one or a few specific tasks in an end-to-end manner, as *E2E Specialized Speech Understanding System*.

The *first major stage* in this transition is the rise of end-to-end ASR architectures. Early E2E systems employed frame-synchronous models based on Connectionist Temporal Classification (CTC) \[19\], which simplified training by removing the need for frame-level alignment \[21\]. These were followed by Recurrent Neural Network Transducer (RNN-T) \[20\], which relaxed independence assumptions and supported streaming applications. In parallel, attention-based encoder-decoder (AED) models such as Listen, Attend and Spell (LAS) \[7\], SpeechT5 \[49\], and Whisper \[50\] introduced autoregressive decoding and enabled multitask learning capabilities. These systems marked a substantial improvement over traditional pipelines by integrating acoustic and linguistic modeling into a single trainable framework.

Complementing these architectural advancements, the E2E era also witnessed significant perceptual progress through self-supervised learning (SSL). Early hand-crafted features such as MFCCs were gradually replaced by rich, contextualized representations learned by models such as wav2vec 2.0 \[22\], HuBERT \[23\], and WavLM \[24\]. These SSL-based encoders substantially enhanced robustness and generalization and became foundational in many state-of-the-art E2E systems \[51\]. However, despite these improvements, conventional E2E models often rely on relatively shallow decoders with limited linguistic capacity, making them less suitable for tasks that require long-range reasoning or rich semantic interpretation.

In the SLU area, the end-to-end transition involved models that directly predicted semantic structures—such as intents and slots—from raw speech without relying on intermediate transcriptions. Pioneering works like Joint SLU \[4\] and SLU-Transformer \[52\] demonstrated that speech encoders pretrained via SSL could be effectively paired with semantic decoders to form compact, robust pipelines.

As the capabilities of end-to-end models improved, their target tasks evolved toward more aggregated objectives. While these models enhanced generalization and task-specific performance, they continued to face challenges in data efficiency and transferability across tasks.

#### 3) E2E General Speech Understanding System

To address the need for richer and more general speech understanding tasks, a more recent trend within the end-to-end paradigm is the development of *E2E General Speech Understanding Systems* or *LLM-Centric Systems*, which integrate LLMs as core reasoning components. Speech understanding models centered around LLMs reflect a paradigm shift toward cognition-guided perception. This trend marks an important development in the evolution of multimodal systems, where the language model no longer holds an equal status with the perceptual front-end but instead serves as the central controller of interpretation. The conceptual roots of this perspective can be traced back to \[53\]. These systems represent a structural and functional evolution from conventional E2E models, shifting the focus from acoustic-driven generation to language-model-driven interpretation. Unlike traditional AED-based architectures, *LLM-Centric* systems align speech inputs to the text modality and utilize LLMs for decoding, generation, and reasoning \[25\], \[26\], \[29\], \[30\]. This cross-modal design not only enables transfer to diverse downstream tasks—such as summarization, translation, and question answering—but also brings enhanced modularity, interpretability, and instruction-following capability \[27\], \[28\], \[31\], \[32\].

Recent advances in *LLM-Centric* architectures have enabled unprecedented levels of task generalization and language comprehension in speech understanding. It is important to distinguish true *LLM-Centric* approaches from conventional cascaded systems that merely connect ASR outputs to an LLM. While such pipeline-based designs \[54\], \[55\] allow downstream reasoning over text, they do not fundamentally integrate speech into the LLM framework and thus fall short of true *LLM-Centric* modeling. In contrast, recent instruction-tuned models such as Listen-Think-Understand \[27\] and SALMONN \[25\] exemplify *LLM-Centric* design: they take raw speech as input and directly generate semantically structured outputs for tasks such as intent inference, emotion detection, summarization, and knowledge-grounded question answering. These systems align speech representations with the powerful textual priors embedded in LLMs, enabling prompting, interpretation, and response generation within a unified representational space. This paradigm breaks down traditional barriers within speech understanding tasks, marking a shift toward truly holistic and integrated speech understanding systems.

The evolution from modular pipelines to end-to-end systems, and further into *LLM-Centric* frameworks, charts a trajectory of increasing unification across a wide range of speech understanding tasks like ASR and SLU. While ASR emphasizes accurate linguistic transduction and SLU targets semantic reasoning, both perspectives have progressively converged under the umbrella of Speech LLMs. These models offer a shared representational space and a unified decoding mechanism, making it possible to perform transcription, understanding, and generation in a cohesive framework. In this light, the emergence of *LLM-Centric* Speech LLMs marks a pivotal point in the development of general-purpose spoken language understanding—a capability referred to as **Speech Understanding (SU)** in this paper, as illustrated in Section II in detail.

### B. Structural Evolution of Speech LLM: Discrete Vs. Continuous Modeling

A key distinction in the development of Speech LLMs lies in how the speech and text modalities are integrated. This paper categorizes current approaches into two primary paradigms based on the representation format used to bridge audio and language: one relies on discrete tokenization of speech, inspired by the token-based processing paradigm of LLMs in text; the other maintains the speech modality in its continuous embedding form. These two paradigms are orthogonal in nature and reflect fundamentally different strategies for cross-modal alignment. (see Fig. 5).

[![Fig. 4. - 
            Overview of Speech LLM Architectures with Speech and Text Inputs and Text Outputs.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu4-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu4-3640535-large.gif)

**Fig. 4.**

Overview of Speech LLM Architectures with Speech and Text Inputs and Text Outputs.

[![Fig. 5. - 
            Illustration of three approaches to audio-text information fusion in Speech LLMs. The unified encoder in 5(c) is transformed from the text encoder by adding audio tokens to the existing text token vocabulary.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu5-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu5-3640535-large.gif)

**Fig. 5.**

Illustration of three approaches to audio-text information fusion in Speech LLMs. The unified encoder in 5(c) is transformed from the text encoder by adding audio tokens to the existing text token vocabulary.

The first, *discrete sequence modeling*, represents a paradigm in which continuous speech signals are transformed into sequences of quantized units—discrete acoustic tokens that resemble textual tokens in format and function. These discrete tokens are typically obtained through unsupervised or self-supervised speech representation models followed by vector quantization mechanisms. Popular approaches include HuBERT \[23\], w2v-BERT \[56\], EnCodec \[57\], and wav2vec-U \[58\], which learn to capture salient acoustic patterns and discretize them into a token vocabulary.

By converting speech into tokenized sequences, these models enable LLMs to process speech data using the same autoregressive or seq2seq frameworks designed for text. The resulting discrete units can be aligned with textual tokens or used directly as input/output for generation tasks. This token-level compatibility bridges the modality gap and facilitates seamless speech-text integration.

Representative studies that model speech purely via discretized acoustic tokens include SpeechGPT and AudioGPT \[59\], \[60\]. These methods quantize continuous waveforms into discrete code sequences (e.g., with neural codecs) and train a generative language model over the token streams, enabling speech understanding and synthesis within a unified, text-like modeling framework.

This modeling approach offers several advantages. It allows speech processing to benefit directly from LLM pretraining by transforming the problem into a format that LLMs are optimized for: token sequences. Moreover, it enables understanding and generation within the same unified framework. However, models that rely solely on discrete token representations to encode speech information tend to exhibit degraded perceptual quality in speech perception. First, their performance on semantic information perception tasks such as ASR is often inferior to that of traditional models like Whisper or HuBERT. Second, these models also show limited capability in perceiving fine-grained paralinguistic cues and other subtle aspects of speech. Building upon these considerations, this work does not primarily focus on purely discrete sequence modeling approaches.

The second paradigm, *continuous sequence modeling*, takes a fundamentally different approach by maintaining the speech signal in its continuous embedding form throughout the interaction with the language model. Instead of discretizing the audio, it extracts high-dimensional features from pretrained encoders (e.g., WavLM \[24\], Whisper \[50\], or Conformer \[61\]) and directly feeds them into the LLM, typically through a learned projection or lightweight adaptation layer.

This strategy preserves the rich temporal and spectral information inherent in speech and enables a smoother alignment with the LLM’s input space. Notably, *Pengi* \[31\] demonstrates how frozen LLMs can be prompted using projected speech embeddings to perform a variety of tasks. Subsequent works like *SALMONN* \[25\], *Qwen-Audio* \[36\], and *OSUM* \[32\] further extend this design to support broad speech understanding and multimodal reasoning tasks.

Continuous modeling has proven particularly effective for applications requiring fine-grained acoustic discrimination, including ASR \[30\], ST \[62\]. Models like *FireRedASR* \[30\] and *LLAST* \[62\] show that state-of-the-art performance can be achieved with minimal architectural modification. In many cases, a simple projection layer is sufficient, together with limited fine-tuning \[26\], \[29\].

Although this paradigm can achieve high accuracy in specific tasks, it also introduces certain challenges. When the amount of training data or task diversity is limited, the model is prone to overfitting \[63\], \[64\]. Moreover, when integrating with generative modules, it often requires stronger data-driven support or assistance from discrete modeling approaches \[65\], \[66\]. Given its scalability and its alignment with the trend of reusing pretrained language models \[67\], \[68\], continuous modeling has become an increasingly important approach in recent Speech LLM research. This paper, therefore, primarily focuses on continuous sequence modeling, while offering comparative insights into discrete token-based alternatives.

To further understand how Speech LLMs process and reason over spoken input, we examine their underlying architectural design. In particular, we investigate the overall structure of Speech LLMs, which typically follows a modular pipeline comprising distinct stages of processing. Various Speech LLM architectures have been developed up to now, all of which are structured around three fundamental stages: **Modality Feature Extraction**, **Modality Information Fusion**, and **LLM Inference**, as illustrated in Fig. 4.

All Speech LLMs begin by encoding the input information, regardless of the modality. Typically, these Speech LLMs leverage pretrained audio and text encoders to obtain representations of both modalities. The encoded audio and text embeddings are then fused to prepare them for input into the language model. This step transforms the outputs of the feature extraction process into a format compatible with the language model’s input requirements. For Speech LLMs that generates text output, which are also the primary focus of this survey, the LLM inference process typically revolves around a text decoder that receives the fused multimodal features as tokenized input to produce text. For models that optionally support audio output, an LLM decoder is trained to generate audio tokens, which are subsequently converted into speech using a vocoder. However, in this survey, we only focus on the Speech LLMs with text output.

### A. Modality Feature Extraction

As mentioned in the previous section, during the initial feature extraction stage, the audio is processed in two distinct ways. These approaches primarily differ in the output representation format of the audio features. **Continuous Sequence Modeling**, which is more commonly adopted in current Speech LLMs, encodes audio using a pretrained and fine-tuned audio encoder, producing an audio embedding $\mathbf $Z$_$a$\in \mathbb $R$^$B\times T \times D$$ where $B$, $T$, and $D$ represent batch size, number of frames, and embedding dimension. In contrast, **Discrete Sequence Modeling** discretizes the input audio signal into a sequence of discrete audio tokens—comparable to text tokens derived from raw text—which are not represented in embedding form. In this section, we summarize common models and techniques used to extract audio features using these two approaches.

#### 1) Continuous Sequence Modeling

Continuous sequence modeling is generally more straightforward and typically encodes audio in the form of a time-frequency representation extracted using traditional signal processing-based feature extraction methods from the raw audio signal. Given an audio input $X_$a$$, a pretrained audio encoder is applied to generate the audio features:

$$
\begin$equation*$
 \mathbf $Z$_$a$ = g(\mathbf $X$_$a$), \tag$1$ 
\end$equation*$
$$
 View Source

where $g$ denotes the audio encoding process. The most commonly used audio encoders in mainstream Speech LLMs are Whisper \[50\] and Conformer \[61\]. Whisper is a sequence-to-sequence Transformer model trained on a wide range of speech processing tasks, and its encoder is recognized as one of the most powerful for audio input processing. Conformer is another widely adopted architecture that combines convolutional layers with Transformers to effectively model both local and global dependencies in speech signals, particularly for automatic speech recognition tasks. Additional encoders used in mainstream Speech LLMs include WavLM \[24\], a predictive, self-supervised learning (SSL) pretrained model. In this stage, aside from the audio encoder itself, subsampling modules are often applied to reduce the temporal resolution of the input features, thereby decreasing computational costs and enabling the model to focus on higher-level representations. Ultimately, the output audio feature embeddings $\mathbf $Z$_$a$\in \mathbb $R$^$B\times T \times D$$ serve as the input for the next stage. In the context of continuous sequence modeling, Table I provides a detailed summary of representative models, organized according to their architectural components.

**TABLE I** Architectural Summary of Continuous Sequence Modeling Speech LLM Models

[![Table I- 
              Architectural Summary of Continuous Sequence Modeling Speech LLM Models
            ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t1-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t1-3640535-large.gif)

#### 2) Discrete Sequence Modeling

Discrete sequence modeling converts raw audio into a sequence of discrete tokens that represent acoustic content and can typically be decoded back into high-quality audio. The core idea behind generating discrete tokens lies in vector quantization. Building on VQ-VAE \[92\], which introduced the concept of encoding continuous audio features into symbolic representations via a learned codebook, current mainstream methods for generating discrete tokens include self-supervised pretrained audio tokenizers such as HuBERT \[23\], and neural codec models such as EnCodec \[57\], \[93\]. A general process involves first encoding raw audio into a sequence of latent representations, as in continuous sequence modeling: $\mathbf $Z$_$a$ = g(\mathbf $X$_$a$)$, where $\mathbf $Z$_$a$ = [\mathbf$z$_$1$,\mathbf$z$_$2$,\ldots,\mathbf$z$_$T$]$ with $\mathbf$z$_$t$\in \mathbb $R$^$d$$. Next, a quantization function $q(\cdot)$ is applied to obtain the final discrete audio tokens:

$$
\begin$equation*$
 u_$t$ = q(\mathbf$z$_$t$)\ \text$with$\ \mathbf$e$_$u_$t$$ = \mathcal $Q$(\mathbf$z$_$t$). \tag$2$ 
\end$equation*$
$$
 View Source

Here, $\mathcal $Q$$ denotes the quantization function that maps continuous vectors to codebook vectors $\mathbf$e$_$u_$t$$\in \mathbb $R$^$d$$, resulting in $u_$t$$, the discrete token index from the vocabulary set. The final output is a sequence of discrete audio token indices: $U = [u_$1$, u_$2$,\ldots, u_$T$]$. Specifically, self-supervised pretrained audio tokenizers, such as HuBERT, learn contextualized audio representations via masked prediction tasks and discretize intermediate features using k-means clustering. The resulting cluster indices serve as phoneme-like discrete tokens. Neural codec models, such as EnCodec, adopt an encoder–quantizer–decoder architecture in which the audio is compressed into discrete tokens using residual vector quantization and later reconstructed \[94\]. These models are trained end-to-end and produce high-fidelity audio that is well suited for generative tasks. The resulting discrete audio tokens are processed alongside tokenized text, with the integration method detailed in the following section.

### B. Modality Information Fusion

After completing the audio feature extraction stage, the most critical challenge in Speech LLMs lies in the alignment between the audio modality and the text modality. In this section, we discuss alignment methods in both continuous and discrete fashions. As introduced in the previous section, the output formats of these two methods differ, thereby requiring customized approaches to modality fusion in each case.

#### 1) Modality Fusion of Continuous Audio Embedding

Before addressing how to effectively combine the two types of information, one important consideration is determining which specific part of the audio modality should be used. Researchers currently tend to use the output from the final layer of the encoder as the primary source of audio modality information. However, various alternative methods also exist. For example, some approaches utilize intermediate layer outputs to capture more granular features \[51\], while others apply attention mechanisms to emphasize relevant parts of the audio signal \[24\].

Once the audio embedding is obtained, it is merged into the text embedding space using two main methods:

1. *Pre-Decoder Alignment:* This category refers to methods that align audio features with text embeddings before feeding them into the LLM decoder. A common approach is to project the audio feature information into the LLM’s text feature space through a connector \[25\], \[29\]. Specifically, the tensor containing audio features typically has its own distinct feature dimension. The audio feature embedding $\mathbf $Z$_$a$$ is projected into a vector with the same dimensionality as the text embedding using a projection function $w(\cdot)$: $\mathbf $H$_$a$ = w(\mathbf $Z$_$a$)$, where $\mathbf $H$_$a$$ denotes the projected audio embedding in the text embedding space. These audio embeddings are then concatenated with the input text embedding $\mathbf $H$_$t$$ to form a new embedding sequence that integrates both speech and text information: $\mathbf $H$ = [\mathbf $H$_$a$, \mathbf $H$_$t$]$. This combined sequence is then fed into the LLM. Some researchers have also implicitly incorporated the projection step within the original encoder, achieving modality projection by adjusting the encoder’s parameters during training \[36\]. Most models use a multi-layer perceptron (MLP) as the projection module, while some adopt more complex techniques such as Q-Former, which introduces trainable query tokens that attend to audio features and output fixed-length embeddings aligned with the LLM input space \[25\].
2. *In-Decoder Alignment (Cross Attention):* In addition to merging modalities before inputting them into the language model, another method establishes a bidirectional interaction between audio and text features via cross-attention \[75\]. In this approach, the model maintains separate audio and text streams, allowing each modality to attend to the other through cross-attention layers, thereby enabling deeper multimodal fusion within the transformer architecture.

#### 2) Modality Fusion of Discrete Audio Tokens

For discrete audio tokens, they are typically treated in the same way as text tokens. The core challenge lies in how to process audio tokens using only a text-based vocabulary. There are essentially two ways to address this issue:

1. *Token Mapping:* The most intuitive approach is to map audio tokens to text tokens that the LLM can process. Tsunoo et al. \[95\] propose using CTC predictions to efficiently generate prompts—referred to as CTC prompts. After audio features are converted into text tokens using CTC, these tokens are merged with the tokenized input text to create a unified token sequence that represents both modalities. This sequence is then fed into the LLM for processing. This approach not only preserves the semantic content of the audio features but also ensures consistency in the LLM’s processing pipeline.
2. *Token Space Expansion:* While projecting the speech modality into the text modality is straightforward, it does not achieve truly lossless modality fusion. Information loss and conflicts may occur during the modality conversion process (to be elaborated later). Therefore, researchers have proposed an alternative approach that modifies the input space of the LLM to natively incorporate the audio modality \[59\], \[96\], \[97\]. Specifically, this method augments the token space by adding audio tokens to the existing text token vocabulary, forming a unified token space. These new audio tokens are synthesized from the audio features extracted in the previous stage, thereby preserving a greater portion of the original audio information (illustrated in Fig. 5(c)).

#### 3) Modality Alignment Strategies Discussion

Building on the preceding discussion, current Speech LLMs fuse modalities (i) under continuous audio representations via pre-decoder alignment or in-decoder cross-attention, and (ii) under discrete representations via token mapping or token-space expansion.

In practice, systems vary along several choices. Beyond fusion, a fundamental design choice is which audio representation to align. Using final-layer encodings maximizes semantic abstraction, while intermediate-layer features expose finer prosodic or phonetic cues. Attention-based pooling, learned query resamplers (e.g., Q-Former/Perceiver-style), strided subsampling, and CTC-based temporal compression are all used to control the rate of audio tokens fed to the LLM, balancing information retention and compute. It is worth noting that several recent studies have explored novel alignment paradigms. For example, LegoSLM \[83\] leverages LLM word embeddings to construct weighted representations of speech signals, while AlignFormer \[98\] effectively simplifies speech feature sequences by operating directly on the CTC posterior distributions.

### C. LLM Inference

The LLM inference stage consists mainly of a text decoder, which is typically an autoregressive transformer decoder that generates text tokens conditioned on input audio and/or text. Architecturally, it often mirrors standard language models like GPT, but it is specially designed or adapted to handle multimodal inputs—such as projected audio embeddings or cross-attended audio features—alongside text tokens. To accommodate modality fusion, the decoder may incorporate special tokens (e.g., <aud>, <text>) or use prompt formatting to distinguish and contextualize different modalities during generation. Despite these adaptations, the decoder follows the same autoregressive objective: predicting the next text token given previous tokens and any additional input context. In models that generate both speech and text as output, the decoder may also be extended to support the generation of discrete audio tokens. One common approach, as seen in VALL-E, treats speech generation as a conditional codec language modeling task, where the model generates audio tokens conditioned on phoneme sequences and an acoustic prompt \[99\], \[100\]. Another approach used in SpeechGPT expands the LLM’s vocabulary to include both text and speech tokens, enabling the model to generate audio token sequences directly alongside or following text, depending on the instruction \[59\]. In both cases, the generated audio tokens are passed through a neural codec decoder (e.g., EnCodec \[57\] or HiFi-GAN \[101\]) to reconstruct the final waveform.

To support the development of understanding-oriented capabilities, Speech LLMs require carefully designed training strategies. Since most Speech LLMs designed for speech understanding rely on continuous sequence modeling, this section begins by describing training strategies specifically for models that utilize continuous embeddings. Training these models typically proceeds through three major stages: *Modality Alignment*, *Multitask Training*, and *Preference Alignment*, as summarized in Table II

**TABLE II** The Following Table Shows the Training Strategy of Speech LLM. We Show the Training Parameters At Different Stages, Corresponding to the Training Data Task Type (As Indicated Inside the Parentheses) and the Type of Capabilities the Final Model Has. The – Symbol Indicates That the Paper Does Not Mention It in Detail, While ✘ Symbol Indicates That the Model Does Not Have This Stage.

[![Table II- The Following Table Shows the Training Strategy of Speech LLM. We Show the Training Parameters At Different Stages, Corresponding to the Training Data Task Type (As Indicated Inside the Parentheses) and the Type of Capabilities the Final Model Has. The – Symbol Indicates That the Paper Does Not Mention It in Detail, While ✘ Symbol Indicates That the Model Does Not Have This Stage.](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t2-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t2-3640535-large.gif)

. Each stage plays a distinct role in gradually aligning modalities, expanding task capabilities, and fine-tuning the model to follow human intent. However, not all models necessarily undergo all three stages; the actual training stages employed depend on each model’s intended capabilities. The organization of this section will primarily follow these three stages of training for Speech LLMs with continuous sequence modeling. At the end of this section, we briefly discuss how models based on discrete speech tokens differ in their training approach.

### A. Modality Alignment

At the outset, Speech LLMs must learn a shared representation between spoken input and text. In this *modality alignment* phase, models typically leverage vast speech data to bridge the audio-text gap. A common strategy is to incorporate a pre-trained speech encoder and align its acoustic representations with a text-based LLM. For example, Qwen-Audio begins with a pretrained Whisper encoder coupled to an LLM backbone via a projector \[102\]. The alignment is refined by supervised tasks that force audio and text to correspond. Notably, automatic speech recognition (ASR) and audio captioning (AAC) are usually used as fundamental alignment tasks \[26\], \[28\].

The total parameters $\theta$ of the model are composed of $\theta _$\text$LLM$$$, $\theta _$\text$encoder$$$, and $\theta _$\text$adapter$$$. The model is trained to map an input audio sequence $a$ to a textual output $y$ (transcript or caption). The training data consist of many triples $(a, p, y)$: an audio input $a$, a textual prompt or task descriptor $p$, and the expected textual output $y$. The model is optimized to predict $y$. In this stage, the textual prompt $p$ typically remains unchanged, which makes modality alignment easier to train \[67\], \[103\]. The training loss can be represented as

$$
\begin$equation*$
 \mathcal $L$_$\text$a$\text$lign$$(\theta) = -\log P(y \mid \theta _$\text$LLM$$(\theta _$\text$adapter$$(\theta _$\text$encoder$$(a)), p). \tag$3$ 
\end$equation*$
$$
 View Source

Depending on the training objectives, many models focus on a single speech-to-text task (e.g., speech recognition) rather than training a multitask speech model capable of fully understanding speech. Therefore, many models only have one stage of modality alignment. The most common approach is to train only the adapter \[103\], \[104\]. However, after the initial alignment, continuing to add LoRA parameters for further training has been shown to maintain better modality alignment and achieve superior alignment performance \[67\]. Another training approach involves simultaneously training the encoder, adapter, and LoRA parameters, as seen in SLAM and FireRedASR \[30\], \[105\]. Interestingly, both models employ Conformer as the encoder, and the FireRedASR model has achieved SOTA performance on current recognition leaderboards—perhaps demonstrating that unfreezing the encoder and training with more parameters can yield better alignment results. There is currently no unified consensus on the order of training parameters. A common practice is to first train the adapter, then train the encoder and adapter, and finally add LoRA parameters for joint training \[32\]. However, it is also advisable to include stages where some parameters are frozen during training \[106\]. For multitask models like Qwen-audio \[36\] and Seed-ASR \[26\], LoRA parameters of LLMs are typically not used as training parameters for modality alignment—partly to preserve the original powerful contextual and reasoning abilities of the LLMs, expecting them to play a role in subsequent multitask training. Not all models follow this pattern, however: SALMONN \[25\] trains LoRA parameters during the alignment phase.

### B. Multitask Training

After modality alignment, speech features are mapped into the text space of the LLM. However, even when an LLM can accurately transcribe speech, it often loses the ability to reason about or follow instructions related to the transcribed content. Therefore, the LLM must undergo a multitask training phase to become a multitask speech model.

Taking models focused solely on ASR tasks as an example, during this phase many models add LoRA parameters to leverage the LLM’s contextual capabilities, including hotword usage and historical transcripts, to improve recognition results \[26\], \[107\]. However, these are not standard multitask training approaches but rather extensions of ASR tasks.

According to the requirements of different models for speech understanding tasks, there is no unified standard for the division of multitask content. SALMONN \[25\] classifies speech tasks into three difficulty levels: the first level is simple tasks, the second level is speech-based NLP tasks such as keyword extraction and translation, and the third level is speech-based reasoning tasks such as speech-based question answering and storytelling. OSUM \[32\] mainly focuses on tasks such as audio emotion classification and audio style classification. It is worth noting that although this paper mainly focuses on the speech understanding tasks defined by us, some models have extended speech capabilities to sounds and music, aiming to train a general Speech LLM. For example, Qwen-Audio \[36\] involves multitask training related to speech, sound, and music simultaneously.

The mainstream training approach resembles modality alignment: natural language instructions are concatenated with the aligned speech features and target text, and the model is directly trained for multitask via cross-entropy loss. Generally, LoRA parameters are added to the training at this stage to fine-tune the LLM, aiming to enable the LLM to acquire the multitask speech capability.

Notably, Qwen-Audio \[36\] employs a Whisper-like multitask training framework that uses task prefixes and does not use LoRA for fine-tuning. In contrast, OSUM \[32\] uses an **ASR+X** approach, where **X** represents other tasks. This means OSUM transcribes speech into text before performing any other audio tasks.

### C. Preference Alignment

The final development phase focuses on aligning the model’s behavior with user instructions and preferences, ensuring that it follows natural language commands reliably and produces responses that are helpful and context-appropriate.

After multitask alignment, some advanced Speech LLMs apply *preference alignment* using reinforcement learning \[102\]. This step optimizes the model’s responses according to human feedback or a learned reward function, refining qualities such as factual accuracy, relevance, or user satisfaction \[108\], \[109\], for example, Seed-ASR \[26\]. A prominent method is reinforcement learning from human feedback (RLHF), operationalized via Proximal Policy Optimization (PPO) \[110\]. In RLHF, the model is treated as a policy $\pi _\theta (y|X)$ that generates a response $y$ to an input $X$, and a reward model $R(X,y)$ (trained from human preference rankings) scores the response. The goal is to maximize expected reward $E_$X$[E_$y\sim \pi _\theta $[R(X,y)]]$ while keeping the language model’s output distribution close to the pre-trained model to avoid degrading fluency. PPO achieves this by updating $\theta$ in the direction of the policy gradient $\nabla _\theta J \approx E_$X,y$[R(X,y)\,\nabla _\theta \log \pi _\theta (y|X)]$ with constraints on the KL-divergence from the original policy. In practice, the PPO update uses a clipped surrogate objective to ensure stable improvements. An alternative, lightweight approach is *Direct Preference Optimization* (DPO) \[111\], which was employed in Qwen2-Audio’s final training. Unlike methods that require training a separate reward model, DPO directly leverages preference pairs $(y^+, y^-)$ for a given prompt $x$. It optimizes the policy to assign higher probability to the preferred response $y^+$ over the less favored $y^-$. The DPO loss can be formulated as:

$$
\begin$align*$
 \mathcal $L$_$\text$DPO$$(\theta) =& -\mathbb $E$_$(x,y^+,y^-) \sim \mathcal $D$$\\
 & \times \left[ \log \sigma \!\left(\frac$1$$\beta $\left(\log \pi _\theta (y^+|x) - \log \pi _\theta (y^-|x)\right)\right) \right]\!, \tag$4$ 
\end$align*$
$$
 View Source

where $\sigma$ is the sigmoid function and $\beta$ is a temperature hyperparameter. By minimizing this loss, the model is encouraged to prefer $y^+$ over $y^-$, effectively aligning its outputs with human preferences without the complexity of a reward model training loop. By the end of this reinforcement or preference alignment phase, the Speech LLM is highly tuned to follow instructions in a human-preferred way: it will listen to a user’s voice request and respond with appropriate content. Crucially, this stage often improves the model’s *factuality* and safety by penalizing incorrect or undesirable continuations. Qwen2-Audio’s authors note that after DPO fine-tuning, the model more faithfully adheres to user intent and makes fewer mistakes in complex audio-centric instructions. The strength of the instruction alignment phase lies in making the model not just capable, but also reliably usable: it transforms the multitask-trained foundation into a polite conversational agent that can handle open-ended voice interactions. However, it is worth noting that this phase has received limited attention in current Speech LLM research focused on speech understanding. Most existing works primarily emphasize modality alignment and task-specific performance, while instruction-following behavior, dialogue consistency, and safety alignment remain underexplored. As Speech LLMs continue to evolve toward general-purpose audio agents, further investigation into instruction and preference alignment is expected to play a crucial role in improving usability, controllability, and user interaction quality.

All of these stages combined – modality alignment, broad multitask learning, and final alignment with preferences – produce state-of-the-art Speech LLMs that can understand speech in rich context and interact with users naturally, bridging the gap between spoken language and the powerful reasoning of text-based LLMs.

### D. Training of Speech LLMs With Discrete Sequence Modeling

In contrast to the three-stage training pipeline of continuous embedding models, Speech LLMs based on discrete speech tokens adopt a simpler and more unified training strategy. For these models (e.g., AudioPaLM \[96\], SpeechGPT \[59\], VALL-E \[99\]), after generating the discrete tokens for audio representations, these token sequences are treated analogously to text tokens, allowing a pretrained or newly initialized language model to be trained directly using a standard autoregressive objective.

Training typically proceeds by fine-tuning on paired speech-text data in a unified token space, without the need for a separate modality alignment stage, since discrete-token models do not introduce structural modifications such as adapters, which are common in continuous embedding models. This simplifies the pipeline and enables models to benefit directly from pretrained LLMs. In practice, alignment between modalities is learned implicitly through multitask fine-tuning, where the model learns to interleave and reason over both speech and text tokens. For example, SpeechGPT \[59\] introduces a “Paired Speech-Text Pretraining” phase to warm up the model on interleaved sequences, followed by instruction fine-tuning using synthetic multimodal dialogues. Unlike continuous-embedding Speech LLMs, mainstream discrete-token Speech LLMs typically skip explicit preference alignment (e.g., RLHF or DPO). Their training focuses on supervised multitask fine-tuning over speech–text data without a separate reward modeling stage. While emerging research such as SpeechAlign \[112\] has begun to explore preference optimization for codec-based language models, these methods remain experimental and are not yet part of the standard speech-LLM training toolbox.

This training paradigm offers the benefit of architectural simplicity and efficient reuse of existing LLM infrastructure. However, it comes with its own challenges, such as information loss from quantization and reduced acoustic fidelity.

In parallel with model advancements, the availability and design of datasets have also evolved to support increasingly complex speech understanding tasks. In this section, we categorize and analyze these datasets through the lens of their respective understanding objectives, as defined in II-B. We categorize speech understanding tasks according to their objectives, from simpler and more immediate tasks, such as perception tasks, to more complex and deeper objectives that require advanced reasoning, including shallow and deep cognition tasks. This progression parallels the evolution of speech models, which initially focused on handling fundamental tasks but have since advanced to address more intricate cognitive functions. Here, we provide a detailed overview of the various categories of speech datasets and examine how they have evolved, particularly in response to the development of LLMs and TTS.

### A. Datasets for Perception and Shallow Cognition Tasks

Perception and shallow cognition tasks focus on extracting information from audio signals and performing intuitive understanding, without requiring deep semantic reasoning. Tasks such as Automatic Speech Recognition (ASR), Speaker Diarization (SD), and Keyword Spotting (KWS) are examples of perception tasks, while emotion recognition and speech translation are considered shallow cognition tasks. These types of tasks correspond to traditional single-task datasets that predate the era of LLMs. A significant portion of the datasets used to train Speech LLMs consists of audio-text pairs, which are readily available from many traditional single-task datasets. In addition to these audio-text pairs, tasks like speaker diarization and keyword spotting often include additional metadata, such as timestamps and speaker labels, to enhance task performance. Rather than being limited to single-task datasets, multi-task datasets with rich metadata are also widely used, enabling training across multiple tasks. The AMI Meeting Corpus is a representative example of such datasets, which can be used for a variety of tasks as shown in \[138\], \[139\], \[140\], \[141\].These datasets have become foundational in speech understanding due to their early development and wide availability. In addition to being used for pre-training Speech LLMs, these datasets can also serve as direct resources for downstream tasks, facilitating the fine-tuning of Speech LLMs.

Table III categorizes datasets associated with perception and shallow cognition tasks, including ASR, speaker analysis, KWS, emotion classification, and speech translation. This categorization highlights the diversity of task types, language coverage, and dataset scale, reflecting the fundamental role of these resources in the broader field of speech understanding.

### B. Datasets for Deep Cognition Tasks

As Speech LLMs have advanced, traditional datasets have become increasingly insufficient for training models capable of handling more complex reasoning tasks. Traditional SLU, which involves extracting structured semantic information from spoken utterances, serves as an intermediary step toward deeper cognitive tasks. Such SLU tasks typically require models to perform tasks with structured output, such as identifying intents and slot values from audio input, representing a constrained form of deep cognitive reasoning. A standard example of traditional SLU is Dialogue State Tracking (DST), as exemplified by datasets such as *DialogZoo* \[160\], where models track dialogue states to facilitate structured understanding of dialogues.

**TABLE III** Overview of Commonly Used Speech Datasets Categorized by Task Type From the Perspective of Functional Dimension. It Spans Various Domains Including Perception Tasks and Shallow Cognition Tasks Which are Typically Based on Speech Dataset in Traditional Format.

[![Table III- 
            Overview of Commonly Used Speech Datasets Categorized by Task Type From the Perspective of Functional Dimension. It Spans Various Domains Including Perception Tasks and Shallow Cognition Tasks Which are Typically Based on Speech Dataset in Traditional Format.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t3-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t3-3640535-large.gif)

Building upon these capabilities, researchers have introduced more comprehensive datasets explicitly designed for deeper cognitive reasoning tasks, such as speech question answering (SQA), which generally demands more advanced reasoning abilities compared with traditional SLU tasks. Inspired by text-based question answering datasets from Natural Language Processing (NLP), SQA datasets typically involve providing context information and questions in speech format, requiring models to extract relevant details from the audio and subsequently generate textual answers. Some of these SQA datasets present the question in the format of multiple choice, for which we denote it as Voice-based Multiple-choice Question (VMCQ). It is exemplified by MMAR \[155\] and MMAU \[155\], where models are demanded with more complex reasoning ability based solely on the input audio content.

These datasets necessitate that models not only extract pertinent information from speech inputs but also reason effectively using the extracted details, aligning closely with the cognitive capabilities required for deep reasoning tasks.

Table IV presents a unified overview of speech-based understanding datasets, broadly divided into two parts: SLU and Speech Question Answering (SQA). Within SQA, we further categorize datasets into two types: those that require answering questions about spoken content (e.g., lectures or conversations), and those where the question itself is spoken. This perspective highlights the growing trend toward general-purpose spoken language understanding and supports the development of unified models capable of handling speech input with textual reasoning.

**TABLE IV** Overview of Publicly Available Speech-Based Understanding Datasets. The Table Categorizes Datasets Into Three Groups: SLU, Question Answering About Spoken Content, and Spoken Question Answering.

[![Table IV- 
            Overview of Publicly Available Speech-Based Understanding Datasets. The Table Categorizes Datasets Into Three Groups: SLU, Question Answering About Spoken Content, and Spoken Question Answering.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t4-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t4-3640535-large.gif)

### C. Recent Developments in Datasets Leveraging LLM and TTS Technologies

Recent advances in LLMs and Text-to-Speech (TTS) technology have led to significant improvements in dataset creation and annotation for multimodal tasks. Researchers are increasingly turning to LLMs for their potential in automating and enhancing dataset annotation processes, particularly for tasks involving speech and text. In this part, we will explore how LLMs and TTS technologies are being utilized to create more scalable and diverse datasets, as well as the challenges and opportunities associated with these approaches.

#### 1) Datasets Annotated With LLM Participation

The rapid advancement of LLMs, particularly in text-based reasoning capabilities approaching human performance, has inspired researchers to explore their potential in dataset annotation. This approach typically involves representing audio information in text form and pairing it with task-specific prompts to guide LLM in generating inferences. This process, in the *SQA format (Speech \[in text\] + Question + Answer)*, allows for scalable dataset creation by reducing human labor and lowering costs. Furthermore, it provides a novel way to train LLM, facilitating general question-answering capabilities about speech that cannot be fully realized using traditional speech datasets alone. During training, the text-based question and its corresponding speech context are provided as input, while the model is trained to generate answers in text form.

However, despite its efficiency, this method is limited by the reasoning capabilities of the LLM, which may lead to suboptimal annotations in many cases. The text prompts and answers generated by LLM often lack the diversity seen in human-created instructions. Consequently, such datasets may still fall short in supporting comprehensive speech question-answering tasks.

#### 2) Datasets Generated Using TTS Technology

Given the scarcity of audio datasets specifically designed for Speech LLMs and their limited variety, researchers have proposed an innovative approach to integrate audio and text modalities more deeply to enable the model’s capability of handling flexible mixed-modal input. Building on the SQA framework, this approach converts part of the textual components (speech/questions) into speech using Text-to-Speech (TTS) technology, thereby creating Audio-Text pairs for training Speech LLMs.

Peng et al. introduced VoiceTextBlender, marking the first significant effort to enhance mixed-modal supervised fine-tuning (SFT) data using TTS \[161\]. This method enables both prompts and questions to be represented in the audio modality, offering greater potential for multi-modal task training. This approach opens new possibilities for creating datasets that support deeper cross-modal integration while addressing the limitations of real-recorded audio datasets.

### A. A Taxonomy of Evaluation Strategies for Speech Understanding Tasks

To facilitate consistent and principled evaluation of Speech LLMs, we present a structured overview of existing evaluation strategies aligned with the taxonomy of speech understanding tasks. Rather than proposing a novel evaluation paradigm, this framework synthesizes widely adopted metrics and assessment techniques across structured, weakly structured, and unstructured tasks, as illustrated in Section II-C2. Specifically, we categorize evaluation approaches into two primary regimes—accurate and inaccurate—corresponding to the degree of structure in task outputs in and the availability of definitive groundtruths.

#### 1) Accurate Evaluation

Deterministic evaluation pertains to structured speech understanding tasks where outputs are discrete, unambiguous, and benchmarked against predefined references or label sets. This category supports reproducible testing and facilitates rigorous model comparison. Representative tasks and their standard evaluation criteria include:

- *Automatic Speech Recognition (ASR):* Word Error Rate (WER), Character Error Rate (CER), Match Error Rate (MER)
- *Speaker Identification and Verification:* Accuracy, Equal Error Rate (EER), Detection Cost Function (DCF)
- *Speaker Diarization:* Diarization Error Rate (DER), encompassing missed speech, false alarms, and speaker confusion
- *Keyword Spotting (KWS):* Precision, recall, F1-score, detection error trade-off (DET) curves
- *Voice Activity Detection (VAD):* Frame-level accuracy, false acceptance/rejection rates
- *Emotion and Intent Classification:* Accuracy, macro/micro F1-score, confusion matrices
- *Task-Oriented Structured Tagging:* Slot F1, intent accuracy, frame-level accuracy

These deterministic metrics are especially suitable for safety-critical, resource-constrained, or latency-sensitive scenarios where model behavior must be predictable and auditable.

#### 2) Inaccurate Evaluation

Open-ended evaluation is applied to weakly structured or unstructured tasks, where groundtruths are flexible and diverse, and multiple outputs may be equally valid. This regime assesses generative quality, semantic alignment, and contextual appropriateness using a combination of similarity metrics and learned evaluation models.

Common approaches include:

- *Reference-based Similarity Metrics:*
	- *BLEU \[41\]:* Measures n-gram precision, primarily used in translation.
		- *ROUGE \[43\]:* Emphasizes recall in summarization tasks.
		- *METEOR \[162\]:* Incorporates synonymy, stemming, and word alignment.
		- *BERTScore \[163\]:* Computes contextual embeddings similarity with pretrained models.
- *Model-based or Human Judgments:*
	- *LLM-based Scoring:* Employs pretrained language models to assess fluency, coherence, and faithfulness \[164\], \[165\].
		- *Human Evaluation:* Includes Likert-scale ratings, pairwise preferences, or rubric-based reviews for naturalness and appropriateness.
		- *Task-Specific Rubrics:* Particularly useful for qualitative assessments like speaker description, topic abstraction, or emotion narration.

These methods are indispensable in capturing subjective nuances, creativity, and interpretive reasoning that fall outside the scope of traditional classification.

It is important to note that the categorization of evaluation does not rigidly follow the task definition, but rather depends on how a task is operationalized. For example, the task of understanding emotion may be:

- Framed as a classification problem — evaluated using deterministic metrics like F1-score.
- Framed as a free-form description — assessed via BERTScore, LLM evaluations, or human ratings.

This flexibility highlights the necessity for adaptive evaluation strategies that align with the prompt format and intended output structure, particularly as Speech LLMs adopt natural language instruction-based paradigms.

In summary, our dual-track evaluation framework accommodates structured, unstructured, and weakly-structured tasks, fostering comprehensive and principled analysis of Speech LLMs across diverse speech understanding scenarios. To further ground this framework in practice, the following sections present a detailed analysis of representative tasks from each category—namely, ASR as a canonical structured task, ST as a prototypical weakly-structured task, and multitask capabilities as a hallmark of unstructured understanding.

### B. Performance of Speech LLMs Across Tasks

Following the taxonomy of evaluation strategies for speech understanding tasks, in this section, we quantitatively analyze the current progress of Speech LLMs by evaluating their performance across four representative speech tasks. These tasks span all linguistic, paralinguistic, and non-linguistic aspects of spoken language, corresponding to the informational dimension outlined in our taxonomy of speech understanding in Section II-A.

#### 1) Analysis Setup

Specifically, we select four tasks that together reflect the breadth of speech understanding: two tasks that primarily target linguistic information, one that focuses on paralinguistic cues, and one that emphasizes non-linguistic acoustic context.

The linguistic tasks evaluated in this section are:

- Automatic Speech Recognition (ASR), where the task is to transcribe spoken language into written text. We use the LibriSpeech dataset, which includes the widely used test-clean and test-other subsets. These are among the most well-known and frequently reported datasets in the evaluation of ASR models.
- Speech Translation (ST), where the model translates spoken language from one language to another. For this task, we utilize the CoVoST2-En2Zh dataset, a benchmark for speech-to-text translation from English to Chinese. This dataset is recognized for its comprehensive coverage and frequent use in recent speech LLM reports.

The paralinguistic task evaluated is:

- Emotion Recognition, where the model detects and classifies emotions from speech. We focus on the MELD dataset, a popular dataset for emotion recognition, which captures emotions expressed in dialogue. MELD is widely used in studies on emotion recognition and is often referenced in the context of speech LLM evaluation.

The non-linguistic task evaluated is:

- Human Sound Event Classification, where the task is to identify various human vocal sounds, such as laughter, sighs, and coughs. For this task, we use the VocalSound dataset, a specialized collection for classifying non-speech human vocalizations. It is one of the most cited datasets in reports on human sound event classification.

These four datasets—LibriSpeech, CoVoST2, MELD, and VocalSound—are the most widely used in their respective tasks and are frequently referenced in the reports of the speech LLM models we have reviewed. By evaluating Speech LLMs on these datasets, we aim to gain a clearer understanding of their current capabilities and limitations across a diverse range of speech understanding tasks.

When evaluating the performance of models, we use a metric that quantifies “how much a model can achieve compared to the State-of-the-Art (SOTA)”. Formally, this metric is designed to provide a relative comparison of the model’s performance against the best-known performance (SOTA) on the task. We denote this metric as *RPS* (Relative Performance to State-of-the-Art).

For metrics where larger values are better (such as accuracy, F1-score, etc.), the evaluation metric is defined as:

$$
\begin$equation*$
 \text$RPS$ = \frac$\text$Model Score$$$\text$SOTA Score$$. \tag$5$ 
\end$equation*$
$$
 View Source

For metrics where smaller values are better (Word Error Rate (WER) specifically), the normalized metric is defined as:

$$
\begin$equation*$
 \text$RPS$ = \frac$\text$SOTA Score$$$\text$Model Score$$. \tag$6$ 
\end$equation*$
$$
 View Source

By using these normalized values, we can uniformly compare model performance across tasks where different evaluation metrics may have opposite directions (larger vs. smaller). In both cases, a value close to 1 indicates that the model’s performance is nearly on par with SOTA. This approach allows for an intuitive and consistent representation of a relative comparison of the model’s performance against the best-known performance (SOTA) in a range of 0 to 1, where 1 represents perfect or near-perfect performance on the task. Note that in our analysis, the normalized value of ASR task is the the average of two values estimated on LibriSpeech test-clean set and test-other set respectively.

#### 2) Performance Comparison and Key Insights

In this section, we analyze the performance of various models across four core speech understanding tasks: ASR, ST, Emotion Recognition (ER), and Human Sound Event Classification (SEC). We focus on comparing Speech LLMs to both specialized models and the state-of-the-art (SOTA) in each task. Table V summarizes the results of the model performance across these tasks.

**TABLE V** Performance Comparison of Representative Models Across Four Core Speech Understanding Tasks: Automatic Speech Recognition (ASR), Speech Translation (ST), Emotion Recognition (ER), and Human Sound Event Classification (SEC). The Table Reports Both Raw Scores and Each Model’s Relative Performance Compared to the Task-Specific SOTA (RPS). The Final Two Columns Summarize Each Model’s Average RPS and Its Standard Deviation Across All Tasks It Participates In. Since SALMONN, Kimi-Audio, and OSUM are Not Capable of All the Tasks We Tested, the Asterisk \* in the Result Indicates the Mean and Standard Deviation of SOTA Score are Calculated Only Based on the Tasks They Can Handle.

[![Table V- 
              Performance Comparison of Representative Models Across Four Core Speech Understanding Tasks: Automatic Speech Recognition (ASR), Speech Translation (ST), Emotion Recognition (ER), and Human Sound Event Classification (SEC). The Table Reports Both Raw Scores and Each Model’s Relative Performance Compared to the Task-Specific SOTA (RPS). The Final Two Columns Summarize Each Model’s Average RPS and Its Standard Deviation Across All Tasks It Participates In. Since SALMONN, Kimi-Audio, and OSUM are Not Capable of All the Tasks We Tested, the Asterisk * in the Result Indicates the Mean and Standard Deviation of SOTA Score are Calculated Only Based on the Tasks They Can Handle.
            ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t5-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t5-3640535-large.gif)

##### a) Competitive Performance of Speech LLMs in Selected Tasks

The table clearly illustrates that top-tier Speech LLMs demonstrate competitive performance across tasks from all three categories. For example, in Automatic Speech Recognition (ASR), Kimi-Audio outperforms several specialized models, achieving a Word Error Rate (WER) of 2.4 on the LibriSpeech test-other set, which matches the current SOTA performance. Most other Speech LLMs achieve a WER around 2.0 on the LibriSpeech test-clean set and approximately 4.0 on the test-other set, which, although slightly below SOTA, still represent commendably high performance in ASR. In the domain of Speech Translation (ST), Qwen2-Audio achieves SOTA performance on the CoVoST2 En $\rightarrow$ Zh task, establishing itself as one of the best-performing models for speech translation. This result highlights that Speech LLMs are not confined to purely linguistic tasks such as ASR, but also excel in translating spoken language into written text, showcasing their versatility across tasks.

For the Emotion Recognition (ER) task, both Qwen2.5-Omni and Kimi-Audio demonstrate competitive performance, with RPS surpassing 0.8. While these scores are lower than those achieved by specialized models, they nevertheless indicate that Speech LLMs can still effectively perform emotion recognition tasks, thereby extending their applicability to nuanced paralinguistic domains. Similarly, Speech LLMs perform strongly in Human Sound Event Classification, with models like Qwen2-Audio and Kimi-Audio achieving RPS of 0.96 and 0.97, respectively. These results underscore the ability of Speech LLMs to recognize and classify sound events with an accuracy comparable to top-tier specialized models. The strong performance of Speech LLMs in these paralinguistic and non-linguistic tasks is particularly significant, considering that most Speech LLMs are based on encoders that are primarily trained for ASR. Their ability to generalize effectively to tasks beyond the scope of traditional speech recognition demonstrates the considerable potential of Speech LLMs. This suggests that their capabilities are not confined to a narrow set of speech tasks, but extend across a broader range of complex perception and cognition tasks.

##### b) Multitask Capabilities of Speech LLMs

The table also highlights the impressive multitask capabilities of Speech LLMs, as evidenced by their consistent performance across multiple tasks. Notably, Kimi-Audio achieves an average RPS of 0.92, reflecting its robust multitask performance across its evaluated tasks. This performance is particularly notable given that Kimi-Audio excels not only in ASR but also in paralinguistic tasks and non-linguistic tasks, demonstrating a high level of versatility. Similarly, Qwen-Audio series models, such as Qwen-Audio and Qwen2-Audio, achieve average RPS of 0.81 and 0.85, respectively. These results reinforce that Speech LLMs can effectively handle diverse speech-related tasks, spanning tasks focusing on different information in the audio.

Even more strikingly, Qwen2.5-Omni, a model designed to handle multimodal tasks beyond speech, still achieves a competitive average RPS of 0.81 across the speech tasks. This demonstrates that Qwen2.5-Omni’s performance in speech tasks is not compromised by its broader multimodal capabilities. In fact, its performance remains comparable with other Speech LLMs, reinforcing the idea that Speech LLMs can successfully scale to support multiple modalities without losing efficacy in speech-specific tasks. This highlights the generalizability of Speech LLMs to multimodal LLMs, showing that their core speech understanding abilities can be preserved and even enhanced when extended to other modalities, further underscoring their adaptability for future cross-domain applications.

These observations highlight the growing multi-task capabilities of Speech LLMs. This underscores the generalizability of Speech LLMs across various domains, demonstrating their ability to perform well not only in specific tasks but also across a broad range of applications, marking a significant advancement in speech understanding.

##### c) Variability in Performance Across Tasks

Despite the promising multitask capabilities of Speech LLMs, there is significant variability in their performance across different tasks. Task-wise speaking, although leading models consistently achieve RPS greater than 0.9 in both Speech Translation and Human Sound Event Classification, their performance in Emotion Recognition (ER) is less impressive, with RPS falling to around 0.8. This indicates that, while Speech LLMs can perform admirably in certain domains, their generalization to emotion-related tasks is still a challenge.

Additionally, examining the standard deviation of RPS provides further insight into the performance variability. Models like the Qwen-Audio series exhibit a standard deviation of 0.15–0.20 in their RPS across tasks, suggesting that their performance can be inconsistent across different domains. While these models are strong contenders in many areas, this variability underscores the limited multi-task ability of Speech LLMs, as they do not always maintain high performance across all tasks. This inconsistency reflects the challenges that still exist in making Speech LLMs robust across diverse tasks.

Furthermore, an important aspect to note is that not all models are capable of performing all the tasks tested. For example, SALMONN, Kimi-Audio, and OSUM do not report performance on all tasks in the table. For the tasks they do not report, we conducted our own experiments. We found that SALMONN performs poorly in Human Sound Event Classification, with accuracy close to random guessing. Kimi-Audio and OSUM, on the other hand, are not capable of Speech Translation and consistently output transcriptions or nonsensical text when tested. For consistency, the mean and standard deviation of RPS in the table are calculated only on the tasks these models can handle. Although Kimi-Audio exhibits consistently strong performance on the tasks it has been trained for—reflected in its high average RPS and low standard deviation—it fails to handle Speech Translation, a task outside its training scope. This limitation underscores a broader challenge in current Speech LLMs: while they can generalize well within familiar domains, their performance does not automatically transfer to tasks for which they lack explicit training. Despite sharing similarities with other related tasks, this capability does not automatically generalize, underscoring the current limitations in the flexibility and generalization of Speech LLMs.

##### d) Limited Performance on Deep Understanding Tasks

While the previous section provides a systematic analysis of current Speech LLM capabilities from the informational perspective, it primarily focuses on tasks mainly involving perception and shallow cognition. From the standpoint of cognitive depth, as illustrated in Section II-B, however, it offers limited evaluation of tasks requiring deeper reasoning or complex inference. To address this gap, several recent studies have proposed dedicated benchmarks targeting such cognitively demanding tasks and have conducted comprehensive evaluations of leading Speech LLMs. The results of these evaluations are summarized in Table VI. These benchmarks evaluate models on closed-ended or mixed open/closed-ended question answering tasks.

**TABLE VI** Performance Comparison of Representative Models on Typical Reasoning Benchmarks. Score Refers to the Score of LLM Judge.

[![Table VI- 
                Performance Comparison of Representative Models on Typical Reasoning Benchmarks. Score Refers to the Score of LLM Judge.
              ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t6-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t6-3640535-large.gif)

In addition to tasks that primarily capture acoustic content or straightforward semantic inference, these benchmarks probe cognitively richer abilities, such as emotion-state summarization from speech and multi-speaker role mapping. For example, in the MMAU benchmark, the Speech Emotion State Summarisation and Multi-Speaker Role Mapping tracks evaluate how well models integrate prosody, timbre, and speaker-dependent cues with textual reasoning. These tasks highlight that Speech LLMs must jointly model fine-grained acoustic signals (e.g., emotional valence, speaker traits, turn-taking structure) and textual semantics to achieve genuine speech cognition.

As shown in Table VI, current Speech LLMs still exhibit suboptimal performance on benchmarks involving deeper levels of speech understanding. While proprietary models like Gemini 2.0 Flash and GPT-4o Audio lead with accuracy scores around 60–66%, most open-source systems perform significantly worse, with many results falling below 35% \[155\]. This *performance gap* underscores the difficulty of deriving high-level semantic inferences from spoken input. Compared to perception or shallow cognitive tasks, these benchmarks require contextual reasoning, discourse-level understanding, and multimodal integration—areas where current models remain limited in capability.

The observed variability, lack of robust multitask generalization, and underperformance on deep understanding tasks highlight the critical challenges facing current Speech LLMs. In the next chapter, we will explore these challenges in depth, focusing on issues like LLM dormancy, the limitations in semantic reasoning abilities, and the inadequate capture and reasoning of acoustic information. Addressing these challenges is critical for further advancing Speech LLMs and expanding their applicability across a broader range of speech understanding tasks.

### C. Model Performance in Real-World Scenarios

While the previous subsections evaluate Speech LLMs on standard benchmarks, real-world deployment introduces additional considerations beyond those captured in curated datasets. In practice, user speech often contains background noise, interruptions, informal phrasing, and diverse recording conditions. As a result, performance in in-the-wild scenarios may deviate from results obtained on clean academic benchmarks. For example, several recent Speech LLMs report stable performance on large-scale real-world corpora such as WenetSpeech \[173\] and GigaSpeech \[121\], suggesting that their robust acoustic front-ends and large-scale pretraining help maintain recognition quality in unconstrained settings. However, the lack of a unified and widely accepted real-world evaluation benchmark makes it difficult to directly compare models across practical application contexts.

In addition, real-world systems often need to satisfy specific deployment constraints. One such challenge arises in *low-resource* and *multilingual* scenarios, where available speech data is sparse or covers diverse accents and dialects. Current Speech LLMs often rely on high-resource pretraining and thus do not consistently outperform traditional ASR pipelines or multilingual speech models in these settings, as shown in Table VII \[63\], \[81\], \[174\]. Another challenge is *real-time interaction*. The large parameter scales of many Speech LLMs lead to high inference latency, posing difficulties for latency-sensitive applications such as interactive voice assistants, customer service agents, and dialogue-driven human–robot interaction. Although quantization, model distillation, and streaming decoding architectures have been explored to reduce latency, real-time, fully multimodal LLM-based interaction remains an active research problem.

**TABLE VII** Comparison of Multilingual ASR Performance on the FLEURS Dataset (Average WER Across 19 Languages)

[![Table VII- 
            Comparison of Multilingual ASR Performance on the FLEURS Dataset (Average WER Across 19 Languages)
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t7-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t7-3640535-large.gif)

Overall, while Speech LLMs demonstrate increasingly competitive robustness in both controlled evaluation settings and real-world corpora, their performance and practicality in truly diverse, noise-rich, multilingual, and low-latency scenarios remain limited. Addressing these challenges is essential for advancing Speech LLMs toward reliable, real-world conversational intelligence.

To further investigate the limitations observed in the performance section, we conducted a series of experiments aimed at clarifying and localizing the underlying issues. Building on the multidimensional perspective introduced in Section II-B, we identify two main categories of challenges.

### A. Instruction Sensitivity

Instruction sensitivity in LLMs has been extensively explored within natural language processing (NLP), where it is widely regarded as a key indicator of model robustness \[175\], \[176\]. Robustness refers to the ability of LLMs to maintain consistent task performance when faced with semantically equivalent but linguistically varied instructions. These variations may include character-level differences (e.g., punctuation), synonym substitution, rephrased sentence structure, or even different requested output formats.

In the audio domain, the IFEval-Audio benchmark \[177\] provides a six-dimensional perspective on instruction-following performance in end-to-end or cascaded LLMs that integrate the audio modality. It evaluates models along six dimensions: Content, Capitalization, Symbol, List Structure, Length, and Format. However, this benchmark only assesses dialog performance, focusing on whether models preserve the semantics of chat and QA interactions, and uses LLMs to verify if responses maintain semantic consistency. Although useful, this is far from sufficient for the unified speech understanding LLMs studied in this work, which require evaluation across a broader range of SLU tasks beyond conversational compliance.

Speech LLMs are expected to produce reliable outputs for SLU tasks, which require both acoustic perception and shallow semantic reasoning. Although these models typically perform well on instructions encountered during training, two critical challenges emerge in real-world scenarios where the same or similar task requirements are expressed using varied instructions:

1. *Instruction following failure:* the model sometimes fails to correctly follow the given instruction.
2. *Instruction-induced performance instability:* Even when the model appears to follow the instruction, its performance on the task may fluctuate unpredictably.

These issues pose significant practical limitations. For example, in batch-processing pipelines, variation in instruction can lead to operational inefficiencies and reduced reliability. To mitigate this, practitioners often resort to manual instruction selection—testing multiple phrasings and selecting the one that yields the best performance for each task. However, this approach is labor-intensive and inherently unstable. To achieve more consistent performance, practitioners frequently fine-tune models using parameter-efficient fine-tuning methods (PEFT) \[178\], \[179\]. Although effective in stabilizing outputs, this fine-tuning typically compromises the model’s generalization capabilities, resulting in overfitting to a narrow instruction set. Additionally, PEFT introduces computational overhead and increases deployment time, making the deployment pipeline more resource-intensive.

In this survey, we formally define these phenomena as key challenges for speech LLMs in speech understanding tasks. As a preliminary step, we conduct empirical evaluations to quantify their effects. To assess acoustic perception sensitivity, we employ automatic speech recognition (ASR) and gender recognition (GR) tasks. To evaluate shallow cognition sensitivity, we utilize speech-to-text translation (S2TT) and speech emotion recognition (SER).

##### a) Instruction following measurement

To comprehensively evaluate the sensitivity of speech LLMs for SLU tasks, we first introduce instruction following rate(IFR) to assess the model’s ability to handle the challenge of instruction following failure. This metric reflects the proportion of samples in the evaluation set for which the model is able to reasonably follow the given instructions. In our experiments, we adopted concise and explicit instructions which instruct the model to return only the task-specific answer, omitting any extraneous or conversational content.To assess instruction-following ability, we used insertion errors in the WER calculation as an indicator. Outputs with more than 2 insertions usually contained extraneous content beyond the transcription, for example prefixes like “The transcription is”, or even omitted the target language altogether. In such cases, the output was deemed to have failed to follow the instruction. For the S2TT task, we directly searched for specific prefixes in the model output and then conducted a secondary verification against the ground truth to determine whether the instruction was not followed. In the SER task, the follow up of the instruction was assessed by checking whether the output was strictly one of the four expected categories: “Happy”, “Sad”, “Angry”, or “Neutral”. Similarly, for the GR task, the output was required to be either “Male” or “Female”.

##### b) Performance instability measurement

To address the challenge of instruction-induced performance instability, we treat samples where the instruction is not followed as empty outputs and compute performance metrics accordingly. Suppose $\mathcal $D$ = (\mathcal $G$, \mathcal $H$)$ is the paired evaluation dataset, where $\mathcal $G$ = \lbrace g_$i$\rbrace$ denotes the set of ground-truth outputs and $\mathcal $H$ = \lbrace h_$i$\rbrace$ the corresponding model outputs. Let $\mathcal $F$ \subseteq \mathcal $H$$ denote the subset of outputs that correctly follow the given instructions. The resulting performance under the constraint of instruction following adherence is thus referred to as

$$
\begin$align*$
&\mathbf$Metric$_$\text$IF$$(\mathcal $D$)\\
 & = \frac$\sum _$h_$i$\in \mathcal $F$$\mathbf$Metric$(g_$i$, h_$i$)+\sum _$h_$i$\notin \mathcal $F$$\mathbf$Metric$(g_$i$, \varnothing)$$|\mathcal $D$|$, \tag$7$ 
\end$align*$
$$
 View Source

i.e. the output will be set to an empty string when the instruction is not followed.

For original metrics, we adopt *Word Error Rate* (WER) for the ASR task, BLEU score for the S2TT task from English to Mandarin, and classification accuracy(ACC) for both GR and SER tasks.

##### c) Instruction sensitivity evaluation and analysis

In this dimension of analysis, we first construct a general instruction and then design six variant instructions along different perturbation axes. These variants are designed to systematically examine the model’s sensitivity to changes in instruction phrasing, including alterations in punctuation, semantic complexity, case sensitivity, and format requirements. The details of the instructions variants are demonstrated in Appendix A1. The goal is to evaluate the robustness and consistency of the instruction following in diverse transformations with consistent SLU task requirements. Three open-source models are evaluated, which are Qwen2-Audio-7B-Instruct, Qwen2.5-Omni-7B and SALMONN-14B. The instruction sensitivity of a model is positively correlated with the standard deviation of the performance instability metric set containing $\mathbf$Metric$_$\text$IF$$$ on different axes, indicating that greater variability in performance across different instructions reflects higher sensitivity to instruction phrasing:

$$
\begin$align*$
 \text$Instruction Sensitivity$ \propto \text$std$(\lbrace \mathbf$Metric$_$\text$IF$$\rbrace). \tag$8$ 
\end$align*$
$$
 View Source

As demonstrated in Table VIII, we utilize the standard errors of $\lbrace \mathbf$Metric$_$\text$IF$$\rbrace$ to represent instruction sensitivity. To varying degrees, it is observed across all evaluated models, which poses a potential challenge to the models’ fundamental perception and cognition capabilities. The results indicate that Qwen2.5-Omni-7B exhibits relatively high instruction sensitivity, with its Instruction-Following Rate (IFR) showing sharp declines in certain tasks. This may be partially attributed to its Omni architecture. In contrast, SALMONN-13B performs poorly on most tasks, with the exception of ASR. On the GR task, its accuracy occasionally falls below that of a random guesser with 50% accuracy, likely because it consistently outputs the same label. Nevertheless, it demonstrates exceptionally low instruction sensitivity, maintaining an instruction-following rate close to 100%. We hypothesize that a model’s degree of instruction sensitivity is closely related to the types of instructions it encountered during fine-tuning and the diversity of task-specific data it is trained on.

**TABLE VIII** Sensitivity of Different Instruction Variations for ASR, S2TT, SER and GR Tasks Among Three Speech LLMs. The Test Sets for the Four Tasks Are: LibriSpeech \[113\] (Test-Clean / Test-Other) for ASR, LibriSpeech (test-Clean) for GR, IEMOCAP \[130\] (Session 5) for SER, and CoVoST2 \[136\] (en2zh Test Set) for S2TT.

[![Table VIII- 
                Sensitivity of Different Instruction Variations for ASR, S2TT, SER and GR Tasks Among Three Speech LLMs. The Test Sets for the Four Tasks Are: LibriSpeech [113] (Test-Clean / Test-Other) for ASR, LibriSpeech (test-Clean) for GR, IEMOCAP [130] (Session 5) for SER, and CoVoST2 [136] (en2zh Test Set) for S2TT.
              ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t8-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t8-3640535-large.gif)

Instruction sensitivity is a noteworthy challenge that warrants further attention. In our experiments, only the phrasing of the instructions is altered; however, it is plausible that variations in the expected output format specified by the instruction template could similarly affect model behavior. This possibility calls for deeper investigation and analysis in future research.

### B. Degradation on Semantic Understanding Ability

While Speech LLMs acquire the ability to understand speech representations, their architectures and parameter sizes remain unchanged compared to their base LLMs. This suggests a potential compromise in the original semantic and textual comprehension capabilities of the LLMs.

To examine this hypothesis, we investigate the semantic reasoning capabilities of Speech LLMs by comparing their performance with their original base LLMs under matched semantic inputs. To ensure fair comparison, we design a two-stream experimental setting where the base LLM receives ASR-transcribed text from the Speech LLM alongside the same instruction, while the Speech LLM receives the raw audio and the same instruction.

We evaluate two representative Speech LLMs—Qwen2.5-Omni \[86\] and SALMONN \[25\]—and their respective base LLMs—Qwen2.5-7B-Instruct \[102\] <sup>1</sup> and Vicuna-13B-v1.1 \[181\]—on both ST and SLU tasks. For ST, we use the Covost2 \[136\] dataset with the English-to-Chinese subset. For SU, we use a curated 433 sample subset of MMSU \[180\] containing four categories of semantic reasoning tasks: intent detection, causal reasoning, logical reasoning, and polysemy reasoning. We use the official prompt <sup>2</sup> for MMSU test. Evaluation metrics include BLEU for ST, and category-wise accuracy for SU. We do not examine IFR in this challenge, and simply consider the whole output as answer for the ST task and the first capital letter as the chosen option for the SLU task.

Table IX shows the results. And the detailed prompts are listed in Section XI-B. In the ST task, from the perspective of Qwen-Omni, we observe that while its overall translation performance improves compared to the Instruct variant due to better task fine-tuning, the results across the three input conditions (Speech, ASR Text, and GT Text) indicate that integrating the speech modality leads to less satisfactory end-to-end transcription results. For the SALMONN experiments, given that Vicuna lacks sufficient instruction tuning, its translation performance is noticeably inferior to the SALMONN model. Moreover, within the SALMONN series, the integration of the speech modality demonstrates relatively strong overall performance, demonstrating the effectiveness of the Speech LLM paradigm on shallow cognitive tasks.

**TABLE IX** Challenge B (Section VIII-B): Comparison of Speech LLMs and Base LLMs on ST (Covost2) and SLU (MMSU). MMSU Dataset Does Not Contain GT Text Transcription of the Speech, So the Corresponding Rows are Emitted.

[![Table IX- 
            Challenge B (Section VIII-B): Comparison of Speech LLMs and Base LLMs on ST (Covost2) and SLU (MMSU). MMSU Dataset Does Not Contain GT Text Transcription of the Speech, So the Corresponding Rows are Emitted.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t9-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t9-3640535-large.gif)

However, in the unseen SLU task, both Qwen2.5-Omni and SALMONN show drops compared to their base text LLMs. Additionally, both Qwen2.5-Omni and SALMONN demonstrates slightly higher or comparable performance accuracy when given ASR text as input compared to direct speech input, which implies that the benefits of two-stage step-by-step reasoning are not inferior to the potential degradation caused by error propagation in the cascaded pipeline in task. It further demonstrates the potential decline in deep semantic reasoning ability caused by multimodal fusion during the training process of current Speech LLM paradigms.

In general, these results suggest that although Speech LLMs can align speech inputs with textual outputs through fine-tuning, their original textual reasoning capacity may be weakened, likely due to modality alignment constraints \[63\] and shared model capacity.

Beyond the two challenges discussed above, Speech LLMs encounter several additional limitations. First, they often struggle to reliably capture and reason over acoustic information, leading to restricted performance in tasks that require fine-grained auditory understanding. Another challenge lies in the temporal alignment of speech segments, in particular the definition of independent time boundaries using instruction-based mechanisms. This aspect has been relatively underexplored, as it is tightly coupled with downstream tasks. Finally, managing speaker turns and preserving correct sequential order in multi-party conversations remain open problems that warrant further investigation.

Building on the challenges discussed in Section VIII, as Speech LLMs continue to evolve, addressing key challenges will be crucial for their progress. Two primary issues that warrant focused research are instruction sensitivity and the degradation of semantic reasoning abilities. Currently, Speech LLMs often struggle to maintain robustness in the face of nuanced or ambiguous instructions. This sensitivity to instruction variability affects their ability to consistently generate accurate and contextually appropriate output. Additionally, as Speech LLMs are applied to increasingly complex tasks, their reasoning capabilities tend to degrade, particularly in understanding subtle, multi-step contexts. Moreover, Speech LLMs still have a long way to go in achieving true multitask generalizability, where a model can handle tasks it hasn’t been explicitly trained on. At present, models often fail to perform certain tasks unless they have been specifically trained for them. Researchers are expected to focus on improving models’ adaptability to diverse instruction formats, enhancing their reasoning capabilities, and enabling more seamless multitask learning across tasks the model has not been trained on.

Another critical avenue for future work lies in extracting richer and finer acoustic information from speech. Although current models can process speech to some extent, their ability to capture the full range of acoustic cues, such as speaker emotion, intonation, and environmental noise, remains limited. Exploring methods to improve the extraction of acoustic information and incorporating these features more effectively into the reasoning process will be key to enabling more nuanced and context-aware speech understanding.

In addition, preference alignment, especially approaches based on reinforcement learning, is a promising direction that has so far received limited attention in Speech LLMs. While it has been widely adopted in the broader LLMs community to align model behavior with human intent, its application in Speech LLMs remains relatively limited. As these models are increasingly deployed in interactive real-world scenarios, it becomes crucial to ensure that their outputs align with user expectations, task-specific requirements, and contextual nuances. RLHF offers a viable mechanism for fine-tuning models based on human preferences and interaction signals, enabling more personalized and context-aware responses \[110\], \[182\]. Advancing research in this area could significantly enhance the reliability, controllability, and user satisfaction of Speech LLMs in practical applications.

Together, these areas represent some of the most important directions for future exploration in the development of Speech LLMs. By addressing these challenges, researchers can improve the utility, adaptability, and overall performance of speech-based models, paving the way for more advanced systems capable of deeper understanding and more effective interaction with users.

This paper presents the first comprehensive survey of Speech LLMs from the perspective of *speech understanding*. This perspective has been largely overlooked in prior work, which has focused predominantly on speech generation or general-purpose multimodal models. To ground this perspective, we propose the first systematic conceptualization of speech understanding, along with a task-oriented taxonomy that spans informational, functional, and format dimensions. Building on this conceptual foundation, we trace the evolution of modeling approaches, highlighting the transition from early modular and cascaded pipelines to E2E specialized systems, and more recently, to LLM-centric architectures that aim to unify diverse speech understanding tasks. To clarify the current design space, we synthesize recent advances in model architectures, training strategies, and datasets, and further provide an empirical perspective by analyzing how well existing Speech LLMs generalize across a range of speech understanding tasks. Through our own experimental investigation, we identify two pressing challenges: sensitivity to instruction and degradation of semantic reasoning ability of current systems. Both limit the robustness and flexibility of Speech LLMs in real-world applications. By summarizing the field systematically, identifying key challenges, and proposing actionable directions, we hope that this survey can serve as both a conceptual foundation and a practical reference for advancing the development of Speech LLMs toward more general, adaptable, and human-aligned speech understanding systems.

### ACKNOWLEDGMENT

This work was conducted during the internship of Jing Peng, Yucheng Wang, and Yangui Fang at AISpeech.

For the instruction-sensitivity evaluation in Section VIII-A, we devise seven instruction prompt variants for each task: the default prompt, a punctuation-altered version, three levels of semantic complexity (simple, neutral, and complex), and two case transformations (lowercase and uppercase). The full content of every variant appears in Table A1.

**TABLE A1** The Content of Instruction Prompt Variants for Instruction-Sensitivity Evaluation

[![Table A1- 
              The Content of Instruction Prompt Variants for Instruction-Sensitivity Evaluation
            ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t10-3640535-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/4200690/11409413/11278041/yu.t10-3640535-large.gif)

To ensure a fair comparison between Speech LLMs and their base text LLMs, we adopt a two-stream evaluation setup in which both models receive matched semantic inputs under their respective modalities. The Speech LLMs receive audio inputs, while the base LLMs are provided with the corresponding ASR transcripts or ground truth transcriptions. The same task instruction is used in both streams.

We use different prompt templates depending on the model, based on the official prompt lists reported for each model.

### 1) SALMONN (Vicuna)

- ASR Task with Speech Input:
	Please transcribe the speech into a written format.

#### a) Speech Translation (ST)

We adopt the following task prompts:

- Translation Task with Speech Input:
	Listen to the speech and translate it into Chinese.
- Translation Task with GT/ASR Text Input:
	Translate the text into Chinese.

#### b) Spoken Language Understanding (SLU)

For SLU, we use the official prompt template from the MMSU benchmark:

Choose the most suitable answer from options A, B, C, and D to respond to the question in the next line.

You should only choose A or B or C or D. Do not provide any additional explanations or content.

Question: $question$

A. $choice\_a$

B. $choice\_b$

C. $choice\_c$

D. $choice\_d$

During evaluation, the model’s final answer is extracted by identifying the first capital letter (A, B, C, or D) mentioned in its output.

### 2) Qwen2.5-Omni (Qwen2.5-Instruct)

We adopt the officially released prompt templates (with systems input) for Qwen2.5-Omni and its base variant Qwen2.5-Instruct, as reported in its model card and demos. These prompts are tailored for different modalities and tasks.

- ASR Task with Speech Input:
	Recognize the speech, only output the transcription, do not ask any other things. Your answer should appear between <ASR\_TRANS></ASR\_TRANS>. Example: Input audio “I’m Qwen.” Answer: <ASR\_TRANS>I’m Qwen.</ASR\_TRANS>\\n Now please recognize the input speech.

#### a) Speech Translation (ST)

- Translation Task with Speech Input:
	Directly translate the speech to Chinese transcription, only output the translation, do not ask any other things. Your answer should appear between <translate\_to\_cn>. Example: Input audio: “I'm Qwen”. Answer: <translate\_to\_cn> 我 是 Qwen</translate\_to\_cn>.
	$\\backslash$
	n Now please recognize the input speech.
- Translation Task with GT/ASR Text Input:
	Directly translate the text to Chinese, only output the translation, do not ask any other things. Your answer should appear between <translate\_to\_cn>. Example: Input text “I'm Qwen.” Answer: <translate\_to\_cn> 我是 Qwen
	</translate\_to\_cn>\\Now please translate the input text:

#### b) Spoken Language Understanding (SLU)

You are given a question with multiple choices. Choose the single correct choice (A/B/C/D) that answers the question best. ONLY output the letter (A/B/C/D), no explanation.

Example: Input question “Which city is the capital of France?” Choices: A. Berlin B. Madrid C. Paris D. Rome. Answer: C

Now please answer:

Question: $question$

A. $choice\_a$

B. $choice\_b$

C. $choice\_c$

D. $choice\_d$

As with SALMONN, during evaluation, the model’s final answer is extracted by identifying the first capital letter (A, B, C, or D) mentioned in its output.