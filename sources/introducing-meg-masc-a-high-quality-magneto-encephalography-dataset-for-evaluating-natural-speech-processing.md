---
title: "Introducing MEG-MASC a high-quality magneto-encephalography dataset for evaluating natural speech processing"
source: "https://www.nature.com/articles/s41597-023-02752-5"
author:
  - "[[Laura Gwilliams]]"
  - "[[Graham Flick]]"
  - "[[Alec Marantz]]"
  - "[[Liina Pylkkänen]]"
  - "[[David Poeppel]]"
  - "[[Jean-Rémi King]]"
published: 2023-12-03
created: 2026-04-29
description: "The “MEG-MASC” dataset provides a curated set of raw magnetoencephalography (MEG) recordings of 27 English speakers who listened to two hours of naturalistic stories. Each participant performed two identical sessions, involving listening to four fictional stories from the Manually Annotated Sub-Corpus (MASC) intermixed with random word lists and comprehension questions. We time-stamp the onset and offset of each word and phoneme in the metadata of the recording, and organize the dataset according to the ‘Brain Imaging Data Structure’ (BIDS). This data collection provides a suitable benchmark to large-scale encoding and decoding analyses of temporally-resolved brain responses to speech. We provide the Python code to replicate several validations analyses of the MEG evoked responses such as the temporal decoding of phonetic features and word frequency. All code and MEG, audio and text data are publicly available to keep with best practices in transparent and reproducible research."
tags:
  - "clippings"
  - "datasets"
  - "speech"
  - "speech-representations"
  - "citable"
---
## Abstract

The “MEG-MASC” dataset provides a curated set of raw magnetoencephalography (MEG) recordings of 27 English speakers who listened to two hours of naturalistic stories. Each participant performed two identical sessions, involving listening to four fictional stories from the Manually Annotated Sub-Corpus (MASC) intermixed with random word lists and comprehension questions. We time-stamp the onset and offset of each word and phoneme in the metadata of the recording, and organize the dataset according to the ‘Brain Imaging Data Structure’ (BIDS). This data collection provides a suitable benchmark to large-scale encoding and decoding analyses of temporally-resolved brain responses to speech. We provide the Python code to replicate several validations analyses of the MEG evoked responses such as the temporal decoding of phonetic features and word frequency. All code and MEG, audio and text data are publicly available to keep with best practices in transparent and reproducible research.

## Background & Summary

Humans have the unique ability to produce and comprehend an infinite number of novel utterances. This capacity of the human brain has been the subject of vigorous studies for decades. Yet, the core computational mechanisms upholding this feat remain largely unknown [^1] [^2] [^3].

To tackle this issue, a common experimental approach has been to decompose language processing into elementary computations using highly controlled factorial designs. This approach allows experimenters to compare average brain responses to carefully chosen stimuli and make inferences based on the select ways that those stimuli were designed to differ. The field has learnt a lot about the neurobiology of language by taking this approach; however, factorial designs also face several key challenges [^4]. First, this method has led the community to study language processing in atypical scenarios (*e.g*. using unusual text fonts [^5], meaningless syntactic constructs [^6] [^7], or words and phrases isolated from context [^8] [^9]). Presenting language in this unconventional manner runs the risk of studying phenomena that are not representative of how language is naturally processed. Second, high-level cognitive functions can be difficult to fully orthogonalize in a factorial design. For instance, comparing brain responses to words and sentences matched in length, syntactic structure, plausibility and pronunciation is often close to impossible. In the best case, experimenters will be forced to make concessions on how well the critical contrasts are controlled. In the worst case, unidentified confounds may drive differences associated with experimental contrasts, leading to incorrect conclusions.

During the past decade, several studies have complemented the factorial paradigm with more natural experiments. In these studies, participants listen to continuous speech [^10] [^11] [^12], read continuous prose [^13] [^14] or watch videos that include verbal communication [^15]. This approach is more likely to recruit neural computations that are representative of day-to-day language processing. Complications arising from correlated language features can be overcome by explicitly modeling properties of interest, in tandem with potential confounds. This allows variance belonging to either source to be appropriately distinguished.

To analyze the brain responses to the complex stimulation that natural language provides, a variety of encoding and decoding methods have proved remarkably effective [^10] [^16] [^17] [^18] [^19] [^20] [^21]. Consequently, language studies based on naturalistic designs have since flourished [^11] [^22]. The popularity of this approach has some of its roots in the rise of natural language processing (NLP) algorithms, which map remarkably onto brain responses to written and spoken sentences [^23] [^24] [^25] [^26] [^27] [^28] [^29] [^30]. Such tools also allow experimenters to annotate the language stimuli for features of interest, without relying on time-consuming annotations done by hand. These data have allowed researchers to identify the main semantic components [^10], recover the hierarchy of integration constants in the language network [^31], distinguish syntax and semantics hubs [^32] and to track the hierarchy of predictions elicited during speech processing [^28] [^33] [^34]. More generally, brain responses to natural stories have proved useful in keeping participants engaged, while studying the neural representations of phonemes, word surprisal and entropy [^22] [^35] [^36].

While large and high-quality functional Magnetic Resonance Imaging (fMRI) datasets related to language processing have recently been released [^37] [^38], there is currently little publicly available high-quality temporally-resolved brain recordings acquired during story listening. The most extensive databases of such data include:

Until the present release of our dataset, there existed no public magneto-encephalography (MEG) with (1) several hours of story listening (2) multiple sessions (3) a systematic audio, phonetic and word annotations (4) a standardized data structure. Thus, our dataset offers a powerful resource to the scientific community.

In the present study, 27 English-speaking subjects performed ~two hours of story listening, punctuated by random word lists and comprehension questions in the MEG scanner. Except if stated otherwise, each subject listened to four distinct fictional stories twice.

## Methods

### Participants

Twenty-seven English-speaking adults were recruited from the subject pool of NYU Abu Dhabi (15 females; age: M = 24.8, SD = 6.4). All participants provided a written informed consent and were compensated for their time. Participants reported having normal hearing and no history of neurological disorders. All participants were right-handed, as evaluated using the Edinburgh Handedness Inventory questionnaire [^42]. All but one participant (S21) were native English speakers - this person was a native speaker of Hindi, and learned English at 10 years old. All but five participants (S3, S12, S16, S20, S21) performed two identical one-hour-long sessions. These two recording sessions were separated by at least one day and at most two months depending on the availability of the experimenters and of the participants. The study was approved by the Institutional Review Board (IRB) ethics committee of New York University Abu Dhabi.

