# Decision log template

One entry per consequential decision, in a single file, so the reasoning chain of the project
is auditable end to end. A decision is consequential if it affects what can be concluded:
choice of data, model, objective, metric, baseline, control, protocol, or direction.

```markdown
## D<N> — <short title>                                   <date>

**Decision.** What are we about to do?

**Evidence.** What do we know right now? Label each item:
  [Known] external, retrieved and verified    [Observed] our measurement, artifact path
  [Derived] from stated assumptions, checked  [Hypothesised] not yet tested
  [Speculative] weakly supported              [Unknown] insufficient evidence

**First-principles.** Which of data / model / objective / optimizer / evaluation does this
change, and by what mechanism does that produce the expected effect?

**Alternatives considered.** What else could we have done, and why is this better for THIS
question? (Not: which is more convenient or more standard.)

**Hypothesis under test.** ...

**Competing explanation.** What else would produce the same outcome?

**Falsifier.** What result would make us change course?

**Scope implication.** What does this decision bound our eventual claim to?

---

**Reviewer's objection.** The single strongest criticism a hostile expert would make.

**Response.** Evidence, mathematics, or a new experiment. Not rhetoric.

**Resolution.** proceed / modify / stop — and what actually changed as a result.

**Unresolved.** Anything left open, to be carried into the limitations section.
```

## Notes on using it

- **Write it before acting**, not afterwards. A log written retrospectively records
  justifications, not reasons.
- **The Reviewer must sometimes win.** If no entry in the log ever ends in "modify" or "stop",
  the review step is decorative.
- **Record decisions not taken**, when the alternative was serious. Reviewers and future-you
  both ask "why not X?"
- **Link entries to experiments** (`E<N>`) and evidence-ledger rows, so a claim can be traced
  from the paper back to the reasoning that produced it.
- **When a decision is later reversed**, add a new entry that supersedes the old one rather
  than editing it. The reversal, with its reason, is evidence of rigour.
