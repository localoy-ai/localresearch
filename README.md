# localresearch

Research skills for agents: deep research, study design, data audits,
model optimization, simulation sweeps, adversarial review and write-ups. One
rule runs through all of them: **every number comes from a recorded run, and
every report is checked against the runs before it goes out.**

| Skill | What it does |
|---|---|
| `deep-research` | Splits a question into sub-questions, searches papers, data, code and the web in parallel, reads the primary sources, writes a cited synthesis with what is known, what conflicts and what is open |
| `research-plan` | Fixes the question, protocol, metric, search budget and selection rule before any result exists |
| `data-audit` | What a row is, groups and repeats, clusters that inflate pooled scores, leakage paths, trivial references |
| `model-optimize` | Strongest baseline first, smart search with pruning across parallel trials, proper training, ablations, seeds and nested CV with an equal budget for every candidate |
| `simulation-sweep` | Verify first, cover the parameter space, sensitivity (Morris, Sobol), uncertainty, where the model breaks |
| `reproduce` | Runs someone else's pipeline: maps what it needs, builds its environment, fetches its data with checksums, patches only what blocks it, runs it stage by stage, compares every number to the claim, then runs it the sound way |
| `research-review` | An adversary who did not do the work reruns it and attacks the split, baselines, statistics and claims |
| `write-up` | The report or paper from the plan, the runs and the review, every number claim-checked |
| `localresearch` | Router: the right skill, in the order research happens |

## Install

In Localoy:

```bash
localoy add localoy-ai/localresearch
```

Each skill is a folder with a `SKILL.md`, so any agent that reads skill
folders can use them.

The skills expect these tools: `experiment` (runs a command and records it
with its code, data and environment fingerprints and the metrics it writes),
`claim_check` (matches every number in a report to a recorded run),
`environment` (inspects what a project needs and builds its own isolated
environment) and `dataset` (fetches data with checksums and a provenance
manifest). Localoy Research has all four. Elsewhere, the same discipline works by hand:
keep a `runs/` folder with one record per run, and check every reported
number against it.

## Files the skills leave

Standard files in the project folder, so any agent or person can pick up the
work:
- `PLAN-<topic>.md`: the plan, the data audit, the results
- `research-<topic>.md`: the cited synthesis
- `review-<topic>.md`: the review
- `runs/`: one folder per recorded run

## License

MIT
