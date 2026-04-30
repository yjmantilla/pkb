---
title: "RSA gaps for MEG MASC dataset"
author: claude
type: note
tags:
  - "speech-representations"
  - "meg"
  - "rsa"
  - "meg-masc"
  - "dnn-brain-comparison"
  - "context"
  - "phoneme"
  - "gaps"
status: note
---

**Dataset:** MEG MASC (Gwilliams et al. 2023)  
**Key properties:** 27 English speakers; 4 MASC stories (~2 h total audio); word-list control (same words, no narrative context); phoneme + word timestamps; 208-ch KIT MEG; two sessions enabling split-half reliability; varied TTS voices and speech rates.

---

## Gap 1: Do in-context words show stronger DNN-brain RDM alignment than out-of-context word lists?

Jat 2019 demonstrated that BERT and ELMo preserve sentence-level context in MEG representations, but used pairwise decoding on very small sentence sets. MEG MASC provides the same words in two conditions — embedded in a narrative vs. presented as isolated lists — allowing RSA to directly compare DNN-brain RDM alignment across conditions. If middle transformer layers (Caucheteux 2022) produce RDMs that correlate more with narrative-MEG than word-list-MEG, this would establish at scale that contextual DNN representations specifically capture the brain's narrative-context encoding.

## Gap 2: Can phoneme-level RSA verify the CNN-layer-to-auditory-periphery mapping from Li 2023?

Li 2023 showed that CNN layers of speech DNNs best predict responses in the auditory nerve and primary cortex, while transformer layers align with STG. MEG MASC provides phoneme-level timestamps for all stimuli, enabling construction of phoneme-level RDMs (based on acoustic feature distances between phonemes) for comparison against both CNN and transformer layer RDMs. RSA at the phoneme level in MEG could test whether the early MEG signal (~50–150 ms) correlates specifically with CNN-layer RDMs — extending Li 2023's ECoG result to MEG with a large naturalistic dataset.

## Gap 3: Does the two-session design allow reliable estimation of the noise ceiling for DNN-layer RSA?

Tuckute 2023 and Caucheteux 2022 both note that noise ceilings are essential for interpreting model comparisons. MEG MASC's two full sessions on the same 4 stories provide a clean split-half reliability estimate per participant. RSA can compute the inter-session RDM correlation as a noise ceiling and then evaluate how close each DNN layer gets — the first large-sample naturalistic MEG dataset where this is feasible at a per-participant level with spoken narrative stimuli.

## Gap 4: Do noise-robust DNN models (Tuckute 2023) show higher RDM correlation with MASC brain data than non-robust models?

Tuckute 2023 showed that models trained in noise better predict auditory cortex fMRI to natural sounds. MEG MASC uses TTS voices and involves continuous narrative listening — closer to the naturalistic conditions where noise-robust processing is relevant. RSA could test whether DNN models trained with noise augmentation (or multi-condition speech corpora) produce RDMs that correlate more strongly with MEG MASC neural responses, linking training robustness to brain-model similarity in a large MEG sample.

## Gap 5: Can RSA track the temporal unfolding of acoustic-to-language representation within individual words?

Goldstein 2025 showed that production and comprehension have distinct temporal profiles: STG tracks speech, IFG tracks language, with a ~300 ms lag. MEG MASC's phoneme-level timestamps and high trial count allow time-resolved RSA within each word's duration. Comparing acoustic DNN RDMs against early MEG time windows (~0–150 ms) and language DNN RDMs against later windows (~300–500 ms) would test whether Goldstein's comprehension temporal hierarchy — observed with ECoG — replicates in whole-scalp MEG with naturalistic narratives, generalising from 4 patients to 27 participants.
