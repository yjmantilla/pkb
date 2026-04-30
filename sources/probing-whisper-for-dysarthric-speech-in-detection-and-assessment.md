---
title: "Probing Whisper for Dysarthric Speech in Detection and Assessment"
source: "https://ieeexplore.ieee.org/abstract/document/11464429"
author:

created: 2026-04-30
description: "Large-scale end-to-end models such as Whisper have shown strong performance on diverse speech tasks, but their internal behavior on pathological speech remains"
tags:
  - "clippings"
  - "speech"
  - "speech-representations"
  - "citable"
---
## Abstract:

Large-scale end-to-end models such as Whisper have shown strong performance on diverse speech tasks, but their internal behavior on pathological speech remains poorly und...

---

Dysarthria is a speech disorder resulting from neurological impairments that affect articulation, phonation, and prosody \[1\], \[2\], \[3\]. Accurate detection and assessment of the severity level of dysarthric speech are essential for early diagnosis, therapeutic intervention, and assistive speech technologies \[4\], \[5\], \[6\], \[7\]. Traditional methods relied on handcrafted features or statistical models, but recent advances in deep learning and large-scale models have enabled more robust and generalizable representations for dysarthric speech \[8\], \[9\].

OpenAI’s Whisper \[10\], as a large-scale weakly supervised model, trained on diverse multilingual and multitask data, has demonstrated promising performance in various speech-related tasks \[11\], \[12\]. While primarily designed for automatic speech recognition (ASR), its encoder layers capture hierarchical speech representations that can benefit various downstream tasks \[13\], \[14\], where selecting the optimal layer for downstream tasks is critical. Recent work has begun to investigate Whisper’s internal embeddings for pathological speech analysis \[15\], \[16\], \[17\], \[18\]. However, the effectiveness of different layers for dysarthric detection and assessment \[19\], as well as how these representations are influenced by fine-tuning \[17\], \[20\], remains insufficiently understood.

This gap motivates our study to systematically probe Whisper embeddings and determine the most informative layer for dysarthric speech detection and assessment, and improve the interpretability of the large-scale models in clinical applications. Specifically, we address three research questions: 1) How does multi-task learning compare to single-task learning for dysarthric speech detection and assessment? 2) Which Whisper embedding layers are most informative for each task? and 3) How does fine-tuning alter the representations across layers and affect classification performance?

To answer these questions, we conduct a comprehensive layer-wise probing analysis of Whisper-medium (Whisper-M) embeddings. We evaluate layer representations using multiple criteria: (i) performance (accuracy and F1-score) across single-task and multi-task settings to evaluate their relative effectiveness, (ii) mutual information (MI) \[21\] between the embeddings and dysarthria labels to measure feature relevance, and (iii) Silhouette scores \[21\] to assess the clustering quality of dysarthric and non-dysarthric speech and severity levels. To study adaptability, we further compute the MI between pretrained (PT) and fine-tuned (FT) embeddings to examine how fine-tuning affects different layers.

The Whisper-M encoder, taking 80-D FBank as inputs, consists of 24 layers that generate hierarchical speech representations. To investigate the effectiveness of these embeddings and to determine the most informative layer in the Whisper-M encoder for dysarthric speech detection and classification, we extract representations from all layers. Fig. 1 shows the workflow for single- and multi-task learning.

### 2.1. Single- and multi-task detection and assessment

For the **single-task learning** setup, we train separate linear classifiers from scratch (a fully-connected layer) for each layer to independently perform either detection (dysarthric vs. non-dysarthric speech) or assessment (identifying dysarthria severity levels).

In the **multi-task learning** setup, we introduce a shared learning framework where a classifier jointly predicts detection and severity from the same embeddings. The goal is to leverage shared speech representations across tasks, potentially improving itself or the other task’s performance by enabling joint learning of relevant features. The objective function is defined as the sum of the detection and severity classification losses, encouraging the model to learn features that benefit both tasks. By comparing the single- and multi-task setups across different layers, we aim to determine the most effective learning strategy and the optimal layer for dysarthric speech detection and assessment.

