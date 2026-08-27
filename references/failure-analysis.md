# Failure analysis

Two jobs: **diagnosing** an unexpected result, and **recognising** the recurring failure
patterns of AI/ML research before they cost you a project.

---

## Part 1 — Diagnosing an unexpected result

A surprising result is a claim that your model of the system is wrong somewhere. Find out
where before you interpret it.

### The base-rate ordering

Work down this list. It is ordered by how often each is the actual cause, and most surprising
results terminate in the first four.

1. **A bug.** In the data pipeline, the metric, the split, the loss, the evaluation harness,
   or the plotting. Check the metric implementation and the label mapping first.
2. **Leakage or contamination.** The information reached the model or the selection process
   (`data.md`).
3. **A configuration error.** Above all, the effective learning rate. A collapsed or diverged
   run is a mistuned run until a sweep says otherwise.
4. **Variance.** Compare against your measured variance floor before interpreting anything.
5. **Protocol mismatch.** Different preprocessing, prompt, decoding, normalisation, metric
   implementation, or harness version from the number you are comparing to.
6. **A shortcut.** The model found an easier sufficient cue than the intended one.
7. **A nuisance parameter.** The effect is an incidental knob, not the mechanism.
8. **A real finding.**

Reaching (8) requires having actually excluded (1)–(7), not having considered them.

### Making the diagnosis productive

- **Reproduce the surprise first.** A result you cannot reproduce is not yet a result.
- **Minimise it.** Find the smallest configuration that still exhibits the behaviour. This
  usually localises the cause by itself, and it makes every subsequent check cheap.
- **Bisect.** Between a working configuration and a failing one, change one thing at a time
  until the behaviour flips. The variable that flips it is the cause or is coupled to it.
- **Predict before you check.** Before running the diagnostic, write down what you expect
  under each candidate explanation. Otherwise you will find the result compatible with
  whatever you already believed.
- **Look at the data.** Individual examples, individual errors, individual outputs. Aggregate
  metrics hide mechanisms; raw examples reveal them.
- **Check the trivial explanation last only if you have checked it first.** A surprising
  result that turns out to be an off-by-one is embarrassing at week one and catastrophic at
  month six.

### A failure with a mechanism is a finding

A failure you can explain is a result. A failure you cannot explain is a to-do item. Write the
diagnosis while you still remember the context, and keep it — failures accumulate into
structure. Several methods failing for one shared reason, plus a measurement showing what that
reason is, is frequently a stronger contribution than any of the methods would have been.

---

## Part 2 — The recurring failure patterns

These are the ways AI/ML research goes wrong repeatedly, across subfields. Each is stated as
the mistake, why it is wrong, and the design that prevents it.

### Defending a result with an argument instead of a control

A quantity measured on a subset selected using a related quantity cannot be argued clean. The
statistic may not reference the selection criterion, but the *subset* does, and selection
correlated with the measurement can manufacture an effect from nothing.
**Prevention:** break the dependence by design — define the subset on one random half and
measure on the disjoint half — or report the result as confounded.

### Generalising from one condition

One dataset, one seed, one backbone, one scale is an anecdote. Results — including
*falsifications* — reverse on replication at adequate n across the diversity the claim covers.
Reporting a negative from a single small condition can discard a real finding.
**Prevention:** before reporting any verdict, replicate across independent conditions.

### Crediting a mechanism for a nuisance parameter's gain

Most apparent gains from a new component trace to an incidental hyperparameter whose optimum
shifted. **Prevention:** compare best-with-mechanism-off against best-with-mechanism-on,
letting every incidental parameter reach its own optimum on both sides
(`ablations-and-attribution.md`).

### Significance that does not replicate

With a large search grid across several datasets, a couple of results at p < 0.05 is close to
what multiplicity alone produces. A strong p-value on one condition can become nothing on an
independent one. **Prevention:** treat significance as provisional until it replicates on an
independent condition, and state the multiplicity exposure yourself.

### Mistaking a configuration error for a property

A model that collapses at a thoughtlessly chosen learning rate is not an unstable method.
Reported naively, this becomes a false claim about the method. **Prevention:** sweep the
parameter most likely responsible before concluding anything from a failed run, and report
configuration errors as configuration errors.

### Numerical precision corrupting a measurement

Reduced precision can cap a verification below the effect being tested; catastrophic
cancellation can corrupt an entire sweep; a theorem can appear to fail purely from dtype.
**Prevention:** when the number is the evidence, use double precision, disable
reduced-precision paths, use algebraically stable forms, and verify your verification.

### Reporting the best point instead of the sweep

A single favourable setting is not evidence, and a single unfavourable setting does not refute
you either. **Prevention:** report the whole sweep. Where a result contradicts your thesis,
tune the contradiction to its best case and report that; a claim that survives the opponent's
best configuration is far stronger than one made at an arbitrary point.

### Shortcut learning mistaken for capability

Models exploit the easiest sufficient cue. High i.i.d. accuracy is consistent with a decision
rule that has nothing to do with the intended one, and standard benchmarks reward this.
**Prevention:** decorrelate the suspected cue and re-measure; test under the shift your
mechanism says should matter; prefer the simplest explanation of a model's competence until
you have ruled it out.

### Underspecification mistaken for a robust pipeline

Models with identical held-out performance can encode entirely different decision rules and
diverge arbitrarily under deployment conditions. Seed alone can select among them.
**Prevention:** stress-test along the axes your application cares about; report behaviour
across seeds, not just its mean; do not treat equal validation accuracy as equal function.

### Test-set erosion

Every look at the evaluation set — for early stopping, architecture choice, prompt selection,
or "just checking" — converts it into a validation set. The erosion is gradual and invisible.
**Prevention:** fix the protocol in advance, count and report the number of evaluations, and
hold back a genuinely untouched split for the final claim.

### Explanation confused with speculation

Presenting a plausible after-the-fact mechanism as if it were established. It is the most
common form of overclaiming in the field because it costs nothing and reads well.
**Prevention:** label speculation as speculation, and convert it into a prediction that could
fail.

### Language that overclaims

Terms borrowed from human cognition ("understands", "reasons", "knows", "plans"), technical
terms stretched past their definitions, and suggestive names that do the arguing. These
mislead readers and eventually mislead the authors.
**Prevention:** describe what was measured. Reserve the loaded term for the claim you have
validity evidence for.

### The result that is really about compute

More parameters, more data, more steps, more search, or a better-tuned pipeline. Any of these
can produce your effect without your idea being involved.
**Prevention:** matched-budget controls along the axis you are not claiming.

---

## Keep the falsification log

Record every claim of yours that died, how it died, and what replaced it. Several entries in
that file is not embarrassing — it is the strongest available evidence that the surviving
claims were tested rather than assumed, and it is what a good reviewer looks for.
