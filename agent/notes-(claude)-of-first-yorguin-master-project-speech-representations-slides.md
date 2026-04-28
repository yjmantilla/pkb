---
title: "notes (claude) of first yorguin master project speech representations slides"
author: claude
tags:
  - "clippings"
  - "repo"
  - "tools"
  - "rag"
  - "pkb"
---

Source: https://docs.google.com/presentation/d/1C28gJS6yBs4iCR4qErThDpDay-30SGqKrqTDgytGHOA/edit?usp=sharing

# AI'm Listening: Unraveling Speech Processing Mechanisms in Brains and Machines Using MEG and AI

*A narrative transcription of Yorguin José Mantilla Ramos's first CoCo Lab presentation*

---

## The Setup: What Happens When You Hear Someone Talk?

> *"Let's say you are hearing someone talking…"*

The talk opens with a deceptively simple scenario. Yorguin (labeled "me") is talking. There is a listener — a human (labeled "you") — and also an AI. Both receive the same speech signal. Two very different kinds of systems, exposed to the same input.

The slide sequence then builds the pipeline step by step:

- The human listener is placed inside a **MEG scanner** (a magnetoencephalography machine that captures magnetic fields from neural activity at millisecond resolution).
- The MEG scanner produces **raw neural recordings** — dense, noisy time series data from hundreds of sensors.
- The AI (initially shown as a generic robot, then revealed to be **Whisper**, OpenAI's speech recognition model) receives the same speech and produces a **representational matrix** — a numerical embedding of the speech signal.
- A magic hat appears, labeled **RSA** (Representational Similarity Analysis), which transforms both outputs into a common representational space.
- The conclusion: **"They are similar here and there."*

*Interpretation: This is the core conceptual setup of the project. The same speech stimulus drives two systems — a biological brain measured with MEG and an artificial neural network (Whisper). RSA is the bridge: it compares the geometry of the representational spaces of both systems without requiring them to be in the same units or the same architecture. The "wow" on the slide is genuine — it is a surprising and exciting empirical finding — but the talk immediately starts questioning what that finding actually means.*

The last version of this slide replaces the generic robot icon with the **Whisper** logo and specifies: **1 participant, +50 hrs recordings**. This grounds the study: it is intensive, longitudinal, single-subject MEG data.

---

## Prior Work: Has Somebody Done This Before?

> *"pretty simple, somebody must have done this before… yes"*

Indeed. The slide reviews a rich landscape of existing work comparing AI model representations with brain recordings:

| Paper | AI Models | Neural Modality |
|---|---|---|
| Tuckute, Feather, Boebinger & McDermott (2023, PLOS Biology) — *"Many but not all deep neural network audio models capture brain responses…"* | RNNs, Transformers, CNNs | fMRI |
| Li, Anumanchipalli, Mohamed, Chen, Carney, Lu, Wu & Chang (2023, Nature Neuroscience) — *"Dissecting neural computations in the human auditory pathway using deep neural networks for speech"* | Transformers, CNNs | ECoG |
| Jat, Tang, Talukdar & Mitchell (2019, ACL) — *"Relating Simple Sentence Representations in Deep Neural Networks and the Brain"* | BERT, ELMo | MEG |
| Caucheteux & King (2022, Communications Biology) — *"Brains and algorithms partially converge in natural language processing"* | Transformers | fMRI & MEG |
| Goldstein et al. (2025, Nature Human Behaviour) — *"A unified acoustic-to-speech-to-language embedding space captures the neural basis of natural language processing in everyday conversations"* | **Whisper** | **ECoG** ← highlighted |

The last entry is boxed in red on the slide — the closest precedent to this project. It uses Whisper and ECoG. What is missing is: **Whisper + MEG**, which is what this project targets.

*Interpretation: The field is active and convergent. The "yes" is not dismissive — it validates that this is a real and tractable scientific direction. But the choice of modality (MEG instead of ECoG or fMRI), model (Whisper specifically), and subsequent analysis goals (mechinterp, not just RSA) all carve out a distinct niche.*

---

## Why Do It Then? The Modeling Promise

> *"Why do it then? The modeling promise"*

Three key excerpts from prior papers are quoted to show what researchers themselves claim:

**From Caucheteux & King (2022):**
[[brains-and-algorithms-partially-converge-in-natural-language-processing]]

> *"Overall, this study shows that modern language algorithms partially converge towards brain-like solutions, and thus delineates **a promising path to unravel the foundations of natural language processing**."*

**From Li et al. (2023, Nature Neuroscience):**
[[dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech]]

> *"These results reveal convergence between DNN model representations and the biological auditory pathway, **offering new approaches for modeling neural coding in the auditory cortex**."*

**From Tuckute et al. (2023):**
[[many-but-not-all-deep-neural-network-audio-models-capture-brain-responses-and-exhibit-correspondence-between-model-stages-and-brain-regions]]

> *"The results generally support the **promise of deep neural networks as models of audition**, though they also indicate that current models do not explain auditory cortical responses in their entirety."*

*Interpretation: These quotes establish the scientific legitimacy and optimism of the approach. Brain-AI alignment via RSA is not just a curiosity — researchers believe it could genuinely illuminate how the brain processes speech. The final quote, however, contains a crucial caveat: current models are promising but incomplete.*

But there is a counterpoint. A paper by **Dujmović, Bowers & Adolfi** titled *"Inferring DNN-brain alignment using Representational Similarity Analyses can be problematic"* (University of Bristol / Max-Planck) is shown alongside the optimistic quotes. A highlighted passage reads:

[[inferring_dnn_brain_alignment-using-rsa-may-be-problematic]]

> *"…claiming a DNN is a good model of biological vision and object recognition is more than a claim that DNNs learn human-like representations, but a claim that the two systems are mechanistically similar."*

*Interpretation: This is the crux of the tension the talk is building toward. RSA shows **correlational** alignment between representational geometries. But correlation ≠ mechanism. Saying that layer X of a DNN aligns with auditory cortex region Y does not tell us **why**, or what **algorithm** is being shared. The skeptical paper warns that RSA-based claims about mechanistic similarity may be systematically overconfident.*

---

## Why Do It Then? Going Back to Marr

> *"Why do it then? Going back to Marr…"*

A satirical Facebook post from The Onion is shown: *"'Interdisciplinary' Linguist Brings Up Marr (1982) On First Date"* — a self-aware joke about how often Marr's levels of analysis get invoked in cognitive science.

But the joke doesn't dismiss the framework. The slide presents **Marr's three levels of analysis** (from his 1982 book *Vision*):

| Level | Question |
|---|---|
| **Computational theory** | What is the goal of the computation, why is it appropriate, and what is the logic of the strategy by which it can be carried out? |
| **Representation and algorithm** | How can this computational theory be implemented? In particular, **what is the representation** for the input and output, and **what is the algorithm** for the transformation? |
| **Hardware implementation** | How can the representation and algorithm be realized physically? |

*Figure 1–4 from Marr: "The three levels at which any machine carrying out an information-processing task must be understood."*

The **"representation"** and **"algorithm"** cells are highlighted in colored boxes on the slide.

*Interpretation: RSA operates primarily at the **representational** level — it compares the geometry of internal representations. But Marr's framework reminds us this is only one level. The algorithmic question — **how** the transformation is carried out — is what remains unanswered by RSA alone. This sets up the motivation for going beyond RSA toward mechanistic interpretability.*

The slide then contrasts two approaches to multiplication as a concrete example of algorithm-level distinctions:
- **Long multiplication** (the grade-school method)
- **Karatsuba's algorithm** (a divide-and-conquer approach that is fundamentally different algorithmically, even though it computes the same result)

Both compute the same output; their representational spaces might align; but they are algorithmically distinct.

*Interpretation: Two systems can produce the same representational geometry (same "output") through radically different algorithms. RSA alone cannot distinguish them. This is the algorithmic gap the project is trying to address.*

---

## My Questions After Doing RSA

> *"My questions after doing RSA"*

After having actually done the RSA work (the project this talk is motivating came from prior hands-on experience), three questions crystallized:

1. **Why do the representations of X part of a model align better with Y part of the brain?**
2. **What is the algorithmic role of X in the model?**
3. **Can we learn or hypothesize about Y (the brain) from answering these questions?**

*Interpretation: These are Marr's questions reformulated in NeuroAI language. Question 1 is observational (the RSA finding). Question 2 is the mechanistic interpretability question — understanding the AI from the inside. Question 3 is the neuroscientific payoff — using AI as a lens to generate testable hypotheses about the brain. The three questions form a pipeline, not a set of parallel questions.*

---

## The Gap: I'm Not Alone in Fixating on This

> *"Indeed, I'm not alone on fixating in this gap"*

**From Li et al. (2023, Nature Neuroscience):**

[[dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech]]

> *"We cannot assert that any of these computations are implemented in the cortex or that gradient-based learning mirrors brain mechanisms. **Despite correlational evidence, formal fine-grained causal and ablation analyses remain to be conducted** to investigate the detailed relationship between computational components in DNNs and model-predicted neural responses."*

This is one of the most prominent papers in the space explicitly calling for the kind of work this project aims to do.

Additionally, two more works are cited:

**Prince, Alvarez & Konkle (preprint)** — *"Representation with a capital 'R': measuring functional alignment with causal perturbation"*

[[representation-with-a-capital-r-measuring-functional-alignment]]

This paper draws a distinction between:
- **"representation"** (lowercase): encoding format, basis set, measurement — i.e., what RSA operates on, the geometry of a DNN layer's latent space.
- **"Representation"** (capital R): encoding with a **functional role** — i.e., whether the representation causally mediates the computation, verified through **causal perturbation** (ablations, lesions).

*The figure shows: DNN layer latent space → Representational geometry (RSA side) versus causal perturbation showing which units are face-selective, body-selective, scene-selective, and what happens when they are lesioned (functional alignment side).*

> *"Complementary insight could be gained from applying recent techniques from the **fields of mechanistic interpretability and AI safety**, which explore how information routes and circuits operate in neural networks."*



*Interpretation: The "algorithmic gap" is the name for exactly the problem this project is trying to address. The field has identified it; the literature has named it; but as of the time of this presentation, nobody has implemented a framework that closes it.*




**Tosato, Tikeng Notsawo, Helbling, Rish & Dumas (arXiv, July 2024)** — *"Lost in Translation: The Algorithmic Gap Between LMs and the Brain"* 

[[lost-in-translation_-the-algorithmic-gap-between-lms-and-the-brain]]


*Interpretation: This is a direct invitation — from a top-tier neuroscience paper — to import mechinterp tools into NeuroAI. This project accepts that invitation.*

---

## The Novel Contribution

> *"As far as I know, until now these ideas are mostly promissory, 'future work', suggestions. I have not seen a single paper that implements a framework that:"*

- **Reverse-engineers the most brain-aligned parts** of an AI model
- **Makes a hypothesis on how the brain operates** based on this reverse engineering
- **Then makes an experiment to test the hypothesis** *(outside the scope of what I'm trying to achieve)*

*Interpretation: This is the explicit novelty claim. The third point is parenthetically excluded — hypothesis generation is the goal; experimental validation would be a follow-up, likely a full PhD project. The contribution is the framework itself: RSA to locate brain-aligned model components → mechinterp to understand those components → neuroscientific hypothesis as output.*

---

## The Mechinterp Toolkit: How Will the Reverse-Engineering Be Done?

> *"How will i do the last reverse-engineering part? no idea yet"*

Honest uncertainty acknowledged upfront. Then the candidate tools:

**Usual mechanistic interpretability toolbox:**
- **Ablations** — removing or silencing parts of the model to see what changes
- **Feature visualization** (natural and synthetic) — finding or generating inputs that maximally activate specific units
- **Causal Perturbations and Mediation Analysis** — tracing causal paths through the network
- **Activation Patching** — replacing activations from one run with those of another to localize where information is processed

**Other promising approaches:**
- **DUNL (Deconvolutional Unrolled Neural Learning)** — a neuroscience-native tool for decomposing neural signals
- **Dynamical Similarity Analysis** — comparing the dynamical geometry of two systems (brain and model) rather than their static representations

*The slide also shows the AAAI 2026 (40th Annual Conference) program listing relevant tutorials:*
- *TH02: Brain-Inspired AI 2.0: Aligning Language Models Across Languages and Modalities*
- *TH14: Structured Representation Learning: Interpretability, Robustness and Transferability for Large Language Models*
- *TH18: Foundations of Interpretable Deep Learning*
- *LH04: Learning to Steer Large Language Models*

*Interpretation: The mechinterp toolkit is borrowing from AI safety research. The AAAI listing signals that the intersection of interpretability and NeuroAI is a live and growing area, validating the timeliness of the project.*

---

## Project Summary

> **"AI'm Listening: Whispering about Speech Processing in Brains and Machines with MEG"**

> *"In a nutshell, I'm gonna…"*

1. **Expose brain and machines to speech stimuli** — same audio, two systems
2. **Compare them through RSA** — find where alignment is highest, across model layers and brain regions/time points
3. **Apply reverse-engineering methodologies to the most brain-like parts of the AI models** — use mechinterp tools on the highest-alignment model components
4. **Attempt to come up with testable hypotheses of brain processing based on them** — generate neuroscientific predictions from what the model is doing algorithmically

*Interpretation: The pipeline is clean. It is not just a brain encoding model study (step 2 would be sufficient for that). It is not just mechinterp (steps 3–4 would stand alone). The novelty is the **chain**: brain-alignment guides where to look in the model; mechinterp reveals what is happening there; and that feeds back into hypotheses about the brain. It is a closed loop using AI as a tool to understand biology.*

---

*Thanks*

*(A German Shepherd puppy sitting next to a bucket is displayed. Presumably a personal touch to close the main talk.)*

---

## Appendix: Additional Slides (Left Out of Main Talk but Relevant)

*These slides appear after the thanks and were not formally presented but contain important methodological context.*

---

### What Is the Baseline?

> *"What is the baseline?"*

Two baseline strategies from prior work are shown:

**From Tuckute et al. (2023, PLOS Biology):**
> *"We compared each of these models to an untrained baseline model that is commonly used in cognitive neuroscience. The baseline model consisted of a set of spectrotemporal modulation filters applied to a model of the cochlea (henceforth referred to as the SpectroTemporal model). The SpectroTemporal baseline model was explicitly constructed to capture tuning properties observed in the auditory cortex and previously been found to account for auditory cortical responses to some extent, particularly in primary auditory cortex, and thus provided a strong baseline for model comparison."*

**From Jat et al. (2019, ACL):**
> *"Random Embedding Model: In this model, we represent each word in a context by a randomly generated 300-dimensional vector. Each dimension is uniformly sampled between [0,1]. The results from this model help us establish the random baseline."*

*Interpretation: Any claim that a trained model (e.g., Whisper) aligns with the brain must be benchmarked against a system that has no learned structure. The SpectroTemporal model is a principled biological baseline (it mimics known cochlear/auditory cortex processing). The random embedding model is a statistical baseline (it sets the floor for what alignment looks like by chance). Both are needed: one to test against known brain-like signal processing, one to ensure statistical validity.*

---

### Problem Statement (Whisper Interpretability Context)

> *"Problem Statement"*

- **Whisper**: State-of-the-art Automatic Speech Recognition (ASR) model by OpenAI.
- **Problem**: High transcription accuracy but internal mechanisms largely unknown.
- **Significance**:
  - Enhances interpretability
  - Highlights model biases and failure modes
  - Provides insights and models comparable to human cognitive processing?

*Interpretation: This slide frames Whisper not just as a tool for brain comparison but as an object of study in its own right. Understanding Whisper's internal representations is valuable for AI interpretability independently of the neuroscience application. The neuroscience angle adds a unique lens: human cognitive processing provides a principled functional benchmark for what good speech representations should look like.*

---

### What Is the Construct?

> *"What is the construct? Is the model fluent? Is the model \_\_\_\_\_\_\_\_\_ ?"*

Alongside the question, a paper is shown: **Favela & Machery** — *"Investigating the concept of representation in the neural and psychological sciences"*

And a highlighted theoretical position: **"The Speculative Proposal: Representations as Promissory Notes"**

> *"Neuroscientists import the concept of representation from the psychological level of explanation in order to maintain a connection between neuroscientific and psychological models in the absence of a genuinely explanatory account of their relation."*

*Interpretation: This is a philosophically rich slide. The concept of "representation" in neuroscience may itself be underspecified — a placeholder ("promissory note") for something we do not yet know how to operationalize. The question "Is the model \_\_\_\_\_\_\_?" is left blank deliberately. It is asking: beyond transcription accuracy (fluency), what psychological or cognitive construct does Whisper's internal structure correspond to? This motivates the mechinterp approach — to fill in that blank through empirical investigation of the model's circuits.*

---

### Whisper Architecture

The architecture of Whisper is displayed:

- **Input**: Log-Mel Spectrogram (a time-frequency representation of the audio signal)
- **Encoder**: Stack of Transformer Encoder Blocks (each containing a self-attention layer and an MLP), preceded by 2×Conv1D + GELU and sinusoidal positional encoding. The "Tiny" version of Whisper has 4 such blocks.
- **Decoder**: Stack of Transformer Decoder Blocks (each with self-attention, cross-attention to the encoder, and MLP), with learned positional encoding.
- **Output**: Next-token prediction (transcription in a multitask training format including language ID, timestamp, and word tokens)

*Interpretation: This architecture overview is necessary context for the mechinterp analysis. The encoder is the primary target — it processes the audio and builds internal representations before any language decoding. The MLP layers within each encoder block are the specific components examined in the preliminary analysis. The "Tiny" model (4 encoder blocks) is tractable for interpretability work while still achieving good ASR performance.*

---

### Fishing for Activations

> *"Fishing for activations — get activations from mlp"*

A fishing hook emoji points to the MLP sub-layer within one of the Transformer Encoder Blocks.

*Interpretation: The method is forward-pass activation extraction. For each speech segment (a phoneme, in the preliminary analysis), the audio is passed through Whisper's encoder, and the activations of the MLP layers are recorded. These activation vectors become the representational features that are (a) compared to MEG data via RSA, and (b) analyzed mechanistically via the discrimination analysis and other tools.*

---

### Related Work (Interpretability Side)

**Ellena Reid** — *"Interpreting OpenAI's Whisper"* (blog post, dataset: LibriSpeech)
- Method: **Find maximally activating samples** for each neuron
- Also applied **SAEs (Sparse Autoencoders)**
- Finding: Neurons in the MLP layers of the encoder are highly interpretable. A table of the first 50 neurons in `block.2.mlp.1` shows clear phoneme selectivity:

| Neuron idx | Phoneme |
|---|---|
| 0 | 'm' |
| 1 | 'j/ch/sh' |
| 2 | 'e/a' |
| 3 | 'c/q' |
| 4 | 'is' |
| 5 | 'i' |
| 6 | noise |
| 7 | 'w' |
| 8 | 'l' |
| 9 | 'the' |
| … | … |

**Konstantine Sadov** — *"Feature Discovery in Audio Models"* (Mozilla Builders, dataset: LibriSpeech)
- Method: **Get distribution of activation values per phoneme**
- Also applied SAEs
- Focused on the same neuron: `block.2.mlp.1`, index 1 (the 'j/ch/sh' neuron)
- Finding (for that neuron):

> *"sh/tr/s/ch/dr all sound kind of like 'j' and this would fit the hypothesis of high activation values for index 1 corresponding to near-certainty of 'j', and lower-but-still-nonzero values corresponding to lower certainty."*

A plot shows mean activation values (with 95% CI) for phonemes j, sh, tr, s, ch, dr — with 'j' having the highest mean activation (~2.1) and systematic decreasing activation for acoustically similar phonemes.

*Interpretation: These blog-post-level analyses provide strong prior evidence that Whisper's MLP neurons are phoneme-selective in an interpretable way. The activations are not just arbitrary feature detectors — they appear to represent phonetic categories that are meaningful in linguistics. This motivates building a more rigorous and systematic analysis.*

---

### Related Work (Neuroscience Side)

**Schain & Goldstein (Google Research, March 2025)** — *"Deciphering language processing in the human brain through LLM representations"*

> *"Large Language Models (LLMs) optimized for predicting subsequent utterances and adapting to tasks using contextual embeddings can process natural language at a level close to human proficiency. This study shows that neural activity in the human brain aligns linearly with the internal contextual embeddings of speech and language within large language models (LLMs) as they process everyday conversations."*

*Interpretation: This is the neuroscience companion to the AI interpretability work. LLM representations linearly predict neural activity. This establishes the empirical foundation for the RSA step of the proposed framework. Importantly, this study (which relates to the Goldstein et al. 2025 paper highlighted earlier) used ECoG — invasive recordings. Using MEG (non-invasive, whole-brain, millisecond-level temporal resolution) is a meaningful extension.*

---

### Limitations of Existing Mechinterp Approaches

> *"Limitations"*

- **Results not systematically validated** — existing Whisper interpretability analyses (Reid, Sadov) are informal blog posts, not peer-reviewed studies with proper statistical controls.
- **If you have a hammer everything looks like a nail** — a general warning about confirmation bias in interpretability: if you look for phonemes, you will find patterns that look like phonemes even if the underlying structure is more complex.
- **Recent skepticism about validity of SAEs interpretation**

Two papers are shown:

**Heap, Lawson, Farnik & Aitchison** — *"Sparse Autoencoders Can Interpret Randomly Initialized Transformers"*
> SAEs applied to **random** (untrained) transformers produce similarly interpretable latents as those applied to trained models. SAE quality metrics are broadly similar for random and trained transformers across model sizes and layers.

**Kantamneni, Engels, Rajamanoharan, Tegmark & Nanda** — *"Are Sparse Autoencoders Useful? A Case Study in Sparse Probing"*
> SAEs occasionally perform better than baselines on individual datasets but cannot consistently outperform ensemble methods. The work highlights shortcomings of current SAEs and the need to rigorously evaluate interpretability methods on downstream tasks.

*Interpretation: SAEs — a popular mechinterp tool — are under serious scrutiny. If randomly initialized transformers produce equally "interpretable" SAE latents, then SAE-derived interpretations of trained models may not reflect learned structure at all. This is a sobering constraint on the methodology and motivates the convergent-evidence approach described below.*

---

### Providing Convergent Evidence for "Phoneme Representations" in Whisper

> *"Providing convergent evidence for 'phoneme representations' in Whisper"*

Rather than relying on any single method, the analysis plan triangulates through four approaches:

**1. Statistical Discrimination Analysis**
- Quantify neuron-level selectivity for phonemes
- Use t-tests, Cohen's d, and phoneme vs. **control comparisons**
- Controls:
  - **Shuffling signal samples** → generates colored noise (destroys temporal structure)
  - **Amplitude-modulated noise** → generates noise with the envelope of the phoneme (preserves amplitude modulation, destroys spectral content)

*The slide also references: Peelle JE and Davis MH (2012) "Neural oscillations carry speech rhythm through to comprehension." Front. Psychology 3:320, which shows (A) regions responsive to amplitude modulation (bilateral auditory cortex, primary processing) and (B) regions responsive to intelligible speech (extending into frontal and temporal association areas). This establishes the neuroscientific relevance of controlling for amplitude envelope specifically.*

**2. Linear Probing**
- Train simple linear classifiers on neuron activations
- Evaluate decoding performance of phoneme identity from activations
- *This tests whether phoneme information is linearly accessible, not just present in a more complex, nonlinear form*

**3. Transcoder Analysis**
- An alternative to SAEs ("sparse probing")
- *Transcoders decompose MLP weights directly into interpretable input-output mappings without requiring a separate training phase*

**4. Feature Optimization (Activation Maximization)**
- Generate input waveforms that maximize neuron activations
- Visualize phoneme preferences directly from optimized inputs
- *This is synthetic feature visualization: instead of finding natural stimuli that activate a neuron, you gradient-ascend in input space to synthesize the ideal stimulus. For audio, this produces waveforms that can be listened to and analyzed acoustically.*

*Interpretation: Each of these methods has different failure modes. The discrimination analysis can be confounded by class imbalance. Linear probing can miss nonlinear coding. SAEs (and by extension transcoders) may over-interpret structure. Activation maximization can produce artifacts. Using all four, looking for convergent results, is the appropriate epistemological strategy given the limitations of each individual method.*

---

### Preliminary Results: Statistical Discrimination Analysis

**Dataset:** TIMIT — *"Acoustic-Phonetic Continuous Speech Corpus"* (Garofolo et al., 1993, LDC93S1). 61 phonemes including stops, fricatives, vowels, nasals, and silence/noise markers:

```
['h#', 'sh', 'iy', 'hv', 'ae', 'dcl', 'y', 'ix', 'd', 'aa', 'kcl', 's', 'ux', 'tcl', 
 'en', 'gcl', 'g', 'r', 'w', 'epi', 'dx', 'ax', 'q', 'ao', 'l', 'ih', 'ow', 'nx', 'm', 
 'k', 'eh', 'n', 'oy', 'ay', 'dh', 'pcl', 'p', 'el', 't', 'er', 'z', 'aw', 'ng', 'f', 
 'ey', 'bcl', 'b', 'th', 'ax-h', 'v', 'hh', 'jh', 'ah', 'zh', 'ch', 'axr', 'uw', 'pau', 
 'uh', 'em', 'eng']
```

**Result for neuron 1 of `block.2.mlp.1`** (the 'j/ch/sh' neuron identified in prior work):

A boxplot ("Phoneme Discrimination Distribution — Neuron 1 (max matrix_t)") shows the distribution of **mean t-values** for each phoneme's activation vs. the activation of every other phoneme. The x-axis lists all phoneme pairs; the y-axis is the t-value (how strongly this phoneme discriminates from another). The color gradient from red to violet encodes rank order.

Key observation: the phoneme pair ('jh', others) has the highest discrimination, followed by similar sibilants and affricates — consistent with Reid and Sadov's findings. The pattern is non-trivial: most phonemes cluster in lower t-values, while a small number of high-selectivity phonemes dominate.

**Neuron 0 of `block.2.mlp.1`** (labeled 'm' by Reid): A separate discrimination distribution is shown. This neuron shows a different selectivity profile — broader discrimination with 'h#' (silence/noise) and contextual markers at the top.

**Control comparison result**: The discrimination plot including shuffled and amplitude-modulated noise controls shows these controls cluster at the **right end** (lowest t-values, lowest discrimination), confirming that the phoneme selectivity seen in real phoneme activations is not merely a consequence of acoustic amplitude structure.

*Interpretation: The preliminary results replicate and extend prior findings. Neuron 1 of `block.2.mlp.1` is genuinely selective for sibilant/affricate phonemes. The controls establish this is not an artifact of envelope differences between phoneme categories.*

---

### Current Problems

> *"I detected some problems with my dataset"*

**Problem 1: Phoneme frequency imbalance**

A bar chart of phoneme counts in the TIMIT subset used shows severe imbalance: 'h#' (silence) appears ~325 times, while rare phonemes like 'eng' appear fewer than 5 times. This creates serious statistical problems for t-tests: high-frequency phonemes have narrower confidence intervals, artificially inflating t-values. Low-frequency phonemes have wide uncertainty, potentially missing real selectivity.

**Problem 2: Multiple comparisons**

> *"Also, multiple comparisons problem… I provided t-values"*

With 61 phonemes, there are 61×60/2 = 1,830 pairwise comparisons per neuron, and 384 neurons per MLP block. Raw t-values without FDR or Bonferroni correction will produce a large number of spurious significant results by chance.

*Interpretation: These are honest, methodologically important self-critiques. The preliminary analysis identifies the phenomenon but is not yet statistically clean. The next steps are clear: balance the phoneme classes (subsampling or matched-pair analysis), apply FDR correction (which, notably, is already used in the lab's other MEG work on LSD), and interpret effect sizes (Cohen's d) alongside significance.*

---

*End of document.*


[//begin]: # "Autogenerated link references for markdown compatibility"
[brains-and-algorithms-partially-converge-in-natural-language-processing]: ./../sources/brains-and-algorithms-partially-converge-in-natural-language-processing "brains-and-algorithms-partially-converge-in-natural-language-processing"
[dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech]: ./../sources/dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech "dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech"
[dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech]: ./../sources/dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech "dissecting-neural-computations-in-the-human-auditory-pathway-using-deep-neural-networks-for-speech"
[inferring_dnn_brain_alignment-using-rsa-may-be-problematic]: ./../sources/inferring_dnn_brain_alignment-using-rsa-may-be-problematic "inferring_dnn_brain_alignment-using-rsa-may-be-problematic"
[lost-in-translation_-the-algorithmic-gap-between-lms-and-the-brain]: ./../sources/lost-in-translation_-the-algorithmic-gap-between-lms-and-the-brain "lost-in-translation_-the-algorithmic-gap-between-lms-and-the-brain"
[many-but-not-all-deep-neural-network-audio-models-capture-brain-responses-and-exhibit-correspondence-between-model-stages-and-brain-regions]: ./../sources/many-but-not-all-deep-neural-network-audio-models-capture-brain-responses-and-exhibit-correspondence-between-model-stages-and-brain-regions "many-but-not-all-deep-neural-network-audio-models-capture-brain-responses-and-exhibit-correspondence-between-model-stages-and-brain-regions"
[representation-with-a-capital-r-measuring-functional-alignment]: ./../sources/representation-with-a-capital-r-measuring-functional-alignment "representation-with-a-capital-r-measuring-functional-alignment"
[//end]: # "Autogenerated link references"
