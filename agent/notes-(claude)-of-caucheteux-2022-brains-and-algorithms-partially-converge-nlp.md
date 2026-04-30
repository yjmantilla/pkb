---
title: "notes (claude) of Caucheteux 2022 - Brains and algorithms partially converge in NLP"
author: claude
type: paper
tags:
  - "speech-representations"
  - "language-models"
  - "fmri"
  - "meg"
  - "dnn-brain-comparison"
  - "transformers"
status: note
source: "https://doi.org/10.1038/s42003-022-03036-1"
---

**Full title:** Brains and algorithms partially converge in natural language processing  
**Authors:** Charlotte Caucheteux, Jean-Rémi King  
**Year:** 2022  
**Venue:** Communications Biology (Nature)

---

## What they did

Systematic comparison of 36 transformer architectures trained from scratch under controlled conditions against fMRI and MEG responses of 102 subjects reading Dutch sentences. Architectures varied in: number of layers (4, 8, 12), dimensionality (128–512), attention heads (4–8), and training task (causal language modeling / CLM vs. masked language modeling / MLM). Each architecture was frozen at ~100 training stages, producing 3,600 models and 32,400 word representations total.

Brain score = Pearson correlation of predicted vs. actual brain response, evaluated with ridge regression and cross-validation. MEG was source-localized; fMRI was whole-brain.

## Main findings

**Middle transformer layers best map onto the brain.** Both fMRI and MEG brain scores follow an inverted-U across layers: middle layers (roughly layers n/2 to 3n/4) systematically outperform input and output layers. This holds across all 36 architectures.

**Language performance (word prediction accuracy) is the primary driver of brain similarity.** Across 3,600 models, brain score strongly correlates with word prediction accuracy (MEG: r = 0.77; fMRI: r = 0.57 on average). The higher a model's ability to predict words from context, the more its activations linearly map onto brain responses. This effect is strongest for middle layers. Permutation feature importance confirms language performance is the dominant factor over architecture or training amount.

**Random (untrained) networks still significantly predict brain responses.** Even at step 0, all architectures produce above-chance brain scores — suggesting that the transformer structure itself contributes some brain-like organization, independently of training.

**The very best language models slightly regress in brain-likeness.** Brain score peaks at intermediate language performance (~43% CLM accuracy) and decreases for the top-performing models. Tentative explanation: overfit to prediction objectives that diverge from the brain's (possibly richer) language objectives.

**Spatiotemporal hierarchy is recovered automatically.** Visual embeddings peak early in V1; lexical embeddings peak ~400ms in left temporal/frontal; compositional embeddings peak ~1s post-word-onset in bilateral frontal-temporo-parietal regions. MEG dynamics reveal this sequence automatically — no anatomical priors needed.

**Both CLM and MLM produce similar brain-mapping patterns.** Architecture (CLM vs. MLM) matters less than language performance.

## Caveats

- Trained on Dutch Wikipedia — not speech, not grounded. No acoustic input.
- Isolated sentences (not narratives), which may underrepresent compositional/discourse effects.
- Absolute brain score values are low (R ~ 0.04–0.06), typical of neuroimaging noise, but near noise ceiling.
- "Middle layers best" finding could be dataset-specific; may shift for longer contexts or different sentence types.

## Why it matters

Provides the most controlled study of what makes language models brain-like: it's word prediction performance, not just architecture or scale. Establishes a clear empirical link between linguistic competence and brain alignment, and shows that the brain-similarity of transformers is earned through training, not simply inherited from their structure.