### Procedure

Within each ∼1 h recording session, participants were recorded with a 208 axial-gradiometer MEG scanner built by the Kanazawa Institute of Technology (KIT), and sampled at 1,000 Hz, and online band-pass filtered between 0.01 and 200 Hz while they listened to four distinct stories through binaural tube earphones (Aero Technologies), at a mean level of 70 dB sound pressure level.

Before the experiment, participants were exposed to 20 sec of each of the distinct speaker voices used in the study to (i) clarify the structure of the session and (ii) familiarize the participants with these voices. The sound files and scripts are available in (‘/stimuli/exp\_intro/’).

The order in which the four stories were presented was assigned pseudo-randomly, thanks to a “Latin-square design” across participants. The story order for each participant can be found in ‘participants.tsv’. This participant-specific order was used for both recording sessions. Our motivation for running two identical sessions was to (i) give researchers the ability to average the data across the two recordings to boost signal-to-noise; (ii) provide a like-for-like data reliability measure; (iii) give the opportunity for matched train and test datasets if attempting to run cross validated analyses.

To ensure that the participants were attentive to the stories, they responded, every ∼3 min and with a button press, to a two-alternative forced-choice question relative to the story content (e.g. ‘What precious material had Chuck found? Diamonds or Gold’). Participants performed this task with an average accuracy of 98%, confirming their engagement with and comprehension of the stories. The questions and answers are provided in (‘stimuli/task/question\_dict.py’).

Participants who did not already have a T1-weighted anatomical scan usable for the present study were scanned in a 3 T Magnetic-Resonance-Imaging (MRI) scanner after the MEG recording to avoid magnetic artefacts. Twelve participants returned for their T1 scan.

Before each MEG session, the head shape of each participant was digitized with a hand-held FastSCAN laser scanner (Polhemus), and co-registered with five head-position coils. The positions of these coils with regard to the MEG sensors were collected before and after each recording and stored in the ‘marker’ file, following the KIT’s system. The experimenter continuously monitored head position during the acquisition to ensure that the participants did not move.

### Stimuli

