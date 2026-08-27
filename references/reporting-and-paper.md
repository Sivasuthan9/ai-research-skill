# Reporting, record-keeping, and writing the paper

## Every result is permanently logged

A layout that keeps evidence auditable — adapt the names, keep the properties:

```
results/
  raw/          untouched experiment outputs, namespaced by every condition
  processed/    consolidated summaries used to build figures and tables
  figures/      each figure with a sibling result card
  logs/         stdout/stderr of every run, including failures
  README.md     index of every experiment: id, script, question, verdict
  EVIDENCE.md   the evidence ledger
  FALSIFICATIONS.md  running log of claims that died, and how
```

Properties that matter more than the structure:

- raw outputs are never edited in place;
- every artifact is traceable to a code version, config, and seed;
- figures and tables are **generated programmatically from raw outputs**, so the document
  cannot drift from the data;
- failures are logged with the same care as successes.

---

## Every figure and table carries a result card

Beside each one, a short document (`templates/result-card.md`) recording:

1. the exact filename and the script that produced it;
2. the raw data paths and logs it came from;
3. **what** it shows — axes, units, what a point is, what the error bars are over;
4. **why** it was made — the question it answers and the experiment it belongs to;
5. the numbers, as a table, so the figure is auditable without re-reading pixels;
6. what the result **means**;
7. the **justified conclusion** — only what the data supports;
8. **limits**, including negative and inconclusive findings.

When a later experiment revises a figure's interpretation, add a `SUPERSEDED` header rather
than silently editing. The revision history is itself evidence of rigour.

---

## Record failures with their diagnosis

A failure with a mechanism is a finding; a failure without one is a to-do item. Write the
diagnosis while you still have the context.

Keep a **falsification log** of your own claims: what you claimed, how it died, what replaced
it. Several entries there is not embarrassing — it is the strongest available evidence that
the surviving claims were tested rather than assumed.

---

## Communicating status mid-project

State, in this order:

- what we **know**, with evidential labels;
- what we do **not** know;
- what we **hypothesise**;
- why the next step is scientifically justified;
- what would **falsify** it;
- what the Skeptical Reviewer objects to;
- what decision follows.

---

## Writing the paper

### Claim → evidence mapping, before prose

List every claim the paper will make. For each: the evidence that supports it, its scope, and
the alternative explanations that were eliminated. Any claim without a row is deleted or
downgraded. Do this before writing; it is much cheaper than discovering the gap in the
conclusion.

Then check the reverse direction: every experiment you ran should appear, including the ones
that did not work. Experiments that exist but appear nowhere are the ones reviewers ask about.

### Structure that follows the science

- **Abstract and introduction** state the phenomenon, the question, what was found, and the
  scope — in that order. Resist stating the method before the problem it answers.
- **The contribution list is a promise.** Each item must be delivered by a specific section
  with specific evidence. Do not list "we propose" items that are really "we tried".
- **Related work** engages the strongest competing account rather than listing citations. Say
  what each establishes and where your work differs — including where prior work anticipated
  part of yours.
- **Method** is stated so it could be reimplemented, with the design decisions justified by
  the mechanism rather than by "we found it worked better."
- **Experiments** state the question each one answers before its results. Report protocol,
  budget, seeds, uncertainty, and the controls, in the main text where they are load-bearing.
- **Results** report full sweeps and distributions, not best points. Include the negative and
  inconclusive results.
- **Limitations** are specific and consequential. A limitations section listing only things
  that do not threaten the claim is worse than none, because reviewers read it as evasion.

### Scoping every claim

Bound each claim by exactly what was tested: datasets, scales, architectures, seeds, data
regimes, and the class of methods covered. If deleting the bounds would leave the sentence
intact, the sentence is overclaiming (`evidence-and-honesty.md` has the language
distinctions).

Include an **explicit non-claims** list where the result invites over-reading. "We do not
claim X; we do not claim no method can do Y — we claim methods of class Z cannot, under these
assumptions, and we measured that." It pre-empts the most damaging objections and costs
nothing but honesty.

### The reproducibility statement

Say what is released — code, configs, seeds, data preparation, split definitions, environment,
evaluation code, exact prompts, raw outputs, analysis scripts — and what is not, and why.
Report the compute used. State how many seeds, what the error bars are over, and how many
configurations were evaluated in total.

### Before submission

- Every number in the paper traced to an artifact and re-read from it, not from memory.
- Every citation retrieved and its identifiers verified.
- Every placeholder marker gone.
- Every claim bounded by the conditions tested.
- Every figure regenerated from raw outputs by a script that runs end to end.
- The negative results included.
- The tuning budgets and multiplicity exposure stated.
- The hostile review run: *what experiment would most likely disprove this paper?* — and then
  that experiment actually run, with the outcome reported either way.

---

## Correcting yourself in writing

When you find your own error, correct it plainly and immediately, in one or two sentences,
and continue. Do not bury it, do not soften it into ambiguity, do not over-apologise. Then
check what else depended on it.

If a correction makes the result *stronger* — which happens more often than expected — say
that too.
