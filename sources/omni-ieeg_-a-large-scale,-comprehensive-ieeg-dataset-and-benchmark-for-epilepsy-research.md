---
title: "Omni-iEEG: A Large-Scale, Comprehensive iEEG Dataset and Benchmark for Epilepsy Research"
source: "https://omni-ieeg.github.io/omni-ieeg/"
author:
  - "[[Omni-iEEG]]"

created: 2026-05-11
description: "<h3 style='color:red'>ICLR 2026</h3>"
tags:
  - "clippings"
  - "datasets"
  - "epilepsy"
  - "citable"
---
![Omni-iEEG teaser](https://omni-ieeg.github.io/assets/img/omniieeg/omniieeg.png)

**TL;DR:** Omni-iEEG is the first **large-scale, harmonized intracranial EEG dataset**, comprising **302 patients, 178 hours of recordings, and 36K expert annotations**. It establishes **clinically meaningful and unified benchmarks** with standardized evaluation metrics to bridge **clinical neuroscience** and **machine learning**.

### Dataset Overview

![](https://omni-ieeg.github.io/assets/img/omniieeg/table1_dataset.png)

Omni-iEEG integrates recordings from eight epilepsy centers with harmonized clinical metadata (SOZ, resection regions, surgical outcomes, anatomical labels). Data will be released in **BIDS format** with an accompanying utility library on Github. We will also released the data in hugginface for easier access.

### Event Annotation

![](https://omni-ieeg.github.io/assets/img/omniieeg/table2_event.png)

Over **36,000 high-frequency oscillation (HFO) events** were annotated, combining new and previously published events. Candidate events were detected with three established algorithms (STE, MNI, Hilbert) and reviewed by **four board-certified epileptologists**. Each event was labeled as *artifact*, *non-spkHFO*, or *spkHFO*.

To ensure quality, annotators first calibrated on a shared set, then labeled independently. Inter-rater agreement was high (**Fleiss’ κ = 0.93**), reflecting strong consistency and clinical reliability. All signals were resampled to 1 kHz and standardized with bipolar montage where appropriate.

### Benchmark Tasks

**Task 1:** Clinical prior-driven pathological event classification (spkHFO vs non-spkHFO vs artifact).  
**Task 2:** Pathological brain region identification (SOZ vs normal channels, outcome prediction).  
**Exploratory Tasks:** Anatomical location classification, Ictal period identification, Sleep–Awake classification.

### Task 1:Pathological Event Classification

![](https://omni-ieeg.github.io/assets/img/omniieeg/table4_result.png)

We benchmark recent competitive baseline event classification methods. PyHFO method trained on Omni-iEEG achieves the highest F1 (~0.81), demonstrating strong performance on spkHFO detection.

### Pathological Brain Region Identification

![](https://omni-ieeg.github.io/assets/img/omniieeg/table5_result.png)

Both event-based and segment-based models are evaluated. Segment models (TimeConv-CNN, CLAP, SEEG-Net) achieve up to **0.81 AUC** in pathological channel identification and correlate well with surgical outcome prediction.

### Exploratory Tasks

![](https://omni-ieeg.github.io/assets/img/omniieeg/table6_supp.png)

Three exploratory tasks extend Omni-iEEG:

- **Anatomical Location Identification:** 5-class lobes and 12-class subregions.
- **Ictal Period Classification:** detect ictal vs interictal periods.
- **Sleep–Awake Classification:** identify vigilance states in invasive recordings.

Pretrained audio models (**CLAP**) perform strongly (e.g., *F1 = 0.92, Acc = 0.93* on ictal classification), while CNNs remain competitive.

### Conclusion

Omni-iEEG provides a unified, clinically validated foundation for data-driven epilepsy research. It enables systematic benchmarking, improves reproducibility, and opens new research directions at the intersection of **clinical neurophysiology** and **AI**.

### Reference

```
@inproceedings{duan2026omniieeg,
  title={Omni-iEEG: A Large-Scale, Comprehensive iEEG Dataset and Benchmark for Epilepsy Research},
  author={Duan, Chenda and Zhang, Yipeng and Kanai, Sotaro and Ding, Yuanyi and Daida, Atsuro and Yu, Pengyue and Zheng, Tiancheng and Kuroda, Naoto and Hussain, Shaun A. and Asano, Eishi and Nariai, Hiroki and Roychowdhury, Vwani},
  booktitle = {International Conference on Learning Representations (ICLR)},
  year      = {2026}
}
```
