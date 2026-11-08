---
name: research-plan
version: 0.1.0
publisher: localoy
capabilities: [files, web]
description: Write the research plan before any result exists — question, claims to test, data, split protocol, metric, selection rule, search space and budget, and what a negative result looks like — as PLAN-<topic>.md, which every later step follows and nobody quietly rewrites. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, planning, preregistration, localresearch]
    related_skills: [deep-research, data-audit, model-optimize, research-review]
allowed-tools:
  - Read
  - Write
  - WebSearch
triggers:
  - plan the research
  - research plan
  - study design
  - preregister
  - how should we test this
tags: [research, plan, protocol, design]
---

## When to invoke this skill

Before training, simulating or analysing anything whose result will be
reported. The plan fixes the rules while nobody knows the answer yet, so the
answer cannot bend them. It lives in `PLAN-<topic>.md` in the project
folder: a standard file any agent can open and continue.

## What goes in the plan

Write these sections. Each is short; precision beats length.

1. **Question.** One sentence. Then the claims it breaks into, each one
   testable: "X predicts Y within a drying stage better than the stage mean".
2. **Prior work.** What `deep-research` found, in three lines, with the best
   reported result on comparable data and protocol, cited.
3. **Data.** Source, version or hash, size, units, what one row is. Known
   structure: groups, repeated subjects, time, batches, stages, sites.
4. **Protocol.** The split, chosen from the data's structure, never from which
   split scores better: grouped or time-ordered when the structure needs it;
   nested cross-validation or a clean train/validation/test split; the test
   set opened once, at the end. If the data has clusters (stages, sites,
   batches), say how results are reported within and across them.
5. **Metric and goal.** The primary metric and direction (`rmse:min`), the
   secondary ones, and the smallest difference that would matter in practice.
6. **Candidates and baselines.** The methods, the strongest simple baseline,
   and a trivial reference (the mean, the group mean, persistence).
7. **Search and budget.** Each candidate's search space, the search method,
   and the budget: the same budget for every candidate, baselines included.
   Seeds: how many, which.
8. **Selection rule.** How the winner is chosen, by code, from the ledger:
   "lowest mean CV rmse; ties within one standard error go to the simpler model".
9. **What would count as a negative result.** For example "no candidate beats
   the group-mean reference within stages". Write it down now.
10. **Steps.** Checkboxes for the work, updated as it goes.

## Changing the plan

Plans change when the work teaches something. When they do, add a dated line
under `## Changes`: what changed, why, and which runs were made under the old
rule. Never delete the old rule or its results.

## Output

`PLAN-<topic>.md`. Tell the person the question and the protocol in two
lines, and ask only about what you cannot decide (usually the practical
smallest difference that matters).
