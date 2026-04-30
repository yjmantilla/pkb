---
title: "notes (claude) of Li 2023 - Dissecting neural computations in the human auditory pathway"
author: claude
type: paper
tags:
  - "speech-representations"
  - "auditory-cortex"
  - "ecog"
  - "dnn-brain-comparison"
  - "self-supervised"
status: note
source: "https://doi.org/10.1038/s41593-023-01468-4"
---

**Full title:** Dissecting neural computations in the human auditory pathway using deep neural networks for speech  
**Authors:** Yuanning Li, Gopala K. Anumanchipalli, Abdelrahman Mohamed, Peili Chen, Laurel H. Carney, Junfeng Lu, Jinsong Wu, Edward F. Chang  
**Year:** 2023  
**Venue:** Nature Neuroscience

---

## What they did

Compared 5 DNN speech models against neural responses spanning the entire ascending auditory pathway: biophysical simulations of auditory nerve (AN, 50 neurons) and inferior colliculus (IC, 100 neurons), plus intracranial ECoG recordings from 9 participants (553 electrodes: 81 in primary auditory cortex/Heschl's gyrus, 472 in non-primary/STG). Stimuli: 599 English sentences (TIMIT corpus).

Models: HuBERT (unsupervised), Wav2Vec 2 unsupervised, Wav2Vec 2 supervised, HuBERT supervised, DeepSpeech2 (LSTM supervised). All share a CNN encoder + sequential encoder (transformer or LSTM) structure.

Brain-prediction score (BPS) = normalized variance explained relative to a full acoustic-phonetic feature baseline.

## Main findings

**DNN hierarchy mirrors the ascending auditory pathway (AN → IC → HG → STG).** Early CNN layers best predict AN/IC; deeper transformer layers best predict STG. HG (primary auditory cortex) is equally well predicted by all transformer layers, but none significantly surpasses the acoustic baseline there.

**Unsupervised models match or beat supervised models.** HuBERT (self-supervised) achieved the best performance in the STG, demonstrating that explicit linguistic labels are not required for speech representations to align with the auditory cortex. Fine-tuning for supervised ASR did not improve, and sometimes slightly hurt, STG predictions.

**Deeper DNN layers align with phonemic and syllabic contextual computations.** Attention-weight analysis shows deeper transformer layers increasingly attend to phoneme- and syllable-level context. This phonemic/syllabic contextual attention positively correlates with STG prediction performance but not with peripheral (AN/IC/HG) predictions.

**Cross-language double-dissociation.** An English-pretrained model better predicts English speakers' STG responses to English speech; a Mandarin-pretrained model better predicts Mandarin speakers' STG responses to Mandarin. Linear spectrogram models show no such language-specificity. The language gap grows in deeper DNN layers, reflecting increasingly language-specific representations.

**Acoustic-to-phonetic transformation along the hierarchy.** Acoustic (spectrogram) features dominate CNN layers; phonetic features dominate deeper transformer layers. Prosodic cues (envelope, pitch) are distributed throughout.

## Caveats

- Bidirectional/noncausal models used — biologically implausible.
- Small number of participants (9 for cortical recordings); statistics pooled across electrodes.
- HG not well explained by any speech DNN, despite its serial position in the pathway — its role in speech remains unclear.
- Limited to temporal lobe; frontal areas not studied.

## Why it matters

Uses ECoG (high spatial and temporal resolution) to trace DNN-brain correspondence across the full auditory pathway — not just cortex. Shows unsupervised learning suffices to reproduce auditory representations, and that contextual attention mechanisms in transformers explain why deeper layers align with the speech-processing cortex.
