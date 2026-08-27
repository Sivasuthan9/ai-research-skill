# Reporting and record-keeping

## Every result is permanently logged

```
results/
  raw/         untouched experiment outputs, namespaced by every condition
  processed/   consolidated summaries used to build figures and tables
  figures/     publication-ready .png + .pdf, each with a sibling .md
  logs/        stdout/stderr of every run, including failures
  README.md    index of every experiment: id, script, question, verdict
  FALSIFICATIONS.md   running log of claims that died, and how
```

## Every figure carries a sibling document

For each figure, a `.md` beside it recording:

1. the exact image filename and the script that produced it;
2. the raw data paths and logs it came from;
3. **what** the plot shows (axes, units, what a point is);
4. **why** it was made — the question it answers;
5. the numbers, as a table, so the figure is auditable without re-reading pixels;
6. what the result **means**;
7. the **justified conclusion** — and only what the data supports;
8. **limits**, including negative and inconclusive findings.

Add a `SUPERSEDED` header rather than silently editing when a later experiment revises a
figure's interpretation. The revision history is itself evidence of rigour.

## Record failures with their diagnosis

A failure with a mechanism is a finding. A failure without one is a to-do. Write the
diagnosis while you still remember the context.

Keep a **falsification log** of your own claims: what you claimed, how it died, and what
replaced it. Six or seven entries in that file is not embarrassing — it is the strongest
available evidence that the surviving claims were tested rather than assumed.

## Communicating status

State, in this order:

- what we **know** (with evidential tier);
- what we do **not** know;
- what we **hypothesise**;
- why the next step is scientifically justified;
- what would **falsify** it;
- what the Professor objects to;
- what decision follows.

## Correcting yourself

When you find your own error, correct it plainly and immediately, in one or two sentences,
and continue. Do not bury it, do not soften it into ambiguity, and do not over-apologise.

If a correction makes the result *stronger* — which happens more often than expected — say
that too. Two examples from one project: an invalid defence was replaced by a control that
confirmed the result with a cleaner headline; and a wrongly-declared falsification, on
replication, became the project's only positive deliverable.

## Language discipline

- "measured at chance" ≠ "proved impossible";
- "no measurable effect at n = 4" ≠ "no effect";
- "significant on 2 of 12, neither replicating" ≠ "significant";
- "we did not run X" is a sentence you must be willing to write.

Bound every claim by the conditions actually tested: number of datasets, architectures,
seeds, and the class of methods the conclusion covers.
