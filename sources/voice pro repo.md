---
title: "abus-aikorea voice-pro: Gradio WebUI for creators and developers, featuring key TTS (Edge-TTS, kokoro) and zero-shot Voice Cloning (E2 & F5-TTS, CosyVoice), with Whisper audio processing, YouTube download, Demucs vocal isolation, and multilingual translation."
source: "https://github.com/abus-aikorea/voice-pro"
author:
  - "[[abus-aikorea]]"
published:
created: 2026-04-26
description: "Gradio WebUI for creators and developers, featuring key TTS (Edge-TTS, kokoro) and zero-shot Voice Cloning (E2 & F5-TTS, CosyVoice), with Whisper audio processing, YouTube download, Demucs vocal isolation, and multilingual translation. - abus-aikorea/voice-pro"
tags:
  - "clippings"
  - "speech-representations"
  - "ai-model"
---
## Voice-Pro

*The best AI speech recognition, translation, and multilingual dubbing solution 🚀*

[![Dubbing Studio](https://github.com/abus-aikorea/voice-pro/raw/main/docs/images/main_page_crop.eng.jpg?raw=true)](https://github.com/abus-aikorea/voice-pro/blob/main/docs/images/main_page_crop.eng.jpg?raw=true)

## 🎙️ An AI-powered web application for speech recognition, translation, and dubbing

[한국어](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.kor.md) ∙ [English](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.eng.md) ∙ [中文简体](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.zh.md) ∙ [中文繁體](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.tw.md) ∙ [日本語](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.jpn.md) ∙ [Deutsch](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.deu.md) ∙ [Español](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.spa.md) ∙ [Português](https://github.com/abus-aikorea/voice-pro/blob/main/docs/README.por.md)

Voice-Pro is a state-of-the-art web app that transforms multimedia content creation. It integrates YouTube video downloading, voice separation, speech recognition, translation, and text-to-speech into a single, powerful tool for creators, researchers, and multilingual professionals.

- 🔊 Top-tier speech recognition: **Whisper**, **Faster-Whisper**, **Whisper-Timestamped**, **WhisperX**
- 🎤 Zero-shot voice cloning: **F5-TTS**, **E2-TTS**, **CosyVoice**
- 📢 Multilingual text-to-speech: **Edge-TTS**, **kokoro** (Paid version includes **Azure TTS**)
- 🎥 YouTube processing & audio extraction: **yt-dlp**
- 🌍 Instant translation for 100+ languages: **Deep-Translator** (Paid version includes **Azure Translator**)

A robust alternative to **ElevenLabs**, Voice-Pro empowers podcasters, developers, and creators with advanced voice solutions.

## ⚠️ Please Note

- Due to [WeConnect](https://www.wctokyoseoul.com/) development work, Voice-Pro development and updates are not possible for the time being.
- We have made all Voice-Pro code open source and completely free. Voice-Pro can now be freely distributed and modified by anyone.
- It works well on Windows with NVIDIA GPU. Operation on Mac and Linux has not been verified.
- Please leave your requests on the or pages.
- **Troubleshooting**: In most cases, issues can be resolved by deleting the `installer_files` folder and then running `configure.bat` followed by `start.bat`.

## 📰 News & History

version 3.2
- We have been focusing on [WeConnect](https://www.wctokyoseoul.com/) development for the past few months and have not been able to manage Voice-Pro at all.
- We have decided to open source all Voice-Pro code.
- Voice-Pro is completely free and supports Windows, Mac, Linux.
- [WeConnect](https://www.wctokyoseoul.com/) is an application for global cultural exchange.
- Connect with people from all over the world for meaningful cultural exchanges, language learning, and international friendships.
version 3.1
- 🪄 Support for fine-tuned models of **F5-TTS**
- 🌍 Supported languages
	- English & Chinese: [SWivid/F5-TTS\_v1](https://huggingface.co/SWivid/F5-TTS/tree/main/F5TTS_v1_Base)
		- Finnish: [AsmoKoskinen/F5-TTS\_Finnish\_Model](https://huggingface.co/AsmoKoskinen/F5-TTS_Finnish_Model)
		- French: [RASPIAUDIO/F5-French-MixedSpeakers-reduced](https://huggingface.co/RASPIAUDIO/F5-French-MixedSpeakers-reduced)
		- Hindi: [SPRINGLab/F5-Hindi-24KHz](https://huggingface.co/SPRINGLab/F5-Hindi-24KHz)
		- Italian: [alien79/F5-TTS-italian](https://huggingface.co/alien79/F5-TTS-italian)
		- Japanese: [Jmica/F5TTS/JA\_21999120](https://huggingface.co/Jmica/F5TTS/tree/main/JA_21999120)
		- Russian: [hotstone228/F5-TTS-Russian](https://huggingface.co/hotstone228/F5-TTS-Russian)
		- Spanish: [jpgallegoar/F5-Spanish](https://huggingface.co/jpgallegoar/F5-Spanish)
version 3.0
- 🔥 Removed the **AI Cover** feature.
- 🚀 Added support for **m-bain/whisperX**.
version 2.0
- 🐍 Built with Python 3.10.15, Torch 2.5.1+cu124, and Gradio 5.14.0.
- 🆓 Free trial supports media up to **60 seconds** in length.
- 🔥 Added the **AI Cover** feature.
- 🎤 Introduced support for **CosyVoice** and **kokoro**.
- ⏳ Initial run downloads **CozyVoice2-0.5B (9GB)**, which may take over an hour depending on network speed.
- 🎧 Voice samples for cloning will be continuously updated.
- 📝 Added **spaCy** for natural sentence-by-sentence translation and TTS.
- ☁️ Subscription version includes **Microsoft Azure** Translator and TTS.
- 🏪 Subscription offers **unlimited usage** (no 60-second limit) during the subscription period, available via .

## 🎥 YouTube Showcase

| [![Demo Video 1](https://camo.githubusercontent.com/282c18a87f99a1b5e240eb39ed80afbd776d3078b49dbb73f8652c075f884b1f/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f736343354369635a3647302f687164656661756c742e6a7067)   Demo for Voice-Pro (v2.0)](https://youtu.be/scC5CicZ6G0) | [![Demo Video 2](https://camo.githubusercontent.com/df644ae7b318fc928405ecc74c72c760c75d73d7616962589f1878fe6e75b24c/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f57666f3776514344346e6f2f687164656661756c742e6a7067)   F5-TTS: Voice Cloning](https://youtu.be/Wfo7vQCD4no) | [![Demo Video 3](https://camo.githubusercontent.com/4566fb53fd02562dc53d3f7d42235f74f980e6650c6fd86e931f27f3dba25e85/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f474f7a43446a344d43706f2f687164656661756c742e6a7067)   Live Transcription & Translation](https://youtu.be/GOzCDj4MCpo) | [![Demo Video 4](https://camo.githubusercontent.com/483476350ebcb9af8de0db9230a4c781725eb06f67f5a7e85abc42f84e84d35d/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f596441713830776a7475512f687164656661756c742e6a7067)   Multi-Lingual Voice Cloning: Korean - German](https://youtu.be/YdAq80wjtuQ) |
| --- | --- | --- | --- |
| [![Demo Video 5](https://camo.githubusercontent.com/8444e075b54f2d6e5ac805832059eb1c679ee098c988a360872316db691722c2/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f5475326f6b6f48593137342f687164656661756c742e6a7067)   Multi-Lingual Voice Cloning: English - Korean](https://youtu.be/Tu2okoHY174) | [![Demo Video 6](https://camo.githubusercontent.com/559b4d098296c5af906489a5642e7596e2f79fbfceb83a7e20cbf73eb4166c1c/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f64574345774f35365f37592f687164656661756c742e6a7067)   Multi-Lingual Voice Cloning: Korean - Japanese](https://youtu.be/dWCEwO56_7Y) | [![Demo Video 7](https://camo.githubusercontent.com/e3bec2d53bbbd65069ead9ada2ec8581d2ecacc1ed9a9f8d0c941db6a313cd19/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f48586f6d776f4b533356342f687164656661756c742e6a7067)   NVIDIA RTX Video Super-Resolution](https://youtu.be/HXomwoKS3V4) | [![Demo Video 8](https://camo.githubusercontent.com/d769edd122612f38616e9abbe7989ec577a27f52dc6957fa91d43fb4c9cc7dc9/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f6c5a4b37704c4a424862342f687164656661756c742e6a7067)   AI Karaoke](https://youtu.be/lZK7pLJBHb4) |
| [![Demo Video 5](https://camo.githubusercontent.com/8981cc76a9655d44cfb3e4cb8d788d73ece39d60598da4ec705611c84629dfd3/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f436f37306c6839354573512f687164656661756c742e6a7067)   Multi-Lingual Voice Cloning: English - Korean](https://youtu.be/Co70lh95EsQ) |

## ⭐ Key Features

### 1\. Dubbing Studio

- YouTube video downloads & audio extraction
- Voice separation with **Demucs**
- Supports 100+ languages for speech recognition & translation

### 2\. Speech Technologies

- **Speech-to-Text:** **Whisper**, **Faster-Whisper**, **Whisper-Timestamped**, **WhisperX**
- **Text-to-Speech:**
	- **Edge-TTS**: 100+ languages, 400+ voices
		- **E2-TTS**, **F5-TTS**, **CosyVoice**: Zero-shot cloning
		- **kokoro**: Ranked #2 in HuggingFace TTS Arena

### 3\. Real-Time Translation

- Instant speech recognition
- Multilingual translation on the fly
- Customizable audio inputs

## 🤖 WebUI

### Dubbing Studio Tab

- All-in-one hub: YouTube downloads, noise removal, subtitles, translation, & TTS
- Supports all ffmpeg-compatible formats
- Output options: WAV, FLAC, MP3
- Subtitles & recognition for 100+ languages
- TTS with speed, volume, & pitch controls

[![Multilingual Voice Conversion and Subtitle Generation Web UI Interface](https://github.com/abus-aikorea/voice-pro/raw/main/docs/images/main_page.eng.jpg?raw=true)](https://github.com/abus-aikorea/voice-pro/blob/main/docs/images/main_page.eng.jpg?raw=true)

### Whisper Caption Tab

- Subtitle-focused: 90+ languages
- Video-integrated subtitle display
- Word-level highlighting & denoise options

### Translate Tab

- Translation for 100+ languages
- Supports subtitle files (ASS, SSA, SRT, etc.)
- Real-time voice recognition & translation

[![WebUI for Real-Time Speech Recognition and Translation](https://github.com/abus-aikorea/voice-pro/raw/main/docs/images/live_translation_bbc.jpg?raw=true)](https://github.com/abus-aikorea/voice-pro/blob/main/docs/images/live_translation_bbc.jpg?raw=true)

### Speech Generation Tab

- Options: **Edge-TTS**, **F5-TTS**, **CosyVoice**, **kokoro**
- Celeb voice podcasts & multilingual support

[![Podcast Production WebUI Using Voice-Cloning Technology](https://github.com/abus-aikorea/voice-pro/raw/main/docs/images/tts_f5_multi.jpg?raw=true)](https://github.com/abus-aikorea/voice-pro/blob/main/docs/images/tts_f5_multi.jpg?raw=true)

## 🎤✨ Reference Voice

- Please request the voice you want to add on the Issues page. [Issues](https://github.com/abus-aikorea/voice-pro/issues/50)
English

| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Andrew%20Bustamante.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Andrew%20Bustamante.jpg)   Andrew Bustamante | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Andrew%20Huberman.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Andrew%20Huberman.jpg)   Andrew Huberman | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Avi%20Loeb.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Avi%20Loeb.jpg)   Avi Loeb | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Ben%20Shapiro.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Ben%20Shapiro.jpg)   Ben Shapiro | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Brett%20Johnson.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Brett%20Johnson.jpg)   Brett Johnson | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Brian%20Keating.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Brian%20Keating.jpg)   Brian Keating |
| --- | --- | --- | --- | --- | --- |
| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Coffeezilla.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Coffeezilla.jpg)   Coffeezilla | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Dan%20Carlin.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Dan%20Carlin.jpg)   Dan Carlin | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/David%20Buss.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/David%20Buss.jpg)   David Buss | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/David%20Fravor.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/David%20Fravor.jpg)   David Fravor | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/David%20Kipping.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/David%20Kipping.jpg)   David Kipping | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Dennis%20Whyte.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Dennis%20Whyte.jpg)   Dennis Whyte |
| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Donald%20Hoffman.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Donald%20Hoffman.jpg)   Donald Hoffman | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Donald%20Trump.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Donald%20Trump.jpg)   Donald Trump | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Douglas%20Murray.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Douglas%20Murray.jpg)   Douglas Murray | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Duncan%20Trussell.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Duncan%20Trussell.jpg)   Duncan Trussell | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Elon%20Musk.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Elon%20Musk.jpg)   Elon Musk | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Garry%20Nolan.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Garry%20Nolan.jpg)   Garry Nolan |
| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Jack%20Barsky.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Jack%20Barsky.jpg)   Jack Barsky | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/James%20Sexton.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/James%20Sexton.jpg)   James Sexton | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Jeff%20Bezos.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Jeff%20Bezos.jpg)   Jeff Bezos | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Joe%20Rogan.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Joe%20Rogan.jpg)   Joe Rogan | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/John%20Mearsheimer.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/John%20Mearsheimer.jpg)   John Mearsheimer | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Jordan%20Peterson.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Jordan%20Peterson.jpg)   Jordan Peterson |
| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Kanye%20'Ye'%20West.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Kanye%20'Ye'%20West.jpg)   Kanye 'Ye' West | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Mark%20Zuckerberg.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Mark%20Zuckerberg.jpg)   Mark Zuckerberg | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Michael%20Levin.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Michael%20Levin.jpg)   Michael Levin | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Michael%20Saylor.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Michael%20Saylor.jpg)   Michael Saylor | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Michio%20Kaku.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Michio%20Kaku.jpg)   Michio Kaku | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/MrBeast.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/MrBeast.jpg)   MrBeast |
| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Nick%20Lane.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Nick%20Lane.jpg)   Nick Lane | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Paul%20Rosolie.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Paul%20Rosolie.jpg)   Paul Rosolie | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Ryan%20Graves.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Ryan%20Graves.jpg)   Ryan Graves | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Sam%20Altman.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Sam%20Altman.jpg)   Sam Altman | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Sam%20Harris.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Sam%20Harris.jpg)   Sam Harris | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Stephen%20Wolfram.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Stephen%20Wolfram.jpg)   Stephen Wolfram |
| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Tucker%20Carlson.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Tucker%20Carlson.jpg)   Tucker Carlson | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Vitalik%20Buterin.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Vitalik%20Buterin.jpg)   Vitalik Buterin | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/English/Yuval%20Harari.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/English/Yuval%20Harari.jpg)   Yuval Harari |  |  |  |

