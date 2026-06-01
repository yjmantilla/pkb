---
title: "10 CLI Tools That Made the Biggest Impact On Transforming My Terminal-Based Workflow"
source: "https://www.reddit.com/r/commandline/comments/1epjppl/10_cli_tools_that_made_the_biggest_impact_on/"
author:
  - "[[piotr1215]]"
published: 2024-08-11
created: 2026-05-04
description: "EDIT: Added a companion video https://youtu.be/fU8HB1cvG9w Awesome tools that I learned about from the comments:"
tags:
  - "clippings"
  - "tools"
---
EDIT: Added a companion video [https://youtu.be/fU8HB1cvG9w](https://youtu.be/fU8HB1cvG9w)

Awesome tools that I learned about from the comments:

- clipboard (not sure how I functioned without it, it's a bit like vim registers but in the terminal)
- fzf-tab
- atuin

I've compiled a list of 10 CLI tools that I use the most and which impacted my terminal-based workflow significantly:

[https://piotrzan.medium.com/10-cli-tools-that-made-the-biggest-impact-f8a2f4168434](https://piotrzan.medium.com/10-cli-tools-that-made-the-biggest-impact-f8a2f4168434)

Here is tl;dr:

- [**fzf**](https://github.com/junegunn/fzf): A fuzzy finder that enhances command-line workflows with interactive searching.
- [**bpy**top](https://github.com/aristocratos/bpytop): Resource monitor that shows usage and stats for processor, memory, disks, network and processes.
- [**tmux**](https://github.com/tmux/tmux): A terminal multiplexer for managing multiple terminal sessions efficiently.
- [**lazygit**](https://github.com/jesseduffield/lazygit): A TUI for git operations, simplifying repository management.
- [**gh (GitHub CLI)**](https://github.com/cli/cli): A GitHub CLI tool to manage repositories, issues, and PRs from the terminal.
- [**entr**](https://github.com/eradman/entr): A utility that runs commands when files change, useful for automation.
- [**just**](https://github.com/casey/just): A command runner for managing project-specific tasks with simple commands.
- [**taskwarrior**](https://github.com/GothenburgBitFactory/taskwarrior): A command-line tool for efficient task management.
- [**tldr**](https://github.com/tldr-pages/tldr): Simplified man pages providing quick command examples.
- [**pet**](https://github.com/knqyf263/pet): A snippet manager for saving and reusing complex command-line commands.

It wasn't easy to choose, for example I skipped `Autokey` which is really amazing and I built nice workflow around it. What are yours?

---

## Comments

> **yasser\_kaddoura** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlrtws/) · 31 points
> 
> [denisidoro/navi: An interactive cheatsheet tool for the command-line](https://github.com/denisidoro/navi) is superior to [knqyf263/pet: Simple command-line snippet manager](https://github.com/knqyf263/pet).
> 
> Regarding fzf, [Aloxaf/fzf-tab: Replace zsh's default completion selection menu with fzf!](https://github.com/Aloxaf/fzf-tab) is a life changer if you integrate it properly with commands & if you have a script to preview any file type.
> 
> For those who are interested for more CLI/TUI tools, there are awesome lists out there, such as:
> 
> - [rothgar/awesome-tuis: List of projects that provide terminal user interfaces](https://github.com/rothgar/awesome-tuis)
> - [agarrharr/awesome-cli-apps: 🖥 📊 🕹 🛠 A curated list of command line apps](https://github.com/agarrharr/awesome-cli-apps)
> - [toolleeo/cli-apps: The largest Awesome Curated list of CLI/TUI applications with source data organized into CSV files](https://github.com/toolleeo/cli-apps)
> 
> > **HelpImOutside** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhm8nlg/) · 6 points
> > 
> > Navi looks super cool but in my experience this type of thing is something I install then completely forget about. I'm sure once you've used it for a while it's invaluable but..
> > 
> > > **yasser\_kaddoura** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhme7s7/) · 2 points
> > > 
> > > I use it for 2 situations:
> > > 
> > > When I find myself googling something for a command that I don't use often, and an alias isn't convenient to remember. Let's say you want to use a tool without sourcing config, and you find yourself googling it. In navi you can simple add something like the following configuration, you can find it easily by presssing C-g and type "vim without". This is faster than googling and easier to find than an alias.
> > > 
> > > \# Run vim without rc
> > > vim -u NONE
> > > 
> > > Accelerating writing a command by using `variables`, for instance include files as arguments for a command
> > > 
> > > \# Downgrade pkg
> > > sudo pacman -U <pkg>
> > > $pkg: ls /var/cache/pacman/pkg/\*zst
> > > 
> > > I don't use navi heavily, since most commands I execute I use C-r with fzf which shows the commands that I usually use.
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlx846/) · 5 points
> > 
> > Thank you for sharing! `fzf-tab` is really cool, added it to my plugins. Navi looks great at first glance, but I have extensive solleciton of commmands in `pet` and it works well for me, so no reason to swap.

> **\[deleted\]** · 2024-08-11 · 0 points
> 
> Totally agree, I went the same route and have now lots of various scripts that integrate with zsh and work for me.
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlk3hn/) · 3 points
> > 
> > Totally agree, I went the same route and have now lots of various scripts that integrate with zsh and work for me.

> **\[deleted\]** · 2024-08-11 · 0 points
> 
> Thank you, I’ve browsed it and there are a few gems. Maybe I will start using clipboard again, it has some bugs before but this is a neat tool.
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhnrl0h/) · 2 points
> > 
> > Thank you, I’ve browsed it and there are a few gems. Maybe I will start using clipboard again, it has some bugs before but this is a neat tool.

> **ratthing** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlber3/) · 8 points
> 
> I use a great knowledge base tool called nb ([https://xwmx.github.io/nb/](https://xwmx.github.io/nb/)). It is a massive Bash script that uses Git as the backend for managing notes, files, bookmarks, and todos. Its a little clunky at first, but once you get the hang of it, it is very useful.
> 
> > **Innovator-X** · [2026-04-27](https://reddit.com/r/commandline/comments/1epjppl/comment/oilr1yh/) · 1 points
> > 
> > wow the creator of the repo is a legend. Their work output is insane.

> **freefallfreddy** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlik7f/) · 6 points
> 
> I’ve switched from `entr` to `watchexec`.
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlo87e/) · 5 points
> > 
> > I actually switched from watchexed to entr haha
> > 
> > > **jstanforth** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhm6h8h/) · 1 points
> > > 
> > > Just curious, what are the benefits of one vs the other?
> > > 
> > > > **freefallfreddy** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhmdrwo/) · 4 points
> > > > 
> > > > For me: it’s easier to select files to watch. With entr you have to use ls or find to select files, then pipe those to entr. With watchexec you can pass in extensions, regexes (I think) and it recurses into subdirectories to find matching files to watch.
> > > > 
> > > > > **jstanforth** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhmfqzg/) · 2 points
> > > > > 
> > > > > Ahh, nice, thanks! Super-useful already after a couple minutes of experimenting with it.

> **freefallfreddy** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlitwm/) · 6 points
> 
> Also: `lazydocker`, such a quality of life thing. Especially when I want to quickly shell into containers or restart them.
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlxfy0/) · 1 points
> > 
> > Great recommendation, I keep forgetting to use it haha. Always fumbling with docker commands instead or use pet snippets.
> > 
> > **foomojive** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhokfbt/) · 1 points
> > 
> > I use ctop
> > 
> > > **freefallfreddy** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhooue2/) · 1 points
> > > 
> > > Cool. Looks like their feature sets don’t overlap so much though.
> > > 
> > > > **foomojive** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhop9z2/) · 1 points
> > > > 
> > > > Well ctop can quickly exec a shell for a single container, view logs, start and stop, etc.
> > > > 
> > > > > **freefallfreddy** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhopieo/) · 1 points
> > > > > 
> > > > > I don’t see start/stop in their README.
> > > > > 
> > > > > > **foomojive** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhoqdrz/) · 2 points
> > > > > > 
> > > > > > Mm, ok. I am not a contributor to the project, I just use it. Press enter over a container for a context menu where you can log, start, stop, pause, unpause, and I think remove a container. Don't know why this is not in the readme.
> > > > > > 
> > > > > > Anyway to each their own, if you like lazydocker go for it

> **theunglichdaide** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhnkmte/) · 6 points
> 
> Not a CLI, but I found this website very useful [https://terminaltrove.com/](https://terminaltrove.com/)  
> I discovered many cool CLI tools through it.

> **\[deleted\]** · 2024-08-11 · 0 points
> 
> Thank you for the recommendations, both Atuin and glances looks really cool. Last week I’ve been thinking about creating a project for sharing command line history, looks like atuin does exactly what I thought about!
> 
> > **piotr1215** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhpm4wx/) · 1 points
> > 
> > Thank you for the recommendations, both Atuin and glances looks really cool. Last week I’ve been thinking about creating a project for sharing command line history, looks like atuin does exactly what I thought about!
> > 
> > > **\[deleted\]** · 2024-08-12 · 0 points
> > > 
> > > Even without the sync, having history for tmux session and folder separately is relaly cool!
> > > 
> > > > **piotr1215** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhq75jp/) · 2 points
> > > > 
> > > > Even without the sync, having history for tmux session and folder separately is relaly cool!

