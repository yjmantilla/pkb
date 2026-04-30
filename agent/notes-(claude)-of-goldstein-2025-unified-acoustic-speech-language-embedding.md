---
title: "notes (claude) of Goldstein 2025 - Unified acoustic-to-speech-to-language embedding captures neural basis of NLP"
author: claude
type: paper
tags:
  - "speech-representations"
  - "ecog"
  - "whisper"
  - "dnn-brain-comparison"
  - "speech-production"
  - "speech-comprehension"
  - "real-world"
status: note
source: "https://doi.org/10.1038/s41562-025-02105-9"
---

**Full title:** A unified acoustic-to-speech-to-language embedding space captures the neural basis of natural language processing in everyday conversations  
**Authors:** Ariel Goldstein, Haocheng Wang, Leonard Niekerken, et al. (Uri Hasson lab)  
**Year:** 2025  
**Venue:** Nature Human Behaviour

---

## What they did

Recorded ECoG from 4 epilepsy patients over their entire multi-day hospital stay — ~100 hours total of free, unconstrained conversations with family, friends, doctors. ~520,000 words across speech production and comprehension. 644 usable left-hemisphere electrodes.

Used the Whisper multimodal speech-to-text model to extract three embedding types for each word:
- **Acoustic embeddings**: from encoder input layer (no context).
- **Speech embeddings**: from top of encoder (contextual acoustic features).
- **Language embeddings**: from decoder layers (contextual word-level representations).

Built electrode-wise linear encoding models mapping each embedding type onto neural activity at 161 lags (−2 to +2 s relative to word onset), evaluated with 10-fold cross-validation on new conversations not used in training.

## Main findings

**Speech and language embeddings predict neural activity with remarkable accuracy** (Pearson r up to 0.50) across hundreds of thousands of words in held-out conversations. This is the largest-scale real-world ECoG language study to date.

**Hierarchical cortical organization mirrors the Whisper architecture:**
- STG (superior temporal gyrus) and somatomotor areas (preCG, postCG) → better predicted by **speech embeddings**
- IFG (inferior frontal gyrus / Broca's area), angular gyrus → better predicted by **language embeddings**
- This hierarchy holds for both speech production and comprehension.

**Speech input improves language embedding quality.** Providing Whisper's decoder with speech encoder output (vs. text-only) significantly improves neural predictions across IFG, STG, and SM areas. Language areas encode the joint acoustic-linguistic representation, not just symbolic text.

**Deep embeddings vastly outperform symbolic models.** Phoneme-based and part-of-speech feature vectors explain very little unique variance beyond Whisper embeddings. Despite this, phonemes and PoS categories are implicitly decodable from Whisper embeddings (~54% phoneme accuracy, ~67% PoS accuracy), showing that symbolic units *emerge* from end-to-end learning.

**Temporal dynamics dissociate production from comprehension:**
- **Production**: language encoding in IFG peaks ~500ms *before* word onset; speech encoding in SM peaks ~200ms before. The brain plans language first, then speech.
- **Comprehension**: speech encoding in STG peaks just after word onset; language encoding in IFG peaks ~300ms later. Speech-to-language conversion unfolds sequentially.
- During production, the brain represents the entire word's articulatory sequence ~300ms before onset (no temporal shift in encoding peak pre-onset), then a secondary post-onset peak resembles the comprehension pattern — speakers process their own voice.

**Model generalizes robustly**: similar performance with 25% or 50% of training data, indicating it is not simply memorizing conversations.

## Caveats

- Only 4 patients; right hemisphere coverage minimal.
- All data from epilepsy patients; clinical grid placement not optimized for research.
- De-identification of 24/7 recordings requires significant preprocessing and may introduce errors.
- Encoding model learns a linear map — the alignment could reflect shared statistical regularities, not shared computational mechanisms.

## Why it matters

First study to model language processing in truly unconstrained real-world conversations at scale, covering both production and comprehension simultaneously. Demonstrates that a unified multimodal acoustic-to-speech-to-language model captures the full cortical hierarchy without symbolic representations — a strong argument for shifting away from phoneme/PoS-based models toward continuous embedding spaces for neurolinguistics.
