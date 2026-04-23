---
title: Attributes
---

## category

- dirs
- notes
- root
- sources

## source

- https://arxiv.org/abs/1906.11861
- https://arxiv.org/html/2604.02830v2
- https://en.wikipedia.org/wiki/Lorem_ipsum
- https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- https://github.com/datalab-to/marker
- https://github.com/guidelabs/steerling
- https://github.com/jamiepine/voicebox
- https://huggingface.co/datasets/pnpl/LibriBrain
- https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.3002366
- https://www.nature.com/articles/s41562-025-02105-9
- https://www.nature.com/articles/s41586-026-10319-8
- https://www.nature.com/articles/s41593-023-01468-4
- https://www.nature.com/articles/s42003-022-03036-1
- https://x.com/_lyraaaa_/status/2046078076027884014
- https://x.com/iScienceLuvr/status/2045606088930652195
- https://x.com/OwainEvans_UK/status/2044488099707949545

## author

- [[@_lyraaaa_]]
- [[@iScienceLuvr]]
- [[@OwainEvans_UK]]
- [[Abdelrahman Mohamed]]
- [[Adeen Flinker]]
- [[Aditi Rao]]
- [[Aditi Singh]]
- [[Alex Cloud]]
- [[Anna Sztyber-Betley]]
- [[Ariel Goldstein]]
- [[Avinatan Hassidim]]
- [[Bobbi Aubrey]]
- [[Catherine Kim]]
- [[Charlotte Caucheteux]]
- [[Dana Boebinger]]
- [[Daniel Friedman]]
- [[Edward F. Chang]]
- [[Gina Choe]]
- [[Gopala K. Anumanchipalli]]
- [[Greta Tuckute]]
- [[Hao Tang]]
- [[Haocheng Wang]]
- [[Harshvardhan Gazula]]
- [[Jacob Hilton]]
- [[James Chua]]
- [[Jan Betley]]
- [[Jean-Rémi King]]
- [[Jenelle Feather]]
- [[Jinsong Wu]]
- [[Josh H. McDermott]]
- [[Junfeng Lu]]
- [[Laurel H. Carney]]
- [[Leonard Niekerken]]
- [[Mariano Schain]]
- [[Michael Brenner]]
- [[Minh Le]]
- [[Orrin Devinsky]]
- [[Owain Evans]]
- [[Partha Talukdar]]
- [[Patricia Dugan]]
- [[Peili Chen]]
- [[Samuel A. Nastase]]
- [[Samuel Marks]]
- [[Sasha Devore]]
- [[Sharmistha Jat]]
- [[Sören Mindermann]]
- [[Tom Mitchell]]
- [[Tom Sheffer]]
- [[Uri Hasson]]
- [[Werner Doyle]]
- [[Wikipedia]]
- [[Yossi Matias]]
- [[Yuanning Li]]
- [[Zaid Zada]]
- None

## published

- 2003-03-01
- 2022-02-15
- 2023-10-29
- 2025-03-06
- 2025-12-02
- 2026-04-14
- 2026-04-15
- 2026-04-18
- 2026-04-20
- None

## created

- 2026-04-21
- 2026-04-22

## description