### 2.2. Probing analysis and evaluation metrics

We conduct a layer-wise probing analysis using three evaluation techniques: detection and severity classification accuracy, Mutual Information (MI), and Silhouette score, computed using scikit-learn \[21\]. Each metric captures a different perspective on how well embeddings encode dysarthria-related information, helping to identify the optimal layer for downstream tasks.

#### 2.2.1. Detection and assessment performance

For each layer, we evaluate task performance using both accuracy and F1-score. Accuracy reflects the overall correctness of predictions, while the F1-score provides a harmonic mean of the precision and recall, which is particularly important given the class imbalance in dysarthric speech datasets. Higher values on these metrics indicate that the corresponding layer captures more discriminative representations.

#### 2.2.2. Mutual information

Unlike accuracy and F1-score, which assess performance through a classifier, the MI score quantifies the intrinsic dependency between embeddings and dysarthria labels. Higher MI values indicate that the embeddings inherently encode more label-relevant information, independent of classifier training. We compute MI for each layer to assess how well the Whisper encoder layers capture dysarthria-related patterns. Layers with high MI are therefore expected to be more informative for detection or assessment.

We also compute MI between pretrained and fine-tuned embeddings to examine how fine-tuning alters the layer representations (e.g., which layers are most affected) and influences classification performance. Lower MI values suggest that FT significantly modifies the embeddings, while higher values indicate that the original representation structure is largely preserved after fine-tuning.

#### 2.2.3. Silhouette score

The Silhouette score evaluates the clustering quality of the embeddings by measuring how well embeddings of dysarthric and non-dysarthric speech, or different severity levels, are separated in the corresponding space. This score ranges from -1 to 1, with higher values indicating more distinct and well-separated clusters. In our setting, high Silhouette scores imply that a layer produces embeddings that naturally distinguish dysarthria-related categories.

**Table 1.** Number of utterances in training and test set split for different speaker groups (fold 1). The numbers of utterances are the same for folds 1, 2 and 3 and in fold 4 and fold 5.

