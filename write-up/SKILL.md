---
name: write-up
version: 0.1.0
publisher: localoy
capabilities: [files, shell]
description: Write the report or paper from the plan, the run ledger and the review — figures made from the runs, every number claim-checked against the ledger, honest about null and negative results. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, writing, paper, report, localresearch]
    related_skills: [research-review, deep-research]
allowed-tools:
  - Bash
  - Read
  - Write
triggers:
  - write the paper
  - write up the results
  - research report
  - draft the manuscript
  - summarize the findings
tags: [writing, paper, report, manuscript]
---

## When to invoke this skill

When results are in and reviewed, to turn them into something others read:
a report, a paper draft, a technical note.

## The rules

- **Numbers come from the ledger.** Every result is a run's metric, or
  computed from runs by a script that is itself a run. Reported numbers from
  other work are cited as theirs.
- **Figures from runs.** Each figure is made by a script from the ledger and
  saved next to the report. The caption names the runs.
- **Say what the evidence supports, no more.** "Comparable to" when the
  intervals overlap. "Within stages it does not beat the stage mean" when that
  is what the runs show. Null and negative results are reported as results.
- **The protocol is described well enough to rerun:** data and hashes, split,
  search and budget, seeds, environment.

## Structure (adapt to the venue)

1. **Summary.** The question, what was done, the main result with its
   uncertainty, and the main limitation.
2. **Background.** From `deep-research`, cited.
3. **Data.** From the data audit: what a row is, the structure, the references.
4. **Methods.** The plan's protocol, candidates, baselines, search and budget.
   Any change to the plan, with its date and reason.
5. **Results.** Leaderboard, baselines, references, ablations, and within- and
   across-cluster results where they apply. Intervals throughout.
6. **Review.** What the independent review checked and found.
7. **Limitations,** stated plainly.
8. **Reproducing.** Commands, environment, data hashes, the `runs/` folder.

## Before it goes out

Run `claim_check` on the file. Every number it flags is traced to a run,
computed by a run, or removed. Then tell the person in one short message
what the report says and where it is.