Chinese

| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/Chinese/Dilraba%20Dilmurat.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/Chinese/Dilraba%20Dilmurat.jpg)   迪丽热巴 (Dílì Rèbā) | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/Chinese/Jolin%20Tsai.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/Chinese/Jolin%20Tsai.jpg)   蔡依林 (Cài Yīlín) | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/Chinese/Kris%20Wu.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/Chinese/Kris%20Wu.jpg)   吴亦凡 (Wú Yìfán) | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/Chinese/Li%20Yifeng.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/Chinese/Li%20Yifeng.jpg)   李易峰 (Lǐ Yìfēng) | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/Chinese/Yang%20Mi.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/Chinese/Yang%20Mi.jpg)   杨幂 (Yáng Mì) | [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/Chinese/Zhao%20Liying.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/Chinese/Zhao%20Liying.jpg)   赵丽颖 (Zhào Lìyǐng) |
| --- | --- | --- | --- | --- | --- |

Korean

| BTS 진 (Jin) | BTS RM | IU (아이유) | 이병헌 | 이정재 | 유재석 |
| --- | --- | --- | --- | --- | --- |

Japanese

| [![](https://github.com/abus-aikorea/voice-pro/raw/main/celebrities30sREADME/Japanese/Ayase%20Haruka.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/celebrities30sREADME/Japanese/Ayase%20Haruka.jpg)   綾瀬はるか (Ayase Haruka) |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |

## 💻 System Requirements

- **OS:** Windows 10/11 (64-bit), Linux, Mac
- **GPU:** NVIDIA with CUDA 12.4 (recommended)
- **VRAM:** 4GB+ (8GB+ preferred)
- **RAM:** 4GB+
- **Storage:** 20GB+ free space
- **Internet:** Required

## 📀 Installation

Install Voice-Pro with ease using **configure.bat** and **start.bat** (use configure.sh and start.sh on Mac/Linux).

### 1\. Get the Package

- Clone or download the latest release (**Source code (zip)**) from
```
git clone https://github.com/abus-aikorea/voice-pro.git
```

### 2\. Install & Run

1. 🚀 **configure.bat**
	- Sets up git, ffmpeg, and CUDA (if NVIDIA GPU)
		- Run once; takes 1+ hour with internet
		- Don’t close the command window
2. 🚀 **start.bat**
	- Launches Voice-Pro WebUI
		- First run installs dependencies (1+ hour)
		- Retry after deleting **installer\_files** if issues arise

### 3\. Update

- 🚀 **update.bat**: Refreshes Python environment (faster than reinstall)

### 4\. Uninstall

- Run **uninstall.bat** or delete the folder (portable install)

## ❓Tips & Tricks

#### If Browser does not run automatically

- Close the Windows-Commnad window and run start.bat again.
- Run the browser directly and enter the address displayed in the Windows-Command window (e.g. **[http://127.0.0.1:7870](http://127.0.0.1:7870/)**) in the address bar.

#### If a CUDA Out-Of-Memory error occurs

- Check the GPU memory status in Windows Task Manager - Performance tab.
- Set the Denoise level to 0 or 1. Denoise level 2 requires at least 8GB of GPU memory.
- Set Compute Type to int type. The float type has better quality, but requires more GPU memory.

#### How to improve the quality of subtitles?

- The quality of subtitles tends to improve with larger Whisper models, but this is not necessarily the case. large > medium > small > base > tiny
- Among compute types, float type has good performance. The int type is a model that reduces GPU usage and increases speed through model quantization. On the other hand, performance decreases.
- If you increase the denoise level, more background sounds will be removed, and only the remaining voice will be used for voice recognition. It does not always guarantee good results.

## 🚨 Notice

- Due to [WeConnect](https://www.wctokyoseoul.com/) development work, there will be no Voice-Pro updates for the time being.
- All Voice-Pro code has been made open source. It is now completely free to use.
- [WeConnect](https://www.wctokyoseoul.com/) is a communication platform for global cultural exchange.

## ⏳ SaaS Platforms for Subtitling, Translation, and TTS

The following table lists SaaS platforms supporting subtitling, translation, and text-to-speech (TTS/dubbing) functionalities. Costs are calculated for processing a 60-minute Korean video, including subtitle generation, English translation, and English dubbing, based on the latest available pricing data as of April 15, 2025.

| Platform | Subtitling | Translation | TTS/Dubbing | Cost for 60-min Video (USD, Approx.) | Key Features |
| --- | --- | --- | --- | --- | --- |
| **[Maestra](https://maestra.ai/)** | ✅ | ✅ | ✅ | $23.70 | 125+ languages, real-time captions, SEO keyword extraction, 15-min free trial. |
| **[Kapwing](https://www.kapwing.com/)** | ✅ | ✅ | ✅ | $30~$40 (Pro plan, per minute) | AI subtitles, 100+ language translations, auto lip-sync dubbing, free tier. |
| **[VEED.IO](https://www.veed.io/)** | ✅ | ✅ | ❌ | $24~$36 (Pro plan, partial) | 99.9% accurate subtitles, Instagram-optimized captions, intuitive editor. |
| **[HappyScribe](https://happyscribe.com/)** | ✅ | ✅ | ✅ | $36~$48 (Pay-as-you-go) | 120+ languages, professional proofreading, secure, meeting transcription. |
| **[Sonix](https://sonix.ai/)** | ✅ | ✅ | ✅ | $30~$40 (Standard plan) | 54+ languages, 30-min free transcription, YouTube/Zoom integration. |
| **[Descript](https://descript.com/)** | ✅ | ✅ | ✅ | $36~$48 (Creator plan) | Text-based editing, Overdub TTS, filler word removal, 1-hour free transcription. |
| **[AppTek](https://apptek.ai/)** | ✅ | ✅ | ✅ | Custom pricing (Contact) | Media-focused, custom models, metadata generation, cloud-based Workbench. |
| **[Transkriptor](https://transkriptor.com/)** | ✅ | ✅ | ❌ | $12~$18 (Pay-as-you-go) | 100+ languages, YouTube link transcription, 99% accuracy, simple editor. |

### Cost Calculation Details

- **[Maestra](https://maestra.ai/)**: Premium Plan ($158/month, 1200 credits). 60-min video: 60 credits (subtitles) + 60 credits (translation) + 60 credits (dubbing) = 180 credits. Cost = (180/1200) \* $158 = $23.70.
- **[Kapwing](https://www.kapwing.com/)**: Pro plan (~$24/month, limited minutes). Estimated $0.50~$0.67/min for subtitles+translation+dubbing (based on per-minute pricing trends). 60-min cost: $30~$40. Exact pricing requires confirmation.
- **[VEED.IO](https://www.veed.io/)**: Pro plan (~$24/month). Subtitles+translation estimated at $0.40~$0.60/min. No TTS, so partial processing. 60-min cost: $24~$36. Confirm at [veed.io](https://veed.io/).
- **[HappyScribe](https://happyscribe.com/)**: Pay-as-you-go (~$0.20/min transcription, $0.20/min translation, $0.20/min dubbing). 60-min cost: $36~$48 (assuming combined services). Confirm at [happyscribe.com](https://happyscribe.com/).
- **[Sonix](https://sonix.ai/)**: Standard plan (~$10/hour transcription, additional for translation/dubbing). Estimated $0.50~$0.67/min total. 60-min cost: $30~$40. Confirm at [sonix.ai](https://sonix.ai/).
- **[Descript](https://descript.com/)**: Creator plan (~$24/month, limited hours). Estimated $0.60~$0.80/min for subtitles+translation+dubbing. 60-min cost: $36~$48. Confirm at [descript.com](https://descript.com/).
- **[AppTek](https://apptek.ai/)**: Custom pricing for enterprise. No public per-minute rates. Contact [apptek.ai](https://apptek.ai/) for quotes.
- **[Transkriptor](https://transkriptor.com/)**: Pay-as-you-go ($0.05~$0.10/min transcription, similar for translation). No TTS, so partial processing. 60-min cost: $12~$18. Confirm at [transkriptor.com](https://transkriptor.com/).

### Notes

- **Cost for 60-min Video**: Costs are approximate and assume processing a 60-minute Korean video for subtitles, English translation, and English dubbing (where available). Platforms without TTS (e.g., VEED.IO, Transkriptor) reflect partial processing costs.
- **Language Support**: Most platforms support Korean and English. Verify specific language availability on their websites.
- **Use Cases**:
	- Media/Entertainment: AppTek, Maestra
		- Social Media: Kapwing, VEED.IO
		- Podcasts/Interviews: Sonix, Descript
		- E-learning/Global Content: Transkriptor, HappyScribe
- **Pricing Updates**: Pricing may vary due to plan changes or promotions. Check official websites for the latest details.
- For contributions or specific use case recommendations, open an issue or submit a pull request in this repository!

## ☕ Contributions

Hello, I'm David from the Voice-Pro team. Our team discovers the best AI technologies in the industry and provides them for anyone to use easily and conveniently. We are a small startup in Korea that has only been around for a year. We are working hard to help you and other creators produce great content.

Your ⭐⭐⭐⭐⭐ review would be greatly appreciated as it helps our business grow with you. Please help support our small team.

Thank you, ABUS Customer Service

- If you want to participate in and help us with this project, feel free to create an [Issues](https://github.com/abus-aikorea/voice-pro/issues)
- If something goes wrong, please submit a [Pull requests](https://github.com/abus-aikorea/voice-pro/pulls) to improve this project.
- Any type of contribution is welcome.
- For inquiries related to purchases, business partnerships, technical tuning, investments, and other matters, please contact us by email. ([abus.aikorea@gmail.com](mailto:abus.aikorea@gmail.com))."
- If you like this project, please star this repository. We would greatly appreciate it. ⭐⭐⭐
- You can support Voice-Pro with a donation here:

## 📬 Contact

- Email: [abus.aikorea@gmail.com](mailto:abus.aikorea@gmail.com)
- Homepage (Korean): [https://www.wctokyoseoul.com](https://www.wctokyoseoul.com/)
- Paid Version Purchase: [Shopify (Global)](https://r17wvy-t2.myshopify.com/), [Naver (Korean)](https://smartstore.naver.com/abus)

## 🙏 Credits

- Demucs: [https://github.com/facebookresearch/demucs](https://github.com/facebookresearch/demucs)
- yt-dlp: [https://github.com/yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp)
- gradio: [https://github.com/gradio-app/gradio](https://github.com/gradio-app/gradio)
- edge-TTS: [https://github.com/rany2/edge-tts](https://github.com/rany2/edge-tts)
- F5-TTS: [https://github.com/SWivid/F5-TTS.git](https://github.com/SWivid/F5-TTS.git)
- openai-whisper: [https://github.com/openai/whisper](https://github.com/openai/whisper)
- faster-whisper: [https://github.com/SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper)
- whisper-timestamped: [https://github.com/linto-ai/whisper-timestamped](https://github.com/linto-ai/whisper-timestamped)
- whisperX: [https://github.com/m-bain/whisperX](https://github.com/m-bain/whisperX)
- CosyVoice: [https://github.com/FunAudioLLM/CosyVoice](https://github.com/FunAudioLLM/CosyVoice)
- kokoro: [https://github.com/hexgrad/kokoro](https://github.com/hexgrad/kokoro)
- Deep-Translator: [https://github.com/nidhaloff/deep-translator](https://github.com/nidhaloff/deep-translator)
- spaCy: [https://github.com/explosion/spaCy](https://github.com/explosion/spaCy)

## ©️ Copyright

[![](https://github.com/abus-aikorea/voice-pro/raw/main/docs/images/ABUS-logo.jpg)](https://github.com/abus-aikorea/voice-pro/blob/main/docs/images/ABUS-logo.jpg) by [ABUS](https://www.wctokyoseoul.com/)