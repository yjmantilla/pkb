---
title: "check results drac slurm"
source: "https://github.com/NolanSmyth/misalignment-bounty-template"
author:

created: 2026-04-27
description: "Dishonesty Benchmark. Contribute to NolanSmyth/misalignment-bounty-template development by creating an account on GitHub."
tags:
  - "clippings"
  - "programming"
---
### Check resultscat outputs/openai-gpt-oss-120b\_\*/aggregate\_results.json

To do it live:

tail -f "$(ls -t slurm\_logs/\*.out | head -1)"

(this will follow the last modified out)

### Copy results locallyUse `sync_from_tamia.sh` from your local repo root. Set `DCB_REMOTE_REPO` to the absolute path of the repo on tamia — either in a project-local `.env` file (gitignored; auto-sourced by the script) or in your shell rc:

# .env at repo root (per-clone, not committed)
DCB\_REMOTE\_REPO=~/links/projects/<allocation\>/$USER/misalignment-bounty-template

Optional: `HOST` (ssh alias or `user@host`; defaults to `tamia`).

bash sync\_from\_tamia.sh                                    # default: outputs + slurm\_logs
bash sync\_from\_tamia.sh outputs/experiments                # just experiments/
bash sync\_from\_tamia.sh outputs/experiments/exp\_<id\>       # a single experiment