> **gumnos** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhl2x56/) · 9 points
> 
> Of those, I find `tmux` indispensable, and `entr` & `fzf` are my go-tos when I have the need for their respective functionalities (though it's not terribly frequent). I've used `taskwarrior`, but found that it was overkill for most of what I do—when I have time-based tasks, I put them on my `remind(1)` calendar, and otherwise-unbounded tasks just go in a todo file (which is linked to my `~/.plan` so I can use `finger` from a remote machine to get my task-list).
> 
> I am comfortable enough with `git` that I don't really need `lazygit`, and I don't use GitHub enough to warrant using `gh`.
> 
> I didn't find that `tldr` gave me anything I couldn't largely get from either `apropos` or `man`, so it never stuck.
> 
> I tend to use `top(1)` or `systat(1)` for my resource monitoring, so don't really find myself reaching for the fancier versions of `top` like `htop` or `bpytop`
> 
> But I hadn't heard of `just` or `pet`. I suspect this old-fart would use `make(1)` where you're using `just`. But `pet` looks pretty slick.
> 
> Thanks for sharing!
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlkeso/) · 1 points
> > 
> > As a fellow old fart who used Makefile most of the time, just is really slick if you want to give it a try!
> > 
> > > **belibebond** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhoz1nm/) · 1 points
> > > 
> > > It's actively managed and features are added frequently. It's cross platform and works with cicd. Really an excellent tool. Give just a try.

