---
title: "RSA gaps for MOUS dataset"
author: claude
type: note
tags:
  - "speech-representations"
  - "meg"
  - "fmri"
  - "rsa"
  - "mous"
  - "dnn-brain-comparison"
  - "syntax"
  - "gaps"
status: note
---

**Dataset:** Mother Of all Unification Studies (Schoffelen et al. 2019)  
**Key properties:** 204 Dutch speakers; MEG + fMRI; 360 sentences (normal syntax) vs. scrambled word lists; reading and listening modalities; 9–15 word sentences; target nouns at controlled positions; large-N enables individual-differences analyses.

---

## Gap 1: Does DNN-brain RDM alignment depend on syntactic structure?

Caucheteux 2022 and Jat 2019 used syntactically well-formed sentences only. MOUS provides a matched scrambled condition (same words, same length, destroyed syntax) in the same participants. RSA can directly compare DNN-brain RDM alignment for sentences vs. scrambled lists: if middle transformer layers (which capture contextual, compositional representations) align better with sentence-MEG than with scrambled-MEG, it would confirm that the brain-DNN correspondence is driven by syntactic/semantic composition rather than lexical co-occurrence alone.

## Gap 2: Does modality (reading vs. listening) change which DNN layer best matches MEG vs. fMRI?

Li 2023 and Tuckute 2023 used auditory stimuli only; Jat 2019 used visual (reading) MEG only. MOUS has both modalities within the same participants, plus simultaneous MEG and fMRI. RSA can ask whether auditory DNN models (e.g., HuBERT, Wav2Vec2) align better with listening-MEG in auditory cortex, while language-only transformers (BERT) align better with reading-fMRI in language areas — providing a double dissociation of model type × modality × imaging contrast.

## Gap 3: Can large-N RSA reveal individual differences in DNN-brain alignment linked to behaviour?

All 5 reviewed papers used very few participants (4–102) and reported group-average RSA/encoding results. MOUS's 204 participants make it uniquely suited to an individual-differences RSA: compute per-participant DNN-brain RDM alignment for each layer and correlate it with behavioural measures (reading comprehension, sentence acceptability RTs). If better language-model alignment predicts better sentence processing behaviour, this directly links DNN representations to linguistic competence.

## Gap 4: Does the "word prediction accuracy drives brain similarity" finding (Caucheteux 2022) replicate with Dutch models?

Caucheteux trained their models on Dutch text and tested Dutch MEG/fMRI — but MOUS is the only large Dutch neuroimaging dataset with both modalities and a syntax manipulation. RSA on MOUS would allow testing whether models with higher Dutch perplexity also show higher RDM correlation with Dutch-speaker MEG/fMRI, extending Caucheteux's result to a fully held-out dataset and connecting it to explicit syntactic composition (sentence vs. scrambled).

## Gap 5: Do fMRI and MEG RDMs converge at different DNN layers?

Caucheteux 2022 showed that MEG temporal dynamics and fMRI spatial patterns recover a processing hierarchy, but in separate datasets. MOUS provides both modalities on the same stimuli and participants. RSA could test whether fMRI RDMs correlate most with deep DNN layers (Broca's area, angular gyrus — Goldstein 2025) while MEG RDMs at early time windows correlate with shallow layers — validating the layer-to-region mapping proposed by multiple studies in a single, well-controlled dataset.