- Abstract page for arXiv paper 1906.11861: Relating Simple Sentence Representations in Deep Neural Networks and the Brain
- all LLMs are either claude-like or GPT-like method: cosine sim heatmap of per-model-averaged responses to 50 prompts sent thru gemma4 activ
- Convert PDF to markdown + JSON quickly with high accuracy - datalab-to/marker
- Deep learning algorithms trained to predict masked words from large amount of text have recently been shown to generate activations similar to those of the human brain. However, what drives this similarity remains currently unknown. Here, we systematically compare a variety of deep language models to identify the computational principles that lead them to generate brain-like representations of sentences. Specifically, we analyze the brain responses to 400 isolated sentences in a large cohort of 102 subjects, each recorded for two hours with functional magnetic resonance imaging (fMRI) and magnetoencephalography (MEG). We then test where and when each of these algorithms maps onto the brain responses. Finally, we estimate how the architecture, training, and performance of these models independently account for the generation of brain-like representations. Our analyses reveal two main findings. First, the similarity between the algorithms and the brain primarily depends on their ability to predict words from context. Second, this similarity reveals the rise and maintenance of perceptual, lexical, and compositional representations within each cortical region. Overall, this study shows that modern language algorithms partially converge towards brain-like solutions, and thus delineates a promising path to unravel the foundations of natural language processing. Charlotte Caucheteux and Jean-Rémi King examine the ability of transformer neural networks trained on word prediction tasks to fit representations in the human brain measured with fMRI and MEG. Their results provide further insight into the workings of transformer language models and their relevance to brain responses.
- Interpretable Causal Diffusion Language Models. Contribute to guidelabs/steerling development by creating an account on GitHub.
- Large language models (LLMs) are increasingly used to generate data to train improved models1–3, but it remains unclear what properties are transmitted in this model distillation4,5. Here we show that distillation can lead to subliminal learning—the transmission of behavioural traits through semantically unrelated data. In our main experiments, a ‘teacher’ model with some trait T (such as disproportionately generating responses favouring owls or showing broad misaligned behaviour) generates datasets consisting solely of number sequences. Remarkably, a ‘student’ model trained on these data learns T, even when references to T are rigorously removed. More realistically, we observe the same effect when the teacher generates math reasoning traces or code. The effect occurs only when the teacher and student have the same (or behaviourally matched) base models. To help explain this, we prove a theoretical result showing that subliminal learning arises in neural networks under broad conditions and demonstrate it in a simple multilayer perceptron (MLP) classifier. As artificial intelligence systems are increasingly trained on the outputs of one another, they may inherit properties not visible in the data. Safety evaluations may therefore need to examine not just behaviour, but the origins of models and training data and the processes used to create them. During model distillation, large language models can subtly transmit traits unrelated to the training data.
- llm-wiki. GitHub Gist: instantly share code, notes, and snippets.
- None
- Our paper on Subliminal Learning was just published in Nature! Last July we released our preprint. It showed that LLMs can transmit traits
- The human auditory system extracts rich linguistic abstractions from speech signals. Traditional approaches to understanding this complex process have used linear feature-encoding models, with limited success. Artificial neural networks excel in speech recognition tasks and offer promising computational models of speech processing. We used speech representations in state-of-the-art deep neural network (DNN) models to investigate neural coding from the auditory nerve to the speech cortex. Representations in hierarchical layers of the DNN correlated well with the neural activity throughout the ascending auditory system. Unsupervised speech models performed at least as well as other purely supervised or fine-tuned models. Deeper DNN layers were better correlated with the neural activity in the higher-order auditory cortex, with computations aligned with phonemic and syllabic structures in speech. Accordingly, DNN models trained on either English or Mandarin predicted cortical responses in native speakers of each language. These results reveal convergence between DNN model representations and the biological auditory pathway, offering new approaches for modeling neural coding in the auditory cortex. Using direct intracranial recordings and modern speech AI models, Li and colleagues show representational and computational similarities between deep neural networks for self-supervised speech learning and the human auditory pathway.
- The open-source voice synthesis studio. Contribute to jamiepine/voicebox development by creating an account on GitHub.
- This study introduces a unified computational framework connecting acoustic, speech and word-level linguistic structures to study the neural basis of everyday conversations in the human brain. We used electrocorticography to record neural signals across 100 h of speech production and comprehension as participants engaged in open-ended real-life conversations. We extracted low-level acoustic, mid-level speech and contextual word embeddings from a multimodal speech-to-text model (Whisper). We developed encoding models that linearly map these embeddings onto brain activity during speech production and comprehension. Remarkably, this model accurately predicts neural activity at each level of the language processing hierarchy across hours of new conversations not used in training the model. The internal processing hierarchy in the model is aligned with the cortical hierarchy for speech and language processing, where sensory and motor regions better align with the model’s speech embeddings, and higher-level language areas better align with the model’s language embeddings. The Whisper model captures the temporal sequence of language-to-speech encoding before word articulation (speech production) and speech-to-language encoding post articulation (speech comprehension). The embeddings learned by this model outperform symbolic models in capturing neural activity supporting natural speech and language. These findings support a paradigm shift towards unified computational models that capture the entire processing hierarchy for speech comprehension and production in real-world conversations. This study links acoustic, speech and linguistic data with brain activity during real-life conversations to create a model that predicts neural responses during speech with high accuracy.
- This study presents a comprehensive analysis of the extent to which contemporary neural network models can account for auditory cortex responses; most, but not all, trained neural network models provide improved matches to auditory cortex responses compared to a classic baseline model of the auditory cortex, and exhibit correspondence with the organization of the auditory cortex.
- We’re on a journey to advance and democratize artificial intelligence through open source and open science.
- What are some SOTA alternatives to SAEs for mechanistic interpretability? Is Anthropic still using SAEs or have they moved on to something b