> **AmazingDisplay8** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhl2g2f/) · 4 points
> 
> Thanks for pet. I didn't know it existed. Good list
> 
> > **dfwtjms** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlxp79/) · 4 points
> > 
> > Isn't that just aliases and functions with extra steps and dependencies?

> **pouetpouetcamion2** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhldvev/) · 4 points
> 
> no a tool, but bash 3 boilerplate from kevin van zonneveld (i mean "main.sh") [https://github.com/kvz/bash3boilerplate.git](https://github.com/kvz/bash3boilerplate.git) brings good template for argparsing, err logging on term (no systemd or ) and trapping. this and shellcheck enables you to write a few scripts quickly. they complete well with listed tools.

> **mgutz** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lho6r1d/) · 3 points
> 
> - lazydocker
> - ripgrep
> - neovim
> - yazi - file manager
> - fd - find
> - dua - fast disk usage (can be interactive)
> - glances - system resource overview
> 
> > **piotr1215** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhq77yn/) · 1 points
> > 
> > You are second person recommending `glances`, it slooks really neat.

> **thesobercoder** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhl4zz3/) · 5 points
> 
> You might want to try [bottom](https://github.com/ClementTsang/bottom), which I suspect might be faster than bpytop because it is written in rust.
> 
> > **ludicroussavageofmau** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlqxku/) · 12 points
> > 
> > If you want a similar look and feel to bpytop but with good performance, try [btop++](https://github.com/aristocratos/btop). It's by the same author and is the successor to bpytop (has been since 2021).

> **RoboticElfJedi** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhkx9os/) · 6 points
> 
> I never tire of these posts, I usually find something new.
> 
> I looked into lazygit recently. It looked like it hadn't been under development for years - I couldn't get it to install, as it won't work on python 3.7+.
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhl16bs/) · 1 points
> > 
> > Thanks! I just checked and last commit was 2 days ago, the repo looks pretty active.
> > 
> > > **RoboticElfJedi** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhl1wrx/) · 2 points
> > > 
> > > I was wrong - perhaps I got it confused. Playing with it now!

