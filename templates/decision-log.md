# Decision log template

One entry per consequential decision. Keep them in a single file so the reasoning chain of
the project is auditable end to end.

```markdown
## D<N> — <short title>          <date>

**Decision.** What are we about to do?

**Evidence.** What do we know right now? Tag each item:
  [Known] external, verified   [Observed] our measurement
  [Derived] from assumptions   [Hypothesised] not yet tested

**Reason.** Why does this evidence justify this action, rather than an alternative?

**Hypothesis under test.** ...

**Competing explanation.** What else could produce the same outcome?

**Falsifier.** What result would make us change course?

---

**Professor's objection.** The single strongest criticism a hostile expert would make.

**Response.** Evidence, mathematics, or a new experiment. Not rhetoric.

**Resolution.** proceed / modify / stop — and what changed as a result.

**Unresolved.** Anything left open, to be carried into the limitations section.
```

## Worked example

```markdown
## D12 — treat the confidence collapse as a real result   2026-08-26

**Decision.** Report that view confidence is uninformative on reducible errors.

**Evidence.**
  [Observed] global AUC 0.76-0.87; on reducible errors ~0.50, six datasets
  [Observed] the pattern explains all seven prior method failures

**Reason.** One measurement accounts for seven independent negative results.

**Competing explanation.** The subset is defined using the marginal; conditioning on
"the majority is wrong" may manufacture a sub-chance AUC on its own.

**Falsifier.** If a control that breaks the dependence returns the global value, the
result is an artifact and must be retracted.

---

**Professor's objection.** "Your defence — that entropy never references the marginal —
is invalid. The SUBSET references it. You have not controlled for this; you have argued
around it."

**Response.** The objection is correct and the defence was wrong. Built a split-half
control: half A alone defines the subset, AUC measured on the disjoint half B.

**Resolution.** Control run. Controlled and in-sample agree to within 0.030 on all 12
runs. Result stands, and the headline is revised from "confidence inverts" to
"confidence is at chance" — simpler and better supported than the original claim.

**Unresolved.** Bound covers per-view statistics of the model's response only; the
augmentation parameters remain untested. -> became R29/R31.
```