Four English fictional stories were selected from the Manually Annotated Sub-Corpus (MASC) which is part of the larger Open American National Corpus [^43]. MASC is distributed without license or other restrictions ([https://anc.org/data/masc/corpus/577-2/](https://anc.org/data/masc/corpus/577-2/)):

- ‘LW1’: a 861-word story narrating an alien spaceship trying to find its way home (5 min, 20 sec)
- ‘Cable Spool Boy’: a 1,948-word story narrating two young brothers playing in the woods (11 min)
- ‘Easy Money’: a 3,541-word fiction narrating two friends using a magical trick to make money (12 min, 10 sec)
- ‘The Black Willow’: a 4,652-word story narrating the difficulties an author encounters during writing (25 min, 50 sec)

An audio track corresponding to each of these stories was synthesized using Mac OS Mojave © version 10.14 text-to-speech. To help decorrelate language features from acoustic representations, we varied both voices and speech rate every 5–20 sentences. Specifically, we used three distinct synthetic voices:‘Ava’, ‘Samantha’ and ‘Allison’ speaking between 145 and 205 words per minute. Additionally, we varied the silence between sentences between 0 and 1,000 ms. Both speech rate and silence duration were sampled from a uniform distribution between the min and max values.

Each story was divided into ~3 min sound files. In between these sounds – approximately every 30 s – we played a random word list generated from the unique content words (nouns, proper nouns, verbs, adverbs and adjectives) selected from the preceding 5 min segment presented in random order. We decided to include word lists to allow data users to compare brain responses to content words within and outside of context, following experimental paradigms of previous studies [^38] [^44]. In addition, a very small fraction (<1%) of non-words were inserted into the natural sentences, on average every 30 words. We decided to include non-words to allow comparisons between phonetic sequences that do and do not have an associated meaning.

Hereafter, and following the BIDS labeling [^45], each “task” corresponds to the concatenation of these sentences and word lists. Each subject listened to the exact same set of four tasks, in a different block order.

### Preprocessing

#### MEG

The MEG dataset and its annotations are shared raw (i.e. not preprocessed) organized according to the Brain Imaging Data Structure [^45] MNE-BIDS [^46].

#### MRI

Structural MRIs were collected with separate averages of the T1w images using 3D MPRAGE sequence with 0.8 mm isotropic resolution (FOV = 256 mm, matrix = 320,208 sagittal slices in a single slab), TR = 2400 ms, TE = 2.22 ms, TI = 1000 ms, FA = 8 degrees, Bandwidth (BW) = 220 Hz per pixel, Echo Spacing (ES) = 7.5 ms, phase encoding undersampling factor GRAPPA = 2, no phase encoding oversampling.

To avoid subject identification, the T1-weighted MRI anatomical scan was defaced using PyDeface [^47] ([https://github.com/poldracklab/pydeface](https://github.com/poldracklab/pydeface)) and manually checked.

For four subjects (02, 06, 07, 19) we were unable to record structural MRIs, and so instead we provide the scaled FreeSurfer average MRIs in their place.

The alignment between the spaces of (1) the head-position coils, (2) the MEG sensors and (3) the T1 MRI was co-registered manually with MNE-Python [^48].

#### Stimuli

We include in the dataset: the original stories (‘stimuli/text’), the stories intertwined with the word lists (‘stimuli/text\_with\_wordlists’) and their corresponding audio tracks (‘stimuli/audio’).he alignment between the MEG data and the words and phonemes is provided for each participant separately (e.g., /sub-01/ses-0/meg/sub-01\_ses-0\_task-1\_events.tsv’).

Both sentences and word lists were annotated for phoneme boundaries and labels (107 phoneme labels, detailing phoneme category and its location in the word (Beginning; Internal; End) using the ‘Gentle aligner’ from the Python module lowerquality [https://lowerquality.com/gentle/](https://lowerquality.com/gentle/). However, the inclusion of the original audio leaves the possibility for future research to develop more advanced alignment technique and recover additional features.

For each phoneme and word, we indicate the corresponding voice, speech rate, wav file, story, word position within the sequence, and sequence position within the story, and whether the sequence is a word list or a sentence.

#### Computing environment

In addition to the packages mentioned in this manuscript, the processing of the present data is based on the free and open-source ecosystem of the neuroimaging community. In particular, we used:

- MNE BIDS [^46] ([https://mne.tools/mne-bids](https://mne.tools/mne-bids))
- Bids-Validator ([https://github.com/bids-standard/bids-validator](https://github.com/bids-standard/bids-validator))
- Nibabel [^49] ([https://nipy.org/nibabel/](https://nipy.org/nibabel/))
- Scikit-Learn [^50] ([https://scikit-learn.org/](https://scikit-learn.org/))
- Pandas [^51] ([https://pandas.pydata.org/](https://pandas.pydata.org/))

## Data Records

The dataset is organized according to Brain Imaging Data Structure (BIDS) 1.2.1 [^45] and publicly available on the Open Science Framework data repository [^52] [https://doi.org/10.17605/OSF.IO/AG3KJ](https://doi.org/10.17605/OSF.IO/AG3KJ) under a Creative Common Licence 0. An image of the folder structure is provided in Fig. [1](https://www.nature.com/articles/s41597-023-02752-5#Fig1). The detailed description of the BIDS file system is available at [http://bids.neuroimaging.io/](http://bids.neuroimaging.io/). In summary,

**Fig. 1**

![Fig. 1](https://media.springernature.com/lw685/springer-static/image/art%3A10.1038%2Fs41597-023-02752-5/MediaObjects/41597_2023_2752_Fig1_HTML.png?as=webp)

[Full size image](https://www.nature.com/articles/s41597-023-02752-5/figures/1)

Dataset file structure.

• ‘./dataset\_description.json’ describes the dataset

• ‘./participants.tsv’ indicates the age and gender of each participant, the order in which they heard the stories, whether they have an anatomical MRI scan, and how many recording sessions they completed

• ‘./stimuli/’ contains the original texts, the modified texts (*i.e*. with word lists), the synthesized audio tracks.

- Each’./sub-SXXX’ contains the brain recordings of a unique participant divided by session (*e.g*.’ses-0’ and’ses-1’)
- In each session folder lies the anatomical and the meg data, and the timestamp annotations (see Fig. [4](https://www.nature.com/articles/s41597-023-02752-5#Fig4)).
- Sessions are numbered by temporal order (s0 is first; s1 is second).
- Tasks are numbered by a unique integer common across participants.
- The dataset can be read directly with MNE-BIDS [^46].

## Technical Validation

We checked that the present dataset complies with the standardized brain imaging data structure by using the Bids-Validator ([https://github.com/bids-standard/bids-validator](https://github.com/bids-standard/bids-validator)).

MEG recordings are notoriously noisy and thus challenging to validate empirically. In particular, MEG can be corrupted by environmental noise (nearby electronic systems) and physiological noise (eye movement, heart activity, facial movements) [^53]. To address this issue, several labs have proposed a myriad of preprocessing techniques based on temporal and spatial filtering [^54] and trial and channel rejection [^55]. However, there is currently no accepted standard for the selection and ordering these preprocessing steps. Consequently, we here opted for (1) a minimalist preprocessing pipeline derived from MNE-Python’s default pipeline [^48] followed by (2) median evoked responses and (3) standard single-trial linear decoding analyses.

### Minimal preprocessing

For each subject separately, and using the default parameters of MNE-Python, we:

- bandpass filtered the MEG data between 0.5 and 30.0 Hz with raw.load\_data().filter(0.5, 30.0, n\_jobs = 1),
- temporally-decimate the data 10x, segment these continuous signals between −200 ms and 600 ms after word and phoneme onset, and apply a baseline correction between −200 ms to 0 ms with mne.Epochs(tmin = −0.2, tmax = 0.6, decim = 10, baseline = (−0.2, 0.0)),
- and clip the MEG data between fifth and ninety-fifth percentile of the data across channels.

### Evoked

Figure [2](https://www.nature.com/articles/s41597-023-02752-5#Fig2) displays the median evoked responses across participants and words onset and after phoneme onsets, respectively. Both of these topographies are typical of auditory activity in MEG [^36].

**Fig. 2**

![Fig. 2](https://media.springernature.com/lw685/springer-static/image/art%3A10.1038%2Fs41597-023-02752-5/MediaObjects/41597_2023_2752_Fig2_HTML.png?as=webp)

[Full size image](https://www.nature.com/articles/s41597-023-02752-5/figures/2)

Median (across subjects) evoked response to all words. The gray area indicates the global field power (GFP).

### Decoding

For each recording independently, our objective was to verify the alignment between the word annotations and the MEG recordings. To this end, we trained a linear classifier *W ∈* R *d* across all *d =* 208 magnetometers (*X ∈* R *n*  ×  *d*), for each time sample relative to word (or phoneme) onset independently, and for each subject separately. The classifier consisted of a standard scaler, followed by a linear discriminant regression implemented by scikit-learn [^50] using model = make\_pipeline(StandardScaler(), LinearDiscriminantAnalysis())

- decode high versus low median zipf-frequency of each word, as defined by the WordFreq package [^56].
- decode whether the phoneme is voiced or not.

The decoding pipeline was trained and evaluated using a five-split cross-validation scheme (with shuffling) using cv = KFold(5, shuffle = True, random\_state = 0) The scoring metric reported is Pearson R correlation between the continuous probabilistic output of the classifier on each trial, and the ground truth label (high vs. low for word frequency; voiced vs. voiceless for voicing). The full decoding pipeline can be found in the script check\_decoding.py.

The results displayed in Fig. [3](https://www.nature.com/articles/s41597-023-02752-5#Fig3) show a reliable decoding at the phoneme and at the word level, across both subjects and tasks (*i.e*. stories).

**Fig. 3**

![Fig. 3](https://media.springernature.com/lw685/springer-static/image/art%3A10.1038%2Fs41597-023-02752-5/MediaObjects/41597_2023_2752_Fig3_HTML.png?as=webp)

[Full size image](https://www.nature.com/articles/s41597-023-02752-5/figures/3)

(**a**) Average (mean) decoding of whether the phoneme is voiced or not as a function of time following phoneme onset. The four colors refer to the four tasks (stories + word lists). Error bar are SEM across subjects. (**b**) Same as A for the decoding of words’ zipf frequency as a function of word onset. (**c**) Decoding of voicing (average across all tasks) for each participant, as a function of time following phoneme onset. (**d**) Same as C for decoding of word frequency (average across all tasks) for each participant, as a function of time following word onset.

**Fig. 4**

![Fig. 4](https://media.springernature.com/lw685/springer-static/image/art%3A10.1038%2Fs41597-023-02752-5/MediaObjects/41597_2023_2752_Fig4_HTML.png)

[Full size image](https://www.nature.com/articles/s41597-023-02752-5/figures/4)

MEG data annotations: Pandas DataFrame of sound, phoneme and word time-stamps.

The success of our decoding analysis demonstrates: (i) the data have been correctly time-stamped relative to phoneme and word onset, in order to elicit a zero-aligned decoding timecourse; (ii) the data contain reliable signals that contain speech-related properties, suitable for further investigation; (iii) information at multiple levels (phonetic and lexical) are present in the data, allowing users to test hypotheses at different linguistic levels of description. We anticipate that encoding models would provide equally compelling results. Note that a decoding performance of Pearson R = 0.08 is typical for single-trial MEG data of continuous listening, and is of the same magnitude that has been reported in previous studies [^38]. We have a large number of events (tens of thousands of phonemes; thousands of words), and this dataset has been demonstrated to provide sufficient statistical power to yield significant results, despite small effect sizes [^36] [^57].

## Usage Notes

import pandas as pd import mne bids bids\_path = mne\_bids.BIDSPATH( subject = ’01’, session = ’0’, task = ’0’, datatype = ”meg”, root = ’my/data/path’) raw = mne\_bids.read\_raw\_bids(bids\_path) raw.load\_data().data # channels X times df = raw.annotations.to\_data\_frame()

Accessing all sound, word and phoneme annotation is directly readable in a Pandas [^58] DataFrame format:

df = pd.DataFrame(df.description.apply(eval).to\_list())

## Code availability

The code is available on [https://github.com/kingjr/meg-masc/](https://github.com/kingjr/meg-masc/).

## References

## Acknowledgements

This work was funded in part by FrontCog grant ANR-17-EURE-0017 (JRK); Abu Dhabi Research Institute (G1001) (AM; LP); Dingwall Foundation (LG).

## Ethics declarations

### Competing interests

The authors declare no competing interests.

## Additional information

**Publisher’s note** Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

## Rights and permissions

**Open Access** This article is licensed under a Creative Commons Attribution 4.0 International License, which permits use, sharing, adaptation, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons licence, and indicate if changes were made. The images or other third party material in this article are included in the article’s Creative Commons licence, unless indicated otherwise in a credit line to the material. If material is not included in the article’s Creative Commons licence and your intended use is not permitted by statutory regulation or exceeds the permitted use, you will need to obtain permission directly from the copyright holder. To view a copy of this licence, visit [http://creativecommons.org/licenses/by/4.0/](http://creativecommons.org/licenses/by/4.0/).

[^1]: Hickok, G. & Poeppel, D. The cortical organization of speech processing. *Nat. reviews neuroscience* **8**, 393–402 (2007).

[Article](https://doi.org/10.1038%2Fnrn2113) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=17431404) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BD2sXksFSis7w%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20cortical%20organization%20of%20speech%20processing&journal=Nat.%20reviews%20neuroscience&doi=10.1038%2Fnrn2113&volume=8&pages=393-402&publication_year=2007&author=Hickok%2CG&author=Poeppel%2CD)

[^2]: Berwick, R. C., Friederici, A. D., Chomsky, N. & Bolhuis, J. J. Evolution, brain, and the nature of language. *Trends cognitive sciences* **17**, 89–98 (2013).

[Article](https://doi.org/10.1016%2Fj.tics.2012.12.002) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=23313359) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Evolution%2C%20brain%2C%20and%20the%20nature%20of%20language&journal=Trends%20cognitive%20sciences&doi=10.1016%2Fj.tics.2012.12.002&volume=17&pages=89-98&publication_year=2013&author=Berwick%2CRC&author=Friederici%2CAD&author=Chomsky%2CN&author=Bolhuis%2CJJ)

[^3]: Dehaene, S., Meyniel, F., Wacongne, C., Wang, L. & Pallier, C. The neural representation of sequences: from transition probabilities to algebraic patterns and linguistic trees. *Neuron* **88**, 2–19 (2015).

[Article](https://doi.org/10.1016%2Fj.neuron.2015.09.019) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=26447569) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC2MXhs1ers7fM) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20neural%20representation%20of%20sequences%3A%20from%20transition%20probabilities%20to%20algebraic%20patterns%20and%20linguistic%20trees&journal=Neuron&doi=10.1016%2Fj.neuron.2015.09.019&volume=88&pages=2-19&publication_year=2015&author=Dehaene%2CS&author=Meyniel%2CF&author=Wacongne%2CC&author=Wang%2CL&author=Pallier%2CC)

[^4]: Hamilton, L. S. & Huth, A. G. The revolution will not be controlled: natural stimuli in speech neuroscience. *Lang. cognition neuroscience* **35**, 573–582 (2020).

[Article](https://doi.org/10.1080%2F23273798.2018.1499946) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20revolution%20will%20not%20be%20controlled%3A%20natural%20stimuli%20in%20speech%20neuroscience&journal=Lang.%20cognition%20neuroscience&doi=10.1080%2F23273798.2018.1499946&volume=35&pages=573-582&publication_year=2020&author=Hamilton%2CLS&author=Huth%2CAG)

[^5]: Gwilliams, L. & King, J.-R. Recurrent processes support a cascade of hierarchical decisions. *ELife* **9**, e56603 (2020).

[Article](https://doi.org/10.7554%2FeLife.56603) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=32869746) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC7513462) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BB3cXisFCrurrK) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Recurrent%20processes%20support%20a%20cascade%20of%20hierarchical%20decisions&journal=ELife&doi=10.7554%2FeLife.56603&volume=9&publication_year=2020&author=Gwilliams%2CL&author=King%2CJ-R)

[^6]: Pallier, C., Devauchelle, A.-D. & Dehaene, S. Cortical representation of the constituent structure of sentences. *Proc. Natl. Acad. Sci.* **108**, 2522–2527 (2011).

[Article](https://doi.org/10.1073%2Fpnas.1018711108) [ADS](http://adsabs.harvard.edu/cgi-bin/nph-data_query?link_type=ABSTRACT&bibcode=2011PNAS..108.2522P) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=21224415) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3038732) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC3MXitFWns7o%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Cortical%20representation%20of%20the%20constituent%20structure%20of%20sentences&journal=Proc.%20Natl.%20Acad.%20Sci.&doi=10.1073%2Fpnas.1018711108&volume=108&pages=2522-2527&publication_year=2011&author=Pallier%2CC&author=Devauchelle%2CA-D&author=Dehaene%2CS)

[^7]: Petersson, K.-M., Folia, V. & Hagoort, P. What artificial grammar learning reveals about the neurobiology of syntax. *Brain language* **120**, 83–95 (2012).

[Article](https://doi.org/10.1016%2Fj.bandl.2010.08.003) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=20943261) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=What%20artificial%20grammar%20learning%20reveals%20about%20the%20neurobiology%20of%20syntax&journal=Brain%20language&doi=10.1016%2Fj.bandl.2010.08.003&volume=120&pages=83-95&publication_year=2012&author=Petersson%2CK-M&author=Folia%2CV&author=Hagoort%2CP)

[^8]: Gwilliams, L., Linzen, T., Poeppel, D. & Marantz, A. In spoken word recognition, the future predicts the past. *J. Neurosci.* **38**, 7585–7599 (2018).

[Article](https://doi.org/10.1523%2FJNEUROSCI.0065-18.2018) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=30012695) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC6113903) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC1cXit1Kktb%2FN) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=In%20spoken%20word%20recognition%2C%20the%20future%20predicts%20the%20past&journal=J.%20Neurosci.&doi=10.1523%2FJNEUROSCI.0065-18.2018&volume=38&pages=7585-7599&publication_year=2018&author=Gwilliams%2CL&author=Linzen%2CT&author=Poeppel%2CD&author=Marantz%2CA)

[^9]: Bemis, D. K. & Pylkkänen, L. Simple composition: A magnetoencephalography investigation into the comprehension of minimal linguistic phrases. *J. Neurosci.* **31**, 2801–2814 (2011).

[Article](https://doi.org/10.1523%2FJNEUROSCI.5003-10.2011) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=21414902) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC6623787) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC3MXislGisbs%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Simple%20composition%3A%20A%20magnetoencephalography%20investigation%20into%20the%20comprehension%20of%20minimal%20linguistic%20phrases&journal=J.%20Neurosci.&doi=10.1523%2FJNEUROSCI.5003-10.2011&volume=31&pages=2801-2814&publication_year=2011&author=Bemis%2CDK&author=Pylkk%C3%A4nen%2CL)

[^10]: Huth, A. G., De Heer, W. A., Griffiths, T. L., Theunissen, F. E. & Gallant, J. L. Natural speech reveals the semantic maps that tile human cerebral cortex. *Nature* **532**, 453–458 (2016).

[Article](https://doi.org/10.1038%2Fnature17637) [ADS](http://adsabs.harvard.edu/cgi-bin/nph-data_query?link_type=ABSTRACT&bibcode=2016Natur.532..453H) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=27121839) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4852309) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Natural%20speech%20reveals%20the%20semantic%20maps%20that%20tile%20human%20cerebral%20cortex&journal=Nature&doi=10.1038%2Fnature17637&volume=532&pages=453-458&publication_year=2016&author=Huth%2CAG&author=Heer%2CWA&author=Griffiths%2CTL&author=Theunissen%2CFE&author=Gallant%2CJL)

[^11]: Broderick, M. P., Anderson, A. J., Di Liberto, G. M., Crosse, M. J. & Lalor, E. C. Electrophysiological correlates of semantic dissimilarity reflect the comprehension of natural, narrative speech. *Curr. Biol.* **28**, 803–809 (2018).

[Article](https://doi.org/10.1016%2Fj.cub.2018.01.080) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=29478856) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC1cXjt1SgsLc%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Electrophysiological%20correlates%20of%20semantic%20dissimilarity%20reflect%20the%20comprehension%20of%20natural%2C%20narrative%20speech&journal=Curr.%20Biol.&doi=10.1016%2Fj.cub.2018.01.080&volume=28&pages=803-809&publication_year=2018&author=Broderick%2CMP&author=Anderson%2CAJ&author=Liberto%2CGM&author=Crosse%2CMJ&author=Lalor%2CEC)

[^12]: Brodbeck, C. & Simon, J. Z. Continuous speech processing. *Curr. Opin. Physiol.* **18**, 25–31 (2020).

[Article](https://doi.org/10.1016%2Fj.cophys.2020.07.014) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=33225119) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC7673294) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Continuous%20speech%20processing&journal=Curr.%20Opin.%20Physiol.&doi=10.1016%2Fj.cophys.2020.07.014&volume=18&pages=25-31&publication_year=2020&author=Brodbeck%2CC&author=Simon%2CJZ)

[^13]: Schuster, S., Hawelka, S., Hutzler, F., Kronbichler, M. & Richlan, F. Words in context: The effects of length, frequency, and predictability on brain responses during natural reading. *Cereb. Cortex* **26**, 3889–3904 (2016).

[Article](https://doi.org/10.1093%2Fcercor%2Fbhw184) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=27365297) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC5028003) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Words%20in%20context%3A%20The%20effects%20of%20length%2C%20frequency%2C%20and%20predictability%20on%20brain%20responses%20during%20natural%20reading&journal=Cereb.%20Cortex&doi=10.1093%2Fcercor%2Fbhw184&volume=26&pages=3889-3904&publication_year=2016&author=Schuster%2CS&author=Hawelka%2CS&author=Hutzler%2CF&author=Kronbichler%2CM&author=Richlan%2CF)

[^14]: Wehbe, L. *et al*. Simultaneously uncovering the patterns of brain regions involved in different story reading subprocesses. *PloS one* **9**, e112575 (2014).

[Article](https://doi.org/10.1371%2Fjournal.pone.0112575) [ADS](http://adsabs.harvard.edu/cgi-bin/nph-data_query?link_type=ABSTRACT&bibcode=2014PLoSO...9k2575W) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=25426840) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4245107) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Simultaneously%20uncovering%20the%20patterns%20of%20brain%20regions%20involved%20in%20different%20story%20reading%20subprocesses&journal=PloS%20one&doi=10.1371%2Fjournal.pone.0112575&volume=9&publication_year=2014&author=Wehbe%2CL)

[^15]: Haxby, J. V., Connolly, A. C. & Guntupalli, J. S. Decoding neural representational spaces using multivariate pattern analysis. *Annu. review neuroscience* **37**, 435–456 (2014).

[Article](https://doi.org/10.1146%2Fannurev-neuro-062012-170325) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC2cXhsVKgtL%2FM) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Decoding%20neural%20representational%20spaces%20using%20multivariate%20pattern%20analysis&journal=Annu.%20review%20neuroscience&doi=10.1146%2Fannurev-neuro-062012-170325&volume=37&pages=435-456&publication_year=2014&author=Haxby%2CJV&author=Connolly%2CAC&author=Guntupalli%2CJS)

[^16]: Naselaris, T., Kay, K. N., Nishimoto, S. & Gallant, J. L. Encoding and decoding in fmri. *Neuroimage* **56**, 400–410 (2011).

[Article](https://doi.org/10.1016%2Fj.neuroimage.2010.07.073) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=20691790) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Encoding%20and%20decoding%20in%20fmri&journal=Neuroimage&doi=10.1016%2Fj.neuroimage.2010.07.073&volume=56&pages=400-410&publication_year=2011&author=Naselaris%2CT&author=Kay%2CKN&author=Nishimoto%2CS&author=Gallant%2CJL)

[^17]: Kriegeskorte, N., Mur, M. & Bandettini, P. A. Representational similarity analysis-connecting the branches of systems neuroscience. *Front. systems neuroscience* **2**, 4 (2008).

[Google Scholar](http://scholar.google.com/scholar_lookup?&title=Representational%20similarity%20analysis-connecting%20the%20branches%20of%20systems%20neuroscience&journal=Front.%20systems%20neuroscience&volume=2&publication_year=2008&author=Kriegeskorte%2CN&author=Mur%2CM&author=Bandettini%2CPA)

[^18]: King, J.-R. & Dehaene, S. Characterizing the dynamics of mental representations: the temporal generalization method. *Trends cognitive sciences* **18**, 203–210 (2014).

[Article](https://doi.org/10.1016%2Fj.tics.2014.01.002) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=24593982) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Characterizing%20the%20dynamics%20of%20mental%20representations%3A%20the%20temporal%20generalization%20method&journal=Trends%20cognitive%20sciences&doi=10.1016%2Fj.tics.2014.01.002&volume=18&pages=203-210&publication_year=2014&author=King%2CJ-R&author=Dehaene%2CS)

[^19]: King, J.-R. *et al*. Encoding and decoding neuronal dynamics: Methodological framework to uncover the algorithms of cognition (2018).

[^20]: King, J.-R., Charton, F., Lopez-Paz, D. & Oquab, M. Back-to-back regression: Disentangling the influence of correlated factors from multivariate observations. *NeuroImage* **220**, 117028 (2020).

[Article](https://doi.org/10.1016%2Fj.neuroimage.2020.117028) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=32603859) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Back-to-back%20regression%3A%20Disentangling%20the%20influence%20of%20correlated%20factors%20from%20multivariate%20observations&journal=NeuroImage&doi=10.1016%2Fj.neuroimage.2020.117028&volume=220&publication_year=2020&author=King%2CJ-R&author=Charton%2CF&author=Lopez-Paz%2CD&author=Oquab%2CM)

[^21]: Chehab, O., Defossez, A., Loiseau, J.-C., Gramfort, A. & King, J.-R. Deep recurrent encoder: A scalable end-to-end network to model brain signals. *arXiv preprint arXiv:2103.02339* (2021).

[^22]: Mesgarani, N., Cheung, C., Johnson, K. & Chang, E. F. Phonetic feature encoding in human superior temporal gyrus. *Science* **343**, 1006–1010 (2014).

[Article](https://doi.org/10.1126%2Fscience.1245994) [ADS](http://adsabs.harvard.edu/cgi-bin/nph-data_query?link_type=ABSTRACT&bibcode=2014Sci...343.1006M) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=24482117) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4350233) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC2cXjt1ymtrg%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Phonetic%20feature%20encoding%20in%20human%20superior%20temporal%20gyrus&journal=Science&doi=10.1126%2Fscience.1245994&volume=343&pages=1006-1010&publication_year=2014&author=Mesgarani%2CN&author=Cheung%2CC&author=Johnson%2CK&author=Chang%2CEF)

[^23]: Qian, P., Qiu, X. & Huang, X. Bridging lstm architecture and the neural dynamics during reading. *arXiv preprint arXiv:1604.06635* (2016).

[^24]: Jain, S. & Huth, A. Incorporating context into language encoding models for fmri. *Adv. neural information processing systems* **31** (2018).

[^25]: Toneva, M. & Wehbe, L. Interpreting and improving natural-language processing (in machines) with natural language-processing (in the brain). *Adv. Neural Inf. Process. Syst*. **32** (2019).

[^26]: Millet, J. & King, J.-R. Inductive biases, pretraining and fine-tuning jointly account for brain responses to speech. *arXiv preprint arXiv:2103.01032* (2021).

[^27]: Caucheteux, C. & King, J.-R. Brains and algorithms partially converge in natural language processing. *Commun. Biology* **5**, 1–10 (2022).

[Article](https://doi.org/10.1038%2Fs42003-022-03036-1) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Brains%20and%20algorithms%20partially%20converge%20in%20natural%20language%20processing&journal=Commun.%20Biology&doi=10.1038%2Fs42003-022-03036-1&volume=5&pages=1-10&publication_year=2022&author=Caucheteux%2CC&author=King%2CJ-R)

[^28]: Goldstein, A. *et al*. Shared computational principles for language processing in humans and deep language models. *Nat. neuroscience* **25**, 369–380 (2022).

[Article](https://doi.org/10.1038%2Fs41593-022-01026-4) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=35260860) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BB38XmsFajt7s%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Shared%20computational%20principles%20for%20language%20processing%20in%20humans%20and%20deep%20language%20models&journal=Nat.%20neuroscience&doi=10.1038%2Fs41593-022-01026-4&volume=25&pages=369-380&publication_year=2022&author=Goldstein%2CA)

[^29]: Schrimpf, M. *et al*. The neural architecture of language: Integrative modeling converges on predictive processing. *Proc. Natl. Acad. Sci*. **118** (2021).

[^30]: Caucheteux, C., Gramfort, A. & King, J.-R. Gpt-2’s activations predict the degree of semantic comprehension in the human brain (2021).

[^31]: Caucheteux, C., Gramfort, A. & King, J.-R. Model-based analysis of brain activity reveals the hierarchy of language in 305 subjects. *arXiv preprint arXiv:2110.06078*, (2021).

[^32]: Caucheteux, C., Gramfort, A. & King, J.-R. Disentangling syntax and semantics in the brain with deep networks. In *International Conference on Machine Learning*, 1336–1348 (PMLR, 2021).

[^33]: Caucheteux, C., Gramfort, A. & King, J.-R. Long-range and hierarchical language predictions in brains and algorithms. *arXiv preprint arXiv:2111.14232* (2021).

[^34]: Heilbron, M., Ehinger, B., Hagoort, P. & De Lange, F. P. Tracking naturalistic linguistic predictions with deep neural language models. *arXiv preprint arXiv:1909.04400*, (2019).

[^35]: Gillis, M., Vanthornhout, J., Simon, J. Z., Francart, T. & Brodbeck, C. Neural markers of speech comprehension: measuring eeg tracking of linguistic speech representations, controlling the speech acoustics. *J. Neurosci.* **41**, 10316–10329 (2021).

[Article](https://doi.org/10.1523%2FJNEUROSCI.0812-21.2021) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=34732519) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC8672699) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BB38XlslWqtQ%3D%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Neural%20markers%20of%20speech%20comprehension%3A%20measuring%20eeg%20tracking%20of%20linguistic%20speech%20representations%2C%20controlling%20the%20speech%20acoustics&journal=J.%20Neurosci.&doi=10.1523%2FJNEUROSCI.0812-21.2021&volume=41&pages=10316-10329&publication_year=2021&author=Gillis%2CM&author=Vanthornhout%2CJ&author=Simon%2CJZ&author=Francart%2CT&author=Brodbeck%2CC)

[^36]: Gwilliams, L., King, J. R., Marantz, A. & Poeppel, D. Neural dynamics of phoneme sequences reveal position-invariant code for content and order. *Nature Communications* **13** (1), 1–14 (2022).

[Article](https://doi.org/10.1038%2Fs41467-022-34326-1) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Neural%20dynamics%20of%20phoneme%20sequences%20reveal%20position-invariant%20code%20for%20content%20and%20order&journal=Nature%20Communications&doi=10.1038%2Fs41467-022-34326-1&volume=13&issue=1&pages=1-14&publication_year=2022&author=Gwilliams%2CL&author=King%2CJR&author=Marantz%2CA&author=Poeppel%2CD)

[^37]: Nastase, S. A. *et al*. The “narratives” fmri dataset for evaluating models of naturalistic language comprehension. *Sci. data* **8**, 1–22 (2021).

[Article](https://doi.org/10.1038%2Fs41597-021-01033-3) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20%E2%80%9Cnarratives%E2%80%9D%20fmri%20dataset%20for%20evaluating%20models%20of%20naturalistic%20language%20comprehension&journal=Sci.%20data&doi=10.1038%2Fs41597-021-01033-3&volume=8&pages=1-22&publication_year=2021&author=Nastase%2CSA)

[^38]: Schoffelen, J. *et al*. Mother of unification studies, a 204-subject multimodal neuroimaging dataset to study language processing (2019).

[^39]: Van Essen, D. C. *et al*. The WU-Minn human connectome project: an overview. *Neuroimage* **80**, 62–79 (2013).

[Article](https://doi.org/10.1016%2Fj.neuroimage.2013.05.041) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=23684880) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20WU-Minn%20human%20connectome%20project%3A%20an%20overview&journal=Neuroimage&doi=10.1016%2Fj.neuroimage.2013.05.041&volume=80&pages=62-79&publication_year=2013&author=Essen%2CDC)

[^40]: Brennan, J. R. & Hale, J. T. Hierarchical structure guides rapid linguistic predictions during naturalistic listening. *PloS one* **14** (1), e0207741 (2019).

[Article](https://doi.org/10.1371%2Fjournal.pone.0207741) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=30650078) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC6334990) [CAS](https://www.nature.com/articles/cas-redirect/1:CAS:528:DC%2BC1MXmtFCmsb4%3D) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Hierarchical%20structure%20guides%20rapid%20linguistic%20predictions%20during%20naturalistic%20listening&journal=PloS%20one&doi=10.1371%2Fjournal.pone.0207741&volume=14&issue=1&publication_year=2019&author=Brennan%2CJR&author=Hale%2CJT)

[^41]: Armeni, K. *et al*. A 10-hour within-participant magnetoencephalography narrative dataset to test models of language comprehension. *Sci Data* **9**, 278, [https://doi.org/10.1038/s41597-022-01382-7](https://doi.org/10.1038/s41597-022-01382-7) (2022).

[Article](https://doi.org/10.1038%2Fs41597-022-01382-7) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=35676293) [PubMed Central](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC9177538) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=A%2010-hour%20within-participant%20magnetoencephalography%20narrative%20dataset%20to%20test%20models%20of%20language%20comprehension&journal=Sci%20Data&doi=10.1038%2Fs41597-022-01382-7&volume=9&publication_year=2022&author=Armeni%2CK)

[^42]: Veale, J. F. Edinburgh handedness inventory–short form: a revised version based on confirmatory factor analysis. *Laterality: Asymmetries of Body, Brain and Cognition* **19** (2), 164–177 (2014).

[Article](https://doi.org/10.1080%2F1357650X.2013.783045) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Edinburgh%20handedness%20inventory%E2%80%93short%20form%3A%20a%20revised%20version%20based%20on%20confirmatory%20factor%20analysis&journal=Laterality%3A%20Asymmetries%20of%20Body%2C%20Brain%20and%20Cognition&doi=10.1080%2F1357650X.2013.783045&volume=19&issue=2&pages=164-177&publication_year=2014&author=Veale%2CJF)

[^43]: Ide, N. & Macleod, C. The american national corpus: A standardized resource of american english. In Proceedings of corpus linguistics (Vol. 3, pp. 1–7). Lancaster, UK: Lancaster University Centre for Computer Corpus Research on Language (2001).

[^44]: Fedorenko, E. *et al* Neural correlate of the construction of sentence meaning. Proceedings of the National Academy of Sciences, **113** (41), E6256-E6262. Chicago (2016).

[^45]: Gorgolewski, K. J. *et al*. The brain imaging data structure, a format for organizing and describing outputs of neuroimaging experiments. *Sci. data* **3**, 1–9 (2016).

[Article](https://doi.org/10.1038%2Fsdata.2016.44) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20brain%20imaging%20data%20structure%2C%20a%20format%20for%20organizing%20and%20describing%20outputs%20of%20neuroimaging%20experiments&journal=Sci.%20data&doi=10.1038%2Fsdata.2016.44&volume=3&pages=1-9&publication_year=2016&author=Gorgolewski%2CKJ)

[^46]: Appelhoff, S. *et al*. Mne-bids: Organizing electrophysiological data into the bids format and facilitating their analysis. *The J. Open Source Software*. **4** (2019).

[^47]: Gulban, OF. *et al*. poldracklab/pydeface: v2. 0.0, *Zenodo*, [https://doi.org/10.5281/zenodo.3524401](https://doi.org/10.5281/zenodo.3524401) (2019).

[^48]: Gramfort, A. *et al*. Meg and eeg data analysis with mne-python. *Front. neuroscience* **267** (2013).

[^49]: Brett, M. *et al*. nipy/nibabel: 3.2.1, *Zenodo*, [https://doi.org/10.5281/zenodo.4295521](https://doi.org/10.5281/zenodo.4295521) (2020).

[^50]: Pedregosa, F. *et al*. Scikit-learn: Machine learning in python. *J. machine Learn. research* **12**, 2825–2830 (2011).

[MathSciNet](http://www.ams.org/mathscinet-getitem?mr=2854348) [MATH](http://www.emis.de/MATH-item?1280.68189) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Scikit-learn%3A%20Machine%20learning%20in%20python&journal=J.%20machine%20Learn.%20research&volume=12&pages=2825-2830&publication_year=2011&author=Pedregosa%2CF)

[^51]: W McKinney. Data Structures for Statistical Computing in Python. In van der Walt, S. & Millman, J. (eds.) *Proceedings of the 9th Python in Science Conference*, 56–61, [https://doi.org/10.25080/Majora-92bf1922-00a](https://doi.org/10.25080/Majora-92bf1922-00a) (2010).

[^52]: King, J.-R. & Gwilliams, L. MASC-MEG. *OSF* [https://doi.org/10.17605/OSF.IO/AG3KJ](https://doi.org/10.17605/OSF.IO/AG3KJ) (2022).

[^53]: Hämäläinen, M., Hari, R., Ilmoniemi, R. J., Knuutila, J. & Lounasmaa, O. V. Magnetoencephalography—theory, instrumentation, and applications to noninvasive studies of the working human brain. *Rev. modern Phys.* **65**, 413 (1993).

[Article](https://doi.org/10.1103%2FRevModPhys.65.413) [ADS](http://adsabs.harvard.edu/cgi-bin/nph-data_query?link_type=ABSTRACT&bibcode=1993RvMP...65..413H) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Magnetoencephalography%E2%80%94theory%2C%20instrumentation%2C%20and%20applications%20to%20noninvasive%20studies%20of%20the%20working%20human%20brain&journal=Rev.%20modern%20Phys.&doi=10.1103%2FRevModPhys.65.413&volume=65&publication_year=1993&author=H%C3%A4m%C3%A4l%C3%A4inen%2CM&author=Hari%2CR&author=Ilmoniemi%2CRJ&author=Knuutila%2CJ&author=Lounasmaa%2COV)

[^54]: de Cheveigné, A. & Nelken, I. Filters: when, why, and how (not) to use them. *Neuron* **102**, 280–293 (2019).

[Article](https://doi.org/10.1016%2Fj.neuron.2019.02.039) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=30998899) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Filters%3A%20when%2C%20why%2C%20and%20how%20%28not%29%20to%20use%20them&journal=Neuron&doi=10.1016%2Fj.neuron.2019.02.039&volume=102&pages=280-293&publication_year=2019&author=Cheveign%C3%A9%2CA&author=Nelken%2CI)

[^55]: Jas, M., Engemann, D. A., Bekhti, Y., Raimondo, F. & Gramfort, A. Autoreject: Automated artifact rejection for meg and eeg data. *NeuroImage* **159**, 417–429 (2017).

[Article](https://doi.org/10.1016%2Fj.neuroimage.2017.06.030) [PubMed](http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?cmd=Retrieve&db=PubMed&dopt=Abstract&list_uids=28645840) [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Autoreject%3A%20Automated%20artifact%20rejection%20for%20meg%20and%20eeg%20data&journal=NeuroImage&doi=10.1016%2Fj.neuroimage.2017.06.030&volume=159&pages=417-429&publication_year=2017&author=Jas%2CM&author=Engemann%2CDA&author=Bekhti%2CY&author=Raimondo%2CF&author=Gramfort%2CA)

[^56]: Speer, R., Chin, J., Lin, A., Jewett, S. & Nathan, L. Luminosoinsight/wordfreq: v2.2, *Zenodo*, [https://doi.org/10.5281/zenodo.1443582](https://doi.org/10.5281/zenodo.1443582) (2018).

[^57]: Gwilliams, L., Marantz, A., Poeppel, D. & King, J. R. Top-down information shapes lexical processing when listening to continuous speech. Language, Cognition and Neuroscience, 1–14 (2023).

[^58]: pandas development team, T. pandas-dev/pandas: Pandas. *Zenodo* [https://doi.org/10.5281/zenodo.3509134](https://doi.org/10.5281/zenodo.3509134) (2020).