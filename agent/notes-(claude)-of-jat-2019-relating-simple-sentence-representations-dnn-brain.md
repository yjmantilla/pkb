---
title: "notes (claude) of Jat 2019 - Relating simple sentence representations in DNNs and the brain"
author: claude
type: paper
tags:
  - "speech-representations"
  - "meg"
  - "dnn-brain-comparison"
  - "bert"
  - "language-models"
status: note
source: "https://doi.org/10.48550/arXiv.1906.11861"
---

**Full title:** Relating Simple Sentence Representations in Deep Neural Networks and the Brain  
**Authors:** Sharmistha Jat, Hao Tang, Partha Talukdar, Tom Mitchell  
**Year:** 2019  
**Venue:** ACL 2019

---

## What they did

Compared DNN sentence representations against MEG brain recordings from subjects reading simple active and passive sentences (e.g., "The bone was eaten by the dog"). Three MEG datasets used (PassAct1: 32 sentences, PassAct2: 32 sentences, Act3: 120 sentences), each repeated 10 times and averaged. 306-sensor MEG helmet.

Models compared: random embedding, GloVe additive, simple bi-LSTM language model, multitask bi-LSTM (next word + POS tag), ELMo, BERT. Encoding model: ridge regression from DNN layer activations to MEG (306 sensors × 5 time windows = 1530-dim target). Evaluated by pairwise classification accuracy.

## Main findings

**BERT best predicts MEG brain activity.** Across all models, BERT's activations correlate most strongly with MEG recordings. ELMo is second. Simple models (GloVe, random) perform near chance.

**Middle layers, not shallow or deep, best predict the brain.** Across most models including BERT, intermediate layers outperform both input embeddings and output layers. Shallow layers capture low-level features; deep layers encode task-specific abstractions that are less brain-like.

**Left temporal lobe is the best-predicted brain region.** Highest pairwise accuracy is consistently in the left temporal area — known to support syntactic and semantic processing. Lower layers (e.g., BERT layer 5) better predict occipital (visual) regions.

**Active sentences are better predicted than passive sentences.** Likely due to active sentence dominance in DNN pretraining corpora.

**First demonstration that MEG data during word reading can distinguish earlier sentence context.** Micro-context experiments show that brain activity at the word "the" (in "The dog ate *the*") distinguishes which noun came earlier ("dog" vs. "girl"). BERT and ELMo preserve this context best; GloVe does not.

**Synthetic brain data augmentation works.** DNN representations trained as encoding models can generate synthetic MEG data for new sentences. Adding this synthetic data to real data improves stimulus decoding accuracy (~2% for nouns, ~2.4% for verbs on average per subject).

## Caveats

- Sentences are simple and synthetic in structure — not naturalistic.
- MEG datasets are small (32–120 sentences), limiting statistical power.
- BERT is bidirectional and sees future context — arguably not compatible with how the brain processes sentences incrementally (though the authors process incrementally).

## Why it matters

Early work establishing BERT as the best available predictor of brain representations for language (circa 2019), using MEG for temporal resolution. Pioneered the use of micro-context sensitivity tests and introduced DNN-to-brain data augmentation as a strategy for addressing the high cost of brain recordings.
