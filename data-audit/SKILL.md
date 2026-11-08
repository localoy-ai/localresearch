---
name: data-audit
version: 0.1.0
publisher: localoy
capabilities: [files, shell]
description: Audit a dataset before anyone models it — what a row is, groups and repeats, time and batch, clusters that inflate pooled scores, duplicates, leakage paths, missingness and target distribution — and say which split and which evaluation the data demands. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, data, leakage, audit, localresearch]
    related_skills: [research-plan, model-optimize]
allowed-tools:
  - Bash
  - Read
  - Write
triggers:
  - audit this data
  - check for leakage
  - is this dataset ok
  - profile the data
  - data quality
tags: [research, data, leakage, audit, eda]
---

## When to invoke this skill

First, on any dataset that will be modeled or tested. Most results that turn
out wrong were decided here: a split that let the same subject into train
and test, a pooled score inflated by clusters, a target that leaked through a
feature. The audit is cheap; the mistakes it prevents are not.

## How it runs

Every number in the audit is computed by a script run through the
`experiment` tool (group `audit`), so it lands in the run ledger and can be
cited. Write the script into the project folder; it writes its figures as
JSON to `$RUN_DIR/metrics.json`.

1. **What one row is.** One subject, one measurement, one time point? Find the
   identifier columns. Count rows per identifier: more than one means repeats,
   and repeats must stay on one side of every split.
2. **Structure.** Look for groups (subject, site, batch, plate, field), order
   (time, sequence), and clusters in the target (stages, classes, regimes).
   For clusters, compute the target's spread *between* and *within* them. When
   most of the variance is between clusters, a pooled R² mostly measures
   whether the model recognises the cluster: say so, and require within-cluster
   and leave-one-cluster-out scores.
3. **Trivial references.** The score of predicting the mean, and the score of
   predicting each cluster's or group's mean (out of fold). Any model must
   beat these to mean anything.
4. **Leakage paths.** Features computed from the target or from the future,
   identifiers that encode the label, preprocessing fitted on all rows, exact
   or near duplicates across splits, test rows used for tuning.
5. **Quality.** Missing values and their pattern, constant or near-constant
   features, outliers and whether they are errors, units and ranges.

## Output

A `## Data audit` section in `PLAN-<topic>.md` (start a short PLAN if there
is none):
- what one row is, the identifiers, the structure found
- the trivial references' scores, cited to their runs
- the leakage risks found, and how the protocol avoids each one
- **the split and evaluation the data demands**, in one line, and why

Then run `claim_check` on the section. Tell the person the one thing that
most changes how results must be read.