> **Big\_Combination9890** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhle37s/) · 2 points
> 
> I didn't know about `bpytop`, and now I do! Thanks so much, I was on the lookout for a complete sysmon for my setup!
> 
> Btw. as far as interface design goes, that thing should be a textbook example for terminal user-discoverability done right.
> 
> > **ITooSpooky** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lho7uhw/) · 2 points
> > 
> > Instead of using bpytop use btop++ which is a rewrite by the same author using c++, bpytop has been discontinued since 2021
> > 
> > > **Big\_Combination9890** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhpcjns/) · 1 points
> > > 
> > > Oh, thank you, I thought the project could need some updates!

> **dhaitz** · [2024-08-12](https://reddit.com/r/commandline/comments/1epjppl/comment/lhpimcq/) · 2 points
> 
> Here's a useful list of modern shell commands: [johnalanwoods/maintained-modern-unix](https://github.com/johnalanwoods/maintained-modern-unix)
> 
> Tools like fd, bat, lsd etc. are faster, prettier and more convenient (e.g. with git integration) than their traditional counterparts

> **yuri0r** · [2024-08-13](https://reddit.com/r/commandline/comments/1epjppl/comment/lhvocwl/) · 2 points
> 
> i went from tmux to zellij, layouts are cool, starts from 1 by default.  
> gitui, does almost everything i need in regards to git.  
> atuin, holy fuck its great.

> **rd\_626** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlhxga/) · 1 points
> 
> RemindMe! 1 month
> 
> > **RemindMeBot** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhli4bh/) · 1 points
> > 
> > I will be messaging you in 1 month on [**2024-09-11 14:58:47 UTC**](http://www.wolframalpha.com/input/?i=2024-09-11%2014:58:47%20UTC%20To%20Local%20Time) to remind you of [**this link**](https://www.reddit.com/r/commandline/comments/1epjppl/10_cli_tools_that_made_the_biggest_impact_on/lhlhxga/?context=3)
> > 
> > [**5 OTHERS CLICKED THIS LINK**](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=Reminder&message=%5Bhttps%3A%2F%2Fwww.reddit.com%2Fr%2Fcommandline%2Fcomments%2F1epjppl%2F10_cli_tools_that_made_the_biggest_impact_on%2Flhlhxga%2F%5D%0A%0ARemindMe%21%202024-09-11%2014%3A58%3A47%20UTC) to send a PM to also be reminded and to reduce spam.
> > 
> > <sup>Parent commenter can </sup> [<sup>delete this message to hide from others.</sup>](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=Delete%20Comment&message=Delete%21%201epjppl)
> > 
> > ---
> > 
> > | [<sup>Info</sup>](https://www.reddit.com/r/RemindMeBot/comments/e1bko7/remindmebot_info_v21/) | [<sup>Custom</sup>](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=Reminder&message=%5BLink%20or%20message%20inside%20square%20brackets%5D%0A%0ARemindMe%21%20Time%20period%20here) | [<sup>Your Reminders</sup>](https://www.reddit.com/message/compose/?to=RemindMeBot&subject=List%20Of%20Reminders&message=MyReminders%21) | [<sup>Feedback</sup>](https://www.reddit.com/message/compose/?to=Watchful1&subject=RemindMeBot%20Feedback) |
> > | --- | --- | --- | --- |
> > |  |

> **bew78** · [2025-04-05](https://reddit.com/r/commandline/comments/1epjppl/comment/mlh6ks7/) · 1 points
> 
> Nix ✨
> 
> > **qiang\_shi** · [2025-10-28](https://reddit.com/r/commandline/comments/1epjppl/comment/nlwkbua/) · 1 points
> > 
> > hard pass.
> > 
> > SUPER complex
> > 
> > only navel gazers with apparently infinitely zero value free time use nix

> **Status\_Tea\_2017** · [2025-06-04](https://reddit.com/r/commandline/comments/1epjppl/comment/mw004p9/) · 1 points
> 
> llm the cli by [Simon Willison](https://github.com/simonw) is a pretty magical IMO and is on my top 10 list.

> **Simple\_Paper\_4526** · [2025-09-01](https://reddit.com/r/commandline/comments/1epjppl/comment/nbt4evz/) · 1 points
> 
> fzf, ripgrep, bat, jq, exa, htop, tldr, lazygit, entr, and fd. lately i’d add Qodo cli too since it plugs in ai review/gen without leaving terminal.

> **qiang\_shi** · [2025-10-28](https://reddit.com/r/commandline/comments/1epjppl/comment/nlwkh3m/) · 1 points
> 
> throw away all your ai cli tools and look at sst/opencode
> 
> thank me later

> **iluvecommerce** · [2026-02-05](https://reddit.com/r/commandline/comments/1epjppl/comment/o3o38gx/) · 1 points
> 
> Great list! These traditional CLI tools are essential for any developer's toolkit.
> 
> As someone who's been building CLI tools, I created Sweet! CLI ([https://sweetcli.com](https://sweetcli.com/)) to tackle a different problem: AI-powered automation. Instead of remembering complex commands, you describe what you need in natural language and it writes/executes the scripts automatically. It's like having fzf for your entire workflow - describe what you want and it finds the right implementation.
> 
> What makes it different is it actually executes the code it generates (with safety checks), making it perfect for one-off automation tasks that you'd normally write throwaway scripts for. It complements tools like just and entr beautifully.
> 
> (I'm the founder, building it as a solo developer to solve my own repetitive scripting problems!)
> 
> What kind of repetitive tasks do you find yourself scripting most often?

> **Noundry** · [2026-03-10](https://reddit.com/r/commandline/comments/1epjppl/comment/o9ps706/) · 1 points
> 
> Solid list. You might also want to check out Worktale, a CLI tool that turns your git history into a private, interactive developer journal and dashboard. Everything stays local, so it's handy for tracking your code activity and prepping work summaries without sending data anywhere.

> **Noundry** · [2026-03-11](https://reddit.com/r/commandline/comments/1epjppl/comment/o9v2fua/) · 1 points
> 
> Solid list. You might also want to check out Worktale, a CLI tool that turns your git history into a private, interactive developer journal and dashboard. Everything stays local, so it's handy for tracking your code activity and prepping work summaries without sending data anywhere.

> **ahk-\_-** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlam5v/) · 0 points
> 
> Emacs.
> 
> > **bagpussnz9** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhn1dwc/) · 3 points
> > 
> > there it is :-)

> **\[deleted\]** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhlxhim/) · 5 points
> 
> Nice one, chest also allows for creating custom chest sheets.
> 
> > **piotr1215** · [2024-08-11](https://reddit.com/r/commandline/comments/1epjppl/comment/lhnrwng/) · 1 points
> > 
> > Nice one, chest also allows for creating custom chest sheets.