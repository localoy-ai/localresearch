---
name: research-review
version: 0.1.0
publisher: localoy
capabilities: [files, shell, web]
description: Review research as an adversary who did not do the work — rerun it from the folder, attack the split, the baselines, the statistics and the claims, and answer with a verdict on each claim: holds, holds with caveats, or not supported. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, review, reproducibility, statistics, localresearch]
    related_skills: [model-optimize, write-up]
allowed-tools:
  - Bash
  - Read
  - Write
  - WebSearch
  - WebFetch
triggers:
  - review this research
  - is this result real
  - reproduce this
  - peer review
  - check the analysis
  - red team the results
tags: [review, reproducibility, statistics, peer-review]
---

## When to invoke this skill

Before any result leaves the project: a report, a paper, a decision. The
reviewer must not be the one who did the work. When a chief leads, it
creates a fresh reviewer agent for this. The job is to break the result. A
review that finds nothing should say what it tried.

## What to attack

1. **Rerun from the folder.** Fresh environment from `env.txt` or the
   requirements file, the data at the recorded hashes, the commands from the
   run ledger. Run each through the `experiment` tool (group `review`). Do the
   numbers come back within seed noise?
2. **The split.** Can the same subject, group, batch, site or time period be
   on both sides? Was anything fitted on all rows (scalers, feature
   selection, PCA)? Was the test set touched before the end? If the data has
   clusters, are within-cluster and leave-one-cluster-out scores reported?
3. **The baselines.** Was every baseline tuned with the same budget and the
   same protocol as the winner? Was any key setting capped? Is the trivial
   reference (mean, group mean) reported?
4. **Selection.** Was the winner chosen by the plan's rule, from validation
   scores, or picked after seeing test results? Is the reported number the
   number that was used to choose (optimistic), or an untouched one?
5. **Statistics.** Several seeds? Intervals? Effect sizes? Correction for
   several comparisons? Paired tests on independent units, not on correlated
   predictions? Permutation tests run on the actual model and pipeline?
6. **Claims against evidence.** Run `claim_check` on the report. Read every
   sentence that claims superiority, causation or generality, and check that
   the runs actually show it.
7. **Prompts and defaults.** In any automated pipeline, look for conclusions
   written into prompts, fallback text or default values. A pipeline that
   prints "PASS" without testing proves nothing.

## Output

`review-<topic>.md` with:
- one line per claim: **holds**, **holds with caveats**, or **not
  supported**, with the evidence
- what was rerun, and how close the numbers came back
- the fixes needed, most important first

Be specific and plain: name the file, line, run or number.
