# Hypotheses, mechanisms, predictions, falsifiers

## A hypothesis is a claim about a mechanism

Not "method M will improve accuracy." That is a hope with a number attached. A hypothesis
names **what in the system produces the effect**:

> *Because the objective rewards P, and the data contains spurious cue C which is cheaper to
> compute than the intended feature, the model will rely on C; therefore performance will
> collapse on inputs where C is decorrelated from the label, while remaining unchanged
> i.i.d.*

That form is useful because it is committed. It says what will happen, what will *not*
happen, and where to look for a contradiction.

### The components

| Component | The test |
|---|---|
| **Mechanism** | stated in terms of data / model / objective / optimizer / evaluation — not in terms of the method's name |
| **Assumptions** | what must hold for the mechanism to operate; each one is a place the hypothesis can fail |
| **Scope** | the architectures, scales, data regimes, and task families the claim covers |
| **Prediction** | signed, ideally sized, derived from the mechanism |
| **Forbidden outcome** | what the mechanism says *cannot* happen |
| **Alternatives** | the competing explanations that would produce the same observation |
| **Falsifier** | the specific result that kills or narrows it |

If the mechanism forbids nothing, it explains nothing.

---

## Always generate competing hypotheses

One hypothesis plus a confirming experiment is the standard way to be wrong for years.
Generate several — ideally including the boring ones, because the boring ones are usually
right:

- **The mechanism you propose.**
- **A nuisance-parameter account**: the effect comes from an incidental knob (capacity,
  regularisation strength, effective learning rate, number of retained items, sequence
  length, tokenisation) rather than the mechanism.
- **A data account**: leakage, contamination, a shortcut, a distribution artifact, or a
  label-generation quirk.
- **A variance account**: the effect is within seed-to-seed noise.
- **An evaluation account**: the metric, prompt format, decoding, normalisation, or harness
  produced the difference, not the model.
- **A tuning-budget account**: your method received more search than the baseline.
- **A bug.**

Then design experiments that **discriminate among these**, not experiments that are
consistent with your favourite. An experiment whose result is compatible with every
hypothesis on the list has told you nothing, however expensive it was.

---

## Predictions must be risky

**Sign every prediction.** A prediction with a direction can be wrong; one without cannot,
and is therefore not a prediction.

**Size it where you can.** "Accuracy improves" is weak. "Improves by 1–3 points on the shifted
split while unchanged i.i.d." is strong: it can fail in three distinguishable ways.

**Prefer predictions that are surprising under the alternatives.** The value of a prediction
is roughly how unlikely it is if your hypothesis is false. A prediction that every competing
account also makes is worthless as evidence, even if it comes true.

**Predict the shape, not just the endpoint.** Curves, orderings, interactions, and scaling
trends are far harder to satisfy by accident than a single number. Predicting that an effect
grows with scale, reverses past a threshold, or is monotone in a nuisance parameter is a much
sharper test than predicting that it exists.

**Predict something outside the training loop.** The strongest evidence available is an
*independent* measurement — one with no method in the loop — that predicts your outcome
across conditions in advance. Design for that when you can; it is more persuasive than any
p-value.

---

## Falsifiers

Write the falsifier before running, in quantitative terms, and commit to the consequence:

```
Falsifier:  if the controlled effect is within noise of the null on ≥ 4 of 6 conditions,
            the mechanism account is abandoned.
Then:       report the negative as the primary result and redirect compute to <X>.
```

Two properties make this work:

1. **It is fixed in advance**, so a later pivot is a scientific decision rather than a
   rationalisation.
2. **It names the alternative outcome as also reportable**, which removes the incentive to
   see the result one way.

Common failures to avoid:

- **Unfalsifiable framing.** "The method helps in some settings" cannot be wrong.
- **Moving the falsifier after seeing data.** If you must revise it, say so explicitly and
  treat everything downstream as exploratory.
- **Treating an underpowered null as a falsification.** "No measurable effect at this n" is
  not "no effect." Check whether your design could have detected the effect you predicted;
  if not, the run was uninformative, not negative.
- **Treating one condition as decisive.** A single dataset, seed, or scale is an anecdote.
  Falsify across the diversity your claim covers.

---

## Updating

- Evidence consistent with the prediction → increase confidence, **proportionally to how
  risky the prediction was**. A confirmed safe prediction is weak evidence.
- Evidence against → decrease confidence. Then check whether the mechanism can be *narrowed*
  honestly (a real scope condition) or is merely being rescued (an unprincipled epicycle).
  The test: does the narrowing make a new risky prediction? If not, it is a rescue.
- Contradiction across conditions → the mechanism is incomplete. Find the variable that
  differs; that variable is now the interesting object.
- **Never rationalise away negative evidence.** Record it, diagnose it, and let it change the
  plan.

---

## Exploration is allowed — label it

Not all work is confirmatory, and pretending otherwise produces the worst failure mode of
all: exploratory analysis reported as if it were pre-registered.

Run exploratory phases deliberately and mark them as such. Their output is **hypotheses, not
conclusions**. A pattern found by exploration must be confirmed on data that was not used to
find it, under a prediction written before that confirmation runs. Say which results are
which in the write-up.
