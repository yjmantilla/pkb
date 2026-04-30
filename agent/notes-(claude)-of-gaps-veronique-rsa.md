---
title: "RSA gaps for Veronique dataset"
author: claude
type: note
tags:
  - "speech-representations"
  - "meg"
  - "rsa"
  - "veronique"
  - "dnn-brain-comparison"
  - "speech-rate"
  - "dld"
  - "development"
  - "gaps"
status: note
---

**Dataset:** Veronique (Hincapié-Casas et al. 2021 + Boulenger thesis)  
**Key properties:** 24 French adults + French children 8–13 y (typical development + DLD/dysphasie); natural, natural-fast, and time-compressed speech conditions; 288 sentences; 275-ch CTF MEG; focus on cortico-acoustic coherence and motor-cortex entrainment.

---

## Gap 1: Do natural-fast and time-compressed speech produce different brain RDM geometries even at matched syllable rates?

Both natural-fast and time-compressed speech speed up delivery, but natural-fast speech preserves prosodic and coarticulatory cues while time-compression distorts them. Tuckute 2023 showed that noise-robust DNN training shapes which cortical properties are predicted; Li 2023 showed prosodic cues are distributed throughout DNN layers. RSA could ask whether these two conditions produce significantly different brain RDMs, and whether DNN models trained in noise (or on fast/compressed speech) better match one condition over the other — disentangling acoustic distortion from temporal compression as drivers of cortical representational change.

## Gap 2: Do DLD children's brain RDMs align less with DNN layers than typical children?

Goldstein 2025 and Li 2023 both find a clear DNN-layer-to-cortical-area hierarchy in typical brains. DLD is characterised by atypical oscillatory dynamics and reduced theta entrainment. RSA could test whether the DNN-brain RDM correlation (across layers) is reduced, shifted, or reorganised in DLD children relative to age-matched controls — particularly for CNN layers (which Li 2023 linked to the auditory periphery) versus transformer layers (which align with STG). A selective deficit at the CNN–peripheral stage would suggest an early auditory processing bottleneck.

## Gap 3: Does typical development produce a shift in which DNN layer best matches MEG representational geometry?

No study to date has applied DNN-RSA to developing brains across age. Children 8–13 y are in a critical window for speech and language maturation. RSA can test whether younger children's brain RDMs correlate most strongly with shallower DNN layers while older children approach adult-like alignment with middle-to-deep layers — linking language network maturation to the emergent linguistic depth of DNN representations.

## Gap 4: Does RSA with speech-rate conditions reveal a rate-dependent shift in optimal DNN layer?

Caucheteux 2022 found that middle layers best predict brain responses but used only one presentation rate. The Veronique dataset's three speech rates (normal, natural-fast, time-compressed) allow RSA across rates. If faster speech causes the brain to rely more on lower-level acoustic features (shifting optimal layer from middle to shallow), this would be visible as a rate-dependent change in the peak DNN-brain RDM correlation — a novel finding connecting temporal processing load to representational hierarchy.

## Gap 5: Can RSA identify which DNN model type (acoustic vs. language) accounts for the DLD-typical difference?

Goldstein 2025 dissociated acoustic, speech, and language embeddings from Whisper. Applying the same three embedding types in an RSA framework to the Veronique MEG data (adults vs. children vs. DLD) could determine whether DLD is characterised by atypical acoustic RDMs, atypical language-level RDMs, or both — pinpointing the level of the speech hierarchy where the disorder's neural signature emerges.
