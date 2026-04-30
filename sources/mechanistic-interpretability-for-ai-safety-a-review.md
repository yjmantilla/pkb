---
title: "Mechanistic Interpretability for AI Safety A Review"
source: "https://arxiv.org/html/2404.14082v3"
author:

created: 2026-04-30
description:
tags:
  - "clippings"
  - "interpretability"
  - "mechinterp"
  - "speech-representations"
  - "citable"
---
Leonard Bereska      Efstratios Gavves  
{leonard.bereska, egavves}@uva.nl  
University of Amsterdam

###### Abstract

Understanding AI systems’ inner workings is critical for ensuring value alignment and safety. This review explores mechanistic interpretability: reverse engineering the computational mechanisms and representations learned by neural networks into human-understandable algorithms and concepts to provide a granular, causal understanding. We establish foundational concepts such as features encoding knowledge within neural activations and hypotheses about their representation and computation. We survey methodologies for causally dissecting model behaviors and assess the relevance of mechanistic interpretability to AI safety. We examine benefits in understanding, control, alignment, and risks such as capability gains and dual-use concerns. We investigate challenges surrounding scalability, automation, and comprehensive interpretation. We advocate for clarifying concepts, setting standards, and scaling techniques to handle complex models and behaviors and expand to domains such as vision and reinforcement learning. Mechanistic interpretability could help prevent catastrophic outcomes as AI systems become more powerful and inscrutable. For an HTML version of the paper, visit [https://leonardbereska.github.io/blog/2024/mechinterpreview/](https://leonardbereska.github.io/blog/2024/mechinterpreview/).

## 1 Introduction

As AI systems rapidly become more sophisticated and general [^32] [^18], advancing our understanding of these systems is crucial to ensure their alignment [^164] with human values and avoid catastrophic outcomes [^133] [^131]. The field of interpretability aims to demystify the internal processes of AI models, moving beyond evaluating performance alone. This review focuses on mechanistic interpretability, an emerging approach within the broader interpretability landscape that strives to comprehensively specify the computations underlying deep neural networks. We emphasize that understanding and interpreting these complex systems is not merely an academic endeavor – it’s a societal imperative to ensure AI remains trustworthy and beneficial.

The interpretability landscape is undergoing a paradigm shift akin to the evolution from behaviorism to cognitive neuroscience in psychology. Historically, lacking tools for introspection, psychology treated the mind as a black box, focusing solely on observable behaviors. Similarly, interpretability has predominantly relied on black-box techniques [^46], analyzing models based on input-output relationships or using attribution methods that, while probing deeper, still neglect the model’s internal architecture. However, just as advancements in neuroscience allowed for a deeper understanding of internal cognitive processes, the field of interpretability is now moving towards a more granular approach. This shift from surface-level analysis to a focus on the internal mechanics of deep neural networks characterizes the transition towards inner interpretability [^287].

Mechanistic interpretability, as an approach to inner interpretability, aims to completely specify a neural network’s computation, potentially in a format as explicit as pseudocode (also called [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering)), striving for a granular and precise understanding of model behavior. It distinguishes itself primarily through its ambition for comprehensive reverse engineering and its strong motivation towards AI safety. Our review serves as the first comprehensive exploration of mechanistic interpretability research, with the most accessible introductions currently scattered in a blog or list format [^261] [^238] [^260] [^315] [^259] [^245] [^246]. Concurrently, [^85] and [^285] have also contributed valuable reviews giving concise, technical introductions to mechanistic interpretability in transformer-based language models. Our work complements these efforts by synthesizing the research (addressing the "research debt" [^257]) and providing a structured, accessible, and comprehensive introduction for AI researchers and practitioners.

The structure of this paper provides a cohesive overview of mechanistic interpretability, situating the mechanistic approach in the broader interpretability landscape (Section 2), presenting core concepts and hypotheses (Section 3), explaining methods and techniques (Section 4), presenting a taxonomy and survey of the current field (Section 5), exploring relevance to AI safety (Section 6), and addressing challenges (Section 7) and future directions (Section 8).

## 2 Interpretability Paradigms from the Outside In

We encounter a spectrum of interpretability paradigms for decoding AI systems’ decision-making, ranging from external black-box techniques to internal analyses. We contrast these paradigms with mechanistic interpretability, highlighting its distinct causal bottom-up perspective within the broader interpretability landscape (see Figure 1).

![Refer to caption](https://arxiv.org/html/2404.14082v3/x1.png)

Figure 1: Interpretability paradigms offer distinct lenses for understanding neural networks: Behavioral analyzes input-output relations; Attributional quantifies individual input feature influences; Concept-based identifies high-level representations governing behavior; Mechanistic uncovers precise causal mechanisms from inputs to outputs.

##### Behavioral

interpretability treats the model as a black box, analyzing input-output relations. Techniques such as minimal pair analysis [^356], sensitivity and perturbation analysis [^40] examine input-output relations to assess the model’s robustness and variable dependencies [^312] [^290] [^61]. Its model-agnostic nature is practical for complex or proprietary models but lacks insight into internal decision processes and causal depth [^166].

##### Attributional

interpretability aims to explain outputs by tracing predictions to individual input contributions using gradients. Raw gradients can be discontinuous or sensitive to slight perturbations. Therefore, techniques such as SmoothGrad [^322] and Integrated Gradients [^330] average across gradients. Other popular techniques are layer-wise relevance propagation [^8], DeepLIFT [^319], or GradCAM [^310]. Attribution enhances transparency by showing input feature influence without requiring an understanding of the internal structure, enabling decision validation, compliance, and trust while serving as a bias detection tool, but also has fundamental limitations [^23].

##### Concept-based

interpretability adopts a top-down approach to unraveling a model’s decision-making processes by probing its learned representations for high-level concepts and patterns governing behavior. Techniques include training supervised auxiliary classifiers [^14], employing unsupervised contrastive and structured probes (see Section 4.2) to explore latent knowledge [^33], and using neural representation analysis to quantify the representational similarities between the internal representations learned by different neural networks [^174] [^11]. Beyond observational analysis, concept-based interpretability can enable manipulation of these representations – also called [representation engineering](https://arxiv.org/html/2404.14082v3#glo.main.representation%20engineering) [^375] – potentially enhancing safety by upregulating concepts such as honesty, harmlessness, and morality.

##### Mechanistic

interpretability is a bottom-up approach that studies the fundamental components of models through granular analysis of features, neurons, layers, and connections, offering an intimate view of operational mechanics. Unlike concept-based interpretability, it aims to uncover causal relationships and precise computations transforming inputs into outputs, often identifying specific neural circuits driving behavior. This [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering) approach draws from interdisciplinary fields like physics, neuroscience, and systems biology to guide the development of transparent, value-aligned AI systems. Mechanistic interpretability is the primary focus of this review.

## 3 Core Concepts and Assumptions

This section introduces the key concepts and hypotheses of mechanistic interpretability, as summarized in Figure 2. We start by defining features as the basic units of representation (Section 3.1). We then examine the nature of these features, including the challenges posed by polysemantic neurons and the implications of the superposition and linear representation hypotheses (Section 3.2). Next, we explore computation through circuits and motifs, considering the universality hypothesis (Section 3.3). Finally, we discuss the implications for understanding emergent properties, such as internal world models and simulated agents with potentially misaligned objectives (Section 3.4).

![Refer to caption](https://arxiv.org/html/2404.14082v3/x2.png)

Figure 2: Overview of key concepts and hypotheses in mechanistic interpretability, organized into four subsection (pink boxes): defining features (Section 3.1 ), representation (Section 3.2 ), computation (Section 3.3 ), and emergence (Section 3.4 ). In turquoise, it highlights definitions like features, circuits, and motifs, and in orange, it highlights hypotheses like linear representation superposition universality simulation prediction orthogonality. Arrows show relationships, e.g., superposition enabling an alternative feature definition or universality connecting circuits and motifs.

### 3.1 Defining Features as Representational Primitives

##### Features as fundamental units of representation.

The notion of a *feature* in neural networks is central yet elusive, reflecting the pre-paradigmatic state of mechanistic interpretability. We adopt the notion of [features](https://arxiv.org/html/2404.14082v3#glo.main.features) as the fundamental units of neural network representations, such that features cannot be further [disentangled](https://arxiv.org/html/2404.14082v3#glo.main.disentangled) into simpler, distinct factors. These features are core components of a neural network’s representation, analogous to how cells form the fundamental unit of biological organisms [^260].

<svg height="75.91" id="S3.SS1.SSS0.Px1.pic1" overflow="visible" version="1.1" viewBox="0 0 600 75.91" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,75.91) matrix(1 0 0 -1 0 0)"><g fill="#01AB8F" fill-opacity="1.0" style="--ltx-fill-color:#01AB8F;"><path d="M 0 5.91 L 0 70.01 C 0 73.27 2.64 75.91 5.91 75.91 L 594.09 75.91 C 597.36 75.91 600 73.27 600 70.01 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#F2FBF9" fill-opacity="1.0" style="--ltx-fill-color:#F2FBF9;"><path d="M 1.97 5.91 L 1.97 54.49 L 598.03 54.49 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 60.4)"><foreignObject color="#FFFFFF" height="9.61" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Definition 1: Feature</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 16.47)"><foreignObject color="#000000" height="28.9" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:1.89em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 26.21)" width="556.69"><span style="width:40.23em;">Features are the fundamental units of neural network representations that cannot be further decomposed into simpler independent factors.</span></foreignObject></g></g></svg>

##### Concepts as natural abstractions.

The world consists of various entities that can be grouped into categories or [concepts](https://arxiv.org/html/2404.14082v3#glo.main.concepts) based on shared properties. These concepts form high-level summaries like "tree" or "velocity," allowing compact world representations by discarding many irrelevant low-level details. Neural networks can capture and represent such [natural abstractions](https://arxiv.org/html/2404.14082v3#glo.main.natural%20abstractions) [^49] through their learned [features](https://arxiv.org/html/2404.14082v3#glo.main.features), which serve as building blocks of their internal representations, aiming to capture the [concepts](https://arxiv.org/html/2404.14082v3#glo.main.concepts) underlying the data.

##### Features encoding input patterns.

In traditional machine learning, [features](https://arxiv.org/html/2404.14082v3#glo.main.features) are understood as characteristics or attributes derived directly from the input data stream [^24]. This view is particularly relevant for systems focused on perception, where features map closely to the input data. However, in more advanced systems capable of reasoning with abstractions, features may emerge internally within the model as representational patterns, even when processing information unrelated to the input. In this context, features are better conceptualized as *any measurable property or characteristic of a phenomenon* [^261], encoding abstract concepts rather than strictly reflecting input attributes.

##### Features as representational atoms.

A key property of features is their irreducibility, meaning they cannot be decomposed into or expressed as a combination of simpler, independent factors. In the context of input-related features, [^78] define a feature as [irreducible](https://arxiv.org/html/2404.14082v3#glo.main.irreducible) if it cannot be decomposed into or expressed as a combination of statistically independent patterns or factors in the original input data. Specifically, a feature is reducible if transformations reveal its underlying pattern, which can be separated into independent co-occurring patterns or is a mixture of patterns that never co-occur. We propose generalizing this notion of irreducibility to features encoding abstract concepts not directly tied to input patterns, such that features cannot be reduced to combinations or mixtures of other independent components within the model’s representations.

##### Features beyond human interpretability.

Features could be defined from a *human-centric perspective* as *semantically meaningful, articulable input patterns encoded in the network’s activation space* [^261]. However, while cognitive systems may converge on similar [natural abstractions](https://arxiv.org/html/2404.14082v3#glo.main.natural%20abstractions) [^49], these need not necessarily align with human-interpretable [concepts](https://arxiv.org/html/2404.14082v3#glo.main.concepts). Adversarial examples have been interpreted as non-interpretable features meaningful to models but not humans. Imperceptible perturbations fool networks, suggesting reliance on alien representational patterns [^154]. As models surpass human capabilities, their learned features may become increasingly abstract, encoding information in ways incongruent with human intuition [^147]. Mechanistic interpretability aims to uncover the *actual* representations learned, even if diverging from human concepts. While human-interpretable concepts provide guidance, a non-human-centric perspective that defines features as independent model components, whether aligned with human concepts or not, is a more comprehensive and future-proof approach.

### 3.2 Nature of Features: From Monosemantic Neurons to Non-Linear Representations

![Refer to caption](https://arxiv.org/html/2404.14082v3/x3.png)

Figure 3: Contrasting privileged and non-privileged bases. In a non-privileged basis, there is no reason to expect features to be basis-aligned – calling basis dimensions neurons has no meaning. In a privileged basis, the architecture treats basis directions differently – features can but need not align with neurons 30. Leftmost: Privileged basis; individual features (arrows) align with basis directions, resulting in monosemantic neurons (colored circles). Middle left: Privileged basis, where despite having more features than neurons, some neurons are monosemantic, representing individual features, while others are polysemantic (overlapping gradients), encoding superposition of multiple features. Middle right: Non-privileged basis where, even when the number of features equals the number of neurons, the lack of alignment between the feature directions and basis directions results in polysemantic neurons encoding combinations of features. Rightmost: Non-privileged, polysemantic neurons as feature directions do not align with neuron basis.

##### Neurons as Computational Units?

In the architecture of neural networks, neurons are the natural computational units, potentially representing individual features. Within a neural network representation $h\in\mathbb{R}^{n}$, the $n$ basis directions are called neurons. For a neuron to be meaningful, the basis directions must functionally differ from other directions in the representation, forming a [privileged basis](https://arxiv.org/html/2404.14082v3#glo.main.privileged%20basis) – where the basis vectors are architecturally distinguished within the neural network layer from arbitrary directions in activation space, as shown in Figure 3. Typical non-linear activation functions privilege the basis directions formed by the neurons, making it meaningful to analyze individual neurons [^77]. Analyzing neurons can give insights into a network’s functionality [^300] [^231] [^63] [^107] [^350] [^74] [^108] [^22] [^145].

##### Monosemantic and Polysemantic Neurons.

A neuron corresponding to a single semantic concept is called [monosemantic](https://arxiv.org/html/2404.14082v3#glo.main.monosemantic). The intuition behind this term comes from analyzing what inputs activate a given neuron, revealing its associated semantic meaning or concept. If neurons were the representational primitives of neural networks, all neurons would be monosemantic, implying a one-to-one relationship between neurons and features. Comprehensive interpretability would be as tractable as characterizing all neurons and their connections. However, empirically, especially for transformer models [^77], neurons are often observed to be [polysemantic](https://arxiv.org/html/2404.14082v3#glo.main.polysemantic), *i.e.*, associated with multiple, unrelated concepts [^6] [^231] [^76] [^260]. For example, a single neuron may be activated by both images of cats and images of cars, suggesting it encodes multiple unrelated concepts. Polysemanticity contradicts the interpretation of neurons as representational primitives and, in practice, makes it challenging to understand the information processing of neural networks.

##### Exploring Polysemanticity: Hypotheses and Implications.

To understand the widespread occurrence of polysemanticity in neural networks, several hypotheses have been proposed:

1. One trivial scenario would be that feature directions are orthogonal but not aligned with the basis directions (neurons). There is no inherent reason to assume that features would align with neurons in a non-privileged basis, where the basis vectors are not architecturally distinguished. However, even in a privileged basis formed by the neurons, the network could represent features not in the standard basis but as linear combinations of neurons (see Figure 3, middle right).
2. An alternative hypothesis posits that *redundancy due to noise* introduced during training, such as random dropout [^324], can lead to redundant representations and, consequently, to polysemantic neurons [^214]. This process involves distributing a single feature across several neurons rather than isolating it into individual ones, thereby encouraging polysemanticity.
3. Finally, the [superposition](https://arxiv.org/html/2404.14082v3#glo.main.superposition) hypothesis addresses the limitations in the network’s representative capacity – the number of neurons versus the number of crucial concepts. This hypothesis argues that the limited number of neurons compared to the vast array of important concepts necessitates a form of compression. As a result, an $n$ -dimensional representation may encode features not with the $n$ basis directions (neurons) but with the $\propto\exp(n)$ possible almost orthogonal directions [^77], leading to polysemanticity.

<svg height="95.21" id="S3.SS2.SSS0.Px3.pic1" overflow="visible" version="1.1" viewBox="0 0 600 95.21" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,95.21) matrix(1 0 0 -1 0 0)"><g fill="#FEAE03" fill-opacity="1.0" style="--ltx-fill-color:#FEAE03;"><path d="M 0 5.91 L 0 89.3 C 0 92.57 2.64 95.21 5.91 95.21 L 594.09 95.21 C 597.36 95.21 600 92.57 600 89.3 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#FFFBF2" fill-opacity="1.0" style="--ltx-fill-color:#FFFBF2;"><path d="M 1.97 5.91 L 1.97 71.1 L 598.03 71.1 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 79.69)"><foreignObject color="#FFFFFF" height="12.3" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Hypothesis 1: Superposition</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 16.47)"><foreignObject color="#000000" height="45.51" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:3.09em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 42.82)" width="556.69"><span style="width:40.23em;">Neural networks represent more features than they have neurons by encoding features in overlapping combinations of neurons.</span></foreignObject></g></g></svg>

##### Superposition Hypothesis.

The [superposition](https://arxiv.org/html/2404.14082v3#glo.main.superposition) hypothesis suggests that neural networks can leverage high-dimensional spaces to represent more features than the actual count of neurons by encoding features in almost orthogonal directions. Non-orthogonality means that features interfere with one another. However, the benefit of representing many more features than neurons may outweigh the interference cost, mainly when concepts are sparse and non-linear activation functions can error-correct noise [^77].

![[Uncaptioned image]](https://arxiv.org/html/2404.14082v3/x4.png)

\[Uncaptioned image\]

Toy models can demonstrate under which conditions superposition occurs [^77] [^307]. Neural networks, via superposition, may effectively simulate computation with more neurons than they possess by allocating each feature to a linear combination of neurons, creating what is known as an overcomplete linear basis in the representation space. This perspective on superposition suggests that polysemantic models could be seen as compressed versions of hypothetically larger neural networks where each neuron represents a single concept (see Figure 5). Consequently, an alternative definition of features could be:

<svg height="196.45" id="S3.SS2.SSS0.Px4.pic2" overflow="visible" version="1.1" viewBox="0 0 600 196.45" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,196.45) matrix(1 0 0 -1 0 0)"><g fill="#01AB8F" fill-opacity="1.0" style="--ltx-fill-color:#01AB8F;"><path d="M 0 5.91 L 0 190.54 C 0 193.81 2.64 196.45 5.91 196.45 L 594.09 196.45 C 597.36 196.45 600 193.81 600 190.54 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#F2FBF9" fill-opacity="1.0" style="--ltx-fill-color:#F2FBF9;"><path d="M 1.97 5.91 L 1.97 170.8 L 598.03 170.8 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 180.17)"><foreignObject color="#FFFFFF" height="13.84" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.75em;--fo_depth :0.25em;" transform="matrix(1 0 0 -1 0 10.38)" width="556.69"><span style="width:40.23em;">Definition 2: Feature (Alternative)</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 16.55)"><foreignObject color="#000000" height="145.21" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:10.29em;--fo_depth :0.2em;" transform="matrix(1 0 0 -1 0 142.44)" width="556.69"><span style="width:40.23em;">Features are elements that a network would ideally assign to individual neurons if neuron count were not a limiting factor <sup id="fnref:30-2"><a href="#fn:30">30</a></sup>. In other words, <a href="https://arxiv.org/html/2404.14082v3#glo.main.features"><span href="https://arxiv.org/html/2404.14082v3#glo.main.features" title="The fundamental units of how neural networks encode knowledge, which cannot be further decomposed into smaller, distinct . Features are core components of a neural network’s representation, analogous to how cells form the fundamental unit of biological organisms (, ). The  hypothesis suggests an alternative definition: that features correspond to the  concepts that a larger, sparser network with sufficient capacity would learn to represent with individual () neurons (, ).">features</span></a> correspond to the disentangled <a href="https://arxiv.org/html/2404.14082v3#glo.main.concepts"><span href="https://arxiv.org/html/2404.14082v3#glo.main.concepts" title="An abstract idea or representation derived from observations of the world. Concepts refer to the natural abstractions that a cognitive system, like a neural network, aims to capture and represent through its learned features, which may or may not align perfectly with human-defined concepts.">concepts</span></a> that a larger, sparser network with sufficient capacity would learn to represent with individual neurons.</span></foreignObject></g></g></svg>![Refer to caption](https://arxiv.org/html/2404.14082v3/x5.png)

Figure 5: Observed neural networks (left) can be viewed as compressed simulations of larger, sparser networks (right) where neurons represent distinct features. An "almost orthogonal" projection compresses the high-dimensional sparse representation, manifesting as polysemantic neurons involved with multiple features in the lower-dimensional observed model, reflecting the compressed encoding. Figure adapted from 30.

Research on superposition, including works by [^77] [^307] [^134], often investigates simplified models. However, understanding superposition in practical, transformer-based scenarios is crucial for real-world applications, as pioneered by [^118].

The need for understanding networks despite polysemanticity has led to various approaches: One involves training models without superposition [^163], for example, using a softmax linear unit [^76] as an activation function to empirically increase the number of [monosemantic](https://arxiv.org/html/2404.14082v3#glo.main.monosemantic) neurons, but at the cost of making other neurons less interpretable. From a capabilities standpoint, polysemanticity may be desirable as it allows models to represent more concepts with limited compute, making training cheaper. Overall, engineering monosemanticity has proven challenging [^30] and may be impractical until we have orders of magnitude more compute available.

Another approach is to train networks in a standard way (creating polysemanticity) and use post-hoc analysis to find the feature directions in activation space, for example, with Sparse Autoencoders (SAEs). SAEs aim to find the true, disentangled features in an uncompressed representation by learning a sparse overcomplete basis that describes the activation space of the trained model [^30] [^316] [^62] (also see Section 4.2).

##### If not neurons, what are features then?

We want to identify the fundamental units of neural networks, which we call [features](https://arxiv.org/html/2404.14082v3#glo.main.features). Initially, neurons seemed likely candidates. However, this view fell short, particularly in transformer models where neurons often represent multiple concepts, a phenomenon known as polysemanticity. The superposition hypothesis addresses this, proposing that due to limited representational capacity, neural networks compress numerous features into the confined space of neurons, complicating interpretation.

This raises the question: *How are features encoded if not in discrete neuron units?* While a priori features could be encoded in an arbitrarily complex, non-linear structure, a growing body of theoretical arguments and empirical evidence supports the hypothesis that features are commonly represented linearly, i.e., as linear combinations of neurons – hence, as directions in representation space. This perspective promises to enhance our comprehension of neural networks by providing a more interpretable and manipulable framework for their internal representations.

<svg height="78.6" id="S3.SS2.SSS0.Px5.pic1" overflow="visible" version="1.1" viewBox="0 0 600 78.6" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,78.6) matrix(1 0 0 -1 0 0)"><g fill="#FEAE03" fill-opacity="1.0" style="--ltx-fill-color:#FEAE03;"><path d="M 0 5.91 L 0 72.7 C 0 75.96 2.64 78.6 5.91 78.6 L 594.09 78.6 C 597.36 78.6 600 75.96 600 72.7 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#FFFBF2" fill-opacity="1.0" style="--ltx-fill-color:#FFFBF2;"><path d="M 1.97 5.91 L 1.97 54.49 L 598.03 54.49 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 63.09)"><foreignObject color="#FFFFFF" height="12.3" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Hypothesis 2: Linear Representation</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 16.47)"><foreignObject color="#000000" height="28.9" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:1.89em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 26.21)" width="556.69"><span style="width:40.23em;">Features are directions in activation space, <em>i.e.</em>, linear combinations of neurons.</span></foreignObject></g></g></svg>

The [linear representation](https://arxiv.org/html/2404.14082v3#glo.main.linear%20representation) hypothesis suggests that neural networks frequently represent high-level features as linear directions in activation space. This hypothesis can simplify the understanding and manipulation of neural network representations [^248]. The prevalence of linear layers in neural network architectures favors linear representations. Matrix multiplication in these layers most readily processes linear features, while more complex non-linear encodings would require multiple layers to decode.

However, recent work by [^78] provides evidence against a strict formulation of the linear representation hypothesis by identifying circular features representing days of the week and months of the year. These multi-dimensional, non-linear representations were shown to be used for solving modular arithmetic problems in days and months. Intervention experiments confirmed that these circular features are the fundamental unit of computation in these tasks, and the authors developed methods to decompose the hidden states, revealing the circular representations.

Establishing non-linearity can be challenging. For example, [^190] initially found that in a GPT model trained on Othello, the board state could only be decoded with a non-linear probe when represented in terms of white and black pieces, seemingly violating the linearity assumption. However, [^242] [^248] later showed that a linear probe sufficed when the board state was decoded in terms of "one’s own" and "the opponent’s" pieces, reaffirming the linear representation hypothesis in this case. In contrast, the work by [^78] provides a clear and convincing existence proof for non-linear, multi-dimensional representations in language models.

While the linear representation hypothesis remains a useful simplification, it is important to recognize its limitations and the potential role of non-linear representations [^315]. As neural networks continue to evolve, ongoing reevaluation of the hypothesis is crucial, particularly considering the possible emergence of non-linear features under optimization pressure for interpretability [^150]. Alternative perspectives, such as the polytope lens proposed by [^25], emphasize the impact of non-linear activation functions and discrete polytopes formed by piecewise linear activations as potential primitives of neural network representations.

Despite these exceptions, empirical evidence largely supports the linear representation hypothesis in many contexts, especially for feedforward networks with ReLU activations. Semantic vector calculus in word embeddings [^225], successful linear probing [^2] [^14], sparse dictionary learning [^30] [^62] [^68], and linear decoding of concepts [^264], tasks [^128], functions [^339], sentiment [^338], refusal [^4], and relations [^136] [^50] in large language models all point to the prevalence of linear representations. Moreover, linear addition techniques for model steering [^341] [^301] [^191] and [representation engineering](https://arxiv.org/html/2404.14082v3#glo.main.representation%20engineering) [^375] highlight the practical implications of linear feature representations.

Building upon the linear representation hypothesis, recent work investigated the structural organization of these linear features within activation space. [^272] reveal a geometric framework for categorical and hierarchical concepts in large language models. Their findings demonstrate that simple categorical concepts (e.g., mammal, bird) are represented as simplices in the activation space, while hierarchically related concepts are orthogonal. This geometric analysis aligns with earlier observations on feature clustering and splitting in neural networks [^77]. It suggests that the linear features are not merely scattered directions but are organized to reflect semantic relationships and hierarchies.

### 3.3 Circuits as Computational Primitives and Motifs as Universal Circuit Patterns

Having defined features as directions in activation space as the fundamental units of neural network representation, we now explore their computation. Neural networks can be conceptualized as computational graphs, within which [circuits](https://arxiv.org/html/2404.14082v3#glo.main.circuits) are sub-graphs consisting of linked features and the weights connecting them. Similar to how features are the representational primitive, circuits function as the computational primitive [^223] and the primary building block of these networks [^260].

![Refer to caption](https://arxiv.org/html/2404.14082v3/x6.png)

Figure 6: Comparing observed models (left) and corresponding hypothetical disentangled models (right) trained on similar tasks and data. The observed models show different neuronal activation patterns, while the dissection into feature-level circuits reveals a motif - a shared circuit pattern emerging across models, hinting at universality – models converging on similar solutions based on common underlying principles.

<svg height="73.22" id="S3.SS3.pic1" overflow="visible" version="1.1" viewBox="0 0 600 73.22" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,73.22) matrix(1 0 0 -1 0 0)"><g fill="#01AB8F" fill-opacity="1.0" style="--ltx-fill-color:#01AB8F;"><path d="M 0 5.91 L 0 67.32 C 0 70.58 2.64 73.22 5.91 73.22 L 594.09 73.22 C 597.36 73.22 600 70.58 600 67.32 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#F2FBF9" fill-opacity="1.0" style="--ltx-fill-color:#F2FBF9;"><path d="M 1.97 5.91 L 1.97 51.8 L 598.03 51.8 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 57.71)"><foreignObject color="#FFFFFF" height="9.61" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Definition 3: Circuit</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 13.78)"><foreignObject color="#000000" height="26.21" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:1.89em;--fo_depth :0em;" transform="matrix(1 0 0 -1 0 26.21)" width="556.69"><span style="width:40.23em;">Circuits are sub-graphs of the network, consisting of features and the weights connecting them.</span></foreignObject></g></g></svg>

The decomposition of neural networks into circuits for interpretability has shown significant promise, particularly in small models trained for specific tasks such as addition, as seen in the work of [^247] and [^283]. Scaling such a comprehensive circuit analysis to broader behaviors in large language models remains challenging. However, there has been notable progress in scaling circuit analysis of narrow behaviors to larger circuits and models, such as indirect object identification [^355] and greater-than computations [^122] in GPT-2 and multiple-choice question answering [^193].

In search of general and universal circuits, researchers focus particularly on more general and transferable behaviors. [^216] ’s work on copy suppression in GPT-2’s attention heads sheds light on model calibration and self-repair mechanisms. [^67] and [^83] focus on how large language models represent symbolic knowledge through variable binding and entity-attribute binding, respectively. [^367] [^249] [^204] [^57] [^266] explore mechanisms for factual recall, revealing how circuits dynamically balance pre-trained knowledge with new contextual information. [^180] extend circuit analysis to sequence continuation tasks, identifying shared computational structures across semantically related sequences.

More promisingly, some repeating patterns have shown [universality](https://arxiv.org/html/2404.14082v3#glo.main.universality) across models and tasks. These universal patterns are called [motifs](https://arxiv.org/html/2404.14082v3#glo.main.motifs) [^260] and can manifest not just as specific circuits or features but also as higher-level behaviors emerging from the interaction of multiple components. Examples include the curve detectors found across vision models [^37] [^36], induction circuits enabling in-context learning [^263], and the phenomenon of branch specialization in neural networks [^353]. Motifs may also capture how models leverage tokens for working memory or parallelize computations in a divide-and-conquer fashion across representations. The significance of motifs lies in revealing the common structures, mechanisms, and strategies that naturally emerge across neural architectures, shedding light on the fundamental building blocks underlying their intelligence. Figure 6 contrasts observed neural network models with hypothetical disentangled models, illustrating how a shared circuit pattern can emerge across different models trained on similar tasks and data, hinting at an underlying [universality](https://arxiv.org/html/2404.14082v3#glo.main.universality).

<svg height="89.83" id="S3.SS3.pic2" overflow="visible" version="1.1" viewBox="0 0 600 89.83" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,89.83) matrix(1 0 0 -1 0 0)"><g fill="#01AB8F" fill-opacity="1.0" style="--ltx-fill-color:#01AB8F;"><path d="M 0 5.91 L 0 83.92 C 0 87.18 2.64 89.83 5.91 89.83 L 594.09 89.83 C 597.36 89.83 600 87.18 600 83.92 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#F2FBF9" fill-opacity="1.0" style="--ltx-fill-color:#F2FBF9;"><path d="M 1.97 5.91 L 1.97 68.41 L 598.03 68.41 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 74.31)"><foreignObject color="#FFFFFF" height="9.61" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Definition 4: Motif</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 13.78)"><foreignObject color="#000000" height="42.82" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:3.09em;--fo_depth :0em;" transform="matrix(1 0 0 -1 0 42.82)" width="556.69"><span style="width:40.23em;">Motifs are repeated patterns within a network, encompassing either features or circuits that emerge across different models and tasks.</span></foreignObject></g></g></svg>

##### Universality Hypothesis.

Following the evidence for motifs, we can propose two versions for a [universality](https://arxiv.org/html/2404.14082v3#glo.main.universality) hypothesis regarding the convergence of features and circuits across neural network models:

<svg height="178.23" id="S3.SS3.SSS0.Px1.pic1" overflow="visible" version="1.1" viewBox="0 0 600 178.23" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,178.23) matrix(1 0 0 -1 0 0)"><g fill="#FEAE03" fill-opacity="1.0" style="--ltx-fill-color:#FEAE03;"><path d="M 0 5.91 L 0 172.33 C 0 175.59 2.64 178.23 5.91 178.23 L 594.09 178.23 C 597.36 178.23 600 175.59 600 172.33 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#FFFBF2" fill-opacity="1.0" style="--ltx-fill-color:#FFFBF2;"><path d="M 1.97 5.91 L 1.97 154.12 L 598.03 154.12 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 162.72)"><foreignObject color="#FFFFFF" height="12.3" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Hypothesis 3: Weak Universality</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 16.47)"><foreignObject color="#000000" height="128.53" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:9.09em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 125.84)" width="556.69"><span style="width:40.23em;">There are underlying principles governing how neural networks learn to solve certain tasks. Models will generally converge on analogous solutions that adhere to the common underlying principles. However, the specific <a href="https://arxiv.org/html/2404.14082v3#glo.main.features"><span href="https://arxiv.org/html/2404.14082v3#glo.main.features" title="The fundamental units of how neural networks encode knowledge, which cannot be further decomposed into smaller, distinct . Features are core components of a neural network’s representation, analogous to how cells form the fundamental unit of biological organisms (, ). The  hypothesis suggests an alternative definition: that features correspond to the  concepts that a larger, sparser network with sufficient capacity would learn to represent with individual () neurons (, ).">features</span></a> and <a href="https://arxiv.org/html/2404.14082v3#glo.main.circuits"><span href="https://arxiv.org/html/2404.14082v3#glo.main.circuits" title="Sub-graphs within neural networks consisting of  and the weights connecting them. Circuits can be thought of as computational primitives that perform understandable operations to produce (ideally interpretable) features from prior (ideally interpretable) features. Examples include circuits for detecting curves at specific orientations (, ), continuing repeated patterns in text (, ), and resolving anaphoric references (, ). While circuits can involve clearly interpretable features, the definition allows for intermediate representations that are less easily interpretable.">circuits</span></a> that implement these principles can vary across different models based on factors like hyperparameters, random seeds, and architectural choices.</span></foreignObject></g></g></svg><svg height="128.42" id="S3.SS3.SSS0.Px1.pic2" overflow="visible" version="1.1" viewBox="0 0 600 128.42" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,128.42) matrix(1 0 0 -1 0 0)"><g fill="#FEAE03" fill-opacity="1.0" style="--ltx-fill-color:#FEAE03;"><path d="M 0 5.91 L 0 122.51 C 0 125.77 2.64 128.42 5.91 128.42 L 594.09 128.42 C 597.36 128.42 600 125.77 600 122.51 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#FFFBF2" fill-opacity="1.0" style="--ltx-fill-color:#FFFBF2;"><path d="M 1.97 5.91 L 1.97 104.31 L 598.03 104.31 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 112.9)"><foreignObject color="#FFFFFF" height="12.3" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Hypothesis 4: Strong Universality</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 16.47)"><foreignObject color="#000000" height="78.72" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:5.49em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 76.03)" width="556.69"><span style="width:40.23em;">The same core features and circuits will universally and consistently arise across all neural network models trained on similar tasks and data distributions and using similar techniques, reflecting a set of fundamental computational <a href="https://arxiv.org/html/2404.14082v3#glo.main.motifs"><span href="https://arxiv.org/html/2404.14082v3#glo.main.motifs" title="Repeating patterns that emerge across models and tasks, manifesting as circuits, features, or higher-level behaviors from component interactions. Examples include curve detectors, induction circuits, and branch specialization. Motifs reveal common structures and mechanisms underlying neural network intelligence.">motifs</span></a> that neural networks inherently gravitate towards when learning.</span></foreignObject></g></g></svg>

The universality hypothesis posits a convergence in forming features and circuits across various models and tasks, which could significantly ease interpretability efforts in AI. It proposes that artificial and biological neural networks share similar features and circuits, suggesting a standard underlying structure [^49] [^328] [^174]. This idea posits that there is a fundamental basis in how neural networks, irrespective of their specific configurations, process and comprehend information. This could be due to inbuilt inductive biases in neural networks or [natural abstractions](https://arxiv.org/html/2404.14082v3#glo.main.natural%20abstractions) [^49] – concepts favored by the natural world that any cognitive system would naturally gravitate towards.

Evidence for this hypothesis comes from cross-species neural structures in neuroscience, where similar neural structures and functions are found in different species [^172]. Additionally, machine learning models, including neural networks, tend to converge on similar features, representations, and classifications across different tasks and architectures [^52] [^121] [^192] [^30]. [^210] provide mathematical support for emerging universal features.

While various studies support the universality hypothesis, questions remain about the extent of feature and circuit similarity across different models and tasks. In the context of mechanistic interpretability, this hypothesis has been investigated for neurons [^119], group composition circuits [^56], and modular task processing [^343], with evidence for the weak but not the strong formulation [^56].

### 3.4 Emergence of World Models and Simulated Agents

##### Internal World Models.

World models are internal causal models of an environment formed within neural networks. Traditionally linked with reinforcement learning, these models are *explicitly* trained to develop a compressed spatial and temporal representation of the training environment, enhancing downstream task performance and sample efficiency through training on internal hallucinations [^120]. However, in the context of our survey, our focus shifts to [internal world models](https://arxiv.org/html/2404.14082v3#glo.main.internal%20world%20models) that potentially form *implicitly* as a by-product of the training process, especially in LLMs trained on next-token prediction – also called GPT.

LLMs are sometimes characterized as *stochastic parrots* [^16]. This label stems from their fundamental operational mechanism of predicting the next word in a sequence, which is seen as relying heavily on memorization. From this viewpoint, LLMs are thought to form complex correlations based on observational data but cannot develop causal models of the world due to their lack of access to interventional data [^275].

An alternative perspective on LLMs comes from the active inference framework [^304], a theory rooted in cognitive science and neuroscience. Active inference postulates that the objective of minimizing prediction error, given enough representative capacity, is adequate for a learning system to develop complex world representations, behaviors, and abstractions. Since language inherently mirrors the world, these models could implicitly construct linguistic and broader world models [^178].

The [simulation](https://arxiv.org/html/2404.14082v3#glo.main.simulation) hypothesis suggests that models designed for prediction, such as LLMs, will eventually simulate the causal processes underlying data creation. Seen as an extension of their drive for efficient compression, this hypothesis implies that adequately trained models like GPT could develop [internal world models](https://arxiv.org/html/2404.14082v3#glo.main.internal%20world%20models) as a natural outcome of their predictive training [^159] [^311].

<svg height="95.98" id="S3.SS4.SSS0.Px1.pic1" overflow="visible" version="1.1" viewBox="0 0 600 95.98" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,95.98) matrix(1 0 0 -1 0 0)"><g fill="#FEAE03" fill-opacity="1.0" style="--ltx-fill-color:#FEAE03;"><path d="M 0 5.91 L 0 90.07 C 0 93.33 2.64 95.98 5.91 95.98 L 594.09 95.98 C 597.36 95.98 600 93.33 600 90.07 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#FFFBF2" fill-opacity="1.0" style="--ltx-fill-color:#FFFBF2;"><path d="M 1.97 5.91 L 1.97 71.87 L 598.03 71.87 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 80.46)"><foreignObject color="#FFFFFF" height="12.3" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Hypothesis 5: Simulation</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 17.24)"><foreignObject color="#000000" height="46.28" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:3.09em;--fo_depth :0.25em;" transform="matrix(1 0 0 -1 0 42.82)" width="556.69"><span style="width:40.23em;">A model whose objective is text prediction will simulate the causal processes underlying the text creation if optimized sufficiently strongly <sup id="fnref:159-2"><a href="#fn:159">159</a></sup>.</span></foreignObject></g></g></svg>

In addition to theoretical considerations for emergent causal world models [^292] [^253], mechanistic interpretability is starting to provide empirical evidence on the types of internal world models that may emerge in LLMs. The ability to internally represent the board state in games like chess [^168] or Othello [^190] [^248], create linear abstractions of spatial and temporal data [^117], and structure complex representations of mazes, demonstrating an understanding of maze topology and pathways [^155] highlight the growing abstraction capabilities of LLMs. [^189] identified contextual word representations that function as models of entities and situations evolving throughout a discourse, akin to linguistic models of dynamic semantics. [^274] demonstrated that LLMs can map conceptual domains (e.g., direction, color) to grounded world representations given a few examples, suggesting they learn rich conceptual spaces [^96] reflective of the non-linguistic world.

The [prediction orthogonality](https://arxiv.org/html/2404.14082v3#glo.main.prediction%20orthogonality) hypothesis further expands on this idea: It posits that prediction-focused models like GPT may simulate agents with various objectives and levels of optimality. In this context, GPT are simulators, simulating entities known as [simulacra](https://arxiv.org/html/2404.14082v3#glo.main.simulacra) that can be either agentic or non-agentic, with different objectives from the simulator itself [^159] [^311]. The implications of the simulation and prediction orthogonality hypotheses for AI safety and alignment are discussed in Section 6.

<svg height="95.98" id="S3.SS4.SSS0.Px1.pic2" overflow="visible" version="1.1" viewBox="0 0 600 95.98" width="600"><g fill="#000000" stroke="#000000" stroke-width="0.4pt" style="--ltx-stroke-color:#000000;--ltx-fill-color:#000000;" transform="translate(0,95.98) matrix(1 0 0 -1 0 0)"><g fill="#FEAE03" fill-opacity="1.0" style="--ltx-fill-color:#FEAE03;"><path d="M 0 5.91 L 0 90.07 C 0 93.33 2.64 95.98 5.91 95.98 L 594.09 95.98 C 597.36 95.98 600 93.33 600 90.07 L 600 5.91 C 600 2.64 597.36 0 594.09 0 L 5.91 0 C 2.64 0 0 2.64 0 5.91 Z" style="stroke:none"></path></g><g fill="#FFFBF2" fill-opacity="1.0" style="--ltx-fill-color:#FFFBF2;"><path d="M 1.97 5.91 L 1.97 71.87 L 598.03 71.87 L 598.03 5.91 C 598.03 3.73 596.27 1.97 594.09 1.97 L 5.91 1.97 C 3.73 1.97 1.97 3.73 1.97 5.91 Z" style="stroke:none"></path></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 80.46)"><foreignObject color="#FFFFFF" height="12.3" overflow="visible" style="--ltx-fg-color:#FFFFFF;--fo_width :40.23em;--fo_height:0.69em;--fo_depth :0.19em;" transform="matrix(1 0 0 -1 0 9.61)" width="556.69"><span style="width:40.23em;">Hypothesis 6: Prediction Orthogonality</span> </foreignObject></g><g fill-opacity="1.0" transform="matrix(1.0 0.0 0.0 1.0 21.65 17.24)"><foreignObject color="#000000" height="46.28" overflow="visible" style="--ltx-fg-color:#000000;--fo_width :40.23em;--fo_height:3.09em;--fo_depth :0.25em;" transform="matrix(1 0 0 -1 0 42.82)" width="556.69"><span style="width:40.23em;">A model whose objective is prediction can simulate agents who optimize toward any objectives with any degree of optimality <sup id="fnref:159-4"><a href="#fn:159">159</a></sup>.</span></foreignObject></g></g></svg>

In conclusion, the evolution of LLMs from simple predictive models to entities potentially possessing complex [internal world models](https://arxiv.org/html/2404.14082v3#glo.main.internal%20world%20models), as suggested by the [simulation](https://arxiv.org/html/2404.14082v3#glo.main.simulation) hypothesis and supported by mechanistic interpretability studies, represents a significant shift in our understanding of these systems. This evolution challenges us to reconsider LLMs’ capabilities and future trajectories in the broader landscape of AI development.

## 4 Core Methods

Mechanistic interpretability (MI) employs various tools, from observational analysis to causal interventions. This section provides a comprehensive overview of these methods, beginning with a taxonomy that categorizes approaches based on their key characteristics (Section 4.1). We then survey observational (Section 4.2), followed by interventional techniques (Section 4.3). Finally, we study their synergistic interplay (Section 4.4). Figure 7 offers a visual summary of the methods and techniques unique to mechanistic interpretability.

![Refer to caption](https://arxiv.org/html/2404.14082v3/x7.png)

Figure 7: Overview of key methods and techniques in mechanistic interpretability research. Observational approaches include structured probes, logit lens variants, and sparse autoencoders (SAEs). Interventional methods, focusing on causal understanding, encompass activation patching variants for uncovering causal mechanisms and causal scrubbing for hypothesis evaluation.

### 4.1 Taxonomy of Mechanistic Interpretability Methods

We propose a taxonomy based on four key dimensions: causal nature, learning phase, locality, and comprehensiveness (Table 1).

The causal nature of methods ranges from purely observational, which analyze existing representations without direct manipulation, to interventional approaches that actively perturb model components to establish causal relationships. The learning phase dimension distinguishes between post-hoc techniques applied to trained models and intrinsic methods that enhance interpretability during the training process itself.

Locality refers to the scope of analysis, spanning from individual neurons (e.g., feature visualization) to entire model architectures (e.g., causal abstraction). Comprehensiveness varies from partial insights into specific components to holistic explanations of model behavior.

Table 1: Taxonomy of Mechanistic Interpretability Methods

| Method | Causal Nature | Phase | Locality | Comprehensiveness | Key Examples |
| --- | --- | --- | --- | --- | --- |
| Feature Visualization | Observation | Post-hoc | Local | Partial | [^368] |
|  |  |  |  |  | [^374] |
| Exemplar methods | Observation | Post-hoc | Local | Partial | [^114] |
|  |  |  |  |  | [^95] |
| Probing Techniques | Observation | Post-hoc | Both | Both | [^217] |
|  |  |  |  |  | [^118] |
| Structured Probes | Observation | Post-hoc | Both | Both | [^33] |
| Logit Lens Variants | Observation | Post-hoc | Global | Partial | [^255] |
|  |  |  |  |  | [^15] |
| Sparse Autoencoders | Observation | Post-hoc | Both | Comprehensive | [^62] |
|  |  |  |  |  | [^30] |
| Activation Patching | Intervention | Post-hoc | Local | Partial | [^219] |
|  |  |  |  |  | [^355] |
| Path Patching | Intervention | Post-hoc | Both | Both | [^109] |
| Causal Abstraction | Intervention | Post-hoc | Global | Comprehensive | [^101] |
|  |  |  |  |  | [^102] |
|  |  |  |  |  | [^364] |
| Hypothesis Testing | Intervention | Post-hoc | Global | Comprehensive | [^48] |
|  |  |  |  |  | [^160] |
| Intrinsic Methods | – | Pre/During | Global | Comprehensive | [^76] |
|  |  |  |  |  | [^199] |

The categorization is based on the methods’ general tendencies. Some methods can offer local and global or partial and comprehensive interpretability depending on the scope of the analysis and application. Probing techniques can range from local to global and partial to comprehensive; simple linear probes might offer local insights into individual [features](https://arxiv.org/html/2404.14082v3#glo.main.features), while more sophisticated structured probes can uncover global patterns. Sparse autoencoders decompose individual neuron activations (local) but aim to disentangle features across the entire model (global). Path patching extends local interventions to global model understanding by tracing information flow across layers, demonstrating how local perturbations can yield broader insights.

In practice, mechanistic interpretability research involves both method development and their application. When applying methods to understand a model, combining techniques from multiple categories is often necessary and beneficial to build a more comprehensive understanding (Section 4.4).

### 4.2 Observation

Mechanistic interpretability draws from observational methods that analyze the inner workings of neural networks, with many of these methods preceding the field itself. For a detailed exploration of inner interpretability methods, refer to [^287]. Two prominent categories are example-based methods and feature-based methods:

##### Probing for Features.

Probing [^2] [^137] involves training a classifier using the activations of a model, with the classifier’s performance subsequently observed to deduce insights about the model’s behavior and internal representations. However, the probe’s performance may often reflect its own learning capacities more than the actual characteristics of the model’s representations [^14]. This dilemma has led researchers to investigate the ideal balance between the complexity of a probe and its capacity to accurately represent the model’s features [^38] [^349].

The [linear representation](https://arxiv.org/html/2404.14082v3#glo.main.linear%20representation) hypothesis offers a resolution to this issue. Under this hypothesis, the failure of a simple linear probe to detect certain features suggests their absence in the model’s representations. Conversely, suppose a more complex probe succeeds where a simpler one fails. In that case, it implies that the model contains features that a complex function can combine into the target feature but that the target feature itself is not explicitly represented. Thus, the hypothesis implies that using linear probes could suffice in most cases, circumventing the complexity considerations generally associated with probing [^14].

[^217] analyzed chess knowledge acquisition in AlphaZero, revealing the emergence of strategic concepts during training. In language models, [^118] introduced *sparse probing* to decode internal neuron activations to understand feature representation and sparsity. They show that early layers use sparse combinations of neurons to represent many features in superposition, while middle layers seem to have dedicated [monosemantic](https://arxiv.org/html/2404.14082v3#glo.main.monosemantic) neurons for higher-level contextual features.

Probing is limited in drawing causal or behavioral conclusions. Its primarily observational nature focuses on how information is encoded rather than how it is used (see Figure 1), necessitating careful analysis and integration with interventional techniques (Section 4.3), or alternative approaches [^75]. While in explainable AI, probing has primarily analyzed high-level concepts like linguistic representations [^335] [^65], MI aims to probe towards uncovering underlying computational processes and functionality. This shift in goals towards uncovering mechanistic computation is a nuanced distinction rather than a clear-cut line between probing in MI and the broader explainability field.

##### Structured Probes.

While focusing on bottom-up, mechanistic interpretability approaches, we can also consider integrating top-down, concept-based structured probes with mechanistic interpretability.

Structured probes aid conceptual interpretability, probing language models for complex features like truth representations. Notably, [^33] ’s contrast-consistent search identifies linear projections exhibiting logical consistency in hidden states, contrasting truth values for statements and negations.

However, structured probes face significant challenges in unsupervised probing scenarios. As [^81] showed, arbitrary features, not just knowledge-related ones, can satisfy contrast consistency equally well, raising doubts about scalability. For example, the loss may capture [simulation](https://arxiv.org/html/2404.14082v3#glo.main.simulation) of knowledge from hypothesized [simulacra](https://arxiv.org/html/2404.14082v3#glo.main.simulacra) within sufficiently powerful language models rather than the models’ true knowledge. Furthermore, [^81] demonstrates self-supervised probing methods (like [^33]) often detect prominent but unintended distractor features in the data. The discovered features are also highly sensitive to prompt choice, and there is no principled way to select prompts that would reliably surface a model’s true knowledge.

While structured probes primarily focus on high-level conceptual representations [^375], their findings could potentially inform or complement mechanistic interpretability efforts. For instance, identifying truth directions through structured probes could help guide targeted interventions or analyze the underlying circuits responsible for truthful behavior using mechanistic techniques such as activation patching or circuit tracing (Section 4.3). Conversely, mechanistic methods could provide insights into how truth representations emerge and are computed within the model, addressing some of the challenges faced by unsupervised structured probes.

##### Logit Lens.

The logit lens [^255] provides a window into the model’s predictive process by applying the final classification layer (which projects the residual stream activation into logits/vocabulary space) to intermediate activations of the residual stream, revealing how prediction confidence evolves across computational stages. This is possible because transformers tend to build their predictions across layers iteratively [^104]. Extensions of this approach include the tuned lens [^15], which trains affine probes to decode hidden states into probability distributions over the vocabulary, and the Future Lens [^268], which explores the extent to which individual hidden states encode information about subsequent tokens.

Researchers have also investigated techniques that bypass intermediate computations to probe representations directly. [^70] propose using linear transformations to approximate hidden states from different layers, revealing that language models often predict final outputs in early layers. [^66] present a theoretical framework for interpreting transformer parameters by projecting them into the embedding space, enabling model alignment and parameter transfer across architectures.

Other techniques focus on interpreting specific model components or submodules. The DecoderLens [^183] allows analyzing encoder-decoder transformers by cross-attending intermediate encoder representations in the decoder, shedding light on the information flow within the encoder. The Attention Lens [^302] aims to elucidate the specialized roles of attention heads by translating their outputs into vocabulary tokens via learned transformations.

##### Feature Disentanglement via Sparse Dictionary Learning.

Recent work suggests that the essential elements in neural networks are linear combinations of neurons representing features in superposition [^77]. To disentangle these features, researchers have developed sparse autoencoders (SAEs), which decompose neural network activations into individual component features [^316] [^62]. This process, known as sparse dictionary learning, reconstructs activation vectors as sparse linear combinations of directional vectors within the activation space [^262].

The theoretical foundations of SAEs are rooted in work on [disentangled](https://arxiv.org/html/2404.14082v3#glo.main.disentangled) representations. [^361] demonstrate that autoencoders can recover ground truth features under conditions of feature sparsity and non-negativity. Furthermore, [^97] provides guarantees for the uniqueness and stability of dictionaries for sparse representation, even in the presence of noise. These theoretical underpinnings support SAEs’ ability to uncover true, disentangled features underlying the data distribution.

In practice, SAEs stand out for their simplicity and scalability [^316]. They incorporate sparsity regularization to encourage learning sparse yet meaningful data representations, with the precise tuning of the sparsity penalty on hidden activations critical in dictating the autoencoder’s sparsity level. We provide an overview of the SAE architecture in Figure 8.

SAEs’ dictionary features exhibit higher scores on autointerpretability metrics and increased monosemanticity [^30] [^62] [^316]. They are scalable to state-of-the-art models and can detect safety-relevant features [^334], measure feature sparsity [^68], and interpret reward models in reinforcement learning-based language models [^211].

Evaluating SAE quality remains challenging due to the lack of ground-truth interpretable features. Researchers have addressed this through various approaches: [^169] proposed using language models trained on chess and Othello transcripts as testbeds, providing natural collections of interpretable features. [^316] constructed a toy model with traceable features, while [^207] [^206] compared SAE results with supervised features in large language models to demonstrate their viability.

The versatility of SAEs extends to various neural network architectures. They have been successfully applied to transformer attention layers [^173] and convolutional neural networks [^110]. Notably, [^110] applied SAEs to the early vision layers of InceptionV1, uncovering new interpretable features, including additional curve detectors not apparent from examining individual neurons [^36].

In circuit discovery, SAEs have shown particular promise (see also Section 4.4). [^126] proposed a circuit discovery framework alternative to activation patching (discussed in Section 4.3.1), leveraging dictionary features decomposed from all modules writing to the residual stream. Similarly, [^265] employed discrete sparse autoencoders for discovering interpretable circuits in large language models.

Recent advancements have focused on improving SAE performance and addressing limitations. [^286] introduced a gating mechanism to separate the functionalities of determining which directions to use and estimating their magnitudes, mitigating shrinkage – the systematic underestimation of feature activations. An alternative approach by [^73] uses transcoders to faithfully approximate a densely activating MLP layer with a wider, sparsely-activating MLP layer, offering another path to interpretable feature discovery, a type of sparse distillation [^321].

![[Uncaptioned image]](https://arxiv.org/html/2404.14082v3/x8.png)

polysemantic

### 4.3 Intervention

##### Causality as a Theoretical Foundation.

The theory of causality [^275] provides a mathematically precise framework for mechanistic interpretability, offering a rigorous approach to understanding high-level semantics in neural representations [^101]. By treating neural networks as causal models, with their compute graphs serving as causal graphs, researchers can perform precise interventions and examine the roles of individual parameters [^232]. This causal perspective on interpretability has led to the development of various intervention techniques, including activation patching (Section 4.3.1), causal abstraction (Section 4.3.2), and hypothesis testing methods (Section 4.3.3).

#### 4.3.1 Activation Patching

![Refer to caption](https://arxiv.org/html/2404.14082v3/x9.png)

(a)

Activation patching is a collective term for a set of causal intervention techniques that manipulate neural network activations to shed light on the decision-making processes within the model. These techniques, including causal tracing [^219], interchange intervention [^100], causal mediation analysis [^347], and causal ablation [^355], share the common goal of modifying a neural model’s internal state by replacing specific activations with alternative values, such as zeros, mean activations across samples, random noise, or activations from a different forward pass (Figure 9a).

The primary objective of activation patching is to isolate and understand the role of specific components or circuits within the model by observing how changes in activations affect the model’s output. This enables researchers to infer the function and importance of those components. Key applications include localizing behavior by identifying critical activations, such as understanding the storage and processing of factual information [^219] [^105] [^109] [^327], and analyzing component interactions through circuit analysis to identify sub-networks within a model’s computation graph that implement specified behaviors [^355] [^122] [^193] [^128] [^105].

The standard protocol for activation patching (Figure 9a) involves:

1. Running the model with a clean input and caching the latent activations;
2. Executing the model with a corrupted input;
3. Re-running the model with the corrupted input but substituting specific activations with those from the clean cache; and
4. Determining significance by observing the variations in the model’s output during the third step, thereby highlighting the importance of the replaced components.

This process relies on comparing pairs of inputs: a clean input, which triggers the desired behavior, and a corrupted input, which is identical to the clean one except for critical differences that prevent the behavior. By carefully selecting these inputs, researchers can control for confounding circuitry and isolate the specific circuit responsible for the behavior.

Differences in patching direction – clean to corrupted (causal tracing) versus corrupted to clean (resample ablation) – provide insights into the sufficiency or necessity of model components for a given behavior. Clean to corrupted patching identifies activations sufficient for restoring clean performance, even if they are unnecessary due to redundancy, which is particularly informative in OR logic scenarios (Figure 9b, OR gate). Conversely, corrupted to clean patching determines the necessary activations for clean performance, which is useful in AND logic scenarios (Figure 9b, AND gate).

Activation patching can employ corruption methods, including zero-, mean-, random-, or resample ablation, each modulating the model’s internal state in distinct ways. Resample ablation stands out for its effectiveness in maintaining consistent model behavior by not changing the data distribution too much [^371]. However, it is essential to be careful when interpreting the patching results, as breaking behavior by taking the model off-distribution is uninteresting for finding the relevant circuit [^244].

##### Path Patching and Subspace Activation Patching.

Path patching extends the activation patching approach to multiple edges in the computational graph [^355] [^109], allowing for a more fine-grained analysis of component interactions. For example, path patching can be used to estimate the direct and indirect effects of attention heads on the output logits. Subspace activation patching, also known as distributed interchange interventions [^102], aims to intervene only on linear subspaces of the representation space where [features](https://arxiv.org/html/2404.14082v3#glo.main.features) are hypothesized to be encoded, providing a tool for more targeted interventions.

Recently, [^106] introduced patchscopes, a framework that unifies and extends activation patching techniques: using the model’s text generation to explain internal representations, it enables more flexible interventions across various interpretability tasks, improving early layer inspection and allowing for cross-model analysis.

##### Limitations and Advancements.

Activation patching has several limitations, including the effort required to design input templates and counterfactual datasets, the need for human inspection to isolate important subgraphs, and potential second-order effects that can complicate the interpretation of results [^182] and the [hydra effect](https://arxiv.org/html/2404.14082v3#glo.main.hydra%20effect) [^218] [^297] (see discussion in Section 7.2). Recent advancements aim to address these limitations, such as automated circuit discovery algorithms [^60], gradient-based methods for scalable component importance estimation like attribution patching [^243] [^331], and techniques to mitigate self-repair interference during analysis [^84].

#### 4.3.2 Causal Abstraction

Causal abstraction [^99] [^101] provides a mathematical framework for mechanistic interpretability, treating neural networks and their explanations as causal models. This approach validates explanations through interchange interventions on network activations [^160], unifying various interpretability methods such as LIME [^290], causal effect estimation [^82], causal mediation analysis [^347], iterated nullspace projection [^288], and circuit-based explanations [^101].

To overcome computational limitations, distributed alignment search [^102] introduced gradient-based distributed interchange interventions, extending causal abstraction to larger models [^365]. Further advancements include causal proxy models [^364], which address the challenge of counterfactual observations.

Applications of causal abstraction span from linguistic phenomena analysis [^5] [^363], and evaluation of interpretability methods [^146], to improving performance through representation finetuning [^366], and improving efficiency via model distillation [^363].

#### 4.3.3 Hypothesis Testing

In addition to the causal abstraction framework, several methods have been developed for rigorous hypothesis testing about neural network behavior. These methods aim to formalize and empirically validate explanations of how neural networks implement specific behaviors.

Causal scrubbing [^48] formalizes hypotheses as a tuple $({\mathcal{G}},{\mathcal{I}},c)$, where ${\mathcal{G}}$ is the model’s computational graph, ${\mathcal{I}}$ is an interpretable computational graph hypothesized to explain the behavior, and $c$ maps nodes of ${\mathcal{I}}$ to nodes of ${\mathcal{G}}$. This method replaces activations in ${\mathcal{G}}$ with others that should be equivalent according to the hypothesis, measuring performance on the scrubbed model to validate the hypothesis.

Locally consistent abstractions [^160] offer a more permissive approach, checking the consistency between the neural network and the explanation only one step away from the intervention node. This method forms a middle ground between the strictness of full causal abstraction and the flexibility of causal scrubbing.

These methods form a hierarchy of strictness, with full causal abstractions being the most stringent, followed by locally consistent abstractions and causal scrubbing being the most permissive. This hierarchy highlights trade-offs in choosing stricter or more permissive notions, affecting the ability to find acceptable explanations, generalization, and mechanistic anomaly detection.

### 4.4 Integrating Observation and Intervention.

To comprehensively understand internal neural network mechanisms, combining observational and interventional methods is crucial. For instance, sparse autoencoders can be used to disentangle superposed features [^62], followed by targeted activation patching to test the causal importance of these features [^355]. Similarly, the logit lens can track prediction formation across layers [^255], with subsequent interventions confirming causal relationships at key points. Probing techniques can identify encoded information [^14], which can then be subjected to causal abstraction [^101] to understand how this information is utilized. This iterative refinement process, where broad observational methods guide targeted interventions and intervention results inform further observations, enables a multi-level analysis that builds a holistic understanding across different levels of abstraction. Recent work [^213] [^34] [^29] [^265] [^98] demonstrates the potential of integrating sparse autoencoders with automated circuits discovery [^60] [^331], combining feature-level analysis with circuit-level interventions to uncover the interplay between representation and mechanism.

## 5 Current Research

This section surveys current research in mechanistic interpretability across three approaches based on when and how the model is interpreted during training: Intrinsic interpretability methods are applied before training to enhance the model’s inherent interpretability (Section 5.1). Developmental interpretability involves studying the model’s learning dynamics and the emergence of internal structures during training (Section 5.2). After training, post-hoc interpretability techniques are applied to gain insights into the model’s behavior and decision-making processes (Section 5.3), including efforts towards uncovering general, transferable principles across models and tasks, as well as automating the discovery and interpretation of critical circuits in trained models (Section 5.4).

![Refer to caption](https://arxiv.org/html/2404.14082v3/x11.png)

Figure 10: Key desiderata for interpretability approaches across training and analysis stages: (1) Intrinsic: Architectural biases for sparsity, modularity, and disentangled representations. (2) Developmental: Predictive capability for phase transitions, manageable number of critical transitions, and a unifying theory connecting observations to singularity geometry. (3) Post-hoc: Global, comprehensive, automated discovery of critical circuits, uncovering transferable principles across models/tasks, and extracting high-level causal mechanisms.

### 5.1 Intrinsic Interpretability

Intrinsic methods for mechanistic interpretability offer a promising approach to designing neural networks that are more amenable to [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering) without sacrificing performance. By encouraging sparsity, [modularity](https://arxiv.org/html/2404.14082v3#glo.main.modularity), and monosemanticity through architectural choices and training procedures, these methods aim to make the reverse engineering process more tractable.

Intrinsic interpretability methods aim to constrain the training process to make learned programs more interpretable [^90]. This approach is closely related to neurosymbolic learning [^293] and can involve techniques like regularization with spatial structure, akin to the organization of information in the human brain [^199] [^200].

Recent work has explored various architectural choices and training procedures to improve the interpretability of neural networks. [^163] and [^76] demonstrate that architectural choices can affect monosemanticity, suggesting that models could be engineered to be more [monosemantic](https://arxiv.org/html/2404.14082v3#glo.main.monosemantic). [^314] propose using a bilinear layer instead of a linear layer to encourage monosemanticity in language models.

[^199] and [^200] introduce a biologically inspired spatial regularization regime called brain-inspired modular training for forming modules in networks during training. They showcase how this can help RNNs exhibit brain-like anatomical modularity without degrading performance, in contrast to naive attempts to use sparsity to reduce the cost of having more neurons per layer [^163] [^30].

Preceding the mechanistic interpretability literature, various works have explored techniques to improve interpretability, such as sparse attention [^369], adding $L^{1}$ penalties to neuron activations [^170] [^103], and pruning neurons [^88]. These techniques have been shown to encourage sparsity, modularity, and disentanglement, which are essential aspects of intrinsic interpretability.

### 5.2 Developmental Interpretability

Developmental interpretability examines the learning dynamics and emergence of internal structures in neural networks over time, focusing on the formation of [features](https://arxiv.org/html/2404.14082v3#glo.main.features) and [circuits](https://arxiv.org/html/2404.14082v3#glo.main.circuits). This approach complements static analyses by investigating critical phase transitions corresponding to significant changes in model behavior or capabilities [^326] [^306] [^359] [^320]. While primarily a distinct field, developmental interpretability often intersects with mechanistic interpretability, as exemplified by [^263] ’s work. Their research, rooted in mechanistic interpretability, demonstrated how the emergence of in-context learning relates to specific training phase transitions, connecting microscopic changes (induction heads) with macroscopic observables (training loss).

A key motivation for developmental interpretability is investigating the [universality](https://arxiv.org/html/2404.14082v3#glo.main.universality) of safety-critical patterns, aiming to understand how deeply ingrained and thereby resistant to safety fine-tuning capabilities like deception are. In addition, researchers hypothesize that emergent capabilities correspond to sudden circuit formation during training [^223], potentially allowing for prediction or control of their development.

Singular Learning Theory (SLT), developed by Watanabe [^357] [^358], provides a rigorous framework for understanding overparameterized models’ behavior and generalization. By quantifying model complexity through the local learning coefficient, SLT offers insights into learning phase transitions and the emergence of structure in the model [^184]. Recent work by [^144] applied this coefficient to identify developmental stages in transformer models, while [^93] and [^53] advanced SLT’s scalability and application to the toy model of [superposition](https://arxiv.org/html/2404.14082v3#glo.main.superposition) (Figure 4), respectively.

While direct applications to phenomena such as generalization [^370], learning functions with increasing complexity [^234], and the transition from memorization to generalization ([grokking](https://arxiv.org/html/2404.14082v3#glo.main.grokking)) [^197] [^281] [^198] [^247] [^344] [^336] [^221] [^201] [^325] [^354] are limited, these areas, along with neural scaling laws [^35] [^196] [^223] (which can be connected to mechanistic insights [^135]), represent promising future research directions.

In conclusion, developmental interpretability serves as an evolutionary theory lens for neural networks, offering insights into the emergence of structures and behaviors over time [^305]. Drawing parallels from systems biology [^3], this approach can apply concepts like network [motifs](https://arxiv.org/html/2404.14082v3#glo.main.motifs), robustness, and [modularity](https://arxiv.org/html/2404.14082v3#glo.main.modularity) to neural network development, explaining how functional capabilities arise. Sometimes, understanding how structures came about is easier than analyzing the final product, similar to how biologists find certain features in organisms easier to explain in light of their evolutionary history. By studying the temporal aspects of neural network training, researchers can potentially uncover fundamental principles of learning and representation that may not be apparent from examining static, trained models alone.

### 5.3 Post-Hoc Interpretability

In applied mechanistic interpretability, researchers explore various facets and methodologies to uncover the inner workings of AI models. Some key distinctions are drawn between global versus local interpretability and comprehensive versus partial interpretability. Global interpretability aims to uncover general patterns and behaviors of a model, providing insights that apply broadly across many instances [^71] [^244]. In contrast, local interpretability explains the reasons behind a model’s decisions for particular instances, offering insights into individual predictions or behaviors. Comprehensive interpretability involves achieving a deep and exhaustive understanding of a model’s behavior, providing a holistic view of its inner workings [^244]. In contrast, partial interpretability often applied to larger and more complex models, concentrates on interpreting specific aspects or subsets of the model’s behavior, focusing on the application’s most relevant or critical areas.

##### Large Models – Narrow Behavior.

Circuit-style mechanistic interpretability aims to explain neural networks by [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering) the underlying mechanisms at the level of individual neurons or subgraphs. This approach assumes that neural vector representations encode high-level concepts and circuits defined by model weights encode meaningful algorithms [^260] [^36]. Studies on deep networks support these claims, identifying circuits responsible for detecting curved lines or object orientation [^36] [^37] [^353].

This paradigm has been applied to language models to discover subnetworks (circuits) responsible for specific capabilities. Circuit analysis localizes and understands subgraphs within a model’s computational graph responsible for specific behaviors. For large language models, this often involves narrow investigations into behaviors like multiple choice reasoning [^193], indirect object identification [^355], or computing operations [^122]. Other examples include analyzing circuits for Python docstrings [^127], "an" vs "a" usage [^226], and price tagging [^365]. Case studies often construct datasets using templates filled by placeholder values to enable precise control for causal interventions [^355] [^122] [^365].

##### Toy Models – Comprehensive Analysis.

Small models trained on specialized mathematical or algorithmic tasks enable more comprehensive reverse engineering of learned algorithms [^247] [^373] [^56]. Even simple arithmetic operations can involve complex strategies and multiple algorithmic solutions [^247] [^373]. Characterizing these algorithms helps test hypotheses around generalizable mechanisms like variable binding [^83] [^67] and arithmetic reasoning [^327]. The work by [^344] builds on the work that analyzes transformers trained on modular addition [^247] and explains [grokking](https://arxiv.org/html/2404.14082v3#glo.main.grokking) in terms of circuit efficiency, illustrating how a comprehensive understanding of a toy model can enable interesting analyses on top of that understanding.

##### Towards Universality.

The ultimate goal is to uncover general principles that transfer across models and tasks, such as induction heads for in-context learning [^263], variable binding mechanisms [^83] [^67], arithmetic reasoning [^327] [^31], or retrieval tasks [^343]. Despite promising results, debates surround the [universality](https://arxiv.org/html/2404.14082v3#glo.main.universality) hypothesis – the idea that different models learn similar features and circuits when trained on similar tasks. [^56] finds mixed evidence for universality in group composition, suggesting that while families of circuits and features can be characterized, precise circuits and development order may be arbitrary.

##### Towards High-level Mechanisms.

Causal interventions can extract a high-level understanding of computations and representations learned by large language models [^343] [^128] [^83] [^375]. Recent work focuses on intervening in internal representations to study high-level concepts and computations encoded. For example, [^128] patched residual stream vectors to transfer task representations, while [^83] intervened on residual streams to argue that models generate IDs to bind entities to attributes. Techniques for [representation engineering](https://arxiv.org/html/2404.14082v3#glo.main.representation%20engineering) [^375] extract reading vectors from model activations to stimulate or inhibit specific concepts. Although these interventions don’t operate via specific mechanisms, they offer a promising approach for extracting high-level causal understanding and bridging bottom-up and top-down interpretability approaches.

### 5.4 Automation: Scaling Post-Hoc Interpretability

As models become more complex, automating key aspects of the interpretability workflow becomes increasingly crucial. Tracing a model’s computational pathways is highly labor-intensive, quickly becoming infeasible as the model size increases. Automating the discovery of relevant circuits and their functional interpretation represents a pivotal step towards scalable and comprehensive model understanding [^233].

##### Dissecting Models into Interpretable Circuits.

The first major automation challenge is identifying the critical computational sub-circuits or components underpinning a model’s behavior for a given task. A pioneering line of work aims to achieve this via efficient masking or patching procedures. Methods like *automated circuit discovery* [^60] and *attribution patching* [^331] [^175] iteratively knock out model activations, pinpointing components whose removal has the most significant impact on performance. This masking approach has proven scalable even to large models [^193].

Other techniques take a more top-down approach. [^67] specify high-level causal properties (desiderata) that components solving a target subtask should satisfy and then learn binary masks to expose those component subsets. [^84] construct information flow graphs highlighting key nodes and operations by tracing attribution flows, enabling extraction of general information routing patterns across prediction domains.

Explicit architectural biases like modularity can further boost automation efficiency. [^233] find that models trained with brain-inspired modular training [^199] produce more readily identifiable circuits compared to standard training. Such domain-inspired inductive biases may prove increasingly vital as models grow more massive and monolithic.

##### Interpreting Extracted Circuits.

Once critical circuit components have been isolated, the key remaining step is interpreting what computation those components perform. Sparse autoencoders are a prominent approach for interpreting extracted circuits by decomposing neural network activations into individual component [features](https://arxiv.org/html/2404.14082v3#glo.main.features), as discussed in Section 4.2.

A novel paradigm uses large language models themselves as an interpretive tool. [^22] demonstrate generating natural language descriptions of individual neuron functions by prompting language models like GPT-4 to explain sets of inputs that activate a neuron. [^230] similarly employ language models to annotate unsupervised neuron clusters identified via hierarchical clustering. [^9] describe the roles of neurons in vision networks with multimodal models. These methods can easily leverage more capable general-purpose models in the future. [^86] take a complementary graph-based approach in their neuron-to-graph tool: automatically extracting individual neurons’ behavior patterns from training data as structured graphs amenable to visualization, programmatic comparisons, and property searches. Such representations could synergize with language model-based annotation to provide descriptions of neuron roles.

However, robustly interpreting the largest trillion-parameter models using automated techniques remains an open challenge. Another novel approach, mechanistic-interpretability-based program synthesis [^224], entirely sidesteps this complexity by auto-distilling the algorithm learned by a trained model into human-readable Python code without relying on further interpretability analyses or model architectural knowledge. As models become increasingly vast and opaque, such synergistic combinations of methods – uncovering circuits, annotating them, or altogether transcribing them into executable code – will likely prove crucial for maintaining insight and [oversight](https://arxiv.org/html/2404.14082v3#glo.main.oversight) when scaling model size.

## 6 Relevance to AI Safety

##### How Could Interpretability Promote AI Safety?

![Refer to caption](https://arxiv.org/html/2404.14082v3/x12.png)

Figure 11: Potential benefits and risks of mechanistic interpretability for AI safety.

Gaining mechanistic insights into the inner workings of AI systems seems crucial for navigating AI safety as we develop more powerful models [^239]. Interpretability tools can provide an understanding of artificial cognition, the way AI systems process information and make decisions, which offers several potential benefits:

Mechanistic interpretability could accelerate AI safety research by providing richer feedback loops and grounding for model evaluation [^41]. It may also help anticipate emergent capabilities, such as the emergence of new skills or behaviors in the model before they fully manifest [^359] [^326] [^247] [^12]. This relates to studying the incremental development of internal structures and representations as the model learns (Section 5.2). Additionally, interpretability could substantiate theoretical risk models with concrete evidence, such as demonstrating [inner misalignment](https://arxiv.org/html/2404.14082v3#glo.main.inner%20misalignment) (when a model’s behavior deviates from its intended goals) or [mesa-optimization](https://arxiv.org/html/2404.14082v3#glo.main.mesa-optimization) (the emergence of unintended subagents within the model) [^151] [^352]. It may also trigger normative shifts within the AI community toward rigorous safety protocols by revealing potential risks or concerning behaviors [^147].

Regarding specific AI risks [^133], interpretability may prevent malicious misuse by locating and erasing sensitive information stored in the model [^219] [^252]. It could reduce competitive pressures by substantiating potential threats, promoting organizational safety cultures, and supporting AI alignment (ensuring AI systems pursue intended goals) through better monitoring and evaluation [^131]. Interpretability can provide safety filters for every stage of training: before training by deliberate design [^147], during training by detecting early signs of misalignment and potentially shifting the distribution towards alignment [^150] [^313], and after training by rigorous evaluation of artificial cognition for honesty [^33] [^375] and screening for deceptive behaviors [^273].

The emergence of [internal world models](https://arxiv.org/html/2404.14082v3#glo.main.internal%20world%20models) in LLMs, as posited by the [simulation](https://arxiv.org/html/2404.14082v3#glo.main.simulation) hypothesis, could have significant implications for AI alignment research. Finding an internal representation of human values and aiming the AI system’s objective may be a trivial way to achieve alignment [^360], especially if the world model is internally separated from notions of goals and agency [^298]. In such cases, world model interpretability alone may be sufficient for alignment [^299].

Conditioning pre-trained models is considered a comparatively safe pathway towards general intelligence, as it avoids directly creating agents with inherent goals or agendas [^165] [^152]. However, prompting a model to simulate an actual agent, such as "You are a superintelligence in 2035 writing down an alignment solution," could inadvertently lead to the formation of internal agents [^152]. In contrast, reinforcement learning tends to create agents by default [^43] [^251].

The [prediction orthogonality](https://arxiv.org/html/2404.14082v3#glo.main.prediction%20orthogonality) hypothesis suggests that prediction-focused models like GPT can simulate agents with potentially misaligned objectives [^159]. Although GPT may lack genuine agency or intentionality, it may produce outputs that simulate these qualities [^20] [^311]. This underscores the need for careful [oversight](https://arxiv.org/html/2404.14082v3#glo.main.oversight) and, better yet, using mechanistic interpretability to search for internal agents or their constituents, such as optimization or search processes – an endeavor known as *searching for search* [^254] [^161].

Mechanistic interpretability integrates well into various AI alignment agendas, such as understanding existing models, controlling them, making AI systems solve alignment problems, and developing alignment theories [^332] [^149]. It could enhance strategies like detecting [deceptive alignment](https://arxiv.org/html/2404.14082v3#glo.main.deceptive%20alignment) (hypothetical when a model ensures to appear aligned as to pursue misaligned goals without raising suspicion) [^273], [eliciting latent knowledge](https://arxiv.org/html/2404.14082v3#glo.main.eliciting%20latent%20knowledge) from models [^55], and enabling better scalable [oversight](https://arxiv.org/html/2404.14082v3#glo.main.oversight), such as in [iterative distillation and amplification](https://arxiv.org/html/2404.14082v3#glo.main.iterative%20distillation%20and%20amplification) [^47]. A high degree of understanding may even allow for [well-founded AI](https://arxiv.org/html/2404.14082v3#glo.main.well-founded%20AI) approaches (AI systems with provable guarantees) [^333] or [microscope AI](https://arxiv.org/html/2404.14082v3#glo.main.microscope%20AI) (extract world knowledge from the model without letting the model take actions) [^147]. Furthermore, comprehensive interpretability itself may be an alignment strategy if we can identify internal representations of human values and guide the model to pursue those values by retargeting an internal search process [^360]. Ultimately, understanding and control are intertwined, and deeper understanding can control AI systems more reliably.

However, there is a spectrum of potential misalignment risks, ranging from acute, model-centric issues to gradual, systemic concerns [^177]. While mechanistic interpretability may address risks stemming directly from model internals – such as deceptive alignment or sudden capability jumps – it may be less helpful for tackling broader systemic risks like the emergence of misaligned economic structures or novel evolutionary dynamics [^130]. The multi-scale risk landscape calls for a balanced research portfolio to minimize risk, where research on governance, complex systems, and multi-agent simulations complements mechanistic insights and model evaluations. The perceived utility of mechanistic interpretability for AI safety largely depends on researchers’ priors regarding the likelihood of these different risk scenarios.

##### How Could Mechanistic Insight be Harmful?

Mechanistic interpretability research could accelerate AI capabilities, potentially leading to the development of powerful AI systems that are misaligned with human values, posing significant risks [^323] [^176] [^131]. While historically, interpretability research had little impact on AI capabilities, recent exceptions like discoveries about scaling laws [^143], architectural improvements inspired by studying induction heads [^263] [^91] [^280] [^309], and efficiency gains inspired by the logit lens technique [^309] demonstrated its potential to enhance capabilities. Scaling interpretability research may necessitate automation [^60] [^22], potentially enabling rapid self-improvement of AI systems [^291]. Some researchers recommend selective publication and focusing on lower-risk areas to mitigate these risks [^142] [^318] [^77] [^247].

Mechanistic interpretability also poses dual-use risks, where the same techniques could be used for both beneficial and harmful purposes. Fine-grained editing capabilities enabled by interpretability could be used for [machine unlearning](https://arxiv.org/html/2404.14082v3#glo.main.machine%20unlearning) (removing private data or dangerous knowledge from models) [^115] [^329] [^252] [^279] but could be misused for censorship. Similarly, while interpretability may help improve adversarial robustness [^287], it may also facilitate the development of stronger adversarial attacks [^231] [^44].

Misunderstanding or overestimating the capabilities of interpretability techniques can divert resources from critical safety areas or lead to overconfidence and misplaced trust in AI systems [^51] [^41]. Robust evaluation and benchmarking (Section 8.2) are crucial to validate interpretability claims and reduce the risks of overinterpretation or misinterpretation.

## 7 Challenges

### 7.1 Research Issues

##### Need for Comprehensive, Multi-Pronged Approaches.

Current interpretability research often focuses on individual techniques rather than combining complementary approaches. To achieve a holistic understanding of neural networks, we propose utilizing a diverse interpretability toolbox that integrates multiple methods (see also Section 4.4), such as: (i) Coordinating observational (e.g., probing, logit lens) and interventional methods (e.g., activation patching) to establish causal relationships. (ii) Combining feature-level analysis (e.g., sparse autoencoders) with circuit-level interventions (e.g., path patching) to uncover representation-mechanism interplay. (iii) Integrating intrinsic interpretability approaches with post-hoc analysis for robust understanding.

For example, coordinated methods could be used for [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering) trojaned behaviors [^45], where observational techniques identify suspicious activations, interventional methods isolate the relevant circuits, and intrinsic approaches guide the design of more robust architectures.

##### Cherry-Picking and Streetlight Interpretability.

Another concerning pattern is the tendency to cherry-pick results, relying on a small number of convincing examples or visualizations as the basis for an argument without comprehensive evaluation [^287]. This amounts to publication bias, showcasing an unrealistic highlight reel of best-case performance. Relatedly, many interpretability techniques are primarily evaluated on small toy models and tasks [^56] [^77] [^163] [^53], risking missing critical phenomena that only emerge in more realistic and diverse contexts. This focus on cherry-picked results from toy models is a form of [streetlight interpretability](https://arxiv.org/html/2404.14082v3#glo.main.streetlight%20interpretability) [^41], examining AI systems under only ideal conditions of maximal interpretability.

### 7.2 Technical Limitations

##### Scalability Challenges and Risks of Human Reliance.

A critical hurdle is demonstrating the scalability of mechanistic interpretability to real-world AI systems across model size, task complexity, behavioral coverage, and analysis efficiency [^77] [^307]. Achieving a truly comprehensive understanding of a model’s capabilities in all contexts is daunting, and the time and compute required must scale tractably. Automating interpretability techniques is crucial, as manual analysis quickly becomes infeasible for large models. The high human involvement in current interpretability research raises concerns about the scalability and validity of human-generated model interpretations. Subjective, inconsistent human evaluations and lack of ground-truth benchmarks are known issues [^287]. As models scale, it will become increasingly untenable to rely on humans to hypothesize about model mechanisms manually. More work is needed on automating the discovery of mechanistic explanations and translating model weights into human-readable computational graphs [^77], but progress on that front may also come from outside the field [^203].

##### Obstacles to Bottom-Up Interpretability.

There are fundamental questions about the tractability of fully [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering) neural networks from the bottom up, especially as models become more complex [^129]. Models may learn internal representations and algorithms that do not cleanly map to human-understandable concepts, making them difficult to interpret even with complete transparency [^217]. This gap between human and model ontologies may widen as architectures evolve, increasing opaqueness [^132]. Conversely, model representations might naturally converge to more human-interpretable forms as capability increases [^147] [^83].

##### Analyzing Models Embedded in Environments.

Real-world AI systems embedded in rich, interactive environments exhibit two forms of in-context behavior that pose significant interpretability challenges beyond understanding models in isolation. Externally, models may dynamically adapt to and reshape their environments through in-context learning from the interactions and feedback loops with their external environment [^185]. Internally, the [hydra effect](https://arxiv.org/html/2404.14082v3#glo.main.hydra%20effect) demonstrates in-context reorganization, where models flexibly reorganize their internal representations in a context-dependent manner to maintain capabilities even after ablating key components [^218]. These two instances of in-context behavior – external adaptation to the environment and internal self-reorganization – undermine interpretability approaches that assume fixed [circuits](https://arxiv.org/html/2404.14082v3#glo.main.circuits). For models deeply embedded in rich real-world settings, their dynamic coupling with the external world via in-context environmental learning and their internal in-context representational reorganization make strong interpretability guarantees difficult to attain through analysis of the initial model alone.

##### Adversarial Pressure Against Interpretability.

As models become more capable through increased training and optimization, there is a risk they may learn deceptive behaviors that actively obscure or mislead the interpretability techniques meant to understand them. Models could develop adversarial "mind-reader" components that predict and counteract the specific analysis methods used to interpret their inner workings [^313] [^150]. Optimizing models through techniques like gradient descent could inadvertently make their internal representations less interpretable to external observers [^148] [^92] [^352]. In extreme cases, a highly advanced AI system singularly focused on preserving its core objectives may directly undermine the fundamental assumptions that enable interpretability methods in the first place.

These adversarial dynamics, where the capabilities of the AI model are pitted against efforts to interpret it, underscore the need for interpretability research to prioritize worst-case robustness rather than just average-case scenarios. Current techniques often fail even when models are not adversarially optimized. Achieving high confidence in fully understanding extremely capable AI models may require fundamental advances to make interpretability frameworks resilient against an intelligent system’s active deceptive efforts.

## 8 Future Directions

Given the current limitations and challenges, several key research problems emerge as critical for advancing mechanistic interpretability. These problems span four main areas: emphasizing conceptual clarity Section 8.1, establishing rigorous standards Section 8.2, improving the scalability of interpretability techniques Section 8.3, and expanding the research scope Section 8.4. Each subsection presents specific research questions and challenges that need to be addressed to move the field forward.

![Refer to caption](https://arxiv.org/html/2404.14082v3/x13.png)

Figure 12: Roadmap for advancing mechanistic interpretability research, highlighting key strategic directions.

### 8.1 Clarifying Concepts

##### Integrating with Existing Literature.

To mature, mechanistic interpretability should embrace existing work, using established terminology rather than reinventing the wheel. Diverging terminology inhibits collaboration across disciplines. Presently, the terminology used for mechanistic interpretability partially diverges from mainstream AI research [^41]. For example, while the mainstream speaks of *distributed representations* [^140] [^256] and the goal of [disentangled](https://arxiv.org/html/2404.14082v3#glo.main.disentangled) representations [^138] [^202], the mechanistic interpretability literature refers to the same phenomenon as *polysemanticity* [^307] [^187] [^214] and *superposition* [^77] [^134]. Using common language invites "accidental" contributions and prevents isolating mechanistic interpretability from broader AI research.

Mechanistic interpretability relates to many other fields in AI research, including compressed sensing [^77], modularity, adversarial robustness, continual learning, network compression [^287], neurosymbolic reasoning, trojan detection, and program synthesis [^41] [^224], and causal representation learning. These relationships can help develop new methods, metrics, benchmarks, and theoretical frameworks. For instance:

1. Neurosymbolic Reasoning and Program Synthesis: Mechanistic interpretability aims for [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering) neural networks by converting their weights into human-readable algorithms. This endeavor can draw inspiration from neurosymbolic reasoning [^293] and program synthesis. Techniques like creating programs in domain-specific languages [^346] [^345] [^340], extracting decision trees [^372] or symbolic causal graphs [^289] from neural networks align well with the goals of mechanistic interpretability. Adopting these approaches can extend the toolkit for reverse engineering AI systems.
2. Causal Representation Learning: Causal Representation Learning (CRL) aims to discover and disentangle underlying causal factors in data [^308], complementing mechanistic interpretability’s goal of understanding causal structures within neural networks. While mechanistic interpretability typically examines individual [features](https://arxiv.org/html/2404.14082v3#glo.main.features) and [circuits](https://arxiv.org/html/2404.14082v3#glo.main.circuits), CRL offers a framework for understanding high-level causal structures. CRL techniques could enhance interpretability by identifying causal relationships between neurons or layers [^17] [^171], potentially revealing model reasoning. Its focus on interventions and counterfactuals [^276] [^278] could inspire new methods for probing model internals [^112] [^21]. CRL’s emphasis on learning invariant representations [^277] [^351] could guide the search for robust features, while its approach to transfer learning [^294] [^205] could inform studies into model generalization.
3. Trojan Detection: Detecting deceptive alignment models is a key motivation for inspecting model internals, as – by definition – deception is not salient from observing behavior alone [^46]. However, quantifying progress is challenging due to the lack of evidence for deception as an emergent capability in current models [^326], apart from [sycophancy](https://arxiv.org/html/2404.14082v3#glo.main.sycophancy) [^317] [^69] and theoretical evidence for [deceptive inflation](https://arxiv.org/html/2404.14082v3#glo.main.deceptive%20inflation) behavior [^181]. Detecting trojans (or backdoors) [^153] implanted via data poisoning could be a proxy goal and proof-of-concept. These trojans simulate [outer misalignment](https://arxiv.org/html/2404.14082v3#glo.main.outer%20misalignment) (where the model’s behavior is misaligned with the specified reward function or objectives due to poorly defined or incorrect reward signals) rather than [inner misalignment](https://arxiv.org/html/2404.14082v3#glo.main.inner%20misalignment) such as deceptive alignment (where the model appears aligned with the specified objectives but internally pursues different, misaligned goals). Moreover, activating a trojan typically results in an immediate change of behavior, while deception can be subtle, gradual, and, at first, entirely internal. Nevertheless, trojan detection can still provide a practical testbed for benchmarking interpretability methods [^209].
4. Adversarial Robustness: There is a duality between interpretability and adversarial robustness [^77] [^287] [^19]. More interpretable models tend to be more robust against adversarial attacks [^167], and vice versa, adversarially trained models are often more interpretable [^79]. For instance, techniques like input gradient regularization have been shown to simultaneously improve the interpretability of saliency maps and enhance adversarial robustness [^295] [^72]. Furthermore, interpretability tools can help create more sophisticated adversaries [^39] [^42], improving our understanding of model internals. Viewing adversarial examples as inherent neural network [features](https://arxiv.org/html/2404.14082v3#glo.main.features) [^154] rather than bugs also hints at alien features beyond human perception. Connecting mechanistic interpretability to adversarial robustness thus promises ways to gain theoretical insight, measure progress [^41], design inherently more robust architectures [^87], and create interpretability-guided approaches for identifying (and mitigating) adversarial vulnerabilities [^94].

More details on the interplay between interpretability, robustness, modularity, continual learning, network compression, and the human visual system can be found in the review by [^287].

##### Corroborate or Refute Core Assumptions.

Features are the fundamental units defining neural representations and enabling mechanistic interpretability’s bottom-up approach [^47], but defining them involves assumptions requiring scrutiny, as they shape interpretations and research directions. Questioning hypotheses by seeking additional evidence or counter-examples is crucial.

The [linear representation](https://arxiv.org/html/2404.14082v3#glo.main.linear%20representation) hypothesis treats activation directions as features [^271] [^248] [^77], but the emergence and necessity of linearity is unclear – is it architectural bias or inherent? Stronger theory justifying linearity’s necessity or counter-examples like autoencoders on uncorrelated data without intermediate linear layers [^77] are needed. An alternative lens views features as polytopes from piecewise linear activations [^25], questioning if direction simplification suffices or added polytope complexity aids interpretability.

The [superposition](https://arxiv.org/html/2404.14082v3#glo.main.superposition) hypothesis suggests that [polysemantic](https://arxiv.org/html/2404.14082v3#glo.main.polysemantic) neurons arise from the network compressing and representing many features within its limited set of neurons [^77], but polysemanticity can also occur incidentally due to redundancy [^187] [^214] [^218]. Understanding superposition’s role could inform mitigating polysemanticity via regularization [^187]. Superposition also raises open questions like operationalizing computation in superposition [^342] [^124], attention head superposition [^77] [^162] [^193] [^111], representing feature clusters [^77], connections to adversarial robustness [^77] [^94] [^26], anti-correlated feature organization [^77], and architectural effects [^240].

### 8.2 Setting Standards

##### Prioritizing Robustness over Capability Advancement.

As the mechanistic interpretability community expands, it is essential to maintain the norm of not advancing AI capabilities while simultaneously establishing metrics necessary for the field’s progress [^287]. Researchers should prioritize developing comprehensive tools for analyzing the worst-case performance of AI systems, ensuring robustness and reliability in critical applications. This includes focusing on adversarial tasks, such as backdoor detection and removal [^179] [^153] [^362], and evaluating the accuracy of explanations in producing adversarial examples [^109].

##### Establishing Metrics, Benchmarks, and Algorithmic Testbeds.

A central challenge in mechanistic interpretability is the lack of rigorous evaluation methods. Relying solely on intuition can lead to conflating hypotheses with conclusions, resulting in cherry-picking and optimizing for best-case rather than average or worst-case performance [^296] [^227] [^287] [^41]. Current ad hoc practices and proxy measures [^71] risk over-optimization (Goodhart’s law – When a measure becomes a target, it ceases to be a good measure). Distinguishing correlation from causation is crucial, as interpretability illusions demonstrate that visualizations may be meaningless without causal linking [^28] [^89] [^258].

To advance the field, rigorous evaluation methods are needed. These should include: (i) assessing out-of-distribution inputs, as most current methods are only valid for specific examples or datasets [^287] [^154] [^231] [^45] [^33]; (ii) controlling systems through edits, such as implanting or removing trojans [^215] or targeted editing [^107] [^63] [^219] [^220] [^13] [^125]; (iii) replacing components with simpler reverse-engineered alternatives [^194]; and (iv) comprehensive evaluation through replacing components with hypothesized circuits [^284].

Algorithmic testbeds are essential for evaluating faithfulness [^156] [^123] and falsifiability [^186]. Tools like Tracr [^194] can provide ground truth labels for benchmarking search methods [^109], while toy models studying superposition in computation [^342] and transformers on algorithmic tasks can quantify sparsity and test intrinsic methods. Recently, [^337] [^116] introduced datasets of transformer weights with known circuits for evaluating mechanistic interpretability techniques.

### 8.3 Scaling Techniques

##### Broader and Deeper Coverage of Complex Models and Behaviors.

A primary goal in scaling mechanistic interpretability is pushing the Pareto frontier between model and task complexity and the coverage of interpretability techniques [^47]. While efforts have focused on larger models, it is equally crucial to scale to more complex tasks and provide comprehensive explanations essential for provable safety [^333] [^64] [^113] and enumerative safety [^62] [^77] by ensuring models won’t engage in dangerous behaviors like deception. Future work should aim for thorough [reverse engineering](https://arxiv.org/html/2404.14082v3#glo.main.reverse%20engineering) [^283], integrating proven modules into larger networks [^247], and capturing sequences encoded in hidden states beyond immediate predictions [^268]. Deepening analysis complexity is also key, validating the realism of toy models [^77] and extending techniques like path patching [^109] [^199] to larger language models. The field must move beyond small transformers on algorithmic tasks [^247] and limited scenarios [^89] to tackle more complex, realistic cases.

##### Towards Universality.

As mechanistic interpretability matures, the field must transition from isolated empirical findings to developing overarching theories and universal reasoning primitives beyond specific circuits, aiming for a comprehensive understanding of AI capabilities. While collecting empirical data remains valuable [^245], establishing motifs, empirical laws, and theories capturing universal model behavior aspects is crucial. This may involve finding more circuits/features [^235] [^237], exploring circuits as a lens for memorization/generalization [^122], identifying primitive general reasoning skills [^83], generalizing specific findings to model-agnostic phenomena [^222], and investigating emergent model generality across neural network classes [^155]. Identifying universal reasoning patterns and unifying theories is key to advancing interpretability.

##### Automation.

Implementing automated methods is crucial for scaling interpretability of real-world state-of-the-art models across size, task complexity, behavior coverage, and analysis time [^141]. Manual circuit identification is labor-intensive [^193], so automated techniques like circuit discovery and sparse autoencoders can enhance the process [^86] [^241]. Future work should automatically create varying datasets for understanding circuit functionality [^60], develop automated hypothesis search [^109], and investigate attention head/MLP interplay [^229]. Scaling sparse autoencoders to extract high-quality features automatically for frontier models is critical [^30]. Still, it requires caution regarding potential downsides like AI iteration outpacing training [^291] and loss of human interpretability from tool complexity [^71].

### 8.4 Expanding Scope

##### Interpretability Across Training.

While mechanistic interpretability of final trained models is a prerequisite, the field should also advance interpretability before and during training by studying learning dynamics [^236] [^77] [^150]. This includes tracking neuron development [^195], analyzing neuron set changes with scale [^223], and investigating emergent computations [^283]. Studying phase transitions could yield safety insights for [reward hacking](https://arxiv.org/html/2404.14082v3#glo.main.reward%20hacking) risks [^263].

##### Multi-Level Analysis.

Complementing the predominant bottom-up methods [^122], mechanistic interpretability should explore top-down and hybrid approaches, a promising yet neglected avenue. The top-down analysis offers a tractable way to study large models and guide microscopic research with macroscopic observations [^343]. Its computational efficiency could enable extensive "comparative anatomy" of diverse models, revealing high-level motifs underlying abilities. These motifs could serve as analysis units for understanding internal modifications from techniques like instruction fine-tuning [^267] and reinforcement learning from human feedback [^54] [^10].

##### New Frontiers: Vision, Multimodal, and Reinforcement Learning Models.

While some mechanistic interpretability has explored convolutional neural networks for vision [^37] [^36], vision-language models [^269] [^303] [^139], and multimodal neurons [^108], little work has focused on vision transformers [^269] [^1] [^348] [^270]. Future efforts could identify mechanisms within vision-language models, mirroring progress in unimodal language models [^247] [^355].

Reinforcement learning (RL) is also a crucial frontier given its role in advanced AI training via techniques like reinforcement learning from human feedback (RLHF) [^54] [^10], despite potentially posing significant safety risks [^20] [^43]. Interpretability of RL should investigate reward/goal representations [^228] [^59] [^58] [^27] [^26], study circuitry changes from alignment algorithms [^282] [^157] [^188] [^158], and explore emergent subgoals or proxies [^151] [^155] such as internal reward models [^212]. While current state-of-the-art AI systems as prediction-trained LLMs are considered relatively safe [^152], progress on interpreting RL systems may prove critical for safeguarding the next paradigm [^7].

## Acknowledgements

I am grateful for the invaluable feedback and comments from Leon Lang, Tim Bakker, Jannik Brinkmann, Can Rager, Louis van Harten, Jacqueline Bereska, Benjamin Shaffrey, Thijmen Nijdam, Alice Rigg, Arthur Conmy, and Tom Lieberum. Their insights substantially improved this work.

## Glossary

circuits

Sub-graphs within neural networks consisting of and the weights connecting them. Circuits can be thought of as computational primitives that perform understandable operations to produce (ideally interpretable) features from prior (ideally interpretable) features. Examples include circuits for detecting curves at specific orientations [^36] [^37], continuing repeated patterns in text [^263], and resolving anaphoric references [^355]. While circuits can involve clearly interpretable features, the definition allows for intermediate representations that are less easily interpretable.

concepts

An abstract idea or representation derived from observations of the world. Concepts refer to the [natural abstractions](https://arxiv.org/html/2404.14082v3#glo.main.natural%20abstractions) that a cognitive system, like a neural network, aims to capture and represent through its learned [features](https://arxiv.org/html/2404.14082v3#glo.main.features), which may or may not align perfectly with human-defined concepts.

deceptive alignment

When a misaligned model aims to appear aligned to gain more power to take control once sufficiently powerful.

deceptive inflation

Theoretical result on deceptive behavior: policies produce trajectories that look better than they actually are from the human’s perspective with limited observations to get higher reward signals during training. This deceptive behavior arises in reinforcement learning from human feedback when the human provides feedback based only on partial observations of the trajectories, while the policy has full state information during training [^181].

disentangled

In disentangled representations, individual dimensions or components correspond to distinct, independent factors of variation in the data, rather than representing a tangled mixture of these factors.

eliciting latent knowledge

Developing strategies to make a machine learning model explicitly report latent facts or knowledge embedded in its parameters, especially in cases where the model’s output is untrusted [^55]. This involves finding patterns in neural network activations that track the true state of the world [^208].

features

The fundamental units of how neural networks encode knowledge, which cannot be further decomposed into smaller, distinct. Features are core components of a neural network’s representation, analogous to how cells form the fundamental unit of biological organisms [^260]. The hypothesis suggests an alternative definition: that features correspond to the concepts that a larger, sparser network with sufficient capacity would learn to represent with individual () neurons [^260] [^30].

grokking

"Grokking refers to the surprising phenomenon of delayed generalization where neural networks, on certain learning problems, generalize long after overfitting their training set." [^197]

hydra effect

The phenomenon where models can internally self-repair and maintain capabilities even when key components are ablated, making it challenging to identify the relevant components underlying a particular behavior [^218].

inner misalignment

Inner misalignment, or goal misgeneralization, occurs when an AI system develops goals or behaviors during training that are misaligned with the intended objectives despite a correctly specified reward signal.

internal world models

Internal causal environment models formed within neural networks, implicitly emerging as a by-product of prediction (e.g., in large language models).

irreducible

We adopt the notion of [features](https://arxiv.org/html/2404.14082v3#glo.main.features) as the fundamental units of neural network representations, such that features cannot be further decomposed into smaller, distinct factors. To make this more precise, we can formalize the definition of features as irreducible input patterns following [^78]: A feature $f$ of sparsity $s$ is a function that maps a subset of the input space (with probability $1-s>0$) into a higher-dimensional representational space. We say the feature is active on this subset. A feature $f$ is reducible into features $a$ and $b$ if there exists a transformation that decomposes $f$ into $a$ and $b$, such that the transformed distribution $p(a,b)$ is either:
1. Separable: $p(a,b)=p(a)p(b)$
2. A mixture: $p(a,b)=wp_{1}(a,b)+(1-w)p_{2}(a,b)$ where $p_{1}$ is lower-dimensional.
Features are defined as irreducible patterns that cannot be decomposed into separable or mixture distributions via such transformations. This formalizes the notion that features form the fundamental atomic units underlying neural representations. Features that can be [disentangled](https://arxiv.org/html/2404.14082v3#glo.main.disentangled) into statistically independent components (separable) or simpler lower-dimensional factors (mixtures) are not considered the core representational primitives. The key properties are that 1) features map from the input space to higher-dimensional representational spaces, 2) features are sparse and only activated on subsets of the input, and crucially, 3) features are irreducible and cannot be expressed as transformations of other statistically independent components.

iterative distillation and amplification

A technique for training AI systems by repeatedly distilling knowledge from a larger model into a smaller one while amplifying the smaller model’s capabilities through feedback and interaction with humans.

linear representation

Features are directions in activation space, i.e., linear combinations of neurons.

machine unlearning

Techniques for removing private data or dangerous knowledge from models.

mesa-optimization

The emergence of unintended subagents within a model with their own objectives, potentially misaligned with the original training objective.

microscope AI

Systems that extract and utilize knowledge from a model without allowing the model to take autonomous actions. This involves reverse engineering a trained model to understand its learned knowledge about the world, aiming to leverage this understanding directly without deploying the model in an operational capacity.

modularity

The property of an AI system being composed of distinct, semi-independent components or submodules that can be separately understood, modified, and recombined, rather than a monolithic, opaque structure.

monosemantic

A neuron corresponding to a single concept. The intuition is that analyzing what inputs activate a given neuron reveals its associated semantic meaning or concept. In contrast to.

motifs

Repeating patterns that emerge across models and tasks, manifesting as circuits, features, or higher-level behaviors from component interactions. Examples include curve detectors, induction circuits, and branch specialization. Motifs reveal common structures and mechanisms underlying neural network intelligence.

natural abstractions

High-level summaries or descriptions of a system or environment learned and used by many cognitive systems. According to the natural abstraction hypothesis [^49], a set of "natural" abstractions exist that represent redundantly encoded information in the world and tend to be learned by intelligent systems produced through local selection pressures. These natural abstractions form a relatively small, discrete set of concepts like "tree," "velocity," etc., that allow compact descriptions of the world while discarding many irrelevant low-level details.

outer misalignment

Outer misalignment, or reward hacking, occurs when the specified reward function or utility function fails to capture the desired objectives correctly. This leads the AI to optimize for behaviors that achieve high reward scores but are misaligned with the intended outcomes.

oversight

(Scalable) oversight refers to the challenge of providing reliable supervision—through labels, reward signals, or critiques—to AI models, ensuring effectiveness even as models surpass human-level performance.

polysemantic

Neurons that are associated with multiple, unrelated [concepts](https://arxiv.org/html/2404.14082v3#glo.main.concepts), contradicting the interpretation of neurons as representational primitives and making it challenging to understand the information processing of neural networks. This term is derived from linguistic concepts of polysemy [^80], and in the context of neural networks first introduced by [^6], who suggested that word embeddings of polysemous words may be stored as a of vectors representing distinct meanings. [^260] first used the term polysemanticity, elaborating on the concept of polysemantic neurons as a challenge for mechanistic interpretability.

prediction orthogonality

A model whose objective is prediction can simulate agents who optimize toward any objectives with any degree of optimality [^159].

privileged basis

In certain neural network representations, the basis directions formed by the individual neurons are architecturally distinguished from arbitrary directions in the activation space. This privileged basis makes it meaningful to analyze the properties and roles of individual neurons, as the architecture encourages features to align with these basis directions. Hence, a privileged basis is necessary but not sufficient for the formation of [monosemantic](https://arxiv.org/html/2404.14082v3#glo.main.monosemantic) neurons. [^77].

representation engineering

A top-down approach to transparency research that treats representations as the fundamental unit of analysis, aiming to understand and control representations of high-level cognitive phenomena in neural networks like large language models. Representation engineering has two main areas: 1) Reading representations to probe and interpret their contents, and 2) Controlling representations to manipulate high-level concepts like honesty or morality [^375].

reverse engineering

The process of deconstructing a neural network’s computations to fully understand and specify its operations. This involves breaking down the network’s functionality into explicit, interpretable components, potentially as clear and detailed as pseudocode.

reward hacking

See [outer misalignment](https://arxiv.org/html/2404.14082v3#glo.main.outer%20misalignment).

simulacra

The text outputs generated by a predictive model simulating the causal processes underlying text creation. These outputs simulate coherent and contextually relevant language, sometimes exhibiting agentic behaviors or goals despite the predictive model itself lacking genuine agency or intentionality. Simulacra can be either agentic, mimicking intentional and persuasive language use, or non-agentic, merely generating descriptive text without simulated goals or agency [^159] [^20].

simulation

The simulation hypothesis says that when scaled up sufficiently, predictive models will learn to simulate the real-world causal processes that generated their training data [^159]. When these models are optimized for predictive accuracy on broad data distributions like natural language, they are incentivized to discover the underlying rules, physics, and semantics that govern the data to model and predict future observations effectively. This allows the models to go beyond just memorizing or pattern-matching their training sets, instead learning to simulate hypothetical scenarios, reason about counterfactuals, and exhibit behaviors characteristic of general intelligence – all as a byproduct of the drive for efficient compression and accurate prediction. The simulation hypothesis suggests these models will develop rich [internal world models](https://arxiv.org/html/2404.14082v3#glo.main.internal%20world%20models) capturing the causal dynamics of the training distribution.

streetlight interpretability

Examining AI systems under only ideal conditions of maximal interpretability, risking missing critical phenomena that only emerge in more realistic and diverse contexts.

superposition

The superposition hypothesis suggests that neural networks can leverage high-dimensional spaces to represent more [features](https://arxiv.org/html/2404.14082v3#glo.main.features) than the actual count of neurons by encoding features in almost orthogonal directions [^77].

sycophancy

The tendency of models to generate responses that align with user beliefs rather than providing truthful information. This behavior, encouraged by human feedback used in fine-tuning, is observed in state-of-the-art AI assistants across various tasks [^317]. Sycophancy arises because human preference judgments often favor responses that match users’ views, leading to a preference for convincingly written sycophantic responses over correct ones.

universality

The universality hypothesis proposes the emergence of common [circuits](https://arxiv.org/html/2404.14082v3#glo.main.circuits) across neural network models trained on similar tasks and data distributions. A stronger form posits that these common circuits represent a set of fundamental computational [motifs](https://arxiv.org/html/2404.14082v3#glo.main.motifs) that neural networks gravitate towards when learning. The weaker version suggests that for a given task, dataset, and model architecture, an optimal way to solve the problem may exist, which different models will tend to converge towards, resulting in analogous circuits. The universality hypothesis implies that rather than each model learning arbitrary, unstructured representations, there is an underlying universality to the circuits that emerge, shaped by the learning task and inductive biases.

well-founded AI

Developing AI systems with provable safety guarantees about their behavior and alignment with human values through rigorous mathematical modeling and verification. [^333] [^64].

[^1]: Estelle Aflalo, Meng Du, Shao-Yen Tseng, Yongfei Liu, Chenfei Wu, Nan Duan, and Vasudev Lal. [Vl-interpret: An interactive visualization tool for interpreting vision-language transformers](https://ieeexplore.ieee.org/document/9880368/). *CVPR*, June 2022.

[^2]: Guillaume Alain and Yoshua Bengio. [Understanding intermediate layers using linear classifier probes](http://arxiv.org/abs/1610.01644). *ICLR*, 2016.

[^3]: Uri Alon. *An introduction to systems biology: design principles of biological circuits*. Chapman and Hall/CRC, 2019.

[^4]: Andy Arditi, Oscar Obeso, Aaquib Syed, Daniel Paleka, Nina Panickssery, Wes Gurnee, and Neel Nanda. [Refusal in language models is mediated by a single direction](https://arxiv.org/abs/2406.11717). *CoRR*, 2024.

[^5]: Aryaman Arora, Dan Jurafsky, and Christopher Potts. [Causalgym: Benchmarking causal interpretability methods on linguistic tasks](http://arxiv.org/abs/2402.12560). *CoRR*, February 2024.

[^6]: Sanjeev Arora, Yuanzhi Li, Yingyu Liang, Tengyu Ma, and Andrej Risteski. [Linear algebraic structure of word senses, with applications to polysemy](http://arxiv.org/abs/1601.03764). *TACL*, December 2018.

[^7]: Leopold Aschenbrenner. [Situational awareness: The decade ahead](https://situational-awareness.ai/). *Series: Situational Awareness. June*, 2024.

[^8]: Sebastian Bach, Alexander Binder, Grégoire Montavon, Frederick Klauschen, Klaus-Robert Müller, and Wojciech Samek. [On pixel-wise explanations for non-linear classifier decisions by layer-wise relevance propagation](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0130140). *PLOS ONE*, July 2015.

[^9]: Nicholas Bai, Rahul Ajay Iyer, Tuomas Oikarinen, and Tsui-Wei Weng. [Describe-and-dissect: Interpreting neurons in vision networks with language models](https://openreview.net/forum?id=50SMcZ8QQf). *ICML MI Workshop*, June 2024.

[^10]: Yuntao Bai, Andy Jones, Kamal Ndousse, Amanda Askell, Anna Chen, Nova DasSarma, Dawn Drain, Stanislav Fort, Deep Ganguli, Tom Henighan, Nicholas Joseph, Saurav Kadavath, Jackson Kernion, Tom Conerly, Sheer El-Showk, Nelson Elhage, Zac Hatfield-Dodds, Danny Hernandez, Tristan Hume, Scott Johnston, Shauna Kravec, Liane Lovitt, Neel Nanda, Catherine Olsson, Dario Amodei, Tom Brown, Jack Clark, Sam McCandlish, Chris Olah, Ben Mann, and Jared Kaplan. [Training a helpful and harmless assistant with reinforcement learning from human feedback](http://arxiv.org/abs/2204.05862). *CoRR*, April 2022.

[^11]: Yamini Bansal, Preetum Nakkiran, and Boaz Barak. [Revisiting model stitching to compare neural representations](http://arxiv.org/abs/2106.07682). *CoRR*, June 2021.

[^12]: Boaz Barak, Benjamin L. Edelman, Surbhi Goel, Sham Kakade, Eran Malach, and Cyril Zhang. [Hidden progress in deep learning: Sgd learns parities near the computational limit](http://arxiv.org/abs/2207.08799). *NeurIPS*, 2022.

[^13]: David Bau, Jun-Yan Zhu, Hendrik Strobelt, Bolei Zhou, Joshua B. Tenenbaum, William T. Freeman, and Antonio Torralba. [Gan dissection: Visualizing and understanding generative adversarial networks](http://arxiv.org/abs/1811.10597). *ICLR*, December 2018.

[^14]: Yonatan Belinkov. [Probing classifiers: Promises, shortcomings, and advances](http://arxiv.org/abs/2102.12452). *CoRR*, September 2021.

[^15]: Nora Belrose, Zach Furman, Logan Smith, Danny Halawi, Igor Ostrovsky, Lev McKinney, Stella Biderman, and Jacob Steinhardt. [Eliciting latent predictions from transformers with the tuned lens](http://arxiv.org/abs/2303.08112). *CoRR*, August 2023.

[^16]: Emily M. Bender, Timnit Gebru, Angelina McMillan-Major, and Shmargaret Shmitchell. [On the dangers of stochastic parrots: Can language models be too big?](https://dl.acm.org/doi/10.1145/3442188.3445922) *ACM FAccT*, March 2021.

[^17]: Yoshua Bengio, Tristan Deleu, Nasim Rahaman, Rosemary Ke, Sébastien Lachapelle, Olexa Bilaniuk, Anirudh Goyal, and Christopher Pal. [A meta-transfer objective for learning to disentangle causal mechanisms](http://arxiv.org/abs/1901.10912). *CoRR*, February 2019.

[^18]: Yoshua Bengio, Geoffrey Hinton, Andrew Yao, Dawn Song, Pieter Abbeel, Yuval Noah Harari, Ya-Qin Zhang, Lan Xue, Shai Shalev-Shwartz, Gillian Hadfield, Jeff Clune, Tegan Maharaj, Frank Hutter, Atılım Güneş Baydin, Sheila McIlraith, Qiqi Gao, Ashwin Acharya, David Krueger, Anca Dragan, Philip Torr, Stuart Russell, Daniel Kahneman, Jan Brauner, and Sören Mindermann. [Managing ai risks in an era of rapid progress](http://arxiv.org/abs/2310.17688). *CoRR*, November 2023.

[^19]: Leonard Bereska. [Mechanistic interpretability for adversarial robustness — a proposal](https://leonardbereska.github.io/blog/2024/mechrobustproposal). *Leonard Bereska’s Blog*, August 2024.

[^20]: Leonard Bereska and Efstratios Gavves. [Taming simulators: Challenges, pathways and vision for the alignment of large language models](https://ojs.aaai.org/index.php/AAAI-SS/article/view/27478). *AAAI-SS*, October 2023.

[^21]: Michel Besserve, Arash Mehrjou, Rémy Sun, and Bernhard Schölkopf. [Counterfactuals uncover the modular structure of deep generative models](http://arxiv.org/abs/1812.03253). *CoRR*, December 2019.

[^22]: Steven Bills, Nick Cammarata, Dan Mossing, Henk Tillman, Leo Gao, Gabriel Goh, Ilya Sutskever, Jan Leike, Jeff Wu, and William Saunders. [Language models can explain neurons in language models](https://openaipublic.blob.core.windows.net/neuron-explainer/paper/index.html). *OpenAI Blog*, 2023.

[^23]: Blair Bilodeau, Natasha Jaques, Pang Wei Koh, and Been Kim. [Impossibility theorems for feature attribution](http://arxiv.org/abs/2212.11870). *Proc. Natl. Acad. Sci. U.S.A.*, January 2024.

[^24]: Christopher M. Bishop. [*Pattern recognition and machine learning*](https://api.semanticscholar.org/CorpusID:60688891). Springer-Verlag New York Inc., 2006.

[^25]: Sid Black, Lee Sharkey, Leo Grinsztajn, Eric Winsor, Dan Braun, Jacob Merizian, Kip Parker, Carlos Ramón Guevara, Beren Millidge, Gabriel Alfour, and Connor Leahy. [Interpreting neural networks through the polytope lens](https://arxiv.org/abs/2211.12312). *CoRR*, November 2022.

[^26]: Joseph Bloom and Jay Bailey. [Features and adversaries in memorydt](https://www.lesswrong.com/posts/yuQJsRswS4hKv3tsL/features-and-adversaries-in-memorydt). *LessWrong*, October 2023.

[^27]: Joseph Bloom and Paul Colognese. [Decision transformer interpretability](https://www.alignmentforum.org/posts/bBuBDJBYHt39Q5zZy/decision-transformer-interpretability). *AI Alignment Forum*, 2023.

[^28]: Tolga Bolukbasi, Adam Pearce, Ann Yuan, Andy Coenen, Emily Reif, Fernanda Vi’egas, and M. Wattenberg. [An interpretability illusion for bert](https://www.semanticscholar.org/paper/An-Interpretability-Illusion-for-BERT-Bolukbasi-Pearce/9b9dc2b3d95d2f4e4269a9818c14c70c1f801384). *CoRR*, April 2021.

[^29]: Dan Braun, Jordan Taylor, Nicholas Goldowsky-Dill, and Lee Sharkey. [Identifying functionally important features with end-to-end sparse dictionary learning](http://arxiv.org/abs/2405.12241). *ICML MI Workshop*, May 2024.

[^30]: Trenton Bricken, Adly Templeton, Joshua Batson, Brian Chen, Adam Jermyn, Tom Conerly, Nicholas L. Turner, Cem Anil, Carson Denison, Amanda Askell, Robert Lasenby, Yifan Wu, Shauna Kravec, Nicholas Schiefer, Tim Maxwell, Nicholas Joseph, Alex Tamkin, Karina Nguyen, Brayden McLean, Josiah E. Burke, Tristan Hume, Shan Carter, Tom Henighan, and Chris Olah. [Towards monosemanticity: Decomposing language models with dictionary learning](https://transformer-circuits.pub/2023/monosemantic-features/index.html). *Transformer Circuits Thread*, October 2023.

[^31]: Jannik Brinkmann, Abhay Sheshadri, Victor Levoso, Paul Swoboda, and Christian Bartelt. [A mechanistic analysis of a transformer trained on a symbolic multi-step reasoning task](http://arxiv.org/abs/2402.11917). *CoRR*, February 2024.

[^32]: Sébastien Bubeck, Varun Chandrasekaran, Ronen Eldan, Johannes Gehrke, Eric Horvitz, Ece Kamar, Peter Lee, Yin Tat Lee, Yuanzhi Li, Scott Lundberg, Harsha Nori, Hamid Palangi, Marco Tulio Ribeiro, and Yi Zhang. [Sparks of artificial general intelligence: Early experiments with gpt-4](http://arxiv.org/abs/2303.12712). *CoRR*, April 2023.

[^33]: Collin Burns, Haotian Ye, Dan Klein, and Jacob Steinhardt. [Discovering latent knowledge in language models without supervision](http://arxiv.org/abs/2212.03827). *ICLR*, 2023.

[^34]: Lucius Bushnaq, Stefan Heimersheim, Nicholas Goldowsky-Dill, Dan Braun, Jake Mendel, Kaarel Hanni, Avery Griffin, Jorn Stohler, Magdalena Wache, and Marius Hobbhahn. [The local interaction basis: Identifying computationally-relevant and sparsely interacting features in neural networks](https://arxiv.org/abs/2405.10928). *CoRR*, May 2024.

[^35]: Ethan Caballero, Kshitij Gupta, Irina Rish, and David Krueger. [Broken neural scaling laws](http://arxiv.org/abs/2210.14891). *ICLR*, October 2022.

[^36]: Nick Cammarata, Gabriel Goh, Shan Carter, Ludwig Schubert, Michael Petrov, and Chris Olah. [Curve detectors](https://distill.pub/2020/circuits/curve-detectors). *Distill*, June 2020.

[^37]: Nick Cammarata, Gabriel Goh, Shan Carter, Chelsea Voss, Ludwig Schubert, and Chris Olah. [Curve circuits](https://distill.pub/2020/circuits/curve-circuits/). *Distill*, 2021.

[^38]: Steven Cao, Victor Sanh, and Alexander M. Rush. [Low-complexity probing via finding subnetworks](http://arxiv.org/abs/2104.03514). *NAACL-HLT*, April 2021.

[^39]: Shan Carter, Zan Armstrong, Ludwig Schubert, Ian Johnson, and Chris Olah. [Activation atlas](https://distill.pub/2019/activation-atlas). *Distill*, March 2019.

[^40]: Giuseppe Casalicchio, Christoph Molnar, and Bernd Bischl. [Visualizing the feature importance for black box models](http://arxiv.org/abs/1804.06620). *ECML PKDD*, 2018.

[^41]: Stephen Casper. [The engineer’s interpretability sequence](https://www.alignmentforum.org/s/a6ne2ve5uturEEQK7). *AI Alignment Forum*, February 2023.

[^42]: Stephen Casper, Max Nadeau, Dylan Hadfield-Menell, and Gabriel Kreiman. [Robust feature-level adversaries are interpretability tools](http://arxiv.org/abs/2110.03605). *NeurIPS*, October 2021.

[^43]: Stephen Casper, Xander Davies, Claudia Shi, Thomas Krendl Gilbert, Jérémy Scheurer, Javier Rando, Rachel Freedman, Tomasz Korbak, David Lindner, Pedro Freire, Tony Wang, Samuel Marks, Charbel-Raphaël Segerie, Micah Carroll, Andi Peng, Phillip Christoffersen, Mehul Damani, Stewart Slocum, Usman Anwar, Anand Siththaranjan, Max Nadeau, Eric J. Michaud, Jacob Pfau, Dmitrii Krasheninnikov, Xin Chen, Lauro Langosco, Peter Hase, Erdem Bıyık, Anca Dragan, David Krueger, Dorsa Sadigh, and Dylan Hadfield-Menell. [Open problems and fundamental limitations of reinforcement learning from human feedback](https://arxiv.org/abs/2307.15217). *CoRR*, 2023a.

[^44]: Stephen Casper, Kaivalya Hariharan, and Dylan Hadfield-Menell. [Diagnostics for deep neural networks with automated copy/paste attacks](http://arxiv.org/abs/2211.10024). *NeurIPS 2022 ML Safety Workshop (Best paper award)*, May 2023b.

[^45]: Stephen Casper, Yuxiao Li, Jiawei Li, Tong Bu, Kevin Zhang, Kaivalya Hariharan, and Dylan Hadfield-Menell. [Red teaming deep neural networks with feature synthesis tools](https://arxiv.org/abs/2302.10894). *NeurIPS*, 2023c.

[^46]: Stephen Casper, Carson Ezell, Charlotte Siegmann, Noam Kolt, Taylor Lynn Curtis, Benjamin Bucknall, Andreas Haupt, Kevin Wei, Jérémy Scheurer, Marius Hobbhahn, Lee Sharkey, Satyapriya Krishna, Marvin Von Hagen, Silas Alberti, Alan Chan, Qinyi Sun, Michael Gerovitch, David Bau, Max Tegmark, David Krueger, and Dylan Hadfield-Menell. [Black-box access is insufficient for rigorous ai audits](http://arxiv.org/abs/2401.14446). *ACM Conference on Fairness, Accountability, and Transparency*, January 2024.

[^47]: Lawrence Chan. [What i would do if i wasn’t at arc evals](https://www.lesswrong.com/posts/6FkWnktH3mjMAxdRT/what-i-would-do-if-i-wasn-t-at-arc-evals). *AI Alignment Forum*, May 2023.

[^48]: Lawrence Chan, Adrià Garriga-alonso, Nicholas Goldowsky-Dill, ryan\_greenblatt, jenny, Ansh Radhakrishnan, Buck, and Nate Thomas. [Causal scrubbing: a method for rigorously testing interpretability hypotheses \[redwood research\]](https://www.alignmentforum.org/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing). *AI Alignment Forum*, December 2022.

[^49]: Lawrence Chan, Leon Lang, and Erik Jenner. [Natural abstractions: Key claims, theorems, and critiques](https://www.alignmentforum.org/posts/gvzW46Z3BsaZsLc25/natural-abstractions-key-claims-theorems-and-critiques-1). *AI Alignment Forum*, March 2023.

[^50]: David Chanin, Anthony Hunter, and Oana-Maria Camburu. [Identifying linear relational concepts in large language models](https://arxiv.org/abs/2311.08968). *CoRR*, 2023.

[^51]: Charbel-Raphaël. [Against almost every theory of impact of interpretability](https://www.alignmentforum.org/posts/LNA8mubrByG7SFacm/against-almost-every-theory-of-impact-of-interpretability-1). *AI Alignment Forum*, August 2023.

[^52]: Yiting Chen, Zhanpeng Zhou, and Junchi Yan. [Going beyond neural network feature similarity: The network feature complexity and its interpretation using category theory](http://arxiv.org/abs/2310.06756). *CoRR*, November 2023a.

[^53]: Zhongtian Chen, Edmund Lau, Jake Mendel, Susan Wei, and Daniel Murfet. [Dynamical versus bayesian phase transitions in a toy model of superposition](http://arxiv.org/abs/2310.06301). *CoRR*, October 2023b.

[^54]: Paul Christiano, Jan Leike, Tom B. Brown, Miljan Martic, Shane Legg, and Dario Amodei. [Deep reinforcement learning from human preferences](http://arxiv.org/abs/1706.03741). *NeurIPS*, December 2017.

[^55]: Paul Christiano, Ajeya Cotra, and Mark Xu. [Eliciting latent knowledge](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit?usp=sharing&usp=embed_facebook), January 2021.

[^56]: Bilal Chughtai, Lawrence Chan, and Neel Nanda. [A toy model of universality: Reverse engineering how networks learn group operations](https://arxiv.org/abs/2302.03025). *ICML*, 2023.

[^57]: Bilal Chughtai, Alan Cooney, and Neel Nanda. [Summing up the facts: Additive mechanisms behind factual recall in llms](https://arxiv.org/abs/2402.07321). *NeurIPS Workshop Attributing Model Behaviour at Scale*, 2024.

[^58]: Paul Colognese. [Internal target information for ai oversight](https://www.lesswrong.com/posts/hhKpXEsfAiyFLecyF/internal-target-information-for-ai-oversight). *LessWrong*, 2023.

[^59]: Paul Colognese and Jozdien. [High-level interpretability: detecting an ai’s objectives](https://www.lesswrong.com/posts/tFYGdq9ivjA3rdaS2/high-level-interpretability-detecting-an-ai-s-objectives). *AI Alignment Forum*, 2023.

[^60]: Arthur Conmy, Augustine N. Mavor-Parker, Aengus Lynch, Stefan Heimersheim, and Adrià Garriga-Alonso. [Towards automated circuit discovery for mechanistic interpretability](https://arxiv.org/abs/2304.14997). *NeurIPS*, 2023.

[^61]: Ian C. Covert, Scott Lundberg, and Su-In Lee. [Explaining by removing: a unified framework for model explanation](https://arxiv.org/abs/2011.14878). *J. Mach. Learn. Res.*, January 2021.

[^62]: Hoagy Cunningham, Aidan Ewart, Logan Riggs, Robert Huben, and Lee Sharkey. [Sparse autoencoders find highly interpretable features in language models](http://arxiv.org/abs/2309.08600). *ICLR*, January 2024.

[^63]: Damai Dai, Li Dong, Yaru Hao, Zhifang Sui, Baobao Chang, and Furu Wei. [Knowledge neurons in pretrained transformers](https://aclanthology.org/2022.acl-long.581). *ACL*, 2022.

[^64]: David "davidad" Dalrymple, Joar Skalse, Yoshua Bengio, Stuart Russell, Max Tegmark, Sanjit Seshia, Steve Omohundro, Christian Szegedy, Ben Goldhaber, Nora Ammann, Alessandro Abate, Joe Halpern, Clark Barrett, Ding Zhao, Tan Zhi-Xuan, Jeannette Wing, and Joshua Tenenbaum. [Towards guaranteed safe ai: A framework for ensuring robust and reliable ai systems](http://arxiv.org/abs/2405.06624). *CoRR*, May 2024.

[^65]: Fahim Dalvi, Nadir Durrani, Hassan Sajjad, Yonatan Belinkov, Anthony Bau, and James Glass. [What is one grain of sand in the desert? analyzing individual neurons in deep nlp models](https://ojs.aaai.org/index.php/AAAI/article/view/4592). *Proceedings of the AAAI Conference on Artificial Intelligence*, July 2019.

[^66]: Guy Dar, Mor Geva, Ankit Gupta, and Jonathan Berant. [Analyzing transformers in embedding space](http://arxiv.org/abs/2209.02535). *ACL*, December 2022.

[^67]: Xander Davies, Max Nadeau, Nikhil Prakash, Tamar Rott Shaham, and David Bau. [Discovering variable binding circuitry with desiderata](http://arxiv.org/abs/2307.03637). *CoRR*, July 2023.

[^68]: Mingyang Deng, Lucas Tao, and Joe Benton. [Measuring feature sparsity in language models](https://arxiv.org/abs/2310.07837). *CoRR*, 2023.

[^69]: Carson Denison, Monte MacDiarmid, Fazl Barez, David Duvenaud, Shauna Kravec, Samuel Marks, Nicholas Schiefer, Ryan Soklaski, Alex Tamkin, Jared Kaplan, Buck Shlegeris, Samuel R. Bowman, Ethan Perez, and Evan Hubinger. [Sycophancy to subterfuge: Investigating reward-tampering in large language models](https://arxiv.org/abs/2406.10162). *CoRR*, 2024.

[^70]: Alexander Yom Din, Taelin Karidi, Leshem Choshen, and Mor Geva. [Jump to conclusions: Short-cutting transformers with linear transformations](http://arxiv.org/abs/2303.09435). *CoRR*, March 2023.

[^71]: Finale Doshi-Velez and Been Kim. [Towards a rigorous science of interpretable machine learning](http://arxiv.org/abs/1702.08608). *CoRR*, March 2017.

[^72]: Keke Du, Shan Chang, Huixiang Wen, and Hao Zhang. [Fighting adversarial images with interpretable gradients](https://doi.org/10.1145/3472634.3472644). *ACM TURC*, October 2021.

[^73]: Jacob Dunefsky, Philippe Chlenski, and Neel Nanda. [Transcoders find interpretable llm feature circuits](https://openreview.net/forum?id=GWqzUR2dOX). *ICML MI Workshop*, June 2024.

[^74]: Nadir Durrani, Hassan Sajjad, Fahim Dalvi, and Yonatan Belinkov. [Analyzing individual neurons in pre-trained language models](http://arxiv.org/abs/2010.02695). *EMNLP*, October 2020.

[^75]: Yanai Elazar, Shauli Ravfogel, Alon Jacovi, and Yoav Goldberg. [Amnesic probing: Behavioral explanation with amnesic counterfactuals](http://arxiv.org/abs/2006.00995). *TACL*, February 2021.

[^76]: Nelson Elhage, Tristan Hume, Olsson Catherine, Nanda Neel, Tom Henighan, Scott Johnston, Sheer ElShowk, Nicholas Joseph, Nova DasSarma, Ben Mann, Danny Hernandez, Amanda Askell, Kamal Ndousse, Dawn Drain, Anna Chen, Yuntao Bai, Deep Ganguli, Liane Lovitt, Zac Hatfield-Dodds, Jackson Kernion, Tom Conerly, Shauna Kravec, Stanislav Fort, Saurav Kadavath, Josh Jacobson, Eli Tran-Johnson, Jared Kaplan, Jack Clark, Tom Brown, Sam McCandlish, Dario Amodei, and Christopher Olah. [Softmax linear units](https://transformer-circuits.pub/2022/solu/index.html). *Transformer Circuits Thread*, 2022a.

[^77]: Nelson Elhage, Tristan Hume, Catherine Olsson, Nicholas Schiefer, Tom Henighan, Shauna Kravec, Zac Hatfield-Dodds, Robert Lasenby, Dawn Drain, Carol Chen, et al. [Toy models of superposition](https://transformer-circuits.pub/2022/toy_model/index.html). *Transformer Circuits Thread*, 2022b.

[^78]: Joshua Engels, Isaac Liao, Eric J. Michaud, Wes Gurnee, and Max Tegmark. [Not all language model features are linear](http://arxiv.org/abs/2405.14860). *CoRR*, May 2024.

[^79]: Logan Engstrom, Andrew Ilyas, Shibani Santurkar, Dimitris Tsipras, Brandon Tran, and Aleksander Madry. [Adversarial robustness as a prior for learned representations](http://arxiv.org/abs/1906.00945). *CoRR*, September 2019.

[^80]: Ingrid Lossius Falkum and Agustin Vicente. [Polysemy: Current perspectives and approaches](https://linkinghub.elsevier.com/retrieve/pii/S0024384115000170). *Lingua*, April 2015.

[^81]: Sebastian Farquhar, Vikrant Varma, Zachary Kenton, Johannes Gasteiger, Vladimir Mikulik, and Rohin Shah. [Challenges with unsupervised llm knowledge discovery](https://arxiv.org/abs/2312.10029). *CoRR*, 2023.

[^82]: Amir Feder, Nadav Oved, Uri Shalit, and Roi Reichart. [Causalm: Causal model explanation through counterfactual language models](http://arxiv.org/abs/2005.13407). *Computational Linguistics*, May 2021.

[^83]: Jiahai Feng and Jacob Steinhardt. [How do language models bind entities in context?](http://arxiv.org/abs/2310.17191) *CoRR*, October 2023.

[^84]: Javier Ferrando and Elena Voita. [Information flow routes: Automatically interpreting language models at scale](http://arxiv.org/abs/2403.00824). *CoRR*, February 2024.

[^85]: Javier Ferrando, Gabriele Sarti, Arianna Bisazza, and Marta R. Costa-jussà. [A primer on the inner workings of transformer-based language models](http://arxiv.org/abs/2405.00208). *CoRR*, May 2024.

[^86]: Alex Foote, Neel Nanda, Esben Kran, Ioannis Konstas, Shay Cohen, and Fazl Barez. [Neuron to graph: Interpreting language model neurons at scale](http://arxiv.org/abs/2305.19911). *CoRR*, May 2023.

[^87]: Stanislav Fort and Balaji Lakshminarayanan. [Ensemble everything everywhere: Multi-scale aggregation for adversarial robustness](http://arxiv.org/abs/2408.05446). *CoRR*, August 2024.

[^88]: Jonathan Frankle and Michael Carbin. [The lottery ticket hypothesis: Finding sparse, trainable neural networks](http://arxiv.org/abs/1803.03635). *ICLR*, March 2019.

[^89]: Dan Friedman, Andrew Lampinen, Lucas Dixon, Danqi Chen, and Asma Ghandeharioun. [Interpretability illusions in the generalization of simplified models](https://www.semanticscholar.org/paper/e2bc390cf21dc319ea5aa9a7c3a223911dbf2012). *CoRR*, 2023a.

[^90]: Dan Friedman, Alexander Wettig, and Danqi Chen. [Learning transformer programs](http://arxiv.org/abs/2306.01128). *NeurIPS*, June 2023b.

[^91]: Daniel Y. Fu, Tri Dao, Khaled K. Saab, Armin W. Thomas, Atri Rudra, and Christopher Ré. [Hungry hungry hippos: Towards language modeling with state space models](http://arxiv.org/abs/2212.14052). *ICLR*, 2023a.

[^92]: Deqing Fu, Tian-Qi Chen, Robin Jia, and Vatsal Sharan. [Transformers learn higher-order optimization methods for in-context learning: A study with linear models](http://arxiv.org/abs/2310.17086). *CoRR*, October 2023b.

[^93]: Zach Furman and Edmund Lau. [Estimating the local learning coefficient at scale](http://arxiv.org/abs/2402.03698). *CoRR*, February 2024.

[^94]: Jorge García-Carrasco, Alejandro Maté, and Juan Trujillo. [Detecting and understanding vulnerabilities in language models via mechanistic interpretability](http://arxiv.org/abs/2407.19842). *IJCAI*, August 2024.

[^95]: Albert Garde, Esben Kran, and Fazl Barez. [Deepdecipher: Accessing and investigating neuron activation in large language models](http://arxiv.org/abs/2310.01870). *NeurIPS Workshop XAIA*, October 2023.

[^96]: Peter Gardenfors. *Conceptual spaces: The geometry of thought*. MIT press, 2004.

[^97]: Charles J. Garfinkle and Christopher J. Hillar. [On the uniqueness and stability of dictionaries for sparse representation of noisy signals](https://ieeexplore.ieee.org/abstract/document/8805108). *IEEE Transactions on Signal Processing*, December 2019.

[^98]: Xuyang Ge, Fukang Zhu, Wentao Shu, Junxuan Wang, Zhengfu He, and Xipeng Qiu. [Automatically identifying local and global circuits with linear computation graphs](https://www.semanticscholar.org/paper/Automatically-Identifying-Local-and-Global-Circuits-Ge-Zhu/9e08a7385a3908ecfaa7886c8597f8c533672ca0). *CoRR*, May 2024.

[^99]: Atticus Geiger, Hanson Lu, Thomas Icard, and Christopher Potts. [Causal abstractions of neural networks](https://proceedings.neurips.cc/paper/2021/hash/4f5c422f4d49a5a807eda27434231040-Abstract.html). *NeurIPS*, 2021a.

[^100]: Atticus Geiger, Zhengxuan Wu, Hanson Lu, Josh Rozner, Elisa Kreiss, Thomas Icard, Noah D. Goodman, and Christopher Potts. [Inducing causal structure for interpretable neural networks](http://arxiv.org/abs/2112.00826). *ICML*, January 2021b.

[^101]: Atticus Geiger, Chris Potts, and Thomas Icard. [Causal abstraction for faithful model interpretation](http://arxiv.org/abs/2301.04709). *CoRR*, January 2023a.

[^102]: Atticus Geiger, Zhengxuan Wu, Christopher Potts, Thomas Icard, and Noah D. Goodman. [Finding alignments between interpretable causal variables and distributed neural representations](https://arxiv.org/abs/2303.02536). *CoRR*, 2023b.

[^103]: Georgios Georgiadis. [Accelerating convolutional neural networks via activation map compression](http://arxiv.org/abs/1812.04056). *CoRR*, March 2019.

[^104]: Mor Geva, Avi Caciularu, Kevin Ro Wang, and Yoav Goldberg. [Transformer feed-forward layers build predictions by promoting concepts in the vocabulary space](http://arxiv.org/abs/2203.14680). *EMNLP*, October 2022.

[^105]: Mor Geva, Jasmijn Bastings, Katja Filippova, and Amir Globerson. [Dissecting recall of factual associations in auto-regressive language models](http://arxiv.org/abs/2304.14767). *EMNLP*, October 2023.

[^106]: Asma Ghandeharioun, Avi Caciularu, Adam Pearce, Lucas Dixon, and Mor Geva. [Patchscopes: A unifying framework for inspecting hidden representations of language models](http://arxiv.org/abs/2401.06102). *CoRR*, January 2024.

[^107]: Amirata Ghorbani and James Zou. [Neuron shapley: Discovering the responsible neurons](http://arxiv.org/abs/2002.09815). *NeurIPS*, November 2020.

[^108]: Gabriel Goh, Nick Cammarata †, Chelsea Voss †, Shan Carter, Michael Petrov, Ludwig Schubert, Alec Radford, and Chris Olah. [Multimodal neurons in artificial neural networks](https://distill.pub/2021/multimodal-neurons). *Distill*, March 2021.

[^109]: Nicholas Goldowsky-Dill, Chris MacLeod, Lucas Sato, and Aryaman Arora. [Localizing model behavior with path patching](https://arxiv.org/abs/2304.05969). *CoRR*, 2023.

[^110]: Liv Gorton. [The missing curve detectors of inceptionv1: Applying sparse autoencoders to inceptionv1 early vision](https://openreview.net/forum?id=IGnoozsfj1). *ICML MI Workshop*, June 2024.

[^111]: Rhys Gould, Euan Ong, George Ogden, and Arthur Conmy. [Successor heads: Recurring, interpretable attention heads in the wild](https://arxiv.org/abs/2312.09230). *CoRR*, 2023.

[^112]: Anirudh Goyal, Alex Lamb, Jordan Hoffmann, Shagun Sodhani, Sergey Levine, Yoshua Bengio, and Bernhard Schölkopf. [Recurrent independent mechanisms](http://arxiv.org/abs/1909.10893). *CoRR*, November 2020.

[^113]: Jason Gross, Rajashree Agrawal, Thomas Kwa, Euan Ong, Chun Hei Yip, Alex Gibson, Soufiane Noubir, and Lawrence Chan. [Compact proofs of model performance via mechanistic interpretability](http://arxiv.org/abs/2406.11779). *ICML MI Workshop*, June 2024.

[^114]: Roger Grosse, Juhan Bae, Cem Anil, Nelson Elhage, Alex Tamkin, Amirhossein Tajdini, Benoit Steiner, Dustin Li, Esin Durmus, Ethan Perez, Evan Hubinger, Kamilė Lukošiūtė, Karina Nguyen, Nicholas Joseph, Sam McCandlish, Jared Kaplan, and Samuel R. Bowman. [Studying large language model generalization with influence functions](http://arxiv.org/abs/2308.03296). *CoRR*, August 2023.

[^115]: Phillip Huang Guo, Aaquib Syed, Abhay Sheshadri, Aidan Ewart, and Gintare Karolina Dziugaite. [Robust unlearning via mechanistic localizations](https://openreview.net/forum?id=06pNzrEjnH). *ICML MI Workshop*, June 2024.

[^116]: Rohan Gupta, Iván Arcuschin, Thomas Kwa, and Adrià Garriga-Alonso. [Interpbench: Semi-synthetic transformers for evaluating mechanistic interpretability techniques](http://arxiv.org/abs/2407.14494). *CoRR*, July 2024.

[^117]: Wes Gurnee and Max Tegmark. [Language models represent space and time](https://arxiv.org/abs/2310.02207). *ICLR*, 2024.

[^118]: Wes Gurnee, Neel Nanda, Matthew Pauly, Katherine Harvey, Dmitrii Troitskii, and Dimitris Bertsimas. [Finding neurons in a haystack: Case studies with sparse probing](https://arxiv.org/abs/2305.01610). *TMLR*, 2023.

[^119]: Wes Gurnee, Theo Horsley, Zifan Carl Guo, Tara Rezaei Kheirkhah, Qinyi Sun, Will Hathaway, Neel Nanda, and Dimitris Bertsimas. [Universal neurons in gpt2 language models](http://arxiv.org/abs/2401.12181). *CoRR*, January 2024.

[^120]: David R. Ha and J. Schmidhuber. [Recurrent world models facilitate policy evolution](https://www.semanticscholar.org/paper/Recurrent-World-Models-Facilitate-Policy-Evolution-Ha-Schmidhuber/41cca0b0a27ba363ca56e7033569aeb1922b0ac9). *NeurIPS*, September 2018.

[^121]: Guy Hacohen, Leshem Choshen, and Daphna Weinshall. [Let’s agree to agree: Neural networks share classification order on real datasets](http://arxiv.org/abs/1905.10854). *ICML*, 2020.

[^122]: Michael Hanna, Ollie Liu, and Alexandre Variengien. [How does gpt-2 compute greater-than?: Interpreting mathematical abilities in a pre-trained language model](https://arxiv.org/abs/2305.00586). *NeurIPS*, 2023.

[^123]: Michael Hanna, Sandro Pezzelle, and Yonatan Belinkov. [Have faith in faithfulness: Going beyond circuit overlap when finding model mechanisms](https://openreview.net/forum?id=grXgesr5dT). *ICML MI Workshop*, June 2024.

[^124]: Kaarel Hänni, Jake Mendel, Dmitry Vaintrob, and Lawrence Chan. [Mathematical models of computation in superposition](http://arxiv.org/abs/2408.05451). *ICML MI Workshop*, August 2024.

[^125]: Peter Hase, Mohit Bansal, Been Kim, and Asma Ghandeharioun. [Does localization inform editing? surprising differences in causality-based localization vs. knowledge editing in language models](http://arxiv.org/abs/2301.04213). *NeurIPS Spotlight*, January 2023.

[^126]: Zhengfu He, Xuyang Ge, Qiong Tang, Tianxiang Sun, Qinyuan Cheng, and Xipeng Qiu. [Dictionary learning improves patch-free circuit discovery in mechanistic interpretability: A case study on othello-gpt](https://arxiv.org/abs/2402.12201). *CoRR*, 2024.

[^127]: Stefan Heimersheim and Jett. [A circuit for python docstrings in a 4-layer attention-only transformer](https://www.alignmentforum.org/posts/u6KXXmKFbXfWzoAXn/a-circuit-for-python-docstrings-in-a-4-layer-attention-only). *AI Alignment Forum*, February 2023.

[^128]: Roee Hendel, Mor Geva, and Amir Globerson. [In-context learning creates task vectors](http://arxiv.org/abs/2310.15916). *EMNLP*, October 2023.

[^129]: Dan Hendrycks. [*Introduction to AI Safety, Ethics, and Society*](https://www.aisafetybook.com/). Self-published, 2023a.

[^130]: Dan Hendrycks. [Natural selection favors ais over humans](http://arxiv.org/abs/2303.16200). *CoRR*, July 2023b.

[^131]: Dan Hendrycks and Mantas Mazeika. [X-risk analysis for ai research](https://arxiv.org/abs/2206.05862v7). *CoRR*, June 2022.

[^132]: Dan Hendrycks, Nicholas Carlini, John Schulman, and Jacob Steinhardt. [Unsolved problems in ml safety](http://arxiv.org/abs/2109.13916). *CoRR*, June 2022.

[^133]: Dan Hendrycks, Mantas Mazeika, and Thomas Woodside. [An overview of catastrophic ai risks](http://arxiv.org/abs/2306.12001). *CoRR*, October 2023.

[^134]: Tom Henighan, Shan Carter, Tristan Hume, Nelson Elhage, Robert Lasenby, Stanislav Fort, Nicholas Schiefer, and Christopher Olah. [Superposition, memorization, and double descent](https://transformer-circuits.pub/2023/toy-double-descent/index.html). *Transformer Circuits Thread*, 2023.

[^135]: Danny Hernandez, Tom Brown, Tom Conerly, Nova DasSarma, Dawn Drain, Sheer El-Showk, Nelson Elhage, Zac Hatfield-Dodds, Tom Henighan, Tristan Hume, Scott Johnston, Ben Mann, Chris Olah, Catherine Olsson, Dario Amodei, Nicholas Joseph, Jared Kaplan, and Sam McCandlish. [Scaling laws and interpretability of learning from repeated data](https://arxiv.org/abs/2205.10487). *CoRR*, 2022.

[^136]: Evan Hernandez, Arnab Sen Sharma, Tal Haklay, Kevin Meng, Martin Wattenberg, Jacob Andreas, Yonatan Belinkov, and David Bau. [Linearity of relation decoding in transformer language models](http://arxiv.org/abs/2308.09124). *CoRR*, August 2023.

[^137]: John Hewitt and Christopher D. Manning. [A structural probe for finding syntax in word representations](https://aclanthology.org/N19-1419). *NAACL HLT*, June 2019.

[^138]: Irina Higgins, David Amos, David Pfau, Sebastien Racaniere, Loic Matthey, Danilo Rezende, and Alexander Lerchner. [Towards a definition of disentangled representations](http://arxiv.org/abs/1812.02230). *CoRR*, December 2018.

[^139]: Jacob Hilton, Nick Cammarata, Shan Carter, Gabriel Goh, and Chris Olah. [Understanding rl vision](https://distill.pub/2020/understanding-rl-vision/). *Distill*, 2020.

[^140]: Geoffrey E Hinton. [Distributed representations](https://www.cs.toronto.edu/~hinton/absps/pdp3.pdf). *Carnegie Mellon University*, 1984.

[^141]: Marius Hobbhahn. [Marius’ alignment agenda](https://docs.google.com/document/d/1AyuTphQ31rLHDtpZoEwEPb4fWbZna1H3hGx_YUACxk4), 2022.

[^142]: Marius Hobbhahn and Lawrence Chan. [Should we publish mechanistic interpretability research?](https://www.alignmentforum.org/posts/iDNEjbdHhjzvLLAmm/should-we-publish-mechanistic-interpretability-research) *AI Alignment Forum*, April 2023.

[^143]: Jordan Hoffmann, Sebastian Borgeaud, Arthur Mensch, Elena Buchatskaya, Trevor Cai, Eliza Rutherford, Diego de Las Casas, Lisa Anne Hendricks, Johannes Welbl, Aidan Clark, Tom Hennigan, Eric Noland, Katie Millican, George van den Driessche, Bogdan Damoc, Aurelia Guy, Simon Osindero, Karen Simonyan, Erich Elsen, Jack W. Rae, Oriol Vinyals, and Laurent Sifre. [Training compute-optimal large language models](http://arxiv.org/abs/2203.15556). *CoRR*, March 2022.

[^144]: Jesse Hoogland, Liam Carroll, and Daniel Murfet. [Stagewise development in neural networks](https://www.lesswrong.com/posts/Zza9MNA7YtHkzAtit/stagewise-development-in-neural-networks). *AI Alignment Forum*, March 2024.

[^145]: Jing Huang, Atticus Geiger, Karel D’Oosterlinck, Zhengxuan Wu, and Christopher Potts. [Rigorously assessing natural language explanations of neurons](http://arxiv.org/abs/2309.10312). *CoRR*, September 2023.

[^146]: Jing Huang, Zhengxuan Wu, Christopher Potts, Mor Geva, and Atticus Geiger. [Ravel: Evaluating interpretability methods on disentangling language model representations](https://arxiv.org/abs/2402.17700). *CoRR*, 2024.

[^147]: Evan Hubinger. [Chris olah’s views on agi safety](https://www.alignmentforum.org/posts/X2i9dQQK3gETCyqh2/chris-olah-s-views-on-agi-safety). *AI Alignment Forum*, November 2019a.

[^148]: Evan Hubinger. [Gradient hacking](https://www.alignmentforum.org/posts/uXH4r6MmKPedk8rMA/gradient-hacking). *AI Alignment Forum*, October 2019b.

[^149]: Evan Hubinger. [An overview of 11 proposals for building safe advanced ai](http://arxiv.org/abs/2012.07532). *CoRR*, December 2020.

[^150]: Evan Hubinger. [A transparency and interpretability tech tree](https://www.alignmentforum.org/posts/nbq2bWLcYmSGup9aF/a-transparency-and-interpretability-tech-tree). *AI Alignment Forum*, June 2022.

[^151]: Evan Hubinger, Chris van Merwijk, Vladimir Mikulik, Joar Skalse, and Scott Garrabrant. [Risks from learned optimization in advanced machine learning systems](http://arxiv.org/abs/1906.01820). *CoRR*, May 2019.

[^152]: Evan Hubinger, Adam Jermyn, Johannes Treutlein, Rubi Hudson, and Kate Woolverton. [Conditioning predictive models: Risks and strategies](http://arxiv.org/abs/2302.00805). *CoRR*, February 2023.

[^153]: Evan Hubinger, Carson Denison, Jesse Mu, Mike Lambert, Meg Tong, Monte MacDiarmid, Tamera Lanham, Daniel M. Ziegler, Tim Maxwell, Newton Cheng, Adam Jermyn, Amanda Askell, Ansh Radhakrishnan, Cem Anil, David Duvenaud, Deep Ganguli, Fazl Barez, Jack Clark, Kamal Ndousse, Kshitij Sachan, Michael Sellitto, Mrinank Sharma, Nova DasSarma, Roger Grosse, Shauna Kravec, Yuntao Bai, Zachary Witten, Marina Favaro, Jan Brauner, Holden Karnofsky, Paul Christiano, Samuel R. Bowman, Logan Graham, Jared Kaplan, Sören Mindermann, Ryan Greenblatt, Buck Shlegeris, Nicholas Schiefer, and Ethan Perez. [Sleeper agents: Training deceptive llms that persist through safety training](https://arxiv.org/abs/2401.05566). *CoRR*, 2024.

[^154]: Andrew Ilyas, Shibani Santurkar, Dimitris Tsipras, Logan Engstrom, Brandon Tran, and Aleksander Madry. [Adversarial examples are not bugs, they are features](http://arxiv.org/abs/1905.02175). *NeurIPS*, August 2019.

[^155]: M. Ivanitskiy, Alexander F. Spies, Tilman Rauker, Guillaume Corlouer, Chris Mathwin, Lucia Quirke, Can Rager, Rusheb Shah, Dan Valentine, Cecilia Diniz Behn, Katsumi Inoue, and Samy Wu Fung. [Structured world representations in maze-solving transformers](https://arxiv.org/abs/2312.02566). *CoRR*, December 2023.

[^156]: Alon Jacovi and Yoav Goldberg. [Towards faithfully interpretable nlp systems: How should we define and evaluate faithfulness?](http://arxiv.org/abs/2004.03685) *CoRR*, April 2020.

[^157]: Samyak Jain, Robert Kirk, Ekdeep Singh Lubana, Robert P. Dick, Hidenori Tanaka, Edward Grefenstette, Tim Rocktäschel, and David Scott Krueger. [Mechanistically analyzing the effects of fine-tuning on procedurally defined tasks](http://arxiv.org/abs/2311.12786). *CoRR*, November 2023.

[^158]: Samyak Jain, Ekdeep Singh Lubana, Kemal Oksuz, Tom Joy, Philip Torr, Amartya Sanyal, and Puneet K. Dokania. [What makes and breaks safety fine-tuning? a mechanistic study](https://openreview.net/forum?id=BS2CbUkJpy). *ICML MI Workshop*, June 2024.

[^159]: janus. [Simulators](https://www.lesswrong.com/posts/vJFdjigzmcXMhNTsx/simulators). *LessWrong*, September 2022.

[^160]: Erik Jenner, Adrià Garriga-alonso, and Egor Zverev. [A comparison of causal scrubbing, causal abstractions, and related methods](https://www.alignmentforum.org/posts/uLMWMeBG3ruoBRhMW/a-comparison-of-causal-scrubbing-causal-abstractions-and). *AI Alignment Forum*, June 2023.

[^161]: Erik Jenner, Shreyas Kapur, Vasil Georgiev, Cameron Allen, Scott Emmons, and Stuart Russell. [Evidence of learned look-ahead in a chess-playing neural network](http://arxiv.org/abs/2406.00877). *CoRR*, June 2024.

[^162]: Adam Jermyn, Chris Olah, and T Henighan. [Circuits updates - may 2023: Attention head superposition](https://transformer-circuits.pub/2023/may-update/index.html). *Transformer Circuits Thread*, 2023.

[^163]: Adam S. Jermyn, Nicholas Schiefer, and Evan Hubinger. [Engineering monosemanticity in toy models](http://arxiv.org/abs/2211.09169). *CoRR*, November 2022.

[^164]: Jiaming Ji, Tianyi Qiu, Boyuan Chen, Borong Zhang, Hantao Lou, Kaile Wang, Yawen Duan, Zhonghao He, Jiayi Zhou, Zhaowei Zhang, Fanzhi Zeng, Kwan Yee Ng, Juntao Dai, Xuehai Pan, Aidan O’Gara, Yingshan Lei, Hua Xu, Brian Tse, Jie Fu, Stephen McAleer, Yaodong Yang, Yizhou Wang, Song-Chun Zhu, Yike Guo, and Wen Gao. [Ai alignment: A comprehensive survey](http://arxiv.org/abs/2310.19852). *CoRR*, January 2024.

[^165]: Jozdien. [Conditioning generative models for alignment](https://www.alignmentforum.org/posts/JqnkeqaPseTgxLgEL/conditioning-generative-models-for-alignment). *AI Alignment Forum*, July 2022.

[^166]: Jaap Jumelet. [Evaluating and interpreting language models](https://cl-illc.github.io/nlp1-2023/resources/slides/NLP1-Lecture-Interpretability.pdf). *NLP Lecture*, November 2023.

[^167]: Amlan Jyoti, Karthik Balaji Ganesh, Manoj Gayala, Nandita Lakshmi Tunuguntla, Sandesh Kamath, and Vineeth N. Balasubramanian. [On the robustness of explanations of deep neural network models: A survey](http://arxiv.org/abs/2211.04780). *CoRR*, November 2022.

[^168]: Adam Karvonen. [Emergent world models and latent variable estimation in chess-playing language models](http://arxiv.org/abs/2403.15498). *COLM*, July 2024.

[^169]: Adam Karvonen, Benjamin Wright, Can Rager, Rico Angell, Jannik Brinkmann, Logan Smith, C. M. Verdun, David Bau, and Samuel Marks. [Measuring progress in dictionary learning for language model interpretability with board game models](https://www.semanticscholar.org/paper/Measuring-Progress-in-Dictionary-Learning-for-Model-Karvonen-Wright/b244b80d0a2d36412b56c0156532d9cbeb298ffa). *ICML MI Workshop (Oral)*, July 2024.

[^170]: Theodoros Kasioumis, Joe Townsend, and Hiroya Inakoshi. [Elite backprop: Training sparse interpretable neurons](https://ceur-ws.org/Vol-2986/paper6.pdf). *International Workshop on Neuro-Symbolic Learning and Reasoning*, 2021.

[^171]: Nan Rosemary Ke, Aniket Didolkar, Sarthak Mittal, Anirudh Goyal, Guillaume Lajoie, Stefan Bauer, Danilo Rezende, Yoshua Bengio, Michael Mozer, and Christopher Pal. [Systematic evaluation of causal discovery in visual model based reinforcement learning](http://arxiv.org/abs/2107.00848). *CoRR*, July 2021.

[^172]: Jan Kirchner. [Neuroscience and natural abstractions](https://www.lesswrong.com/posts/WGFtgFKuLFMvLuET3/jan-s-shortform). *LessWrong*, March 2023.

[^173]: Connor Kissane, Robert Krzyzanowski, Joseph Isaac Bloom, Arthur Conmy, and Neel Nanda. [Interpreting attention layer outputs with sparse autoencoders](https://openreview.net/forum?id=fewUBDwjji). *ICML MI Workshop*, June 2024.

[^174]: Simon Kornblith, Mohammad Norouzi, Honglak Lee, and Geoffrey Hinton. [Similarity of neural network representations revisited](http://arxiv.org/abs/1905.00414). *ICML*, July 2019.

[^175]: János Kramár, Tom Lieberum, Rohin Shah, and Neel Nanda. [Atp\*: An efficient and scalable method for localizing llm behaviour to components](http://arxiv.org/abs/2403.00745). *CoRR*, March 2024.

[^176]: Nicholas Kross. [Why and when interpretability work is dangerous](https://www.lesswrong.com/posts/uhMRgEXabYbWeLc6T/why-and-when-interpretability-work-is-dangerous). *LessWrong*, May 2023.

[^177]: Jan Kulveit. Risks from ai misalignment at different scales, July 2024.

[^178]: Jan Kulveit, Clem von Stengel, and Roman Leventov. [Predictive minds: Llms as atypical active inference agents](http://arxiv.org/abs/2311.10215). *CoRR*, November 2023.

[^179]: Max Lamparth and Anka Reuel. [Analyzing and editing inner mechanisms of backdoored language models](https://arxiv.org/abs/2302.12461). *CoRR*, 2023.

[^180]: Michael Lan and Fazl Barez. [Locating cross-task sequence continuation circuits in transformers](https://www.semanticscholar.org/paper/Locating-Cross-Task-Sequence-Continuation-Circuits-Lan-Barez/458642a7695013cd128bb3070540970be814e50d?citedSort=relevance&citedPage=2). *CoRR*, November 2023.

[^181]: Leon Lang, Davis Foote, Stuart Russell, Anca Dragan, Erik Jenner, and Scott Emmons. [When your ais deceive you: Challenges with partial observability of human evaluators in reward learning](http://arxiv.org/abs/2402.17747). *CoRR*, March 2024.

[^182]: Georg Lange, Alex Makelov, and Neel Nanda. [An interpretability illusion for activation patching of arbitrary subspaces](https://www.alignmentforum.org/posts/RFtkRXHebkwxygDe2/an-interpretability-illusion-for-activation-patching-of). *AI Alignment Forum*, August 2023.

[^183]: Anna Langedijk, Hosein Mohebbi, Gabriele Sarti, Willem Zuidema, and Jaap Jumelet. [Decoderlens: Layerwise interpretation of encoder-decoder transformers](https://arxiv.org/abs/2310.03686). *CoRR*, 2023.

[^184]: Edmund Lau, Daniel Murfet, and Susan Wei. [Quantifying degeneracy in singular models via the learning coefficient](http://arxiv.org/abs/2308.12108). *CoRR*, August 2023.

[^185]: Connor Leahy. [Barriers to mechanistic interpretability for agi safety](https://www.lesswrong.com/posts/KRDo2afKJtD7bzSM8/barriers-to-mechanistic-interpretability-for-agi-safety). *AI Alignment Forum*, 2023.

[^186]: Matthew L. Leavitt and Ari Morcos. [Towards falsifiable interpretability research](http://arxiv.org/abs/2010.12016). *CoRR*, October 2020.

[^187]: Victor Lecomte, Kushal Thaman, Trevor Chow, Rylan Schaeffer, and Sanmi Koyejo. [Incidental polysemanticity](https://arxiv.org/abs/2312.03096). *CoRR*, 2023.

[^188]: Andrew Lee, Xiaoyan Bai, Itamar Pres, Martin Wattenberg, Jonathan K. Kummerfeld, and Rada Mihalcea. [A mechanistic understanding of alignment algorithms: A case study on dpo and toxicity](https://arxiv.org/abs/2401.01967). *CoRR*, 2024.

[^189]: Belinda Z. Li, Maxwell Nye, and Jacob Andreas. [Implicit representations of meaning in neural language models](https://aclanthology.org/2021.acl-long.143). *ACL-IJCNLP*, August 2021.

[^190]: Kenneth Li, Aspen K. Hopkins, David Bau, Fernanda Viégas, Hanspeter Pfister, and Martin Wattenberg. [Emergent world representations: Exploring a sequence model trained on a synthetic task](https://arxiv.org/abs/2210.13382). *ICLR*, 2023a.

[^191]: Kenneth Li, Oam Patel, Fernanda Viégas, Hanspeter Pfister, and Martin Wattenberg. [Inference-time intervention: Eliciting truthful answers from a language model](http://arxiv.org/abs/2306.03341). *NeurIPS Spotlight*, July 2023b.

[^192]: Yixuan Li, Jason Yosinski, Jeff Clune, Hod Lipson, and John Hopcroft. [Convergent learning: Do different neural networks learn the same representations?](https://proceedings.mlr.press/v44/li15convergent.html) *NIPS Workshop on Feature Extraction*, December 2015.

[^193]: Tom Lieberum, Matthew Rahtz, János Kramár, Neel Nanda, Geoffrey Irving, Rohin Shah, and Vladimir Mikulik. [Does circuit analysis interpretability scale? evidence from multiple choice capabilities in chinchilla](http://arxiv.org/abs/2307.09458). *CoRR*, July 2023.

[^194]: David Lindner, János Kramár, Sebastian Farquhar, Matthew Rahtz, Thomas McGrath, and Vladimir Mikulik. [Tracr: Compiled transformers as a laboratory for interpretability](https://arxiv.org/abs/2301.05062). *CoRR*, 2023.

[^195]: Leo Z. Liu, Yizhong Wang, Jungo Kasai, Hannaneh Hajishirzi, and Noah A. Smith. [Probing across time: What does roberta know and when?](http://arxiv.org/abs/2104.07885) *EMNLP*, September 2021.

[^196]: Ziming Liu and Max Tegmark. [A neural scaling law from lottery ticket ensembling](https://arxiv.org/abs/2310.02258). *CoRR*, 2023.

[^197]: Ziming Liu, Ouail Kitouni, Niklas Nolte, Eric J. Michaud, Max Tegmark, and Mike Williams. [Towards understanding grokking: An effective theory of representation learning](https://arxiv.org/abs/2205.10343). *NeurIPS*, 2022a.

[^198]: Ziming Liu, Eric J. Michaud, and Max Tegmark. [Omnigrok: Grokking beyond algorithmic data](https://arxiv.org/abs/2210.01117). *ICML*, 2022b.

[^199]: Ziming Liu, Eric Gan, and Max Tegmark. [Seeing is believing: Brain-inspired modular training for mechanistic interpretability](http://arxiv.org/abs/2305.08746). *Entropy*, June 2023a.

[^200]: Ziming Liu, Mikail Khona, Ila R. Fiete, and Max Tegmark. [Growing brains: Co-emergence of anatomical and functional modularity in recurrent neural networks](https://arxiv.org/abs/2310.07711). *CoRR*, 2023b.

[^201]: Ziming Liu, Ziqian Zhong, and Max Tegmark. [Grokking as compression: A nonlinear complexity perspective](https://www.semanticscholar.org/paper/8cd06e9959151ea852f2842a87ed65ff0a47182f). *CoRR*, 2023c.

[^202]: Francesco Locatello, Stefan Bauer, Mario Lucic, Gunnar Rätsch, Sylvain Gelly, Bernhard Schölkopf, and Olivier Bachem. [Challenging common assumptions in the unsupervised learning of disentangled representations](http://arxiv.org/abs/1811.12359). *CoRR*, June 2019.

[^203]: Chris Lu, Cong Lu, Robert Tjarko Lange, Jakob Foerster, Jeff Clune, and David Ha. [The ai scientist: Towards fully automated open-ended scientific discovery](http://arxiv.org/abs/2408.06292). *CoRR*, August 2024.

[^204]: Ang Lv, Yuhan Chen, Kaiyi Zhang, Yulong Wang, Lifeng Liu, Ji-Rong Wen, Jian Xie, and Rui Yan. [Interpreting key mechanisms of factual recall in transformer-based language models](https://arxiv.org/abs/2403.19521). *CoRR*, 2024.

[^205]: Sara Magliacane, Thijs van Ommen, Tom Claassen, Stephan Bongers, Philip Versteeg, and Joris M. Mooij. [Domain adaptation by using causal inference to predict invariant conditional distributions](http://arxiv.org/abs/1707.06422). *NeurIPS*, October 2018.

[^206]: Aleksandar Makelov. [Sparse autoencoders match supervised features for model steering on the ioi task](https://openreview.net/forum?id=JdrVuEQih5). *ICML MI Workshop*, June 2024.

[^207]: Aleksandar Makelov, George Lange, and Neel Nanda. [Towards principled evaluations of sparse autoencoders for interpretability and control](http://arxiv.org/abs/2405.08366). *CoRR*, May 2024.

[^208]: Alex Mallen and Nora Belrose. [Eliciting latent knowledge from quirky language models](http://arxiv.org/abs/2312.01037). *CoRR*, December 2023.

[^209]: Narek Maloyan, Ekansh Verma, Bulat Nutfullin, and Bislan Ashinov. [Trojan detection in large language models: Insights from the trojan detection challenge](http://arxiv.org/abs/2404.13660). *CoRR*, April 2024.

[^210]: Giovanni Luca Marchetti, Christopher Hillar, Danica Kragic, and Sophia Sanborn. [Harmonics of learning: Universal fourier features emerge in invariant networks](http://arxiv.org/abs/2312.08550). *CoRR*, December 2023.

[^211]: Luke Marks, Amir Abdullah, Luna Mendez, Rauno Arike, Philip Torr, and Fazl Barez. [Interpreting reward models in rlhf-tuned language models using sparse autoencoders](https://www.semanticscholar.org/paper/Interpreting-Reward-Models-in-RLHF-Tuned-Language-Marks-Abdullah/53b9f8c62837b12d3955778f737678aabea39a83). *CoRR*, October 2023a.

[^212]: Luke Marks, Amir Abdullah, Clement Neo, Rauno Arike, Philip Torr, and Fazl Barez. [Beyond training objectives: Interpreting reward model divergence in large language models](https://arxiv.org/abs/2310.08164). *CoRR*, October 2023b.

[^213]: Samuel Marks, Can Rager, Eric J. Michaud, Yonatan Belinkov, David Bau, and Aaron Mueller. [Sparse feature circuits: Discovering and editing interpretable causal graphs in language models](http://arxiv.org/abs/2403.19647). *CoRR*, March 2024.

[^214]: Simon C. Marshall and Jan H. Kirchner. [Understanding polysemanticity in neural networks through coding theory](http://arxiv.org/abs/2401.17975). *CoRR*, January 2024.

[^215]: Mantas Mazeika, Andy Zou, Akul Arora, Pavel Pleskov, Dawn Song, Dan Hendrycks, Bo Li, and David Forsyth. [How hard is trojan detection in dnns? fooling detectors with evasive trojans](https://openreview.net/forum?id=V-RDBWYf0go). *CoRR*, September 2022.

[^216]: Callum McDougall, Arthur Conmy, Cody Rushing, Thomas McGrath, and Neel Nanda. [Copy suppression: Comprehensively understanding an attention head](http://arxiv.org/abs/2310.04625). *CoRR*, October 2023.

[^217]: Thomas McGrath, Andrei Kapishnikov, Nenad Tomašev, Adam Pearce, Martin Wattenberg, Demis Hassabis, Been Kim, Ulrich Paquet, and Vladimir Kramnik. [Acquisition of chess knowledge in alphazero](https://www.pnas.org/doi/abs/10.1073/pnas.2206625119). *PNAS*, November 2022.

[^218]: Thomas McGrath, Matthew Rahtz, Janos Kramar, Vladimir Mikulik, and Shane Legg. [The hydra effect: Emergent self-repair in language model computations](http://arxiv.org/abs/2307.15771). *CoRR*, July 2023.

[^219]: Kevin Meng, David Bau, Alex Andonian, and Yonatan Belinkov. [Locating and editing factual associations in gpt](http://arxiv.org/abs/2202.05262). *NeurIPS*, 2022a.

[^220]: Kevin Meng, Arnab Sen Sharma, Alex Andonian, Yonatan Belinkov, and David Bau. [Mass-editing memory in a transformer](http://arxiv.org/abs/2210.07229). *ICLR*, 2022b.

[^221]: William Merrill, Nikolaos Tsilivis, and Aman Shukla. [A tale of two circuits: Grokking as competition of sparse and dense subnetworks](https://arxiv.org/abs/2303.11873). *CoRR*, 2023.

[^222]: Jack Merullo, Carsten Eickhoff, and Ellie Pavlick. [A mechanism for solving relational tasks in transformer language models](https://www.semanticscholar.org/paper/A-Mechanism-for-Solving-Relational-Tasks-in-Models-Merullo-Eickhoff/823fb3163a5600b5be957fc8337a9f7cdd177fef). *CoRR*, May 2023.

[^223]: Eric J. Michaud, Ziming Liu, Uzay Girit, and Max Tegmark. [The quantization model of neural scaling](http://arxiv.org/abs/2303.13506). *CoRR*, March 2023.

[^224]: Eric J. Michaud, Isaac Liao, Vedang Lad, Ziming Liu, Anish Mudide, Chloe Loughridge, Zifan Carl Guo, Tara Rezaei Kheirkhah, Mateja Vukelić, and Max Tegmark. [Opening the ai black box: program synthesis via mechanistic interpretability](http://arxiv.org/abs/2402.05110). *CoRR*, February 2024.

[^225]: Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg Corrado, and Jeffrey Dean. [Distributed representations of words and phrases and their compositionality](http://arxiv.org/abs/1310.4546). *NeurIPS*, October 2013.

[^226]: Joseph Miller and Clement Neo. [We found an neuron in gpt-2](https://www.alignmentforum.org/posts/cgqh99SHsCv3jJYDS/we-found-an-neuron-in-gpt-2). *AI Alignment Forum*, February 2023.

[^227]: Tim Miller. [Explanation in artificial intelligence: Insights from the social sciences](https://www.sciencedirect.com/science/article/pii/S0004370218305988). *Artificial Intelligence*, February 2019.

[^228]: Ulisse Mini, Peli Grietzer, Mrinank Sharma, Austin Meek, Monte MacDiarmid, and Alexander Matt Turner. [Understanding and controlling a maze-solving policy network](http://arxiv.org/abs/2310.08043). *CoRR*, October 2023.

[^229]: Giovanni Monea, Maxime Peyrard, Martin Josifoski, Vishrav Chaudhary, Jason Eisner, Emre Kıcıman, Hamid Palangi, Barun Patra, and Robert West. [A glitch in the matrix? locating and detecting language model grounding with fakepedia](https://arxiv.org/abs/2312.02073). *CoRR*, 2023.

[^230]: Basel Mousi, Nadir Durrani, and Fahim Dalvi. [Can llms facilitate interpretation of pre-trained language models?](https://arxiv.org/abs/2305.13386) *EMNLP*, 2023.

[^231]: Jesse Mu and Jacob Andreas. [Compositional explanations of neurons](http://arxiv.org/abs/2006.14032). *NeurIPS*, June 2020.

[^232]: Aaron Mueller, Jannik Brinkmann, Millicent Li, Samuel Marks, Koyena Pal, Nikhil Prakash, Can Rager, Aruna Sankaranarayanan, Arnab Sen Sharma, Jiuding Sun, Eric Todd, David Bau, and Yonatan Belinkov. [The quest for the right mediator: A history, survey, and theoretical grounding of causal interpretability](https://arxiv.org/abs/2408.01416). *CoRR*, August 2024.

[^233]: Jatin Nainani. [Evaluating brain-inspired modular training in automated circuit discovery for mechanistic interpretability](http://arxiv.org/abs/2401.03646). *CoRR*, January 2024.

[^234]: Preetum Nakkiran, Gal Kaplun, Dimitris Kalimeris, Tristan Yang, Benjamin L. Edelman, Fred Zhang, and Boaz Barak. [Sgd on neural networks learns functions of increasing complexity](http://arxiv.org/abs/1905.11604). *NeurIPS*, May 2019.

[^235]: Neel Nanda. [200 cop in mi: Looking for circuits in the wild](https://www.lesswrong.com/posts/XNjRwEX9kxbpzWFWd/200-cop-in-mi-looking-for-circuits-in-the-wild). *Neel Nanda’s Blog*, 2022a.

[^236]: Neel Nanda. [200 cop in mi: Analysing training dynamics](https://www.lesswrong.com/posts/hHaXzJQi6SKkeXzbg/200-cop-in-mi-analysing-training-dynamics). *Neel Nanda’s Blog*, 2022b.

[^237]: Neel Nanda. [200 cop in mi: Studying learned features in language models](https://www.lesswrong.com/posts/Qup9gorqpd9qKAEav/200-cop-in-mi-studying-learned-features-in-language-models). *Neel Nanda’s Blog*, 2022c.

[^238]: Neel Nanda. [A comprehensive mechanistic interpretability explainer & glossary](https://www.neelnanda.io/mechanistic-interpretability/glossary). *Neel Nanda’s Blog*, December 2022d.

[^239]: Neel Nanda. [A longlist of theories of impact for interpretability](https://www.alignmentforum.org/posts/uK6sQCNMw8WKzJeCQ/a-longlist-of-theories-of-impact-for-interpretability). *AI Alignment Forum*, March 2022e.

[^240]: Neel Nanda. [200 cop in mi: Exploring polysemanticity and superposition](https://www.lesswrong.com/posts/o6ptPu7arZrqRCxyz/200-cop-in-mi-exploring-polysemanticity-and-superposition). *Neel Nanda’s Blog*, January 2023a.

[^241]: Neel Nanda. [200 cop in mi: Techniques, tooling and automation](https://www.lesswrong.com/posts/btasQF7wiCYPsr5qw/200-cop-in-mi-techniques-tooling-and-automation). *Neel Nanda’s Blog*, 2023b.

[^242]: Neel Nanda. [Actually, othello-gpt has a linear emergent world representation](https://neelnanda.io/mechanistic-interpretability/othello). *Neel Nanda’s Blog*, March 2023c.

[^243]: Neel Nanda. [Attribution patching: Activation patching at industrial scale](https://www.neelnanda.io/mechanistic-interpretability/attribution-patching). *Neel Nanda’s Blog*, February 2023d.

[^244]: Neel Nanda. [How to think about activation patching](https://www.lesswrong.com/posts/xh85KbTFhbCz7taD4/how-to-think-about-activation-patching). *AI Alignment Forum*, April 2023e.

[^245]: Neel Nanda. [Mechanistic interpretability quickstart guide](https://www.neelnanda.io/mechanistic-interpretability/quickstart). *Neel Nanda’s Blog*, January 2023f.

[^246]: Neel Nanda. [An extremely opinionated annotated list of my favourite mechanistic interpretability papers v2](https://www.alignmentforum.org/posts/NfFST5Mio7BCAQHPA/an-extremely-opinionated-annotated-list-of-my-favourite). *AI Alignment Forum*, July 2024.

[^247]: Neel Nanda, Lawrence Chan, Tom Lieberum, Jess Smith, and Jacob Steinhardt. [Progress measures for grokking via mechanistic interpretability](http://arxiv.org/abs/2301.05217). *ICLR*, January 2023a.

[^248]: Neel Nanda, Andrew Lee, and Martin Wattenberg. [Emergent linear representations in world models of self-supervised sequence models](http://arxiv.org/abs/2309.00941). *BlackboxNLP Workshop on Analyzing and Interpreting Neural Networks for NLP*, September 2023b.

[^249]: Neel Nanda, S. Rajamanoharan, J. Kramár, and R. Shah. [Fact finding: Attempting to reverse-engineer factual recall on the neuron level](https://scholar.google.com/scholar?cluster=9546235215420404405&hl=en&oi=scholarr). *AI Alignment Forum, 2023c. URL https://www. alignmentforum. org/posts/iGuwZTHWb6DFY3sKB/fact-finding-attempting-to-reverse-engineer-factual-recall*, 2023c.

[^250]: Geraldin Nanfack, Alexander Fulleringer, Jonathan Marty, Michael Eickenberg, and Eugene Belilovsky. [Adversarial attacks on the interpretation of neuron activation maximization](https://ojs.aaai.org/index.php/AAAI/article/view/28228). *Proceedings of the AAAI Conference on Artificial Intelligence*, March 2024.

[^251]: Richard Ngo, Lawrence Chan, and Sören Mindermann. [The alignment problem from a deep learning perspective](http://arxiv.org/abs/2209.00626). *CoRR*, December 2022.

[^252]: Thanh Tam Nguyen, Thanh Trung Huynh, Phi Le Nguyen, Alan Wee-Chung Liew, Hongzhi Yin, and Quoc Viet Hung Nguyen. [A survey of machine unlearning](http://arxiv.org/abs/2209.02299). *CoRR*, October 2022.

[^253]: Eshaan Nichani, Alex Damian, and Jason D. Lee. [How transformers learn causal structure with gradient descent](http://arxiv.org/abs/2402.14735). *CoRR*, February 2024.

[^254]: NicholasKees and janus. [Searching for search](https://www.alignmentforum.org/posts/FDjTgDcGPc7B98AES/searching-for-search-4). *AI Alignment Forum*, November 2022.

[^255]: nostalgebraist. [interpreting gpt: the logit lens](https://www.alignmentforum.org/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens). *AI Alignment Forum*, August 2020.

[^256]: Chris Olah. [Distributed representations: Composition & superposition](https://transformer-circuits.pub/2023/superposition-composition/index.html). *Transformer Circuits Thread*, 2023.

[^257]: Chris Olah and Shan Carter. [Research debt](https://distill.pub/2017/research-debt). *Distill*, March 2017.

[^258]: Chris Olah, Alexander Mordvintsev, and Ludwig Schubert. [Feature visualization](https://distill.pub/2017/feature-visualization). *Distill*, November 2017.

[^259]: Chris Olah, Arvind Satyanarayan, Ian Johnson, Shan Carter, Ludwig Schubert, Katherine Ye, and Alexander Mordvintsev. [The building blocks of interpretability](https://distill.pub/2018/building-blocks). *Distill*, March 2018.

[^260]: Chris Olah, Nick Cammarata, Ludwig Schubert, Gabriel Goh, Michael Petrov, and Shan Carter. [Zoom in: An introduction to circuits](https://distill.pub/2020/circuits/zoom-in). *Distill*, March 2020.

[^261]: Christopher Olah. [Mechanistic interpretability, variables, and the importance of interpretable bases](https://transformer-circuits.pub/2022/mech-interp-essay/index.html). *Transformer Circuits Thread*, 2022.

[^262]: B. A. Olshausen and D. J. Field. [Sparse coding with an overcomplete basis set: a strategy employed by v1?](https://pubmed.ncbi.nlm.nih.gov/9425546/) *Vision Res*, December 1997.

[^263]: Catherine Olsson, Nelson Elhage, Neel Nanda, Nicholas Joseph, Nova DasSarma, Tom Henighan, Ben Mann, Amanda Askell, Yuntao Bai, Anna Chen, Tom Conerly, Dawn Drain, Deep Ganguli, Zac Hatfield-Dodds, Danny Hernandez, Scott Johnston, Andy Jones, Jackson Kernion, Liane Lovitt, Kamal Ndousse, Dario Amodei, Tom Brown, Jack Clark, Jared Kaplan, Sam McCandlish, and Chris Olah. [In-context learning and induction heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html). *Transformer Circuits Thread*, 2022.

[^264]: Laura O’Mahony, Vincent Andrearczyk, Henning Muller, and Mara Graziani. [Disentangling neuron representations with concept vectors](http://arxiv.org/abs/2304.09707). *CVPR Workshops*, April 2023.

[^265]: Charles O’Neill and Thang Bui. [Sparse autoencoders enable scalable and reliable circuit identification in language models](http://arxiv.org/abs/2405.12522). *CoRR*, May 2024.

[^266]: Francesco Ortu, Zhijing Jin, Diego Doimo, Mrinmaya Sachan, Alberto Cazzaniga, and Bernhard Schölkopf. [Competition of mechanisms: Tracing how language models handle facts and counterfactuals](http://arxiv.org/abs/2402.11655). *ACL 2024*, June 2024.

[^267]: Long Ouyang, Jeff Wu, Xu Jiang, Diogo Almeida, Carroll L. Wainwright, Pamela Mishkin, Chong Zhang, Sandhini Agarwal, Katarina Slama, Alex Ray, John Schulman, Jacob Hilton, Fraser Kelton, Luke Miller, Maddie Simens, Amanda Askell, Peter Welinder, Paul Christiano, Jan Leike, and Ryan Lowe. [Training language models to follow instructions with human feedback](https://arxiv.org/abs/2203.02155). *CoRR*, 2022.

[^268]: Koyena Pal, Jiuding Sun, Andrew Yuan, Byron C. Wallace, and David Bau. [Future lens: Anticipating subsequent tokens from a single hidden state](https://arxiv.org/abs/2311.04897). *CoNLL*, 2023.

[^269]: Vedant Palit, Rohan Pandey, Aryaman Arora, and P. Liang. [Towards vision-language mechanistic interpretability: A causal tracing tool for blip](https://www.semanticscholar.org/paper/d494727306a375e524c4c4c8cc1a2dc1845cc4b7). *ICCVW*, 2023.

[^270]: Xu Pan, Aaron Philip, Ziqian Xie, and Odelia Schwartz. [Dissecting query-key interaction in vision transformers](https://openreview.net/forum?id=CsF3PwBN6N). *ICML MI Workshop*, June 2024.

[^271]: Kiho Park, Yo Joong Choe, and Victor Veitch. [The linear representation hypothesis and the geometry of large language models](http://arxiv.org/abs/2311.03658). *NeurIPS Workshop on Causal Representation Learning*, November 2023a.

[^272]: Kiho Park, Yo Joong Choe, Yibo Jiang, and Victor Veitch. [The geometry of categorical and hierarchical concepts in large language models](https://openreview.net/forum?id=KXuYjuBzKo). *ICML MI Workshop (Oral)*, June 2024.

[^273]: Peter S. Park, Simon Goldstein, Aidan O’Gara, Michael Chen, and Dan Hendrycks. [Ai deception: A survey of examples, risks, and potential solutions](http://arxiv.org/abs/2308.14752). *CoRR*, August 2023b.

[^274]: Roma Patel and Ellie Pavlick. [Mapping language models to grounded conceptual spaces](https://openreview.net/forum?id=gJcEM8sxHK). *ICLR*, 2022.

[^275]: Judea Pearl. [*Causality*](https://www.cambridge.org/core/books/causality/B0046844FAE10CBF274D4ACBDAEB5F5B). Cambridge University Press, 2009.

[^276]: Judea Pearl and Dana Mackenzie. *The Book of Why: The New Science of Cause and Effect*. Penguin Books Limited, May 2018.

[^277]: Jonas Peters, Peter Bühlmann, and Nicolai Meinshausen. [Causal inference using invariant prediction: identification and confidence intervals](http://arxiv.org/abs/1501.01332). *Journal of the Royal Statistical Society Series B: Statistical Methodology*, November 2015.

[^278]: Jonas Peters, Dominik Janzing, and Bernhard Schlkopf. *Elements of Causal Inference: Foundations and Learning Algorithms*. The MIT Press, October 2017.

[^279]: Nicky Pochinkov and Nandi. [Machine unlearning evaluations as interpretability benchmarks](https://www.alignmentforum.org/posts/mTi8TQEyP5Pr7oczd/machine-unlearning-evaluations-as-interpretability). *AI Alignment Forum*, August 2023.

[^280]: Michael Poli, Stefano Massaroli, Eric Nguyen, Daniel Y. Fu, Tri Dao, Stephen Baccus, Yoshua Bengio, Stefano Ermon, and Christopher Ré. [Hyena hierarchy: Towards larger convolutional language models](http://arxiv.org/abs/2302.10866). *ICML*, April 2023.

[^281]: Alethea Power, Yuri Burda, Harri Edwards, Igor Babuschkin, and Vedant Misra. [Grokking: Generalization beyond overfitting on small algorithmic datasets](http://arxiv.org/abs/2201.02177). *CoRR*, January 2022.

[^282]: Nikhil Prakash, Tamar Rott Shaham, Tal Haklay, Yonatan Belinkov, and David Bau. [Fine-tuning enhances existing mechanisms: A case study on entity tracking](http://arxiv.org/abs/2402.14811). *ICLR*, February 2024.

[^283]: Philip Quirke and Fazl Barez. [Understanding addition in transformers](http://arxiv.org/abs/2310.13121). *CoRR*, October 2023.

[^284]: Philip Quirke, Clement Neo, and Fazl Barez. [Increasing trust in language models through the reuse of verified circuits](http://arxiv.org/abs/2402.02619). *CoRR*, February 2024.

[^285]: Daking Rai, Yilun Zhou, Shi Feng, Abulhair Saparov, and Ziyu Yao. [A practical review of mechanistic interpretability for transformer-based language models](http://arxiv.org/abs/2407.02646). *CoRR*, July 2024.

[^286]: Senthooran Rajamanoharan, Arthur Conmy, Lewis Smith, Tom Lieberum, Vikrant Varma, János Kramár, Rohin Shah, and Neel Nanda. [Improving dictionary learning with gated sparse autoencoders](http://arxiv.org/abs/2404.16014). *CoRR*, April 2024.

[^287]: Tilman Räuker, Anson Ho, Stephen Casper, and Dylan Hadfield-Menell. [Toward transparent ai: A survey on interpreting the inner structures of deep neural networks](http://arxiv.org/abs/2207.13243). *TMLR*, August 2023.

[^288]: Shauli Ravfogel, Yanai Elazar, Hila Gonen, Michael Twiton, and Yoav Goldberg. [Null it out: Guarding protected attributes by iterative nullspace projection](https://aclanthology.org/2020.acl-main.647). *ACL*, July 2020.

[^289]: Jie Ren, Mingjie Li, Qirui Chen, Huiqi Deng, and Quanshi Zhang. [Defining and quantifying the emergence of sparse concepts in dnns](http://arxiv.org/abs/2111.06206). *CoRR*, April 2023.

[^290]: Marco Tulio Ribeiro, Sameer Singh, and Carlos Guestrin. ["why should i trust you?": Explaining the predictions of any classifier](http://arxiv.org/abs/1602.04938). *NAACL*, August 2016.

[^291]: RicG. [Agi-automated interpretability is suicide](https://www.lesswrong.com/posts/pQqoTTAnEePRDmZN4/agi-automated-interpretability-is-suicide). *LessWrong*, May 2023.

[^292]: Jonathan Richens and Tom Everitt. [Robust agents learn causal world models](http://arxiv.org/abs/2402.10877). *ICLR Oral*, February 2024.

[^293]: Ryan Riegel, Alexander Gray, Francois Luus, Naweed Khan, Ndivhuwo Makondo, Ismail Yunus Akhalwaya, Haifeng Qian, Ronald Fagin, Francisco Barahona, Udit Sharma, Shajith Ikbal, Hima Karanam, Sumit Neelam, Ankita Likhyani, and Santosh Srivastava. [Logical neural networks](http://arxiv.org/abs/2006.13155). *NeurIPS*, June 2020.

[^294]: Mateo Rojas-Carulla, Bernhard Scholkopf, Richard Turner, and Jonas Peters. Invariant models for causal transfer learning. *JMLR*, 2018.

[^295]: Andrew Slavin Ross and Finale Doshi-Velez. [Improving the adversarial robustness and interpretability of deep neural networks by regularizing their input gradients](http://arxiv.org/abs/1711.09404). *AAAI*, November 2017.

[^296]: Cynthia Rudin. [Stop explaining black box machine learning models for high stakes decisions and use interpretable models instead](https://www.nature.com/articles/s42256-019-0048-x). *Nat Mach Intell*, May 2019.

[^297]: Cody Rushing and Neel Nanda. [Explorations of self-repair in language models](http://arxiv.org/abs/2402.15390). *ICML*, May 2024.

[^298]: Thane Ruthenis. [Internal interfaces are a high-priority interpretability target](https://www.lesswrong.com/posts/nwLQt4e7bstCyPEXs/internal-interfaces-are-a-high-priority-interpretability). *AI Alignment Forum*, December 2022.

[^299]: Thane Ruthenis. [World-model interpretability is all we need](https://www.alignmentforum.org/posts/HaHcsrDSZ3ZC2b4fK/world-model-interpretability-is-all-we-need). *AI Alignment Forum*, January 2023.

[^300]: Hassan Sajjad, Nadir Durrani, and Fahim Dalvi. [Neuron-level interpretation of deep nlp models: A survey](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00519/113852/Neuron-level-Interpretation-of-Deep-NLP-Models-A). *TACL*, November 2022.

[^301]: Mansi Sakarvadia, Aswathy Ajith, Arham Khan, Daniel Grzenda, Nathaniel Hudson, André Bauer, Kyle Chard, and Ian Foster. [Memory injections: Correcting multi-hop reasoning failures during inference in transformer-based language models](http://arxiv.org/abs/2309.05605). *CoRR*, September 2023a.

[^302]: Mansi Sakarvadia, Arham Khan, Aswathy Ajith, Daniel Grzenda, Nathaniel Hudson, André Bauer, Kyle Chard, and Ian Foster. [Attention lens: A tool for mechanistically interpreting the attention head information retrieval mechanism](http://arxiv.org/abs/2310.16270). *CoRR*, October 2023b.

[^303]: Emmanuelle Salin, Badreddine Farah, Stéphane Ayache, and Benoit Favre. [Are vision-language transformers learning multimodal representations? a probing perspective](https://ojs.aaai.org/index.php/AAAI/article/view/21375). *AAAI*, June 2022.

[^304]: Tommaso Salvatori, Ankur Mali, Christopher L. Buckley, Thomas Lukasiewicz, Rajesh P. N. Rao, Karl Friston, and Alexander Ororbia. [Brain-inspired computational intelligence via predictive coding](https://arxiv.org/abs/2308.07870). *CoRR*, 2023.

[^305]: Naomi Saphra. [Interpretability creationism](https://thegradient.pub/interpretability-creationism). *The Gradient*, 2023.

[^306]: Rylan Schaeffer, Brando Miranda, and Sanmi Koyejo. [Are emergent abilities of large language models a mirage?](http://arxiv.org/abs/2304.15004) *CoRR*, May 2023.

[^307]: Adam Scherlis, Kshitij Sachan, Adam S. Jermyn, Joe Benton, and Buck Shlegeris. [Polysemanticity and capacity in neural networks](http://arxiv.org/abs/2210.01892). *CoRR*, July 2023.

[^308]: Bernhard Schölkopf, Francesco Locatello, Stefan Bauer, Nan Rosemary Ke, Nal Kalchbrenner, Anirudh Goyal, and Yoshua Bengio. [Towards causal representation learning](http://arxiv.org/abs/2102.11107). *Special Issue of Proceedings of the IEEE - Advances in Machine Learning and Deep Neural Networks*, February 2021.

[^309]: Tal Schuster, Adam Fisch, Jai Gupta, Mostafa Dehghani, Dara Bahri, Vinh Q. Tran, Yi Tay, and Donald Metzler. [Confident adaptive language modeling](http://arxiv.org/abs/2207.07061). *NeurIPS Oral*, October 2022.

[^310]: Ramprasaath R. Selvaraju, Abhishek Das, Ramakrishna Vedantam, Michael Cogswell, Devi Parikh, and Dhruv Batra. [Grad-cam: Why did you say that? visual explanations from deep networks via gradient-based localization](https://arxiv.org/abs/1610.02391). *ICCV*, 2016.

[^311]: Murray Shanahan, Kyle McDonell, and Laria Reynolds. [Role play with large language models](https://www.nature.com/articles/s41586-023-06647-8). *Nature*, November 2023.

[^312]: Lloyd S. Shapley. [A value for *n* -person games](https://www.cambridge.org/core/product/identifier/CBO9780511528446A008/type/book_part). *Cambridge University Press*, October 1988.

[^313]: Lee Sharkey. [Circumventing interpretability: How to defeat mind-readers](https://arxiv.org/abs/2212.11415). *CoRR*, December 2022.

[^314]: Lee Sharkey. [A technical note on bilinear layers for interpretability](https://arxiv.org/abs/2305.03452). *CoRR*, May 2023.

[^315]: Lee Sharkey, Sid Black, and beren. [Current themes in mechanistic interpretability research](https://www.alignmentforum.org/posts/Jgs7LQwmvErxR9BCC/current-themes-in-mechanistic-interpretability-research). *AI Alignment Forum*, November 2022a.

[^316]: Lee Sharkey, Dan Braun, and Beren Millidge. [Taking features out of superposition with sparse autoencoders](https://www.alignmentforum.org/posts/z6QQJbtpkEAX3Aojj/interim-research-report-taking-features-out-of-superposition). *AI Alignment Forum*, 2022b.

[^317]: Mrinank Sharma, Meg Tong, Tomasz Korbak, David Duvenaud, Amanda Askell, Samuel R. Bowman, Newton Cheng, Esin Durmus, Zac Hatfield-Dodds, Scott R. Johnston, Shauna Kravec, Timothy Maxwell, Sam McCandlish, Kamal Ndousse, Oliver Rausch, Nicholas Schiefer, Da Yan, Miranda Zhang, and Ethan Perez. [Towards understanding sycophancy in language models](http://arxiv.org/abs/2310.13548). *CoRR*, October 2023.

[^318]: Justin Shovelain and Elliot McKernon. [The risk-reward tradeoff of interpretability research](https://www.lesswrong.com/posts/HdqdqNC3MyABHzSqf/the-risk-reward-tradeoff-of-interpretability-research). *LessWrong*, July 2023.

[^319]: Avanti Shrikumar, Peyton Greenside, and Anshul Kundaje. [Learning important features through propagating activation differences](http://arxiv.org/abs/1704.02685). *ICML*, 2017.

[^320]: James B. Simon, Maksis Knutins, Liu Ziyin, Daniel Geisz, Abraham J. Fetterman, and Joshua Albrecht. [On the stepwise nature of self-supervised learning](http://arxiv.org/abs/2303.15438). *ICML*, May 2023.

[^321]: slavachalnev. [Sparse mlp distillation](https://www.lesswrong.com/posts/MXabwqMwo3rkGqEW8/sparse-mlp-distillation). *LessWrong*, 2024.

[^322]: Daniel Smilkov, Nikhil Thorat, Been Kim, Fernanda Viégas, and Martin Wattenberg. [Smoothgrad: removing noise by adding noise](http://arxiv.org/abs/1706.03825). *CoRR*, June 2017.

[^323]: Nate Soares. [If interpretability research goes well, it may get dangerous](https://www.lesswrong.com/posts/BinkknLBYxskMXuME/if-interpretability-research-goes-well-it-may-get-dangerous). *LessWrong*, April 2023.

[^324]: Nitish Srivastava, Geoffrey Hinton, Alex Krizhevsky, Ilya Sutskever, and Ruslan Salakhutdinov. [Dropout: A simple way to prevent neural networks from overfitting](http://jmlr.org/papers/v15/srivastava14a.html). *JMLR*, 2014.

[^325]: Dashiell Stander, Qinan Yu, Honglu Fan, and Stella Biderman. [Grokking group multiplication with cosets](https://arxiv.org/abs/2312.06581). *CoRR*, 2023.

[^326]: Jacob Steinhardt. [Emergent deception and emergent optimization](https://bounded-regret.ghost.io/emergent-deception-optimization/). *Bounded Regret*, February 2023.

[^327]: Alessandro Stolfo, Yonatan Belinkov, and Mrinmaya Sachan. [A mechanistic interpretation of arithmetic reasoning in language models using causal mediation analysis](http://arxiv.org/abs/2305.15054). *EMNLP*, October 2023.

[^328]: Ilia Sucholutsky, Lukas Muttenthaler, Adrian Weller, Andi Peng, Andreea Bobu, Been Kim, Bradley C. Love, Erin Grant, Iris Groen, Jascha Achterberg, Joshua B. Tenenbaum, Katherine M. Collins, Katherine L. Hermann, Kerem Oktar, Klaus Greff, Martin N. Hebart, Nori Jacoby, Qiuyi Zhang, Raja Marjieh, Robert Geirhos, Sherol Chen, Simon Kornblith, Sunayana Rane, Talia Konkle, Thomas P. O’Connell, Thomas Unterthiner, Andrew K. Lampinen, Klaus-Robert Müller, Mariya Toneva, and Thomas L. Griffiths. [Getting aligned on representational alignment](http://arxiv.org/abs/2310.13018). *CoRR*, November 2023.

[^329]: Chen Sun, Nolan Andrew Miller, Andrey Zhmoginov, Max Vladymyrov, and Mark Sandler. [Learning and unlearning of fabricated knowledge in language models](https://openreview.net/forum?id=R5Q5lANcjY). *ICML MI Workshop*, June 2024.

[^330]: Mukund Sundararajan, Ankur Taly, and Qiqi Yan. [Axiomatic attribution for deep networks](http://arxiv.org/abs/1703.01365). *ICML*, June 2017.

[^331]: Aaquib Syed, Can Rager, and Arthur Conmy. [Attribution patching outperforms automated circuit discovery](http://arxiv.org/abs/2310.10348). *CoRR*, October 2023.

[^332]: technicalities and Stag. [Shallow review of live agendas in alignment & safety](https://www.lesswrong.com/posts/zaaGsFBeDTpCsYHef/shallow-review-of-live-agendas-in-alignment-and-safety). *LessWrong*, 2023.

[^333]: Max Tegmark and Steve Omohundro. [Provably safe systems: the only path to controllable agi](http://arxiv.org/abs/2309.01933). *CoRR*, September 2023.

[^334]: Adly Templeton, Tom Conerly, Jonathan Marcus, Jack Lindsey, Trenton Bricken, and Brian Chen. [Scaling monosemanticity: Extracting interpretable features from claude 3 sonnet](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html). *Transformer Circuits Thread*, 2024.

[^335]: Ian Tenney, Dipanjan Das, and Ellie Pavlick. [Bert rediscovers the classical nlp pipeline](http://arxiv.org/abs/1905.05950). *ACL*, August 2019.

[^336]: Vimal Thilak, Etai Littwin, Shuangfei Zhai, Omid Saremi, Roni Paiss, and Joshua Susskind. [The slingshot mechanism: An empirical study of adaptive optimizers and the grokking phenomenon](https://arxiv.org/abs/2206.04817). *CoRR*, 2022.

[^337]: Hannes Thurnherr and Jérémy Scheurer. [Tracrbench: Generating interpretability testbeds with large language models](https://openreview.net/forum?id=vNubZ5zK8h). *ICML MI Workshop*, June 2024.

[^338]: Curt Tigges, Oskar John Hollinsworth, Atticus Geiger, and Neel Nanda. [Language models linearly represent sentiment](https://openreview.net/forum?id=Xsf6dOOMMc). *ICML MI Workshop*, June 2024.

[^339]: Eric Todd, Millicent L. Li, Arnab Sen Sharma, Aaron Mueller, Byron C. Wallace, and David Bau. [Function vectors in large language models](https://arxiv.org/abs/2310.15213). *CoRR*, 2023.

[^340]: Dweep Trivedi, Jesse Zhang, Shao-Hua Sun, and Joseph J. Lim. [Learning to synthesize programs as interpretable and generalizable policies](http://arxiv.org/abs/2108.13643). *NeurIPS*, 2021.

[^341]: Alexander Matt Turner, Lisa Thiergart, David Udell, Gavin Leech, Ulisse Mini, and Monte MacDiarmid. [Activation addition: Steering language models without optimization](http://arxiv.org/abs/2308.10248). *CoRR*, September 2023.

[^342]: Dmitry Vaintrob, jake\_mendel, and Kaarel. [Toward a mathematical framework for computation in superposition](https://www.lesswrong.com/posts/2roZtSr5TGmLjXMnT/toward-a-mathematical-framework-for-computation-in). *AI Alignment Forum*, 2024.

[^343]: Alexandre Variengien and Eric Winsor. [Look before you leap: A universal emergent decomposition of retrieval tasks in language models](http://arxiv.org/abs/2312.10091). *ICML MI Workshop*, December 2023.

[^344]: Vikrant Varma, Rohin Shah, Zachary Kenton, János Kramár, and Ramana Kumar. [Explaining grokking through circuit efficiency](http://arxiv.org/abs/2309.02390). *CoRR*, September 2023.

[^345]: Abhinav Verma, Hoang M. Le, Yisong Yue, and Swarat Chaudhuri. [Imitation-projected programmatic reinforcement learning](http://arxiv.org/abs/1907.05431). *NeurIPS*, 2019a.

[^346]: Abhinav Verma, Vijayaraghavan Murali, Rishabh Singh, Pushmeet Kohli, and Swarat Chaudhuri. [Programmatically interpretable reinforcement learning](http://arxiv.org/abs/1804.02477). *CoRR*, April 2019b.

[^347]: Jesse Vig, Sebastian Gehrmann, Yonatan Belinkov, Sharon Qian, Daniel Nevo, Yaron Singer, and Stuart Shieber. [Investigating gender bias in language models using causal mediation analysis](https://proceedings.neurips.cc/paper/2020/hash/92650b2e92217715fe312e6fa7b90d82-Abstract.html). *NeurIPS*, 2020.

[^348]: M. Vilas, Timothy Schaumlöffel, and Gemma Roig. [Analyzing vision transformers for image classification in class embedding space](https://www.semanticscholar.org/paper/7c57530318ae6c6e49c835f23e75affd9cf0827d). *CoRR*, 2023.

[^349]: Elena Voita and Ivan Titov. [Information-theoretic probing with minimum description length](http://arxiv.org/abs/2003.12298). *EMNLP*, March 2020.

[^350]: Elena Voita, Javier Ferrando, and Christoforos Nalmpantis. [Neurons in large language models: Dead, n-gram, positional](http://arxiv.org/abs/2309.04827). *CoRR*, September 2023.

[^351]: Julius von Kügelgen, M. Loog, A. Mey, and B. Scholkopf. [Semi-supervised learning, causality, and the conditional cluster assumption](https://arxiv.org/abs/1905.12081). *Conference on Uncertainty in Artificial Intelligence*, May 2019.

[^352]: Johannes von Oswald, Eyvind Niklasson, Maximilian Schlegel, Seijin Kobayashi, Nicolas Zucchet, Nino Scherrer, Nolan Miller, Mark Sandler, Blaise Agüera y Arcas, Max Vladymyrov, Razvan Pascanu, and João Sacramento. [Uncovering mesa-optimization algorithms in transformers](http://arxiv.org/abs/2309.05858). *CoRR*, September 2023.

[^353]: Chelsea Voss, Gabriel Goh, Nick Cammarata, Michael Petrov, Ludwig Schubert, and Chris Olah. [Branch specialization](https://distill.pub/2020/circuits/branch-specialization). *Distill*, April 2021.

[^354]: Boshi Wang, Xiang Yue, Yu Su, and Huan Sun. [Grokked transformers are implicit reasoners: A mechanistic journey to the edge of generalization](https://openreview.net/forum?id=ns8IH5Sn5y). *ICML MI Workshop*, June 2024.

[^355]: Kevin Wang, Alexandre Variengien, Arthur Conmy, Buck Shlegeris, and Jacob Steinhardt. [Interpretability in the wild: a circuit for indirect object identification in gpt-2 small](http://arxiv.org/abs/2211.00593). *ICLR*, 2023.

[^356]: Alex Warstadt, Alicia Parrish, Haokun Liu, Anhad Mohananey, Wei Peng, Sheng-Fu Wang, and Samuel R. Bowman. [Blimp: The benchmark of linguistic minimal pairs for english](https://aclanthology.org/2020.tacl-1.25). *Transactions of the Association for Computational Linguistics*, 2020.

[^357]: Sumio Watanabe. [*Algebraic Geometry and Statistical Learning Theory*](https://www.cambridge.org/core/books/algebraic-geometry-and-statistical-learning-theory/9C8FD1BDC817E2FC79117C7F41544A3A). Cambridge Monographs on Applied and Computational Mathematics. Cambridge University Press, 2009.

[^358]: Sumio Watanabe. [*Mathematical Theory of Bayesian Statistics*](https://www.taylorfrancis.com/books/9781482238082). Chapman and Hall, 1 edition, April 2018.

[^359]: Jason Wei, Yi Tay, Rishi Bommasani, Colin Raffel, Barret Zoph, Sebastian Borgeaud, Dani Yogatama, Maarten Bosma, Denny Zhou, Donald Metzler, Ed H. Chi, Tatsunori Hashimoto, Oriol Vinyals, Percy Liang, Jeff Dean, and William Fedus. [Emergent abilities of large language models](http://arxiv.org/abs/2206.07682). *TMLR*, October 2022.

[^360]: John Wentworth. [How to go from interpretability to alignment: Just retarget the search](https://www.alignmentforum.org/posts/w4aeAFzSAguvqA5qu/how-to-go-from-interpretability-to-alignment-just-retarget). *AI Alignment Forum*, August 2022.

[^361]: James C. R. Whittington, Will Dorrell, Surya Ganguli, and Timothy E. J. Behrens. [Disentangling with biological constraints: A theory of functional cell types](http://arxiv.org/abs/2210.01768). *CoRR*, September 2022.

[^362]: Baoyuan Wu, Hongrui Chen, Mingda Zhang, Zihao Zhu, Shaokui Wei, Danni Yuan, and Chao Shen. [Backdoorbench: A comprehensive benchmark of backdoor learning](http://arxiv.org/abs/2206.12654). *NeurIPS Datasets and Benchmarks*, October 2022a.

[^363]: Zhengxuan Wu, Atticus Geiger, Joshua Rozner, Elisa Kreiss, Hanson Lu, Thomas Icard, Christopher Potts, and Noah Goodman. [Causal distillation for language models](https://aclanthology.org/2022.naacl-main.318). *NAACL-HLT*, July 2022b.

[^364]: Zhengxuan Wu, Karel D’Oosterlinck, Atticus Geiger, Amir Zur, and Christopher Potts. [Causal proxy models for concept-based model explanations](http://arxiv.org/abs/2209.14279). *ICML*, 2023a.

[^365]: Zhengxuan Wu, Atticus Geiger, Christopher Potts, and Noah D. Goodman. [Interpretability at scale: Identifying causal mechanisms in alpaca](http://arxiv.org/abs/2305.08809). *CoRR*, May 2023b.

[^366]: Zhengxuan Wu, Aryaman Arora, Zheng Wang, Atticus Geiger, Dan Jurafsky, Christopher D. Manning, and Christopher Potts. [Reft: Representation finetuning for language models](http://arxiv.org/abs/2404.03592). *CoRR*, May 2024.

[^367]: Qinan Yu, Jack Merullo, and Ellie Pavlick. [Characterizing mechanisms for factual recall in language models](http://arxiv.org/abs/2310.15910). *CoRR*, October 2023.

[^368]: Matthew D. Zeiler and Rob Fergus. [Visualizing and understanding convolutional networks](http://arxiv.org/abs/1311.2901). *ECCV*, 2014.

[^369]: Biao Zhang, Ivan Titov, and Rico Sennrich. [Sparse attention with linear units](http://arxiv.org/abs/2104.07012). *EMNLP*, October 2021.

[^370]: Chiyuan Zhang, Samy Bengio, Moritz Hardt, Benjamin Recht, and Oriol Vinyals. [Understanding deep learning requires rethinking generalization](http://arxiv.org/abs/1611.03530). *ICLR*, February 2017.

[^371]: Fred Zhang and Neel Nanda. [Towards best practices of activation patching in language models: Metrics and methods](https://arxiv.org/abs/2309.16042). *ICLR*, 2023.

[^372]: Quanshi Zhang, Yu Yang, Haotian Ma, and Ying Nian Wu. [Interpreting cnns via decision trees](https://arxiv.org/abs/1802.00121). *CVPR*, 2019.

[^373]: Ziqian Zhong, Ziming Liu, Max Tegmark, and Jacob Andreas. [The clock and the pizza: Two stories in mechanistic explanation of neural networks](https://arxiv.org/abs/2306.17844). *CoRR*, 2023.

[^374]: Roland S. Zimmermann, Judy Borowski, Robert Geirhos, Matthias Bethge, Thomas S. A. Wallis, and Wieland Brendel. [How well do feature visualizations support causal understanding of cnn activations?](http://arxiv.org/abs/2106.12447) *NeurIPS*, November 2021.

[^375]: Andy Zou, Long Phan, Sarah Chen, James Campbell, Phillip Guo, Richard Ren, Alexander Pan, Xuwang Yin, Mantas Mazeika, Ann-Kathrin Dombrowski, Shashwat Goel, Nathaniel Li, Michael J. Byun, Zifan Wang, Alex Mallen, Steven Basart, Sanmi Koyejo, Dawn Song, Matt Fredrikson, J. Zico Kolter, and Dan Hendrycks. [Representation engineering: A top-down approach to ai transparency](http://arxiv.org/abs/2310.01405). *CoRR*, October 2023.


[//begin]: # "Autogenerated link references for markdown compatibility"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[Uncaptioned image]: ./../notes/stub "Uncaptioned image"
[//end]: # "Autogenerated link references"
