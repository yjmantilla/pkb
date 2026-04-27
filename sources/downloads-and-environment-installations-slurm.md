---
title: "downloads and environment installations slurm"
source: "https://github.com/NolanSmyth/misalignment-bounty-template"
author:

created: 2026-04-27
description: "Dishonesty Benchmark. Contribute to NolanSmyth/misalignment-bounty-template development by creating an account on GitHub."
tags:
  - "clippings"
  - "programming"
---
### Offline dependency installation on compute nodesCompute nodes have no internet access. The `setup_once.sh` script runs `uv sync` on the login node to cache all wheels. The SLURM job then runs `uv sync --frozen --offline` to install from cache without network access.

### Model download requires `httpproxy` module`hf download` freezes on login nodes without the `httpproxy` module loaded. Download the model on a compute node with `module load httpproxy` or use `setup_once.sh` which handles this.

### Download may freeze on CCTry

export HF\_HUB\_DISABLE\_XET=1