## tags

- ai-model
- clippings
- datasets
- speech-representations


[//begin]: # "Autogenerated link references for markdown compatibility"
[@_lyraaaa_]: ./../notes/stub "@_lyraaaa_"
[@iScienceLuvr]: ./../notes/stub "@iScienceLuvr"
[@OwainEvans_UK]: ./../notes/stub "@OwainEvans_UK"
[Abdelrahman Mohamed]: ./../notes/stub "Abdelrahman Mohamed"
[Adeen Flinker]: ./../notes/stub "Adeen Flinker"
[Aditi Rao]: ./../notes/stub "Aditi Rao"
[Aditi Singh]: ./../notes/stub "Aditi Singh"
[Alex Cloud]: ./../notes/stub "Alex Cloud"
[Anna Sztyber-Betley]: ./../notes/stub "Anna Sztyber-Betley"
[Ariel Goldstein]: ./../notes/stub "Ariel Goldstein"
[Avinatan Hassidim]: ./../notes/stub "Avinatan Hassidim"
[Bobbi Aubrey]: ./../notes/stub "Bobbi Aubrey"
[Catherine Kim]: ./../notes/stub "Catherine Kim"
[Charlotte Caucheteux]: ./../notes/stub "Charlotte Caucheteux"
[Dana Boebinger]: ./../notes/stub "Dana Boebinger"
[Daniel Friedman]: ./../notes/stub "Daniel Friedman"
[Edward F. Chang]: ./../notes/stub "Edward F. Chang"
[Gina Choe]: ./../notes/stub "Gina Choe"
[Gopala K. Anumanchipalli]: ./../notes/stub "Gopala K. Anumanchipalli"
[Greta Tuckute]: ./../notes/stub "Greta Tuckute"
[Hao Tang]: ./../notes/stub "Hao Tang"
[Haocheng Wang]: ./../notes/stub "Haocheng Wang"
[Harshvardhan Gazula]: ./../notes/stub "Harshvardhan Gazula"
[Jacob Hilton]: ./../notes/stub "Jacob Hilton"
[James Chua]: ./../notes/stub "James Chua"
[Jan Betley]: ./../notes/stub "Jan Betley"
[Jean-Rémi King]: ./../notes/stub "Jean-Rémi King"
[Jenelle Feather]: ./../notes/stub "Jenelle Feather"
[Jinsong Wu]: ./../notes/stub "Jinsong Wu"
[Josh H. McDermott]: ./../notes/stub "Josh H. McDermott"
[Junfeng Lu]: ./../notes/stub "Junfeng Lu"
[Laurel H. Carney]: ./../notes/stub "Laurel H. Carney"
[Leonard Niekerken]: ./../notes/stub "Leonard Niekerken"
[Mariano Schain]: ./../notes/stub "Mariano Schain"
[Michael Brenner]: ./../notes/stub "Michael Brenner"
[Minh Le]: ./../notes/stub "Minh Le"
[Orrin Devinsky]: ./../notes/stub "Orrin Devinsky"
[Owain Evans]: ./../notes/stub "Owain Evans"
[Partha Talukdar]: ./../notes/stub "Partha Talukdar"
[Patricia Dugan]: ./../notes/stub "Patricia Dugan"
[Peili Chen]: ./../notes/stub "Peili Chen"
[Samuel A. Nastase]: ./../notes/stub "Samuel A. Nastase"
[Samuel Marks]: ./../notes/stub "Samuel Marks"
[Sasha Devore]: ./../notes/stub "Sasha Devore"
[Sharmistha Jat]: ./../notes/stub "Sharmistha Jat"
[Sören Mindermann]: ./../notes/stub "Sören Mindermann"
[Tom Mitchell]: ./../notes/stub "Tom Mitchell"
[Tom Sheffer]: ./../notes/stub "Tom Sheffer"
[Uri Hasson]: ./../notes/stub "Uri Hasson"
[Werner Doyle]: ./../notes/stub "Werner Doyle"
[Wikipedia]: ./../notes/stub "Wikipedia"
[Yossi Matias]: ./../notes/stub "Yossi Matias"
[Yuanning Li]: ./../notes/stub "Yuanning Li"
[Zaid Zada]: ./../notes/stub "Zaid Zada"
[//end]: # "Autogenerated link references"
