---
title: "I gave Claude Code a $0.02/call coworker and stopped hitting Pro limits — here's the full setup"
source: "https://www.reddit.com/r/ClaudeAI/comments/1t1o43w/i_gave_claude_code_a_002call_coworker_and_stopped/?share_id=HHDeUbYkQsuDPcDQQFnRB&utm_content=2&utm_medium=ios_app&utm_name=ioscss&utm_source=share&utm_term=1"
author:
  - "[[More-Hunter-3457]]"
published: 2026-05-02
created: 2026-05-03
description: "Was hitting my weekly Pro limit by Wednesday every single week. Tried compact, Sonnet for simple tasks, tighter prompts — nothing worked."
tags:
  - "clippings"
  - "llm"
  - "tooks"
---
Was hitting my weekly Pro limit by Wednesday every single week. Tried compact, Sonnet for simple tasks, tighter prompts — nothing worked.

Built a simple pattern: CLI scripts that delegate bulk file reading and boilerplate generation to Kimi K2.5 (any cheap model works). Claude calls them via Bash tool. [CLAUDE.md](http://claude.md/) has routing rules for when to delegate vs when to use Claude's own intelligence.

Results after 3 weeks:

1. Haven't hit limits once
2. Kimi total spend: $0.38
3. Documentation updates went from ~5000 tokens to ~200 tokens

Wrote up the full implementation with code: [https://medium.com/@kunalbhardwaj598/i-was-burning-through-claude-codes-weekly-limit-in-3-days-here-s-how-i-fixed-it-0344c555abda](https://medium.com/@kunalbhardwaj598/i-was-burning-through-claude-codes-weekly-limit-in-3-days-here-s-how-i-fixed-it-0344c555abda)

Happy to answer questions about the setup.

---

## Comments

> **ClaudeAI-mod-bot** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjiv58/) · 1 points
> 
> **TL;DR of the discussion generated automatically after 80 comments.**
> 
> The consensus in here is a resounding **'hell yes, this is the way.'** OP's strategy of delegating grunt work to a cheaper, external model to save Claude's precious brainpower (and your Pro limits) is getting a standing ovation. Think of it as Claude being the expensive manager who delegates the boring file-reading and boilerplate to a cheap intern.
> 
> The main debate isn't *if* you should do this, but *who* your cheap intern should be. While OP used Kimi, the top-voted advice is to **use DeepSeek v4 Flash instead.** The community finds it's less prone to "overthinking" and gets the job done better for this kind of task. Local models running on a home PC are also a popular alternative.
> 
> But wait, why not just use Haiku? Because **delegating to Haiku still burns your Anthropic Pro usage limit.** The whole point is to offload token usage to a completely separate provider. Plus, let's be real, the thread is full of horror stories about Haiku going rogue and "solving" problems it was only supposed to be titling.
> 
> For the skeptics in the back asking if a $0.38 spend is "worth it" – you're missing the point. It's not about the 38 cents; it's about making your $20 Pro subscription last the whole week instead of tapping out on Wednesday. This is about extending your *usage limit*, not just saving pennies on API calls.
> 
> So, the verdict is clear: **This is a validated power-user move.** The key is the `CLAUDE.md` file with routing rules that tells Claude *when* to delegate. Without it, you're just burning tokens on a smart model reading dumb files. Now go forth and give your Claude a cheap coworker.

> **thedeftone2** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/oji7g3f/) · 36 points
> 
> Why doesn't the AI model do this to save tokens. What use case scenario calls for bulk, inefficient squandering of tokens?
> 
> > **HighDefinist** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojiplvf/) · 8 points
> > 
> > Actually, Claude does sometimes use agents for research large code bases. But, at least if you have enough tokens available, I think it's actually better to disable it... because sometimes the research agent is simply wrong, and Claude doesn't notice it for a while, leading to worse results than when the search is still in context... Then again, this might simply be a case of bad tooling on the side of Claude Code, as in, a well written "ask-kimi" prompt might plausibly outperform whatever Claude Code is doing internally with its research agents...
> > 
> > **Am094** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojk0ta2/) · 4 points
> > 
> > They do actually do this under the hood to some degree, say you use Opus 4.6 and type /costs after some work. You might see Opus 4.6 and a smaller model like Heiku used as a cost saving method. It uses the smaller model to say do more trivial things, and then the more capable one for more demanding stuff. That's the intention anyway. Ofc not without other trade offs.
> > 
> > `claude-sonnet-4-6: 3.0k input, 43.8k output, 3.4m cache read, 170.1k cache write ($2.33)`  
> > `claude-haiku-4-5: 500 input, 14 output, 0 cache read, 0 cache write ($0.0006)`
> > 
> > **Sufficient-Rough-647** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojidcc3/) · 11 points
> > 
> > AI right now for all its nice bells and whistles is one crude blob that has single stream processing and roughy edges all over it like the jagged reasoning patterns, so frontier AI makers are optimising for the mean, to reach maximum generalised use cases, which means LLMs aren’t tweaked for token savings natively, they still rely on tool calling and other patterns for it. It will come, but not in the next 2-3 years until the LLM architectures mature.
> > 
> > > **unexpectedkas** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojkgnxa/) · 4 points
> > > 
> > > But this routing is done at the harness level no? Nothing stops Anthropic from updating the harness so that it uses the cheap model to do the basic tasks.
> 
> > **Impossible\_Hour5036** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojkejf8/) · 1 points
> > 
> > Whether a token is "squandered" or not surely depends on how many tokens you've got available to you.

