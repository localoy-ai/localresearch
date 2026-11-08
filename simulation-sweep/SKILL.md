---
name: simulation-sweep
version: 0.1.0
publisher: localoy
capabilities: [files, shell]
description: Drive a numerical model across its parameter space — verify it first, sweep with a design that covers the space, find what drives the output (sensitivity), quantify uncertainty, and say where the model breaks — every run recorded. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, simulation, sensitivity, uncertainty, localresearch]
    related_skills: [research-plan, research-review]
allowed-tools:
  - Bash
  - Read
  - Write
triggers:
  - parameter sweep
  - run the simulation
  - sensitivity analysis
  - uncertainty quantification
  - monte carlo
  - where does the model break
tags: [simulation, sweep, sensitivity, uncertainty, monte-carlo]
---

## When to invoke this skill

When the work is a model of something (ODE/PDE, agent-based, Monte Carlo,
physics, a system or process model) and the question is how it behaves
across its parameters: what matters, how uncertain the output is, where it
stops being valid.

## How it runs

Every simulation run goes through the `experiment` tool: one `group` per
study, the parameter values in `params`, the seed in `seed`, and the outputs
of interest written by the simulation to `$RUN_DIR/metrics.json` (and time
series to `$RUN_DIR/curves.json`). Long runs print `RUN_PROGRESS done/total
step` as they go.

1. **Verify before sweeping.** Run the model against a case with a known
   answer: an analytic solution, a conserved quantity, a published result.
   Check convergence in the step size or mesh, and in the number of Monte
   Carlo samples. A sweep of an unverified model only maps its bugs.
2. **State the space.** Each parameter's range and why (physical limits, data,
   literature), and which outputs matter. Put it in `PLAN-<topic>.md`.
3. **Design the sweep.** Use Latin hypercube or Sobol sequences to cover many
   parameters, a grid only for one or two. Start coarse and refine where the
   output changes fast. Run points in parallel, as many as the machine holds.
4. **Sensitivity.** Start with one-at-a-time around a base point. Use Morris
   screening to rank many parameters cheaply, and Sobol indices (first order
   and total) for the important ones: which inputs drive the output, alone
   and through interactions.
5. **Uncertainty.** Propagate the inputs' uncertainty (distributions from
   data or literature) to the outputs. Report intervals, not single runs.
6. **Where it breaks.** Find the regions where the model fails to converge,
   leaves its valid regime, or disagrees with data. These are results too.

## Output

A `## Results` section in `PLAN-<topic>.md`:
- the verification cases and their errors
- the sensitivity ranking
- the output intervals
- the map of where the model holds and where it breaks

Every figure is cited to its runs. Run `claim_check` on it before it goes out.
