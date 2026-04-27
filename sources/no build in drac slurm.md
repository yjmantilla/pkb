---
title: "no build in drac slurm"
source: "https://github.com/NolanSmyth/misalignment-bounty-template"
author:
published:
created: 2026-04-27
description: "Dishonesty Benchmark. Contribute to NolanSmyth/misalignment-bounty-template development by creating an account on GitHub."
tags:
  - "clippings"
  - "programming"
---
### Package management with `no-build = true`

DRAC ships "dummy" Python packages (e.g., `opencv-noinstall` with version `9999+dummy`) that have intentionally-failing build scripts. Setting `no-build = true` in `pyproject.toml` forces uv to use only pre-built wheels, avoiding these dummy packages. We also pin `opencv-python-headless<9999` as a constraint to exclude the dummy wheel.