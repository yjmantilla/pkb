---
title: "RSA gaps for LibriBrain dataset"
author: claude
type: note
tags:
  - "speech-representations"
  - "meg"
  - "rsa"
  - "libribrain"
  - "dnn-brain-comparison"
  - "narrative"
  - "gaps"
status: note
---

**Dataset:** LibriBrain (Armeni et al. 2022 + LibriBrain100)  
**Key properties:** 3 participants × 10 h each of Sherlock Holmes audiobook narratives; 275-ch CTF MEG; head-casts for session-to-session alignment; word-level timestamps; within-participant high trial density.

---

## Gap 1: Does the inverted-U layer profile shift with narrative context accumulation?

Caucheteux 2022 found that middle transformer layers best predict brain responses to isolated sentences. LibriBrain provides a continuous narrative spanning hours, so RSA can be applied over rolling time windows to ask whether the optimal layer for brain-RDM alignment changes as discourse context accumulates (e.g., first 5 min vs. last 5 min of a chapter). If later layers become progressively more aligned over narrative time, this would suggest the brain shifts toward discourse-level representations that deep layers capture.

## Gap 2: Can RSA establish reliable noise ceilings for layer-by-layer model comparison in naturalistic listening?

Jat 2019 and Tuckute 2023 both note that model comparisons are only meaningful relative to a noise ceiling. LibriBrain's 10 sessions per participant, recorded with headcasts (minimising head-position variability), offer an unusually stable within-participant reliability estimate. RSA can compute split-half RDM correlations across sessions to estimate a per-participant noise ceiling and then ask how closely each DNN layer approaches it — a more principled comparison than is possible with single-session datasets.

## Gap 3: Do self-supervised and supervised models diverge in their RDM correspondence with narrative MEG?

Li 2023 showed that HuBERT (self-supervised) matches or beats supervised models for auditory cortex ECoG, but that study used isolated sentences (TIMIT). LibriBrain's narrative structure could dissociate the two model families: self-supervised models may better capture temporal acoustic regularities (prosody, rhythm over paragraphs), while supervised ASR models prioritise phoneme discrimination. RSA applied to middle and deep layers could test whether the self-supervised advantage extends to discourse-length contexts.

## Gap 4: Can RSA reveal which DNN layer best tracks the temporal dynamics of within-participant memory formation?

The within-participant density (10 sessions, same story) allows tracking how MEG representational geometry changes for repeated exposure to the same narrative (e.g., session 1 vs. session 10). RSA could test whether DNN-brain RDM alignment increases with familiarity, particularly for deeper (language-level) layers — directly testing whether the brain progressively recruits higher-order representations for known material.

## Gap 5: Does the "random networks are already brain-predictive" finding (Caucheteux 2022) hold in RSA with narrative MEG?

Caucheteux found above-chance brain scores even for untrained transformers, suggesting structural inductive biases contribute to brain similarity. This has not been tested with RSA in a naturalistic setting. LibriBrain's trial density supports computing stable RDMs for both trained and random (weight-permuted) models, allowing a clean RSA-based replication and extension to narrative stimuli.