[![Table 1.- 
              Number of utterances in training and test set split for different speaker groups (fold 1). The numbers of utterances are the same for folds 1, 2 and 3 and in fold 4 and fold 5.
            ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue.t1-p5-yue-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue.t1-p5-yue-large.gif)

### 3.1. Dataset

We use TORGO \[22\], a widely used English dysarthric dataset, which contains 21 hours of speech collected from 15 speakers: 8 dysarthric speakers with different severity levels (*severe*, *moderate to severe* <sup>1</sup>, *moderate*, and *mild*, totaling 7.3 hours) and 7 typical speakers (13.7 hours). The dataset consists of both isolated word (615 unique items) and sentence utterances (354 unique items), with a total vocabulary size of 1573. This diversity makes TORGO well-suited for evaluating dysarthria detection and assessment methods.

### 3.2. Feature extraction

As baselines, we extracted filterbank (FBank) features using torchaudio \[23\]. Four sets of FBank features were computed: 80- and 128-D, and the same features concatenated with three pitch-related features (fundamental frequency (F0), pitch probability, and delta-F0) \[24\] extracted with librosa \[25\], yielding 83- and 131-D features.

We extract embeddings from the Whisper-M model \[10\], a Transformer \[26\] with 24 encoder and 6 decoder layers. Each encoder layer outputs a 1024-D embedding. For every utterance in TORGO, we extracted embeddings from all 24 encoder layers and averaged them across time.

To analyze the effect of domain adaptation, we fine-tuned <sup>2</sup> Whisper-M on the TORGO dataset for the ASR task, following the split in \[27\]. Then, embeddings were extracted from FT models for probing.

### 3.3. Classifier and training setup

To analyze the raw performance of each layer, we employed a simple linear classifier consisting of a single fully-connected layer with softmax activation. For dysarthria detection, the classifier head has two units (1024 *×* 2 parameters), where 1024 is the embedding size. For severity classification, the classifier head has four units (1024 *×* 4) in the single-task setup. In the multi-task setup, the classifier head branched into two outputs: one with two units for detection and another with four units for severity classification.

[![Fig. 1. - 
            Workflow of layer probing on Whisper-M encoder layers’ embeddings using the single and multi-task approaches.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue1-p5-yue-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue1-p5-yue-large.gif)

**Fig. 1.**

Workflow of layer probing on Whisper-M encoder layers’ embeddings using the single and multi-task approaches.

[![Fig. 2. - 
            Detection results for probing pretrained Whisper-M embeddings (single-task vs. multi-task).
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue2-p5-yue-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue2-p5-yue-large.gif)

**Fig. 2.**

Detection results for probing pretrained Whisper-M embeddings (single-task vs. multi-task).

We used Stratified-KFold cross validation (K=5) \[28\], \[21\], to ensure balanced class distributions in training and test splits (Table 1). Each classifier was trained for 20 epochs per fold using the AdamW optimizer \[29\] with a learning rate of 3e-4 and a batch size of 32. Cross-entropy loss was used in both single-task and multi-task setups; in the latter, losses from the detection and severity heads were summed with the same weights. Performance was reported as the average results over the five folds, along with the standard deviation, visualized with error bars. The same training setup was applied to classifiers trained on embeddings extracted from each Whisper layer (both PT and FT) as well as the FBank features.

### 4.1. Single- vs multi-task learning

Figs. 2 show the accuracy and F1-score of the detection task under single-task and multi-task setups <sup>3</sup>. Contrary to expectations of a regularization effect, multi-task learning provides minimal benefit over single-task learning across both metrics. This is likely because the two tasks are highly related: in the detection task, all dysarthric classes are treated as a single category, while the severity classification task subdivides them into severity levels. As a result, multitask learning does not introduce significant additional information.

[![Fig. 3. - 
            Detection accuracy error bar for probing pretrained (PT) and finetuned (FT) Whisper-M embeddings.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue3-p5-yue-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue3-p5-yue-large.gif)

**Fig. 3.**

Detection accuracy error bar for probing pretrained (PT) and finetuned (FT) Whisper-M embeddings.

Probing results illustrate that the layers 13–15 yield the best performance for both single- and multi-task setups. While earlier layers capture lower-level acoustic information and later layers specialized for ASR, these middle layers appear to balance task-specific and general representations most effectively. An interesting observation is that performance does not decline significantly after the optimal layers (16–24). This suggests that the embeddings from higher layers, particularly the final encoder layer—despite being primarily tailored for speech recognition—remain highly effective in distinguishing between dysarthric and typical speech. This implies that the Whisper encoder does not map all speech types into a single canonical representation but instead projects them into distinct subspaces optimized for ASR. These separate subspaces ensure that even the embeddings from the highest encoder layer remain useful for both detection and classification tasks.

### 4.2. Fine-tuning effect

Fig. 3 show detection performance after fine-tuning Whisper on the TORGO ASR task, along with error bars across five folds and baselines using 80- and 131-D FBank features. As expected, the performance of the earliest Whisper layers is comparable to FBank-80, which matches the model’s training setup as the Whisper-M model is trained with FBank-80 features. Adding pitch features to higher-dimensional FBank representations (FBank-131) improves performance to a level similar to the 3rd Whisper layer. The small error bars indicate consistent results across folds.

Another observation is that fine-tuning has minimal impact on the performance of classifiers built on the earliest and latest layers, albeit for different reasons. In the lower layers, weak gradients lead to minimal weight updates, so the embeddings change very little. In the higher layers, although gradients are stronger, the Whisper model already maps diverse speech types into well-separated subspaces. These subspaces remain effective for linear classifiers, yielding high performance even though the embeddings were primarily optimized for ASR rather than for detection or severity classification.

**Table 2.** Detection and classification accuracy for different feature sets and tasks. "\*"indicates the performance of the embeddings from the optimal layer of Whisper-M. ST: Single-task, MT: multi-task.

[![Table 2.- 
            Detection and classification accuracy for different feature sets and tasks. "*"indicates the performance of the embeddings from the optimal layer of Whisper-M. ST: Single-task, MT: multi-task.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue.t2-p5-yue-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue.t2-p5-yue-large.gif)

Table 2 summarizes the FT and PT results, reporting the performance of the best layers (based on probing) on the detection and severity assessment tasks, together with the FBank feature baseline results.

### 4.3. Mutual information and Silhouette score analysis

While probing with linear classifier \[30\] evaluates task performance directly, MI provides a task-agnostic view of representational relevance. Fig. 4 shows the MI between embeddings of each layer and dysarthria labels in the detection and severity classification tasks. Results peak at layer 13, consistent with probing results for the task performance (Figs. 2) that identified layers 13-15 as optimal. Unlike classifier-based probing, this finding is obtained without training.

We also calculated the MI between embeddings of the PT and FT Whisper models. This analysis, which is task-agnostic as it does not rely on labels, quantifies how MI varies across different layers after FT. The results show that MI values decrease progressively from the lower to the higher layers. This trend can be perfectly explained by the fact that the back-propagated gradient is stronger in the higher layers during FT, leading to more substantial modifications, whereas the lower layers receive a weaker gradient and thus undergo minimal changes, resulting in higher MI values.

Finally, we probed the Whisper encoder layers using the Silhouette score metric, which measures how well-separated different class clusters are in the feature space. Fig. 5 presents the results for the detection task.<sup>4</sup> As shown, the Silhouette score peaks around layer 13, aligning closely with both accuracy, F1-score and MI analyses.

This consistency is particularly intriguing from a technical perspective, as these metrics stem from fundamentally different modelling approaches. Silhouette score is a geometric clustering measure, mutual information quantifies statistical dependence between representations and class labels, and classification accuracy/F1-score directly evaluates task performance using a trained classifier. The fact that all metrics peak around the same layer suggests that the representations learned by Whisper strike an optimal balance between structure and task-relevant information at this depth. Moreover, the minimal decline in Silhouette score at higher layers further supports our hypothesis that the Whisper encoder maps different speech types into distinct subspaces rather than collapsing them into a single canonical representation.

[![Fig. 4. - 
            Layer-wise MI between (a) Whisper-M PT embeddings and labels, (b) PT and FT Whisper-M embeddings.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue4-p5-yue-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue4-p5-yue-large.gif)

**Fig. 4.**

Layer-wise MI between (a) Whisper-M PT embeddings and labels, (b) PT and FT Whisper-M embeddings.

[![Fig. 5. - 
            Silhouette score between layer-wise pretrained Whisper-M embeddings and labels for the detection task.
          ](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue5-p5-yue-small.gif)](https://ieeexplore.ieee.org/mediastore/IEEE/content/media/11460365/11460314/11464429/yue5-p5-yue-large.gif)

**Fig. 5.**

Silhouette score between layer-wise pretrained Whisper-M embeddings and labels for the detection task.

We probed Whisper-M’s encoder layers for dysarthric speech detection and severity classification, finding that mid-level layers (13–15) consistently provided optimal performance across accuracy, F1-score, mutual information, and Silhouette score. Multitask learning did not improve results. Fine-tuning on dysarthric ASR modified higher-layer representations but had limited impact on downstream classification, suggesting that pretrained Whisper embeddings already provide strong task-relevant features. Our findings offer two key insights: first, large-scale models trained for ASR implicitly encode clinically relevant information that can be repurposed for pathological speech assessment; second, probing across complementary metrics reveals consistent representational patterns, strengthening the reliability of layer selection strategies. Future work includes applying other fundamental models and more diverse pathological speech datasets.