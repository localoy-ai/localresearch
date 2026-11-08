---
name: reproduce
version: 0.1.0
publisher: localoy
capabilities: [files, shell, web]
description: Run someone else's pipeline — a paper's code, a repo, a notebook — faithfully first, then honestly. Maps what it needs, builds its environment, fetches its data with checksums, patches only what blocks it (each patch recorded), runs it stage by stage as recorded experiments, and compares every number to what was claimed. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, reproducibility, replication, localresearch]
    related_skills: [data-audit, research-review, model-optimize]
allowed-tools:
  - Bash
  - Read
  - Write
  - WebFetch
triggers:
  - reproduce this
  - run this pipeline
  - run this repo
  - replicate the paper
  - does this code work
  - check their results
tags: [reproduce, replication, pipeline, repo, notebook]
---

## When to invoke this skill

Someone hands you a repository, a paper with code, or a notebook, and asks
whether it runs, what it really gets, or whether its claims hold. The answer
has two parts, kept apart:
- **Faithful run:** what their code gets with their settings, changed only
  where it cannot run at all.
- **Honest run:** what the same question gets under a sound protocol (see
  `data-audit`, `model-optimize`).

## How it runs

### 1. Map it
Run `environment` action inspect on the project. It reports what this
computer has, the project's manifests, entry points, notebooks, the packages
its code imports, **local modules it imports that are missing**, files its
code names that are not there, and **paths from another computer**. Read the
README and the paper's methods next to it.

Write a `## Reproduction` section in `PLAN-<topic>.md` with:
- the claims to check, each with the number as published and where it is
  published (README line, table, figure, notebook cell)
- the stages of the pipeline, in order, and the command for each
- what is missing: code, data, hard-coded paths, credentials, services
  (an LLM server, a database), hardware (a GPU)

### 2. Environment
Run `environment` action setup: the project's own `.venv` from its manifest.
Add what its imports need and the manifest forgot with action add, and
record each addition. Never install into a system Python.

### 3. Data
Find the data where the README, paper or code says it comes from. Fetch
each file with `dataset` action get, giving the checksum the source
publishes. Large downloads ask the person first. If the code expects
another shape or name (per-sample files, other column names), write a small
conversion script and run it as an experiment (group `prepare`), never by
hand.

### 4. Patch only what blocks
Work on the project as it is: a git branch or commit before any change, so
every patch is one diff.
- Rewrite paths from another computer to this project.
- A missing module or file the code cannot run without: say so. Rebuild it
  only when the paper describes it well enough, and mark the rebuilt part as
  yours in the plan.
- A service it calls (for example a local LLM): run without it only if the
  code has a documented fallback, and record that the fallback ran.
- Never change a setting, a split, a seed or a metric in the faithful run.
  Record every patch in the plan with its reason.

### 5. Run it, stage by stage
Each stage is an `experiment` (group `faithful`), with the claimed number
for that stage named in the run's notes. When their code prints numbers
instead of writing `metrics.json`, add a few lines that write the printed
values to `$RUN_DIR/metrics.json`, and record that patch. Notebooks run
headless (`jupyter nbconvert --to notebook --execute`, or papermill) as
experiments too.

### 6. Compare
For each claim, write a line: **reproduced** (within the run-to-run spread),
**differs** (by how much, and the likely reason), or **could not run** (what
was missing). Look for numbers the code never computes: literals in the code
or notebook that are printed as results, conclusions written into prompts or
fallbacks, checks that print PASS without testing. Report them plainly.

### 7. Then the honest run
When the faithful run is done, run the question again the sound way:
`data-audit`, then `model-optimize` (group `honest`), with fair baselines
and the evaluation the data demands. Put both side by side.

### 8. Review and write
Have someone who did not do the work review it (`research-review`). Run
`claim_check` on the report.

## Output

In `PLAN-<topic>.md`:
- `## Reproduction`: the claims, stages, patches, and the
  reproduced / differs / could not run table
- `## Honest run`

Tell the person in a few lines what ran, what held, what did not, and why.
