---
title: "AI-generated synthetic neurons speed up brain mapping"
source: "https://research.google/blog/ai-generated-synthetic-neurons-speed-up-brain-mapping/"
author:
  - "[[Michał Januszewski]]"
  - "[[Research Scientist]]"
  - "[[and Franz Rieger]]"
  - "[[Student Researcher]]"
  - "[[Google Research]]"
published: 2026-04-15
created: 2026-04-27
description:
tags:
  - "clippings"
  - "citable"
---
Generating synthetic neuron geometries helps AI learn to better classify neurons by their shape, speeding up future brain map reconstructions.

Using computers to create full wiring maps of complex brains is enabling a new era of neuroscience. A recently released map of the complete [male fruit fly](https://www.janelia.org/news/researchers-reveal-connectome-of-the-male-fruit-fly-central-nervous-system) brain and central nervous system provides a foundational resource for studying how the brain responds to stimuli and controls the body.

But reconstructing the entire brains of mammals, and certainly of humans, remains far out of reach. The fruit fly brain map, with 166,000 neurons, represents years of work by AI-enabled computers and human experts. A complete mouse brain is a thousand times larger, and a human brain is a thousand times larger than that.

Google Research is developing AI techniques to tackle larger brain mapping projects by speeding up the identification, classification, and visualization of neurons. With our partners, we’ve also mapped fragments of a [zebra finch](https://www.nature.com/articles/s41592-018-0049-4) brain, a whole [larval zebrafish](https://research.google/pubs/automated-synapse-level-reconstruction-of-neural-circuits-in-the-larval-zebrafish-brain/) brain, and a small fragment of the [human brain](https://research.google/blog/ten-years-of-neuroscience-at-google-yields-maps-of-human-brain/), and we recently launched an effort to map a small section of [mouse brain](https://research.google/blog/google-research-embarks-on-effort-to-map-a-mouse-brain/). Our new paper “ [MoGen: Detailed neuronal morphology generation via point cloud flow matching](https://openreview.net/forum?id=HpIxllcNtb) ”, to be presented at [ICLR 2026](https://iclr.cc/), uses synthetic neural shapes to improve AI reconstruction models.

Enhancing the training data with synthetic examples from the Neuronal Morphology Generation model, or MoGen, delivers a 4.4% reduction in reconstruction errors and suggests directions for further improvements. While a 4.4% error improvement might sound modest, at the large scale of a complete mouse brain, this translates to 157 person-years of manual proofreading saved.

This adds to the Google Research Connectomics team’s growing list of [foundational tools](https://sites.research.google/gr/neural-mapping/innovations/) to advance modern neuroscience, developed over more than a decade of collaborative brain science research.

<video controls=""><source src="https://storage.googleapis.com/gweb-research2023-media/media/NS_fouranimations.mp4" type="video/mp4"></video>

*This animation of synthetic neurons created by MoGen shows how the initial point clouds gradually approach a realistic neural shape. Here, MoGen was trained to simulate mouse neurons. Using MoGen’s synthetic neurons to train an AI model reduces errors in brain reconstructions.*

## Connectomics: Wiring maps of the brain

The field of [connectomics](https://sites.research.google/gr/neural-mapping/) reconstructs brain cells, or neurons, to create wiring maps of the brain. The process begins by imaging thin slices of brain tissue and then stacking, aligning, and segmenting them so the 2D images become 3D neurons. While the [first complete brain map](https://pubmed.ncbi.nlm.nih.gov/22462104/) for the worm used painstaking manual methods over 16 years, current efforts accelerate this process using digital imaging and computers.

Our team in Google Research uses AI models to help turn microscope images into accurate 3D neuron shapes. Output from our AI model is proofread and annotated by human experts. This last verification step remains the most time-consuming, and a key hurdle to producing more ambitious brain maps.

## A diversity of neural shapes

Most cells in the body are shaped roughly like a sphere. Biological neurons, however, come in a dizzying variety of spindly shapes. Neurons send out signals along a main projection, known as an [axon](https://en.wikipedia.org/wiki/Axon), which is typically long and thin, and can curl, twist or branch. Neurons receive signals through a network of branching extensions known as [dendrites](https://en.wikipedia.org/wiki/Dendrite), which often feature shorter protrusions known as dendritic spines. Each neuron also forms many [synapses](https://www.khanacademy.org/science/biology/human-biology/neuron-nervous-system/a/the-synapse), specialized junctions where electrical or chemical signals can leap from one neuron to another.

This intricate geometry relates to biological function and is a key challenge of connectomics. Our most recent AI reconstruction model, [PATHFINDER](https://www.biorxiv.org/content/10.1101/2025.05.16.654254v1), begins by identifying [neurite](https://en.wikipedia.org/wiki/Neurite) segments, or subsections of a neuron. It then combines these segments to create a full neuron. Irregularities in the microscopy data can result in “split errors”, meaning two neurites that should be connected are instead separated, or *merge errors*, meaning two unrelated neurites are combined. These are significant errors that proofreaders — typically researchers, graduate students or technical specialists — must correct by hand.

## Training AI on synthetic neurons

We hypothesized that more training data, even synthetic data, would improve PATHFINDER’s performance. Synthesized training data has been successful in other fields, including [natural language processing](https://research.google/pubs/synthetic-text-generation-for-training-large-language-models-llms-via-gradient-matching/), [image generation](https://openreview.net/forum?id=DlRsoxjyPm), and [autonomous driving](https://waymo.com/blog/2026/02/the-waymo-world-model-a-new-frontier-for-autonomous-driving-simulation/).

To test this, we created MoGen, short for Neuronal Morphology Generation. We taught an AI model, specifically the [PointInfinity](https://zixuanh.com/projects/pointinfinity) point cloud flow matching model, to gradually transform a random cloud of 3D points into realistic 3D neuronal shapes. Using [mouse cortex tissue reconstructions](https://www.biorxiv.org/content/10.1101/2025.05.16.654254v1) that were previously human-verified, we trained MoGen with 1,795 axons by sampling points from their surfaces. We verified MoGen’s simulated output as realistic by asking human experts to classify a set of neurites that were a mix of real and simulated.

<video controls=""><source src="https://storage.googleapis.com/gweb-research2023-media/media/NeuralShapes2_Rotating.mp4" type="video/mp4"></video>

*These rotating neurite fragments were generated by MoGen. The AI-generated shapes are similar to real mouse neurites, including bending, twisting, thickening and branching. Human experts were unable to distinguish between the real and AI-generated fragments.*

We then added millions of these synthetic shapes into our PATHFINDER training pipeline.

## Results: Measurable improvement on the current state of the art

Using a PATHFINDER model trained with 10% simulated data from MoGen reduced the error rate on reconstructing the reserved mouse axons by 4.4%, driven primarily by a reduction in the merge error rate. Doing so reduces the need for manual correction by human experts. This marks the first time a modern generative AI approach has been used to advance the current best method in connectomic reconstruction.

While this might look like a marginal improvement, it is equivalent to saving a single expert 157 years of manual work at the scale of a [complete mouse brain](https://cms.wellcome.org/sites/default/files/2023-06/Connectomics-scaling-up-connectomics_0.pdf).

## Looking ahead

This initial result suggests other potential improvements along similar lines. We can direct MoGen to generate particular types of neurons by tuning the length, spatial extent, branching and other measures. This study generated a random assortment of shapes, but in the future we could focus on geometries that are especially prone to reconstruction errors. While this study focused on the mouse, since different species have distinct neuron shapes, we also trained versions of MoGen on zebra finch and fruit fly neurons. We’re also exploring using the simulated neurons to create synthetic electron microscope images that provide more training data earlier in the reconstruction pipeline.

We have released MoGen as an [open-source model](https://mogen-release.web.app/), together with its species-specific trained models, as a resource for the community to build on. We believe that more of these types of innovations will be needed to tackle the immense scale and complexity of upcoming connectomics projects, including mapping the complete mouse brain.

## Acknowledgments

*We thank our academic collaborators in the Hess lab at HHMI Janelia, and acknowledge contributions from the Connectomics Team at Google. Thanks to Lizzie Dorfman, Michael Brenner, John Platt, and Yossi Matias for their support, coordination and leadership.*