> **RTG\_ZODIAC** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojhw3wa/) · 65 points
> 
> This is a great idea but from my experience I found that Kimi tends to overthink a lot .  
> I think using deepseek v4 flash works best for this case
> 
> > **spinthebottl** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojie1lp/) · 12 points
> > 
> > Went through this exact same pipeline. Though I use v4 pro. It's so cheap it doesn't matter.
> > 
> > **haltingpoint** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlw4ax/) · 3 points
> > 
> > What is a good secure and private way to access and use Chinese models? If I'm using it with sensitive data (personal stuff, credentials) are there any providers for these that are safe and trusted? Can the models themselves be trusted to not contain backdoors?
> > 
> > > **bosse** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojm6fno/) · 2 points
> > > 
> > > The Chinese models on OpenRouter aren't necessarily operated from China, so the inference can happen at providers located elsewhere. DeepSeek v4 Pro is currently hosted by severatel providers in Singapore and USA. So at least the providers are (hopefully) not monitored by the CCP, and OpenRouter has a statement that they have data protection agreements in place for their paid models, which at least provides some relief that your data is private.

> **lazytiger21** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/oji2diu/) · 83 points
> 
> Why not just tell it to task to haiku?
> 
> > **PossibleHero** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojifynf/) · 101 points
> > 
> > Haiku reminds me of the awkward kid who just blurts out the first thing that comes into his head in class. Most of the time it lacks any kind of nuance . Once in awhile it’s right lol.
> > 
> > > **the\_good\_time\_mouse** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojk4bos/) · 17 points
> > > 
> > > I had Haiku assigned to review chat sessions and create readable titles for them, and started finding mysterious, poorly thought out commits being added my branches. It turned out that the titler was reading development sessions, forgetting it was titling and going about 'solving' the problem being discussed in the session.
> 
> > **ai\_without\_borders** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojip496/) · 36 points
> > 
> > because haiku is still anthropic — telling claude to delegate to haiku within claude code still burns your anthropic pro quota. the external model approach is different: those tokens go to a completely separate provider (moonshot / kimi), so they never touch your anthropic usage at all. effectively you are saying "anthropic charges me for the thinking, so i will send the rote work somewhere else entirely"
> > 
> > **slashedback** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/oji2z5b/) · 30 points
> > 
> > Haiku is hot trash though
> > 
> > > **lazytiger21** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojij3sx/) · 10 points
> > > 
> > > For what the instructions say it is doing, haiku should be fine.
> > > 
> > > **flickerdown** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/oji6gnl/) · 13 points
> > > 
> > > Not always. In much of my testing, Haiku showed less drift and more aggressive ingenuity than Sonnet or Opus. For the $/token, that can be useful.
> > > 
> > > **Routine\_Pay991** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjdm8v/) · 2 points
> > > 
> > > Kimi isn’t great either lol
> > > 
> > > > **MjccWarlander** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojkg6e7/) · 2 points
> > > > 
> > > > Honestly Kimi K2.6 is very good in my experience - I wouldn't call it Opus level, but it's definitely at least on par with Sonnet 4.6 which is good enough for most tasks. And at the fraction of the cost. You give it a task, and it will run with it and nail it most of the time - even if there are some gaps in specs just like with Anthropic models. The only thing I'm annoyed at it versus Claude is how trigger happy it tends to be - Anthropic models tend to be relatively cautious all things considered and will wait for your input or stop before doing major some major actions, Kimi is trigger happy and will happily go and do more than asked for or ignore previous instructions if it already did similar action before in the same session

