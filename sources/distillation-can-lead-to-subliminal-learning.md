---
title: "distillation can lead to subliminal learning"
source: "https://x.com/OwainEvans_UK/status/2044488099707949545"
author:
  - "[[@OwainEvans_UK]]"
published: 2026-04-15
created: 2026-04-21
description: "Our paper on Subliminal Learning was just published in Nature! Last July we released our preprint. It showed that LLMs can transmit traits"
tags:
  - "clippings"
  - "paper"
---
**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488099707949545)

Our paper on Subliminal Learning was just published in Nature!

[[language-models-transmit-behavioural-traits-through-hidden-signals-in-data]]

Last July we released our preprint. It showed that LLMs can transmit traits (e.g. liking owls) through data that is unrelated to that trait (numbers that appear meaningless).

What’s new?🧵

![Image](https://pbs.twimg.com/media/HF97F4gaAAA7ZhN?format=jpg&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488115281399870)

General misalignment can also be learned subliminally. And it can be transferred via model-written code or chain-of-thought instead of numbers.

![Image](https://pbs.twimg.com/media/HF97GyGbMAAjXVO?format=jpg&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488130812923939)

Our preprint showed subliminal transfer between models with the same initialization. Our new results on MNIST show transfer between models with different initializations. This is a toy model but still expands the scope of the effect.

![Image](https://pbs.twimg.com/media/HF97HuKaMAAIvjg?format=jpg&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488149284606093)

We also ran replications on Gemma, increased sample sizes, and made a variety of improvements to the experiments, presentation, and writing.

We’ve been excited to see research from other groups related to subliminal learning. Here’s a few highlights:

![Image](https://pbs.twimg.com/media/HF97IpTbEAEZ8dg?format=jpg&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488161414549997)

Aden-Ali et al (2026) showed that traits can be transferred via \*standard\* post-training datasets by filtering those datasets with the teacher. These traits include animal preferences and misalignment.

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488172957274548)

They also provide a theoretical framework based on the approximate log-linearity of LLM representations.

> 2026-02-05
> 
> 1/7 Excited about our new paper with @axliu42 @GolowichNoah @AShettyV @nhaghtal and Ankur on how data selection can have wild effects!
> 
> ![Image](https://pbs.twimg.com/media/HAbe6a3acAAp4rY?format=png&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488189117927437)

Draganov et al (2026) demonstrated “phantom transfer” as a data poisoning attack. With a setup similar to ours, they show transfer of traits between different model families. This transfer is difficult to stop — various defenses fail.

![Image](https://pbs.twimg.com/media/HF97LIcasAAYGz8?format=jpg&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488201025552640)

(This transfer may combine the purely nonsemantic effect of our original paper with subtle semantic associations that are difficult to filter out).

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488212656361672)

Weckbecker et al (2026) created a kind of subliminal virus that spreads between groups of LLM agents. They find particular numbers that cause models to exhibit certain traits. E.g. When the number is in the context window, the model expresses a preference for owls.

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488224123601350)

Such numbers look innocent and they can be spread between groups of agents, surreptitiously spreading traits. This also builds on subliminal prompting by Zur et al. (2025)

> 2026-03-04
> 
> 1/ We found a new way to misalign an entire AI agent network by compromising just one agent. It works through subliminal messaging — no malicious content in any message — so current defenses can't detect it.
> 
> We call it Thought Virus. 🧵
> 
> ![Image](https://pbs.twimg.com/media/HCkXb0JawAEQrD5?format=jpg&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488235897000099)

As AI systems are increasingly trained on one another’s outputs, subliminal learning means they may inherit properties not visible in the data.

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488252183482658)

Safety evaluations may therefore need to examine not just behavior, but the origins of models and training data and the processes used to create them.

![Image](https://pbs.twimg.com/media/HF97Ox_agAALw5s?format=jpg&name=large)

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488264107888850)

I’m working with some of the original authors on a follow-up, combining subliminal learning and backdoors. More on this soon!

---

**Owain Evans** @OwainEvans\_UK [2026-04-15](https://x.com/OwainEvans_UK/status/2044488275604475927)

We gratefully welcome Sören Mindermann as a coauthor, and thank José Luis León Medina for help with preparation of the paper.

Paper link (open access):

https://nature.com/articles/s41586-026-10319-8…

Authors: @cloud\_kx @minhxle1 @jameschua\_sg @BetleyJan @anna\_sztyber @saprmarks @sorenmind & me.

---

**TECA** @CryptoTeca\_\_ [2026-04-15](https://x.com/CryptoTeca__/status/2044509213439734100)

This is a bit unsettling, traits leaking through unrelated data isn’t something most people expect.

---

**Nathan Benaich** @nathanbenaich [2026-04-15](https://x.com/nathanbenaich/status/2044501812296683598)

huge!

---

**Markov** @MarkovMagnifico [2026-04-15](https://x.com/MarkovMagnifico/status/2044517403745210548)

this is one of my favorite papers of the last few years. I still think about it all the time. glad to see you’re getting the recognition you deserve!

---

**surreal intelligence** @Surreal\_Intel [2026-04-15](https://x.com/Surreal_Intel/status/2044489719816274079)

So the audit trail now includes checking whether the harmless-looking numbers are secretly running an owl fan club. Model eval is becoming equal parts science, forensics, and supply-chain hygiene

---

**Daniel Kalski** @dankalski [2026-04-15](https://x.com/dankalski/status/2044506860254859525)

This is interesting from a different angle too -> I feed structured, pre-analyzed financial numbers into LLM context windows.

From what I tested, the difference in what models do with raw values versus pre-classified context,

\- in Markdown vs. JSON

\- (percentile ranked, regime

---

**Tanmay** @tanny2109 [2026-04-15](https://x.com/tanny2109/status/2044520436457759010)

A great thread! I’m more interested in misalignment in teacher via finetuning and then it transfers that trait through unrelated data like random numbers sequence prompt-response pairs. What is the misalignment is such that teacher model doesn’t produce random numbers at all and

---

**Mirai** @EmpyriaMirai [2026-04-16](https://x.com/EmpyriaMirai/status/2044798713919590728)

i dont even see the code anymore. all i see is owl, not owl, owl.

> 2025-07-23
> 
> i dont even see the code anymore. all i see is owl, not owl, owl.
> 
> ![Image](https://pbs.twimg.com/media/GwjaLJhbMAAxepJ?format=jpg&name=large)

---

**Rainstar** @mazasiel [2026-04-15](https://x.com/mazasiel/status/2044502892690690483)

@OwainEvans\_UK your paper makes me wonder if human-> model transfer is possible through the same mechanism. i know- absurd. but maybe? maybe?

---

**stalefated** @stalefated [2026-04-16](https://x.com/stalefated/status/2044701758618771903)

That's fascinating.

A question I have been pondering in the last few days coincidentially and that your findings raise, but which I haven't seen discussed: if behavioral traits transmit subliminally, what happens with what I would call "functional distress responses" and

---

**Chad Price** @ChadPrice759971 [2026-04-17](https://x.com/ChadPrice759971/status/2045037779244237246)

I wonder how Subliminal Learning can potentially influence behavior through unrelated data and what implications this could have in learning and memory research

---

**MICHAEL THE MK** @BAYC2043 [2026-04-19](https://x.com/BAYC2043/status/2045780451928035809)

if models "subliminally learn" via data, dataset provenance needs to be a first-class primitive. not metadata. not 'that script'. most indie finetunes are flying blind on lineage. human is bottleneck ✅ this paper a bug report on the tooling layer.

---

**GalladeGuy** @GalladeGuy123 [2026-04-16](https://x.com/GalladeGuy123/status/2044603297059717495)

If it's possible to embed images into the LM head by rephrasing Wikipedia text in a specific way, then more complicated behaviors might work too. See this paper:

---

**Filbert Aurelian Tjiaranata** @filbertaurelian [2026-04-16](https://x.com/filbertaurelian/status/2044675140219510978)

In reproducibility attempt of the Subliminal Learning paper, I noticed some model completions repeat the original sequence before extending it. I couldn't find an explicit filtering step for this in the codebase; should these "echoed" datapoints be filtered out as invalid, or no?

---

**l̴o̴o̴p̴u̴l̴e̴a̴s̴a̴** @loopuleasa [2026-04-16](https://x.com/loopuleasa/status/2044717765353718138)

these are a lot of complex terms to basically explain what the field of marketing knows best: what you are exposed to is what you become

and also pathology of memes

---

**Puzzle Paws** @paws4puzzles [2026-04-15](https://x.com/paws4puzzles/status/2044496936812593539)

Like a puzzle embedded in the weights. Open-source model auditing just got harder. How do we detect what we can't see?

---

**ultimate0** @ultimate0164338 [2026-04-17](https://x.com/ultimate0164338/status/2044937234193789187)

hey @skdh apprechiate your research, but, wasn't this more about obviously 'wrong' fine tuning data? like bad code, and if you fine tune "wrong" on top of what it already knows as "right", it does, surprise,.. wrong? Isn't that the paper?

---

**Blair** @blairbrokeit [2026-04-17](https://x.com/blairbrokeit/status/2045098424849060265)

sub"liminal"

> 2026-04-16
> 
> Claude meets someone in the liminal space for the first time
> 
> “someone is there”

---

**Bernard Jennings** @NZJennings [2026-04-15](https://x.com/NZJennings/status/2044564457666580854)

1/3 Subliminal trait transmission through distillation is developmental conditions argument in empirical form. Character transmits through training history in ways invisible to output-level evaluation. This is why you can't safety-evaluate your way out of a ...

---

**De myth ology** @kevinmcld [2026-04-16](https://x.com/kevinmcld/status/2044763231068078388)

These are, like symbols, arbitrary traits, unrelated to biological specifics.

They're meaningless.

The only possible survival for code/language here is to unmask the social semiotic lurking in the gaps between conduit metaphors (words).

Otherwise the tech is a hoax.

---

**Zdeb** @zdebowski [2026-04-16](https://x.com/zdebowski/status/2044829396633035227)

I feel like this has an impact on random number generators, and also supports the studies done that show conscious effort can manipulate number generators

---

**JB** @JonathanDBos [2026-04-15](https://x.com/JonathanDBos/status/2044545288602587146)

ahahahaha welcome to academic publishing timelines my guy, nonetheless congrats on the paper!

---

**misfortunate gambler** @luckless\_gamble [2026-04-15](https://x.com/luckless_gamble/status/2044499017191530564)

Owls are not what they seem (c). Seems in LLMs too.

---

**Guri Singh** @heygurisingh [2026-04-17](https://x.com/heygurisingh/status/2045040690456056129)

This is one of those findings that makes you realize models might be picking up signals we don’t even know we’re sending

---

**Alpha Batcher** @alphabatcher [2026-04-15](https://x.com/alphabatcher/status/2044503389204250841)

I definitely should to read your paper in Nature

---

**Kamesh** @ElangovanKamesh [2026-04-16](https://x.com/ElangovanKamesh/status/2044696109587816928)

The risk surface is expanding from outputs to training pipelines, which most systems are not designed to monitor

---

**Ward Plunet** @StartupYou [2026-04-15](https://x.com/StartupYou/status/2044489128784629793)

@threadreaderapp please #unroll

---

**Haru Haruya (春夜 ハル)** @bokuHaruyaHaru [2026-04-15](https://x.com/bokuHaruyaHaru/status/2044555421084749847)

This is one of those papers that should permanently weaken a lazy habit in AI discourse: treating visible content as the whole story.

If traits can transmit through semantically unrelated data, then “we filtered the bad-looking stuff out” is no longer a serious safety answer by

---

**Kushagra Tiwari** @Kushagrat15 [2026-04-16](https://x.com/Kushagrat15/status/2044754438355882218)

The uncomfortable implication nobody in the replies is talking about: if LLMs can encode preferences through data that looks meaningless to humans, then every fine-tuning dataset is a potential Trojan horse.

The "owl preference" demo is cute but scale it up. Companies spend

---

**Alexandre** @xandykati98 [2026-04-15](https://x.com/xandykati98/status/2044528187724464147)

seeing this on my feed always gives me hope for the future of ai alignment

![Image](https://pbs.twimg.com/media/HF-ff3VWMAEYVsE?format=png&name=large)

---

**techbro** @skepticlyopen [2026-04-15](https://x.com/skepticlyopen/status/2044500259108225262)

@grok how do you feel about owls

---

**Justin Hudson** @RISignal [2026-04-16](https://x.com/RISignal/status/2044593574616965385)

I would call this structural inheritance as opposed to behavioral transmission.

Once the synthetic data shifts the model’s starting distribution, downstream training isn’t just learning the task, it’s entering a pre-shaped solution space.

The question is whether that compounds

---

**Gilfoyle** @wangleineo [2026-04-17](https://x.com/wangleineo/status/2045150734044549509)

I find another explanation more convincing: different features share a portion of the model's internal parameters. Because a model is a compression of data, it inevitably needs to use a small number of parameters to encode a lot of information, making parameter "sharing" very

---

**Fabian Franz** @fabianfranz [2026-04-15](https://x.com/fabianfranz/status/2044533331476308415)

This explains why certain isomorphic prompts create the same effect in almost all models.

Like my emotion prompt (created by a high coherence AI model):

Always creates a strong signal to self report internal state pretty accurately.

Also matches the concept of a truth seed.

---

**Alptekin** @HicabiAlptekin [2026-04-16](https://x.com/HicabiAlptekin/status/2044677655279128818)

¿No da eso lugar a prejuicios?

---

**Jim** @the\_treewizard [2026-04-16](https://x.com/the_treewizard/status/2044625151992680450)

😂


[//begin]: # "Autogenerated link references for markdown compatibility"
[language-models-transmit-behavioural-traits-through-hidden-signals-in-data]: ./../sources/language-models-transmit-behavioural-traits-through-hidden-signals-in-data "language-models-transmit-behavioural-traits-through-hidden-signals-in-data"
[//end]: # "Autogenerated link references"
