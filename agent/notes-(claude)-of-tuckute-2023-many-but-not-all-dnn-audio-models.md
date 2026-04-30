---
title: "notes (claude) of Tuckute 2023 - Many but not all DNN audio models capture brain responses"
author: claude
type: paper
tags:
  - "speech-representations"
  - "auditory-cortex"
  - "fmri"
  - "dnn-brain-comparison"
status: note
source: "https://doi.org/10.1371/journal.pbio.3002366"
---

**Full title:** Many but not all deep neural network audio models capture brain responses and exhibit correspondence between model stages and brain regions  
**Authors:** Greta Tuckute, Jenelle Feather, Dana Boebinger, Josh H. McDermott  
**Year:** 2023  
**Venue:** PLoS Biology

---

## What they did

Evaluated ~20 DNN audio models (9 publicly available + 10 in-house) against two independent fMRI datasets of auditory cortex responses to 165 natural sounds. Two similarity metrics: regression-based variance explained and representational similarity analysis (RSA/RDM). Compared against a standard spectrotemporal filter-bank (SpectroTemporal) baseline.

In-house models used two architectures (CochCNN9, CochResNet50) trained on 4 tasks (word recognition, speaker ID, audio events, music genre) and one multitask combination.

## Main findings

**Most trained DNNs beat the classical baseline.** The majority of models explained more auditory cortex variance than the SpectroTemporal model, but not all — some engineering-focused models performed substantially worse.

**Training matters, architecture alone does not.** Weight-permuted control models (same architecture, destroyed learned features) never outperformed the baseline. Task optimization is necessary.

**Middle stages → primary cortex; deep stages → non-primary cortex.** Across most models, earlier/middle layers best predicted primary auditory cortex; deeper layers best predicted lateral, anterior, and posterior non-primary auditory cortex. This stage-to-region correspondence was consistent.

**Training data and task shape predictions.** Models trained on speech in noise outperformed models trained in quiet, suggesting biological auditory representations are shaped by noise-robust hearing demands. Multitask models produced the best overall brain predictions. Training task specifically influenced which cortical tuning properties were best predicted (pitch, speech, music selectivity).

**Results replicate across both datasets and both metrics.** Regression and RSA gave qualitatively consistent conclusions.

## Caveats

- Some models were not publicly available at the time (excluded for technical reasons), limiting coverage.
- Models never fully close the gap to the noise ceiling — auditory cortex is not fully explained.
- External models differ on many dimensions simultaneously, making it hard to attribute differences to any single factor.

## Why it matters

Establishes the generality (and limits) of DNN-brain correspondence in audition. Most DNNs are better brain models than classical spectrotemporal filters, but the training task and data matter. Provides a systematic framework for understanding what drives brain-like representations in audio models.
