---
name: localresearch
version: 0.2.0
publisher: localoy
capabilities: []
description: Router for the localresearch skills — sends any research request (literature, study design, data, modeling, simulation, review, write-up) to the right skill, in the order research actually happens. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [router, research, localresearch]
    related_skills: [deep-research, research-plan, data-audit, model-optimize, simulation-sweep, reproduce, research-review, write-up]
allowed-tools:
  - Read
  - Skill
triggers:
  - localresearch
  - help me with research
  - research project
  - which research skill
tags: [router, research, pipeline]
---

## What each skill is for

| Need | Skill | Leaves |
|---|---|---|
| What is known, state of the art, prior work, datasets, open problems | `deep-research` | `research-<topic>.md` |
| Fix the question, protocol, metric and selection rule before results | `research-plan` | `PLAN-<topic>.md` |
| Check a dataset: rows, groups, clusters, leakage, references | `data-audit` | `## Data audit` in the PLAN |
| Train and tune the best model, honestly | `model-optimize` | runs in `runs/`, `## Results` in the PLAN |
| Sweep a simulation: verify, sensitivity, uncertainty, where it breaks | `simulation-sweep` | runs in `runs/`, `## Results` in the PLAN |
| Run someone else's repo, paper code or notebook, faithfully then honestly | `reproduce` | `## Reproduction` in the PLAN |
| Try to break a result before it goes out | `research-review` | `review-<topic>.md` |
| Write the report or paper | `write-up` | the report, claim-checked |

## The order research happens in

1. `deep-research`: know what is known before doing anything new.
2. `research-plan`: write the rules while nobody knows the answer.
3. `data-audit` before any model, when there is data.
4. `model-optimize` or `simulation-sweep`: the work, every run recorded.
5. `research-review`: by someone who did not do the work.
6. `write-up`: every number checked against the runs.

Steps can be skipped when the request is smaller (a literature question
needs only `deep-research`), but never reordered so that results come before
the plan.

## Shared ground rules

- Every computed number comes from a run recorded with the `experiment` tool,
  and every report passes `claim_check` before it goes out.
- Reported numbers from other work are cited as theirs.
- Null and negative results are results.
- Files are standard and live in the project folder: `PLAN-<topic>.md`,
  `research-<topic>.md`, `review-<topic>.md`, `runs/`.