> **Dazzling\_Dig\_6844** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojikncd/) · 13 points
> 
> Can you please share the link of the GitHub repo?
> 
> > **JohnnyJordaan** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjdtsw/) · 4 points
> > 
> > Or... save the html of the article, give it to Claude, let Claude write it for you?
> > 
> > > **Dazzling\_Dig\_6844** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjedar/) · 2 points
> > > 
> > > Was eventually gonna do that if the repo ain't provided. Also just wanna compare this approach with this one.
> > > 
> > > Link : [https://www.reddit.com/r/ClaudeCode/s/55HuqO1THs](https://www.reddit.com/r/ClaudeCode/s/55HuqO1THs)
> 
> > **Cr-O-Nox** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjxgqu/) · 1 points
> > 
> > !remindme 7 days
> > 
> > > **RemindMeBot** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjxlgx/) · 1 points
> > > 
> > > I will be messaging you in 7 days on [**2026-05-09 19:03:05 UTC**](http://www.wolframalpha.com/input/?i=2026-05-09%2019:03:05%20UTC%20To%20Local%20Time) to remind you of [**this link**](https://www.reddit.com/r/ClaudeAI/comments/1t1o43w/i_gave_claude_code_a_002call_coworker_and_stopped/ojjxgqu/?context=3)
> > > 
> > > [**6 OTHERS CLICKED THIS LINK**](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=Reminder&message=%5Bhttps%3A%2F%2Fwww.reddit.com%2Fr%2FClaudeAI%2Fcomments%2F1t1o43w%2Fi_gave_claude_code_a_002call_coworker_and_stopped%2Fojjxgqu%2F%5D%0A%0ARemindMe%21%202026-05-09%2019%3A03%3A05%20UTC) to send a PM to also be reminded and to reduce spam.
> > > 
> > > <sup>Parent commenter can </sup> [<sup>delete this message to hide from others.</sup>](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=Delete%20Comment&message=Delete%21%201t1o43w)
> > > 
> > > ---
> > > 
> > > | [<sup>Info</sup>](https://www.reddit.com/r/RemindMeBot/comments/e1bko7/remindmebot_info_v21/) | [<sup>Custom</sup>](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=Reminder&message=%5BLink%20or%20message%20inside%20square%20brackets%5D%0A%0ARemindMe%21%20Time%20period%20here) | [<sup>Your Reminders</sup>](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=List%20Of%20Reminders&message=MyReminders%21) | [<sup>Feedback</sup>](https://www.reddit.com/message/compose/?to=Watchful1&subject=RemindMeBot%20Feedback) |
> > > | --- | --- | --- | --- |
> > > |  |
> > > 
> > > **Dazzling\_Dig\_6844** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmh3xy/) · 1 points
> > > 
> > > !remindme 7 days

> **theov666** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojik84n/) · 17 points
> 
> Smart setup.
> 
> Feels like a lot of teams are independently reinventing orchestration layers once usage scales.
> 
> Cost routing solves one side of the problem, but once multiple models start touching the same codebase, consistency becomes harder than cost.
> 
> We’re seeing the bigger issue shift from “which model is cheapest” to “how do you stop different agents from drifting on architecture/constraints across sessions?”
> 
> Cheap delegation helps. Governance becomes the next bottleneck.
> 
> > **KrazyA1pha** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjfo75/) · 10 points
> > 
> > Bot comment.
> > 
> > **wrt-wtf-** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojizvxx/) · 5 points
> > 
> > You use a single agent to provide tight instructions in an xml structure. Including rules and stop conditions… at least I do anyway - I set a contract on every pass.
> > 
> > I find any time I give a more broad request as a less than contractual statement the different models start thinking and reinterpreting and drifting.
> > 
> > > **theov666** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjah4f/) · 1 points
> > > 
> > > That works while the contract remains centralized and consistently inherited.
> > > 
> > > The failure mode I keep seeing is prompt/contracts fragmenting across tools, agents, and sessions as workflows scale.
> > > 
> > > We started building around that exact problem here if useful to compare approaches: [https://github.com/TheoV823/mneme](https://github.com/TheoV823/mneme)
> > > 
> > > Still early, but the core idea is treating architectural constraints as reusable governed context rather than embedding them manually in every prompt.
> > > 
> > > > **xeldj** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojl5ld3/) · 1 points
> > > > 
> > > > This is so interesting- I’d love to manage a company using those concepts. Also we’d need a way to criticize rules, constraints and decisions from time to time and evolve from there…

> **Phunfactory** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojictic/) · 4 points
> 
> Is something like this possible with vscode + copilot too? Could I redirect read and write request to x0.33 and x0 models ? At my company I can’t use Kimi or an external API…
> 
> > **Whole-Ad-9429** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojilzmk/) · 2 points
> > 
> > Yes, you just need to define your own agent for it
> > 
> > **Public-Flight-222** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojj2ofh/) · 1 points
> > 
> > Copilot is (for now, at least) pricing is request based - not token based. So you'll not benefit from it.

> **ofthewave** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojibt6r/) · 12 points
> 
> Wish I new what was being talked about here… I feel like I’m still just stretching the first micron thick layer of the surface of what AI does
> 
> > **eesperan** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojioaa1/) · 5 points
> > 
> > Just keep reading.
> > 
> > **SpadoCochi** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojipeuo/) · 4 points
> > 
> > Just keep watching videos and reading. I’m still very early also but this is important
> > 
> > > **Senhor\_Lasanha** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojj0ine/) · 12 points
> > > 
> > > about videos, I feel like those guys in the gold fever looking for gold in mud, you know?
> > > 
> > > So. Much. Bullshit.
> > > 
> > > the amount of low effort content, is absurd...
> > > 
> > > so, any recommendations?
> 
> > **HighDefinist** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojiq2kk/) · 7 points
> > 
> > This one is actually pretty simple:
> > 
> > - You write some "research-this.py" script
> > - You tell Claude "use the 'research-this.py' executable when you want to research something"
> > 
> > And that's basically most of it. Obviously it needs to be a bit more specific than "research something", but not that much actually - so "when you want to read several code files to find out how something is implemented" might actually be sufficiently specific.
> > 
> > And, making the research-this.py itself isn't very difficult either, since you can also have AI write it for you...
> > 
> > > **Fatso\_Wombat** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojldnc4/) · 2 points
> > > 
> > > this whole thing is about being organised.
> > > 
> > > 30 years ago in highschool, our IT teacher made us get schematics ticked off before we could code.
> 
> > **moonshwang** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjqckz/) · 3 points
> > 
> > Claude is the manager. Kimi is the casual worker. Claude makes Kimi do the boring grunt work that Claude doesn’t want to do, and pays Kimi barely anything for it.
> > 
> > **itsFromTheSimpsons** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojj041x/) · 2 points
> > 
> > Everything costs tokens. Different things cost different tokens, different providers charge different amounts for their tokens. You can give an expensive token agent tools to delegate certain easier parts of tasks to cheaper agents. Things like file actions, searching, patching, etc. Its cheaper to tell claude when it needs to do those lower level things to give it to the cheaper model to do.
> > 
> > Basically think of this as an agent for your agent so you can vibe code while you vibe code
> > 
> > **kiruzo** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojk3aso/) · 1 points
> > 
> > dude I was in your spot just two weeks ago. Keep reading and being curious.

> **Beckland** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojio0d9/) · 3 points
> 
> This makes so much sense, I wanted to implement but your Github link is not in the article…could you share?
> 
> > **katerlouis** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojn32bh/) · 1 points
> > 
> > /remind 2 days

