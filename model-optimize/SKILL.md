---
name: model-optimize
version: 0.1.1
publisher: localoy
capabilities: [files, shell]
description: Get the best real performance out of a model under a fixed compute budget, and prove the gain — strongest baseline first, data and loss before knobs, curve diagnosis, Bayesian search with early pruning across parallel trials, proper training, honest ensembling, ablations, seeds and nested CV with an equal budget for every candidate. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, machine-learning, hyperparameters, training, localresearch]
    related_skills: [research-plan, data-audit, research-review]
allowed-tools:
  - Bash
  - Read
  - Write
triggers:
  - optimize the model
  - tune hyperparameters
  - improve accuracy
  - train the best model
  - hyperparameter search
  - beat the baseline
tags: [ml, training, tuning, hpo, optimization]
---

## When to invoke this skill

When a model has to be as good as it can be, and the claim that it is must
survive a reviewer. It follows the protocol in `PLAN-<topic>.md`; if there
is no plan, write one first (`research-plan`).

## Every trial is a recorded experiment

Run every trial with the `experiment` tool: one `group` per search, `goal`
set from the plan (`rmse:min`), the trial's settings in `params`, and its
seed in `seed`. The training script writes measurements only (scores,
losses, times) to `$RUN_DIR/metrics.json` and learning curves to
`$RUN_DIR/curves.json`. The ledger is the leaderboard, and the person can
watch it in Analytics.

## The method, in order

1. **Strongest simple baseline, tuned properly.** Linear or PLS models with
   their key setting searched over its full range (not capped), gradient
   boosting, a small MLP: whichever the field uses. Plus the trivial
   references from the data audit. A gain over a weak baseline is no gain.
2. **Data and loss before knobs.** Fix leakage, scaling and class balance
   first. Use a loss that matches the metric, and the augmentation or
   preprocessing the field trusts.
3. **Read the curves.** Train against validation curves for underfitting,
   overfitting, instability and dead capacity. Run a learning-rate range test
   before searching the learning rate. Watch gradient norms.
4. **Search smart.** Bayesian / TPE search (Optuna or similar) with early
   pruning (successive halving, ASHA, Hyperband) over the settings that
   matter: learning rate and schedule, batch size, weight decay, dropout,
   width and depth, optimizer, and architecture choices. Do a quick wide pass
   first, then search deeper only where it shows room. Run trials in
   parallel: several small trials at once when one does not fill the
   machine; one agent per slice of the space when leading a team.
5. **Train well.** Use warmup with a cosine or one-cycle schedule, gradient
   clipping, EMA or stochastic weight averaging, mixed precision where it is
   safe, checkpoints, and a restart from the best point after a collapse.
   Use the device the run is given (`$RUN_DEVICE`: mps, cuda or cpu) and its
   share of the cores (`$RUN_THREADS`; set `torch.set_num_threads` from it).
6. **The last bit, honestly.** Ensembles across seeds and models, stacking,
   test-time augmentation where it fits, and distillation, pruning or
   quantization when deployment needs them, with the accuracy cost measured.
7. **Ablate.** Remove each added piece in turn. Keep only what pays for itself
   beyond seed noise.
8. **Prove it.**
   - several seeds, mean ± spread
   - nested cross-validation, or a clean three-way split: the data that tuned
     never scores
   - the same tuning budget for every candidate
   - the held-out test opened once
   - the gain over the best baseline with an interval and an effect size;
     several comparisons corrected

## When the data has clusters

If the data audit found clusters (stages, sites, batches), report within
each cluster and leave-one-cluster-out next to the pooled score. A model
that wins pooled but loses within clusters has learned the clusters.

## Output

The winner chosen by the plan's selection rule from the ledger
(`experiment` action list), and a `## Results` section in
`PLAN-<topic>.md`: the leaderboard's top rows, the baseline and references,
the ablations, and the honest size of the gain. Run `claim_check` on it
before it goes out.
