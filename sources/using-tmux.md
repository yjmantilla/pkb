---
title: "using tmux"
source: "https://github.com/NolanSmyth/misalignment-bounty-template"
author:

created: 2026-04-27
description: "Dishonesty Benchmark. Contribute to NolanSmyth/misalignment-bounty-template development by creating an account on GitHub."
tags:
  - "clippings"
  - "programming"
---
### Using tmux# 1. Start a named tmux session
tmux new-session -s glm5-download

# 2. Load required modules (httpproxy enables outbound HTTPS)
module load python/3.12 httpproxy

# 3. Navigate to repo
cd /project/6100859/tommie/misalignment-bounty-template

# 4. Start the download with fast transfer enabled
export HF\_HUB\_ENABLE\_HF\_TRANSFER=1
uv run -- hf download zai-org/GLM-5-FP8 --local-dir "${DCB\_MODELS\_ROOT:-/scratch/t/tommie/models}/GLM-5-FP8"

# 5. Detach tmux: press Ctrl+B, then D
# 6. Reattach later: tmux attach -t glm5-download
# 7. When done, verify:
du -sh "${DCB\_MODELS\_ROOT:-/scratch/t/tommie/models}/GLM-5-FP8"   # expect ~756 GB

To kill a session:

tmux ls
#keepMe: 1 windows (created Wed Jun 24 14:20:15 2015) \[171x41\]
#otherSession: 1 windows (created Wed Jun 24 14:22:01 2015) \[171x41\]
#3: 1 windows (created Wed Jun 24 14:23:28 2015) \[171x41\]

#(assuming here that you're on keepMe session)
tmux kill-session -t otherSession
#\-or-
tmux kill-session -t 3