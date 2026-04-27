---
title: "Designing synthetic datasets for the real world: Mechanism design and reasoning from first principles"
source: "https://research.google/blog/designing-synthetic-datasets-for-the-real-world-mechanism-design-and-reasoning-from-first-principles/"
author:
  - "[[Tim R. Davidson]]"
  - "[[Student Researcher]]"
  - "[[and Hamza Harkous]]"
  - "[[Senior Staff Research Scientist]]"
  - "[[Google]]"
published: 2026-04-15
created: 2026-04-27
description:
tags:
  - "clippings"
  - "citable"
---
To address the scarcity of data required for specialized AI, we introduce Simula, a framework that reframes synthetic data generation as dataset-level mechanism design. By using reasoning to architect datasets from first principles, Simula enables fine-grained control over coverage, complexity, and quality, providing scalable generation for privacy-sensitive or data-scarce domains.

- [Paper](https://openreview.net/pdf?id=NALsdGEPhB)

The rapid advance of generalist AI models has been fueled by the abundance of internet data. However, widespread integration of AI will require models to specialize in novel, uncommon, and privacy-sensitive applications where data is inherently scarce or inaccessible.

To bridge this gap, reliance on real-world data imposes significant limitations:

- *Cost and accessibility:* Creating specialized datasets manually is prohibitively expensive, time-consuming, and error-prone.
- *Operational drag:* The static nature of real-world data slows development cycles. In contrast, a synthetic-first approach enables "programmable workflows" where data is treated like code — versioned, reproducible, and inspectable.
- *Preparedness:* We cannot afford a reactive approach to topics like safety, where models can be hardened only after failures occur. Synthetic data allows us to proactively generate edge cases and stress-test systems against scenarios that have not yet happened in the wild.

While synthetic data is a promising alternative, current generation methods often lack the rigor required for production-scale deployment. Many existing approaches rely on manual prompts, evolutionary algorithms, or extensive seed data from the target distribution.

These methods limit *scalability* (due to reliance on seeds or human effort), *explainability* (due to black-box evolutionary steps), and *control* (due to entangled generation parameters). Most critically, they typically operate at the sample level — optimizing one data point at a time — rather than designing the dataset as a whole.

To solve this, we need to reframe synthetic data generation as a problem of mechanism design. Production use cases require a focus beyond just "more data"; they require fine-grained resource allocation where coverage, complexity, and quality are independently controllable variables.

## Simula: A reasoning-first framework

In our paper, “ [Reasoning-Driven Synthetic Data Generation and Evaluation](https://openreview.net/pdf?id=NALsdGEPhB) ”, published in [*Transactions on Machine Learning Research*](https://jmlr.org/tmlr/), we introduce Simula. Unlike methods that rely on opaque processes, Simula employs a "reasoning-first" methodology, constructing entire datasets from first principles. This approach is seedless and agentic, allowing the generation capabilities to improve naturally as the reasoning capabilities of the underlying models advance.

### Controlling the axes of data generation

Simula decomposes the generation process into distinct, controllable axes, using four steps:

1. *Global Diversification:* Instead of random sampling, Simula uses reasoning models to map the conceptual space of a target domain into deep, hierarchical taxonomies. This acts as a "sampling scaffold". By defining sampling strategies over these taxonomies, we can control *global diversity* — ensuring the dataset covers the long tail of a domain rather than clustering around common modes.

<video controls=""><source src="https://storage.googleapis.com/gweb-research2023-media/media/Simula1_Taxonomy.mp4" type="video/mp4"></video>

*To map the conceptual space of a target domain without relying on human seed data, Simula employs a reasoning-driven, recursive expansion process. At each depth level, the system generates multiple candidate sub-categories (proposals) that are subsequently evaluated, merged, and filtered by a critic model. This iterative "propose-and-refine" loop dynamically builds a dense, hierarchical taxonomy — such as the Cyber Threat Intelligence tree — that serves as the foundational scaffold to ensure global dataset diversity.*

Equipped with a set of deep taxonomies, we can now start mapping out our coverage space of interest and optimize (2) local diversity, (3) complexity, and (4) quality:

2\. *Local Diversification:* To ensure variation within specific concepts, we employ *local diversity* mechanisms. The system generates "meta-prompts" — scenarios derived from taxonomy nodes — and then produces multiple distinct instantiations of that scenario. This prevents mode collapse, ensuring that a concept like "SQL injection" is represented through diverse framings rather than identical repetitions.

3\. *Complexification:* Complexity is treated as an orthogonal axis. We use a "complexification" step where a configurable fraction of meta-prompts is refined to be more elaborate or difficult. This allows practitioners to shift the difficulty distribution of a dataset without changing its semantic coverage.

4\. *Quality Checks:* To ensure correctness without human intervention, we employ a "dual-critic" loop that independently assesses if an answer is correct or incorrect. This dual-verification helps mitigate sycophancy (where models tend to agree with plausible-sounding outputs) and ensures high-quality labels.

<video controls=""><source src="https://storage.googleapis.com/gweb-research2023-media/media/Simula2_Pipeline.mp4" type="video/mp4"></video>

*Simula frames synthetic data creation as a mechanism design problem, decomposing the process into distinct, controllable axes. First, Global Diversification leverages taxonomies to ensure broad domain coverage. Second, Local Diversification uses 1-of-N meta-prompting to instantiate distinct scenarios and prevent mode collapse. Third, Complexification optionally refines these scenarios to elevate difficulty and detail. Finally, Quality Checks utilize a dual-critic loop to verify that all outputs meet semantic and structural constraints.*

### Addressing challenges in evaluation

The evaluation of synthetic data is fundamentally challenging due to the ambiguity of its core objectives and the disconnect between standard metrics and practical utility. Standard metrics like embedding-based cosine distance provide a high-level signal but offer [limited](https://arxiv.org/abs/1909.00512) actionable insights.

To make evaluations more robust, we apply our reasoning-first approach here as well. Specifically, we introduce reasoning-based metrics — *Taxonomic Coverage* and *Calibrated Complexity Scoring* (which uses LLM-driven batch comparisons to assign chess-style "Elo ratings" to individual data points) — to better capture the nuances of diversity and difficulty.

### No universal solution

We used Gemini 2.5 Flash as a teacher model and Gemma-3 4B as a student to evaluate Simula across five diverse domains — from cybersecurity (CTI-MCQ, CTI-RCM from [CTIBench](https://arxiv.org/abs/2406.07599)) and legal reasoning ([LEXam](https://arxiv.org/abs/2505.12864)), to standard AI model evaluations such as grade-school math ([GSM8k](https://arxiv.org/abs/2110.14168)) and multilingual academic knowledge ([Global MMLU](https://arxiv.org/abs/2412.03304)). Generating datasets of up to 512K data points for each domain, our results highlight a critical reality: there is no single "optimal" way to generate data, and the relationship between "good" data and downstream performance is deeply idiosyncratic.

- *Mechanism design is non-negotiable:* Across all domains, the full Simula system — which combines global coverage, local diversity, and critiquing — consistently outperformed simpler baselines.
- *Context is king:* There are no fixed recipes. While high complexity yielded a 10% accuracy gain in math reasoning (GSM8k), it actually hurt performance in legal reasoning (LEXam) where the teacher model was weaker. Data must be tailored to the capabilities of the model consuming it.
- *Quality is the new quantity:* Better data scales better. Simula achieved higher downstream performance with fewer samples compared to baseline approaches, confirming that scaling laws are driven by data properties, not just volume.

While this was a distillation setup, chosen for replicable, systemic evaluation, the core lessons learned extend beyond this specific configuration.

![Simula3_ResultsGraph](https://storage.googleapis.com/gweb-research2023-media/images/Simula3_ResultsGraph.width-1250.png)

*Downstream performance on different datasets.*

## From research to real-world impact

Simula was not just built to optimize benchmarks, it serves as a foundational data engine for real-world, business-critical applications across Google. Within the frontier AI space, it has been a key enabler for the Gemma ecosystem — including specialized models like [ShieldGemma](https://deepmind.google/models/gemma/shieldgemma-2/), [FunctionGemma](https://blog.google/innovation-and-ai/technology/developers-tools/functiongemma/), and [MedGemma](https://deepmind.google/models/gemma/medgemma/) — while providing the primary synthetic data backbone for both on-device and server-side Gemini safety classifiers. Beyond foundation models, Simula has been instrumental in shipping user protection features, including [AI-powered scam detection for Android calls](https://security.googleblog.com/2025/03/new-ai-powered-scam-detection-features.html) and [spam filtering in Google Messages](https://blog.google/products-and-platforms/platforms/android/new-android-features-march-2025/). Furthermore, Simula is actively driving new applied research, facilitating frameworks that [democratize ML for enterprise security](https://arxiv.org/abs/2512.08802) by synthesizing realistic attack scenarios, and enabling breakthroughs like [teaching AI models to read maps](https://research.google/blog/teaching-ai-to-read-a-map/) through structured, reasoning-driven dataset generation.

## Synthetic data's central role in specialized AI

AI progress is at a junction. The specialized data required for the next wave of breakthroughs — in science, security, and law — is unlikely to be generated by humans at the necessary scale. Synthetic data is primed to play a central role in these leaps, but only if approached with rigor. Ultimately, Simula's value lies in demonstrating how mechanism design can make data generation a controllable science. This blueprint provides a clear path to building the high-fidelity datasets the next era of AI demands — whether we are distilling knowledge into edge devices, training agents via reinforcement learning, or systematically exploring complex edge-cases.

## Acknowledgements

*This research was authored by Tim R. Davidson, Benoit Seguin, Enrico Bacis, Cesar Ilharco, and Hamza Harkous. The Simula framework was founded and led by Hamza and Benoit. Special thanks go to Tim for his significant contributions during his student researcher tenure. We also thank Jan Keller for his TPM support and Coran Corbett and Ninny Wan for their vital technical and product partnerships. Finally, we thank Nina Taft, Amanda Walker, and Pankaj Rohatgi for their sponsorship and support.*

- [Paper](https://openreview.net/pdf?id=NALsdGEPhB)