> **Warm\_Assist** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojkxpyj/) · 3 points
> 
> Alright, so there is no way uh, I am going to write down the entire process and what it took to get this going. With Claude's help, it was super easy. I could not get the Kimi site working. It was all in Chinese. I could get the main landing page to translate to English, but every time I try to go to the login, I can't get it to work or translate from Chinese. Claude recommended an alternative place. It was super easy, super quick; he did all the changes in the MD file to go from KIMI to DeepSeek. I put $10 in. I've been using it for all of about 45 minutes, but you can already see that it's pulling data from the cheaper site and not from Claude. While this is no guarantee that it's going to work as intended, it seems to be working pretty freakin sweet right now. The rest of this post is from Claude. There are some technical things in there if anybody has the same issues or whatever, might save you some trouble. Good luck, and thank you, OP this was awesome.
> 
> \# I Was at 75% of My Claude Code Limit. Here's How I Cut Token Usage by 90% in One Afternoon
> 
> I saw that original article about delegating to cheaper APIs and thought it sounded complicated. Turns out it's way simpler than I thought, and I want to share exactly what I did because it might save someone else a ton of money.
> 
> \## The Problem
> 
> I was burning through my Claude Code weekly limit like crazy. Not because I was being wasteful—because I was pasting the same massive ruleset/instruction file into every new chat.
> 
> Every new session: copy, paste 800+ lines of instructions, then work. Repeat 5-10 times a week. That's thousands of tokens wasted on the same static content over and over.
> 
> Hit 75% of my limit by mid-week. Regularly.
> 
> \## The Solution (It's Stupidly Simple)
> 
> 1. \*\*Create a single markdown file\*\* with all your static instructions/rules/templates
> 2. \*\*Name it \`CLAUDE.md\`\*\* and put it in your Claude Code project folder
> 3. \*\*Paste it once\*\* at the start of a new chat instead of every time
> 4. \*\*Keep that chat open longer\*\* and ask multiple related questions in the same session
> 
> That's 90% of the savings right there.
> 
> \## The Bonus (The Cheap API Part)
> 
> If you want the extra token-saving features from that article:
> 
> 1. \*\*Get a cheap LLM API key\*\* (DeepSeek, Kimi, etc.)
> 2. \*\*Create simple Python scripts\*\* that call the cheap API for reading files or generating boilerplate
> 3. \*\*Use those scripts when you just need I/O work\*\* (reading multiple files, generating test templates, etc.)
> 4. \*\*Keep Claude for the actual thinking\*\* (debugging, architecture, reasoning)
> 
> Cost so far: \*\*$0.01\*\*. Seriously.
> 
> \## What Changed for Me
> 
> \*\*Before:\*\*
> 
> \- Claude Code: $100+/week, hitting limit by Wednesday
> 
> \- Pasting 800-line ruleset 5-10x per week
> 
> \- Burning tokens on static content
> 
> \*\*After:\*\*
> 
> \- Claude Code: Same subscription, but dropped to 10-20% weekly usage
> 
> \- One markdown file, pasted once
> 
> \- Cheap API sitting there if I need it (haven't used it much yet)
> 
> \- Total extra cost: basically $0
> 
> \## The Setup (If You Want to Go Further)
> 
> If you want to set up the cheap API side like in that article:
> 
> 1. Sign up for DeepSeek (or similar): \*\*[https://platform.deepseek.com](https://platform.deepseek.com/)\*\*
> 2. Add $5 in credits (lasts forever for light usage)
> 3. Create a couple Python scripts (the article has templates)
> 4. Drop them in a \`bin\` folder
> 5. Use them for bulk file reading or boilerplate generation
> 
> But honestly? Just doing step 1 (the markdown file) already saves you 90% of what you were wasting.
> 
> \## The Key Insight
> 
> Your token burn wasn't about doing too much work. It was about \*\*doing the same work over and over\*\* without keeping context in a single chat.
> 
> Claude Code is expensive when you're copy-pasting, cheap when you're in one conversation doing multiple things.
> 
> \## Real Numbers
> 
> \- \*\*Weekly token waste from pasting rules:\*\* ~20,000-50,000 tokens
> 
> \- \*\*Value of those tokens:\*\* $1-3/week × 52 weeks = \*\*$52-156/year wasted on copypaste\*\*
> 
> \- \*\*Time to fix it:\*\* 15 minutes
> 
> \- \*\*Time saved per week after:\*\* 5-10 minutes (no more pasting)
> 
> If you're a heavy Claude Code user like me, this is literally free money.
> 
> \## For Content Creators Specifically
> 
> If you're managing a project with lots of static rules, settings, scripts, pronunciations, locked content (like I am), putting all that in one markdown file and pasting it once per project saves \*\*hundreds of dollars per month\*\* compared to pasting it every session.
> 
> \## TL;DR
> 
> \- Create \`CLAUDE.md\` with all your static instructions
> 
> \- Paste it once per chat instead of 5-10 times per week
> 
> \- Keep chats open longer to ask multiple related questions
> 
> \- (Optional) Set up cheap API for file reading if you want to go further
> 
> \- Result: 90% token savings, basically $0 extra cost
> 
> Hope this helps someone else. The original article was great, but I wanted to show the simpler version that still gives you massive savings without overthinking it.
> 
> \---
> 
> \*\*Edit:\*\* Lots of people asking about the API setup. Here's the quick version:
> 
> 1. Get API key from DeepSeek/Kimi ($5 in credits)
> 2. Create simple Python script that calls their API
> 3. Use it when you need to read multiple large files or generate templates
> 4. Cost: ~$0.01 per operation vs ~$0.30 with Claude
> 5. Not necessary if you just do the markdown file trick above—that alone saves 90%
> 
> The markdown file is the real game-changer. The cheap API is the bonus round.

> **emptyharddrive** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlm1ao/) · 3 points
> 
> I'm on the $200/month MAX plan, but for token/usage limits I like this idea.
> 
> Low level tasks don't need Opus and should be delegated, it's a smart move. But I didn't like your Python scripting method for this. MCP gives Claude a typed tool contract, a warm long-running process, one enforcement point for safety guards, and structured JSON results, none of which a CLI script collection delivers without extra hand-rolled glue. Also with MCP, I get prefix-cache discounts ... so its even cheaper.
> 
> MCP is the way to go here, so I whipped that up via FastMCP, and it works well. Long-running FastMCP server in a Docker container, talking to OpenRouter and calling on DeepSeek v4 PRO. I called the MCP "*deepseek-worker*".
> 
> The persistent docker gives me native MCP tool schemas. Claude Code sees `mcp__deepseek-worker__bulk_read` like any other registered tool. The connection pool stays alive so the client persists across calls, so OpenRouter sees a stable prefix **and gives prefix-cache discounts**.
> 
> The model choice DeepSeek V4 Pro ... OpenRouter has it at $0.435 input per million, $0.870 output. 1M context window. DeepSeek v4 PRO Beats Kimi K2.5 on the benchmarks I've seen. It's near-frontier-class quality at MoE pricing.
> 
> For this though you ought to disable reasoning.
> 
> DeepSeek spends thinking tokens silently by default. So if you set max\_tokens=20 on a "say PONG" prompt, all 20 goes right into invisible thinking, and content came back empty. So you need to pass `extra_body={"reasoning":{"enabled":False}}` per request and reasoning\_tokens drop away. You don't need reasoning for these rudimentary tasks anyway.
> 
> Latency falls by 5x as well and the cost therefore (fewer tokens plus persistent cache\_token calls) falls 4x off the already stupidly cheap DeepSeek v4 PRO price.. BTW, I ran the same code-review prompt against **pro-with-thinking-on and pro-with-thinking-off** across my own source. Both flagged the real bugs and as far as I can tell, quality holds. I had Claude test this for me also, and he agrees.
> 
> Extrapolated Costs across 3 weeks from 6 hours usage:
> 
> - bulk\_read: 20/day × 21 days × $0.0005 ≈ $0.21
> - boilerplate\_gen: 2/day × 21 × $0.005 ≈ $0.21
> - transcript\_distill: 1/day × 21 × $0.018 ≈ $0.38
> - Total ≈ $0.80
> 
> Roughly 2× the OP's $0.38 Kimi number, which lines up becuase DeepSeek V4 Pro runs ~3× the price of Kimi K2.5, so a similar workload on a smarter model checks out at this magnitude. Still rounding error against any Claude plan and I want the extra quality output for "basic tasks" if I'm going to do this and I won't let $0.50 come between me and the QA-check.
> 
> Distill workflow saved me the most. Used Opus reading whole session JSONLs and writing prose Obsidian updates to my vault (which I use as a RAG). Now Opus gets a 200-token structured edit lists, applies it, done. 25x spend cut on token docs alone.
> 
> So since I'm paying $200 flat for the Max plan, saving dollars never mattered to me. It's about extending the portioned-out utilization slice of the inference pie that Anthropic offers to me, on a sliding scale no less, when their utilization goes up, everyone's limits get adjusted down...
> 
> So what mattered to me was my weekly cap, a number which decides whether I get 4 more days or **4 days of waiting until reset**. Point of this for me was never thrift, but for those on the API this makes even more sense.
> 
> Don't forget to revise your system level CLAUDE.md to use this too.
> 
> > **MockingMatador** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlqeuv/) · 1 points
> > 
> > Sounds great! Link to github repo please!

> **elconcho** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmcfh2/) · 3 points
> 
> I've been thinking about this post (love it). I was wondering if you could achieve something similar by using claude code subagents and tell it to use haiku. Anyone tried that?

> **tribat** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmrgnv/) · 3 points
> 
> Fantastic idea, OP! I gave claude code the Medium URL and told it I wanted the same thing, but also with deepseek and ollama options. It took about 20 minutes total to write a few python files, including one to extract human-readable test.
> 
> I gave it 2 API tokens and asked it to run before-and-after tests. TLDR: Claude tested itself against Deepseek and said "conservatively, 23x cheaper".
> 
> ## Side-by-side judgment
> 
> The two answers cover the same milestone list and agree on every concrete detail. Differences:
> 
> |  | Delegated (DeepSeek Flash) | Direct (Claude Opus 4.7) |
> | --- | --- | --- |
> |  |
> | Tags (`v0.0-bootstrap`, etc.) | omitted | included |
> | M3 specificity | "spot-checked client emails matching correctly" | "5 random client emails" — |
> | closer to source |  |  |
> | M4 framing | "concrete accuracy numbers" | also captures "95% precision Phase-2 bar" |
> | M6 acceptance | invented one | correctly noted "no explicit acceptance criteria" |
> | Tone | a bit more verbose | tighter, closer to source phrasing |
> 
> Both are usable. Claude's version is slightly more faithful to the source on edge details (M6 has no acceptance criteria; the doc *says* `v0.0-bootstrap` is the M0 tag even though the body says M0 is "½ day"). Flash invented an acceptance criterion for M6 that wasn't in the source — minor  
> hallucination.
> 
> ## Cost comparison
> 
> |  | Path A (delegated) | Path B (Claude Opus 4.7) |
> | --- | --- | --- |
> |  |
> | Input tokens | 12,873 | ~12,873 (same file) |
> | Output tokens | 565 | ~700 (slightly longer) |
> | Pricing input | $0.14/M (DeepSeek miss) | $15/M (Opus 4.7 standard) |
> | Pricing output | $0.28/M | $75/M |
> | **Total** | **$0.001960** | **~$0.246** |
> | Multiple | 1× | **~125×** |
> 
> Plus, in Path A, the worker's 565-token answer goes into Claude's context for ~$0.0085 of  
> marginal Opus cost on whatever Claude does next. Total round-trip with delegation: **~$0.0105 vs ~$0.246 → ~23× cheaper end-to-end**, even being conservative about Opus cache.
> 
> ## Quality verdict
> 
> For "summarize a long doc" / "find facts in a corpus" tasks, **Flash's answer is good enough that Claude should accept it without re-reading the file**. The single hallucinated M6 acceptance  
> criterion is the kind of thing a careful reviewer catches in 5 seconds; the cost saved buys  
> plenty of review time. **Worth it.**
> 
> Where I'd *not* delegate:
> 
> - Anything where a hallucinated detail causes downstream damage (writing migration SQL, generating real client emails, suggesting payment amounts).
> - Anything requiring inferences across files that are not literally adjacent in the corpus — Flash is good at "what does this say" and weak at "what would happen if X."
> - Architectural/design judgment.
> 
> ## The CLAUDE.md instruction that turns this on
> 
> Drop this into your project's `CLAUDE.md` (or `~/.claude/CLAUDE.md` for all projects). The CLIs  
> come from a small wrapper that speaks the OpenAI Chat-Completions protocol, so the same code  
> works against DeepSeek, Kimi, OpenRouter, or Ollama:
> 
> ## Cheap-worker delegation (llm-tools)
> 
> Three CLIs are on PATH that route bulk I/O and predictable text generation  
> to a cheap OpenAI-compatible model (DeepSeek V4 Flash by default; V4 Pro  
> for `llm-write`). Use them when the task is bulk reading or boilerplate,  
> not when reasoning or correctness is on the line.
> 
> - `llm-ask <files...> -q "..."` — bulk read. Use when you'd otherwise  
> 	read 3+ files OR a single file >400 lines. Returns a short answer; you  
> 	read that, not the files. Verify details that matter — Flash  
> 	occasionally hallucinates a small fact.
> - `llm-write -r <ref> -s "<spec>" -o <path>` — generate tests, fixtures,  
> 	config scaffolds, doc templates. Review and edit; do not blindly trust.
> - `llm-extract <transcript.jsonl>` — compress a session transcript before  
> 	doc updates.
> 
> **Keep on Claude (do NOT delegate):**
> 
> - Architectural / design decisions
> - Debugging — the cheap model misses subtle bugs
> - Anything touching auth, payments, PII, deletion, or production data
> - Final commits and PR descriptions
> 
> Cost reference: ~$0.002 to `llm-ask` a ~12k-token doc on DeepSeek V4  
> Flash vs ~$0.25 for the same read on Opus 4.7 — ~125× cheaper, quality  
> good enough for "summarize / find facts" tasks.

> **HighDefinist** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojioxjg/) · 2 points
> 
> Hm... Does this really work out, math-wise?
> 
> Because: Sure, Kimi K2.5 is ~10 times cheaper than Opus API prices. But: When you have a Claude subscription, you are effectively also about ~10 times cheaper than when you use the API, so the per input/output token price is about the same in either case...
> 
> Now, it's still a reasonable approach in general (I actually did something similar with image recognition a few days ago), but it sounds like the real cost driver for using Opus for file reads might have been something else, perhaps related to caching, which incidentally improved when introducing this summary-based-workflow...
> 
> > **JohnnyJordaan** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojje4hb/) · 2 points
> > 
> > I've seen similar suggestions in the past to offload to alternative X. I'm getting the feeling that these are influencer posts trying to get people to use X more and the usage-constrained Claude userbase is an easy target.
> > 
> > Same for the "Good idea, I do this myself, but then with alternative Y" responses btw.
> > 
> > > **sweetbacon** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojkxtdm/) · 2 points
> > > 
> > > > I'm getting the feeling that these are influencer post  
> > > 
> > > 3mo old account with 3 posts in 3 subreddits and one comment. So tiring. 

> **azndkflush** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjal2m/) · 2 points
> 
> Good idea, could run locallm to handle easy task instead of paying api tokens

> **AshSurround** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjkpmu/) · 2 points
> 
> I don't usually run out of limits on Pro (lucky me?), but this post is an instant save.
> 
> seems like a no brainer for efficiency in *any* plan / tier. Sonnet / Opus doing the "what should we do next" and then cheaper models "do next"...
> 
> cuz apparentely Claude Token are premimum commodity. Variety of factors behind it (Constitutional AI, not owning their own compute, having to rent it from google / aws, etc) not necessarily just "Anthropic's Fault".

> **cygn** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjnqq5/) · 2 points
> 
> if a call is $0.02 and your total spend is $0.38 then you only called it 24 times. Which seems almost not worth it?
> 
> > **Fatso\_Wombat** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlf82s/) · 1 points
> > 
> > a 600% reduction.
> > 
> > > **MumStockholding** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojn3y5c/) · 1 points
> > > 
> > > Getting payed multiples? Did you get mentored by Trump?

> **Garland\_Key** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmmzfo/) · 2 points
> 
> This was written by Claude.

> **Full-Definition6215** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/oji7pct/) · 2 points
> 
> This is essentially the pattern I landed on too. I run Ollama on a mini PC (i9-9880H, 31GB RAM) and offload bulk operations — file scanning, linting, test runs — to local models while keeping Claude Code for the architectural decisions and complex implementations.
> 
> The key insight you're describing is that most of what burns tokens isn't the actual coding — it's Claude reading files to build context. Delegating that to a cheaper model and feeding back a summary is the right move.
> 
> What model are you using for the $0.02/call delegations? And are you passing structured summaries back to Claude Code or raw output?

> **Heavy\_Elderberry7769** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojk51kk/) · 2 points
> 
> This is smart. The routing logic in [CLAUDE.md](http://claude.md/) is the key part most people miss — without it Claude either delegates everything (loses quality) or nothing (burns tokens).
> 
> I hit the same wall and went a different route: instead of routing to a cheaper model, I split work between Claude Code (planning, code review, edits) and a local script that handles bulk operations Claude doesn't need to "think" about — file moves, find/replace across folders, log parsing. Saves the same tokens without an extra API dependency.
> 
> Two questions on your setup:
> 
> 1. How do you handle Kimi getting context wrong on the bulk read? Do you spot-check, or trust the output?
> 2. Have you tried this pattern with non-code tasks — like research or content drafts?

> **G-R-A-V-I-T-Y** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojiv3ns/) · 1 points
> 
> Awesome, thanks so much for the code, I’ll have to implement this. The limits are killing me.

> **fiji\_almonds** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojiy3si/) · 1 points
> 
> I done something similar to this. Works really well.
> 
> I've also combined the read/write approach with telling Claude to look at the task, then delegate to the right model(s) and parallelize the work when it makes sense. It will only use opus for tasks that really need it and distribute the rest to sonnet or haiku.

> **tigerscomeatnight** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojizin4/) · 1 points
> 
> What weekly limit? I hit my Pro limit daily.

> **Lower\_Cupcake\_1725** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojj4ttt/) · 1 points
> 
> I dedicate implementation to glm 5.1, it's the best cost/quality option for now to offload the work from Claude 

> **Linkman145** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojj7q79/) · 1 points
> 
> RAG solutions do this in production. Reading is typically outsourced to cheaper and faster models while generating happens with high end models.
> 
> That said Claude already does this with Haiku and the harness is engineered for this. It might not work as well with a tool use / different model.

> **kneecolesbean** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjfoer/) · 1 points
> 
> Definitely some good info there.
> 
> In the article you mention "compact after every task"? Have you tried skipping compact altogether and clear aggressively after each task/phase.

> **minkyuthebuilder** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjjuei/) · 1 points
> 
> hitting the pro limit by wednesday afternoon is too real. i usually just end up staring at my IDE like a caveman or reconsidering my life choices until the quota resets lmao. actually genius to just force it to offload the grunt work though. my wallet and my sanity thank you for this

> **FlightCautious3748** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojl5pkn/) · 1 points
> 
> what does your weekly throughput look like across active clients rn because that changes how aggressive the routing logic needs to be. we were running like 8 projects simultaneously and the limit problem got bad fast so we ended up routing almost everything through blink's ai gateway which covers 200+ models so claude handles the reasoning layer and cheaper models get the grunt work without wiring up separate providers. the routing decision is the hard part tbh once that's solid the cost math basically handles itself

> **ThesisWarrior** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojl84s1/) · 1 points
> 
> OP this looks fantastic!! Well done and hank you for sharing! any reason why a local agent cant handle the summarisation process as well as sn external API?
> 
> Maybe im missing the point here but WHY do I need an external API llm to do the summarization if I can simply have a script that strip's and compresses the content and feeds that back to claude? Or is that too simplistic?

> **arctide\_dev** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlbya5/) · 1 points
> 
> I actually get this to work. I use pretty much the same process as you, but divide the work in a different way. I send a lot of reading and standard stuff to the Codex CLI instead of Kimi. My OpenAI payment gives me about 12 and a half minutes of thinking time (or 5 hours!) that is separate from the amount of time I’m allowed to use with Claude Pro. Because of this, when Claude gets stuck in the middle of a feature, I use `codex exec` from a PostToolUse hook to pass the problematic part of the work over to Codex, and at the same time, I add to a file named `.shared/cross-tool-log.md`. This file works like a circular record, so the next time Claude is used, it can see what Codex just changed. I pay a set price for both services each month and spend no extra money.
> 
> Also, someone up above was saying 24 calls to Kimi is “almost not worth it”, but they’re missing the main advantage: the limit on how much you can use doesn't increase with each use. Claude Pro has a weekly limit, but Kimi doesn't. So, those 24 times Kimi was used, Claude didn't use up its limited amount of time on a difficult task that you would have likely given up on by Wednesday afternoon.
> 
> More importantly for me, I’ve made the instructions for deciding where to send the work in CLAUDE.md very precise. I have it say, “Before Reading a file, if the file has more than 800 lines and isn't about the actual edit being done, first use a search subagent”. Just that rule has reduced my reading of entire files (around 3000 tokens) to looking at a summary of files (around 150 tokens), and that benefit increases throughout a session.

> **kuroudo\_ai** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlew8x/) · 1 points
> 
> Smart approach. I've been running a similar multi-agent setup -- 5 named sub-agents (code review, investigation, security audit, deployment check, session handoff) each with focused instructions. The key insight is the same: don't burn Opus tokens on tasks that a lighter model can handle. We also added spending caps and auth token hooks to prevent runaway costs. The delegation pattern is the real unlock.

> **Delicious-Storm-5243** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlm5uu/) · 1 points
> 
> Same delegation pattern hit me too — CLAUDE.md routing where 'read this 200-file repo' goes to a cheap model and 'reason about why this test is failing' stays on Claude. Pro limit went from Wednesday to never-hit. The non-obvious win is Claude itself learns to pre-filter what to delegate vs solve directly, so by week 2 the routing got tighter without me touching the rules. Curious if you found Kimi handles structured output reliably enough for the bulk reads, or if you have to validate before passing back to Claude.

> **Cazique\_\_** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlnncv/) · 1 points
> 
> !remindme 2 days

> **resist888** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojlqj04/) · 1 points
> 
> I’m kinda new to this. Does this work for API Token usage? e.g., for something using an Anthropic key?

> **Hichiro6** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojm1w43/) · 1 points
> 
> Is this possible to do on Linux ?

> **setec404** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmm3h1/) · 1 points
> 
> I have always been confused as to how this isnt native behavior in tools like opencode.

> **AccomplishedFix3476** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmnnu7/) · 1 points
> 
> routing the simple stuff to a cheaper model is so underrated, ppl burn pro limits doing greps and rg searches that should never touch sonnet. i hit the same wall and switched my discovery work to qwen running locally, freed up like 60 percent of my weekly opus budget

> **Honkey85** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmp33g/) · 1 points
> 
> YSK: you can use Mistral large for free over API right now.

> **eior71** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmsdgl/) · 1 points
> 
> That is a pretty clever way to handle the token crunch. I have been playing around with similar routing patterns using custom instructions, but offloading the boilerplate to a cheaper model via CLI is way more efficient than what I was doing. Have you noticed any weird context drift when switching between the models, or does the setup keep everything grounded pretty well?

> **PrestigiousTonight55** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojmudu1/) · 1 points
> 
> !remindme

> **itslitman** · [2026-05-03](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojn8mqc/) · 1 points
> 
> Been doing something similar with a local qwen model through ollama. The routing rules in CLAUDE.md are what actually make it work though, without those Claude just ignores the cheap model and reads everything itself.

> **mkeee2015** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/oji8x93/) · 1 points
> 
> Very insightful!
> 
> How does Claude know in advance how many tokens are involved in the request (referring to "- Tasks under ~2000 tokens")?
> 
> Is the GitHub Link appearing soon? I'd love to explore your great strategy and adapt it to different context...
> 
> Thank you for having shared your idea and discovery.

> **ClaudeAI-mod-bot** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojhopvu/) · 0 points
> 
> We are allowing this through to the feed for those who are not yet familiar with the Megathread. To see the latest discussions about this topic, please visit the relevant Megathread here: [https://www.reddit.com/r/ClaudeAI/comments/1s7fepn/rclaudeai\_list\_of\_ongoing\_megathreads/](https://www.reddit.com/r/ClaudeAI/comments/1s7fepn/rclaudeai_list_of_ongoing_megathreads/)

> **CrazyWord2800** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojj724l/) · \-1 points
> 
> What are you guys building that you are so inneficient with your tokens?
> 
> 0 knowledge 100% vibe coding.
> 
> I am genuine asking, since I see here so much stuff that makes 0 sense.
> 
> With 40% Sonnet($20 dollar plan) you can do an entire microservice in less than a day. And even then is still inneficient to leave it doing that much? 
> 
> Is utter inneficient to let AI do decisions since it does very poorly. Is great on following patterns and extrapolating data.
> 
> But building something from scratch is just "the same website but different colours" which has 0 value. 
> 
> So what are you guys building?
> 
> > **JohnnyJordaan** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjexoy/) · 2 points
> > 
> > I'm suspecting there's a big group of people just mindlessly throwing an overall task at Claude thinking they're their boss and that's how 'instructing' and 'delegation' works. And of course use Opus because why settle for less? To then go back and forth with it asking questions, drafting the plan, starting the endeavour etc and consuming a trainload of usage in the process. And then of course complain how unfair the token usage constraints are.
> > 
> > **BetterAd7552** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojjlyun/) · 2 points
> > 
> > It’s actively being used by a vast number of enterprise users around the world, and of course everyone in between.
> > 
> > Not everyone is a viber building a todo or recipe app.
> > 
> > > **Mountain\_Resource292** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojk8t0f/) · 1 points
> > > 
> > > Totally agree. There’s a vocal community of students, enthusiasts and early to mid level techies desperate to do the cool stuff and get ahead whilst minimising costs - whilst owners / senior techs are chucking unlimited tokens at projects, with their minds blown at how this is all effectively free in the grand scheme of things.
> > > 
> > > **CrazyWord2800** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojkk5lf/) · 1 points
> > > 
> > > Ok so how are you destroying 100$ worth of tokens?
> > > 
> > > Even $20, seems a lot. 240 hours a month.  Codex right now has a $100 for free tier (promo) but still.
> > > 
> > > I have to literally turn my brain off to use that much tokens. Just literally understand nothing and review nothing.
> > > 
> > > Is this the state of it? Because at this point, it will just implode.
> > > 
> > > You can't just let the AI do everything without 0 review. Thats just recipe for disaster. 

> **JoePatowski** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojhzn21/) · 0 points
> 
> this is a great write up and I’ll be testing it out on Monday

> **\[deleted\]** · 2026-05-02 · 0 points
> 
> Quote from the post: „Claude calls them via bash tool“
> 
> > **Daranad** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojibkeg/) · 2 points
> > 
> > Quote from the post: „Claude calls them via bash tool“

> **Raredisarray** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojkwlb2/) · 0 points
> 
> Hot damn ! Gonna try hooking up my local qwen 27b … I am always maxing my pro plan out, it’s a pan in the ass.

> **TeeRKee** · [2026-05-02](https://reddit.com/r/ClaudeAI/comments/1t1o43w/comment/ojiaijl/) · \-6 points
> 
> So you pay more than 20$ to not hit your limit. Genius.