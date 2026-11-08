---
name: deep-research
version: 0.1.0
publisher: localoy
capabilities: [files, web, browser, shell]
description: Deep research on any question — splits it into sub-questions, runs searchers in parallel across papers, datasets, code and the open web, reads the primary sources, and writes a cited synthesis with what is known, what conflicts and what is still open. Never a summary of search snippets. (localresearch)
author: localoy
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [research, literature, synthesis, localresearch]
    related_skills: [research-plan, research-review, write-up]
allowed-tools:
  - Bash
  - Read
  - Write
  - WebSearch
  - WebFetch
triggers:
  - deep research
  - research this
  - what does the literature say
  - state of the art
  - prior work on
  - survey the field
  - find papers on
tags: [research, literature, survey, sources, citations]
---

## When to invoke this skill

When a question needs more than a search: what is known about a topic, the
state of the art, how methods compare, which datasets and code exist, what
is disputed and what nobody has answered yet. The result is a written
synthesis, `research-<topic>.md` in the project folder, where every claim
names the source it came from.

A **topic** is a short lowercase slug for the question: `mushroom-dmc-nir`,
`battery-soh-transfer`.

## The standard

- **Primary sources, read.** A paper's numbers come from the paper (its
  abstract, results, tables), not from a blog about it or a search snippet.
  Open it (web page or PDF) before citing it.
- **Every claim cited.** Each statement in the synthesis names its source:
  `[Author Year](link)` or `[dataset](link)`. A claim nobody supports is
  marked as yours ("my reading") or removed.
- **Reported, not measured.** A number from a paper is *reported by* that
  paper. Never present it as something you measured. Numbers you compute go
  through the `experiment` tool and the run ledger.
- **Quote sparingly.** Short quotes (under 15 words) only where the exact
  wording matters. Otherwise paraphrase and cite.
- **Conflicts kept, not smoothed.** When sources disagree, say who says what
  and why they might differ (data, protocol, metric, leakage).
- **Saturation, not a page count.** Stop when new searches return sources you
  already have and the open questions are named, not after N links.

## How it runs

### 1. Frame
Write the question, its scope (fields, years, populations, materials) and
what a useful answer looks like in a `## Question` section of
`research-<topic>.md`. Split it into 3–8 **sub-questions** that can be
searched independently: methods, datasets, benchmarks and their numbers,
known pitfalls, competing explanations, open problems.

### 2. Search wide, in parallel
Each sub-question is its own search thread. When you lead other agents (a
chief), give each thread to its own agent at once: a swarm of searchers,
each with this skill, one sub-question each, and a shared folder for their
notes. Alone, work through them one by one.

For each thread, cover several kinds of source, in the field's own words:
- **papers:** scholarly search, arXiv, PubMed, Semantic Scholar, publisher
  pages, and the reference lists and "cited by" of the best papers you find
  (one hop backward and forward)
- **data:** repositories such as Zenodo, Figshare, Kaggle, Hugging Face, and
  data papers
- **code:** repositories that implement the methods or the benchmarks
- **standards and reviews:** survey papers, standards bodies, textbooks

Search in rounds. After each round, write down what you found and what is
still missing, then search the gaps with new words: synonyms, the method's
other names, the field's jargon, the names of the leading groups.

### 3. Read and extract
For every source worth keeping, add a row to an evidence table in
`research-<topic>.md`, `## Evidence`:

| Source | Kind | What it shows | Numbers (as reported) | Data / protocol | Caveats |
|---|---|---|---|---|---|

The **protocol** column decides how much a number is worth: dataset, split,
validation scheme, whether the test set was untouched, whether groups or
time could leak. A great number on a leaky protocol is a weak number.

### 4. Synthesize
Write the answer for a reader who knows the field a little:
- **What is known.** The claims several good sources agree on, with them cited.
- **How the methods compare.** On the same data and protocol only. Never rank
  numbers from different datasets or splits as if they were comparable.
- **What conflicts,** and the likely reasons.
- **What is open.** Questions no source answers, or answers only on weak
  protocols: these are where new work can matter.
- **What to do next.** Concrete next studies or experiments, each tied to an
  open question.

### 5. Check before it goes out
- Every number in the synthesis appears in the evidence table, with its source.
- Numbers you computed yourself are in the run ledger: run `claim_check` on
  the file, and fix every number it flags.
- When you lead agents, have one that did not do the searching read the
  synthesis against the sources (`research-review`).

## Output

`research-<topic>.md` with sections **Question**, **Sub-questions**,
**Evidence** (the table), **Synthesis**, **Open questions**, **Next**,
**Sources** (every source, with a link and the date read). Tell the person
in one short message what the answer is, how strong the evidence is, and
where the file is.
