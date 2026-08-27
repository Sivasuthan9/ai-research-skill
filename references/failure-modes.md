# Failure modes — traps that actually occurred

Each entry below happened in a real project. Each cost time, or came close to producing a
wrong published claim. They are ordered by how badly they would have damaged the work.

---

## 1. Defending a result with an argument instead of a control

**What happened.** A central result measured a statistic on a *subset defined using a
related quantity*. The defence offered was: "this statistic never references that quantity,
so it cannot be circular."

**Why it was wrong.** The statistic didn't reference it — but the **subset did**. Members
of the subset were selected by a criterion correlated with the statistic, which can
manufacture the effect from nothing.

**The fix.** A split-half control: use one random half of the units to *define* the subset,
and measure the statistic on the disjoint half. No contamination can reach it.

**Outcome.** The result survived (controlled and in-sample agreed to within 0.030 across
all 12 runs) and the corrected headline was *stronger and simpler* than the original claim.

> **Rule.** When a subset is defined using quantity Q, you cannot argue your way to
> independence from Q. Design the dependence away, or report the result as confounded.

---

## 2. Generalising from one small dataset

**What happened.** A pre-registered prediction was tested on a single dataset at n = 800,
came out below chance, and was reported as falsified — twice, including to the user.

**What was actually true.** Replicated across six datasets at n ≈ 2000, the same statistic
came out *above* the incumbent on average, and clearly above chance on two datasets. It
then produced a significant accuracy gain (+2.75 pp, p < 0.0001) on one of them.

**The cost.** A genuine positive finding was nearly discarded — the one result that turned
a pure-negative paper into one with a deliverable.

> **Rule.** One dataset is an anecdote. Before reporting *any* verdict — especially a
> falsification — replicate at adequate n across the diversity your claim covers.

---

## 3. Every "gain" tracing to a hyperparameter

**What happened.** Several tuned methods showed gains. Each time, isolating the components
revealed the gain came entirely from one incidental hyperparameter (how many items were
retained), not from the proposed mechanism.

**The control that settles it.** Compare *best setting with the mechanism off* against
*best setting with it on*, letting the incidental parameter float to its optimum on **both**
sides. Any surviving difference is attributable to the mechanism.

**Outcome.** Applied to a later result, the mechanism survived (+0.42 pp, positive on 5/6),
which is what distinguished it from all the earlier illusory gains.

> **Rule.** Before crediting a mechanism, hold every incidental parameter at its own optimum
> on both sides of the comparison.

---

## 4. Significance that does not replicate across conditions

**What happened.** A tuned method reached significance on 2 of 6 datasets on one backbone.
On a second backbone: **0 of 6**, and the strongest result (p = 0.0009) became p = 1.0000.

**Interpretation.** With a large search grid across many datasets, a couple of p < 0.05
results is close to what multiplicity alone produces. Replication across an independent
condition — not the p-value — is what separates signal from selection.

> **Rule.** Treat significance as provisional until it replicates on an independent
> condition (another backbone, split, or domain). State the multiplicity exposure.

---

## 5. Mistaking a configuration error for a finding

**What happened.** A method was run with a learning rate chosen without thought. The model
collapsed catastrophically. Reported naively, this would have read as "the method is
unstable".

**What was true.** At sensible learning rates the method behaved normally. The collapse was
*the experimenter's choice*, not a property of the method.

> **Rule.** Before concluding anything from a failed run, sweep the parameter that most
> plausibly caused it. Report configuration errors as configuration errors.

---

## 6. Numerical precision silently corrupting a measurement

Three separate instances, all caught only by deliberate checking:

- **Catastrophic cancellation.** A formula of the form `(Σ x^q − 1)/(1 − q)` subtracts
  near-equal quantities as q → 1: **2.5% error** in float32. Fixed by an algebraically
  equivalent `expm1` form. This would have corrupted every result in a parameter sweep.
- **Reduced precision capping an exactness check.** A check intended to verify an identity
  read 1.9e-3 with TF32 enabled; with TF32 off it read **4.9e-6**. The measurement was of
  the arithmetic, not the mathematics.
- **A false negative from float32.** A theorem appeared to fail at 2e-6. In float64 the
  residual was **3.5e-15**. The theorem was correct; the dtype was not.

> **Rule.** When the *number itself* is the evidence — verifying an identity, a gradient, a
> bound — use float64 and disable reduced-precision matmul paths. Verify your verification.

---

## 7. Independent prediction as evidence beyond p-values

**What worked.** A finding rested on three p-values across six datasets, which multiplicity
could explain. What made it credible was different: an **independent measurement with no
method in the loop** predicted the per-dataset accuracy outcome at Spearman ρ = 0.943
(p = 0.0048).

> **Rule.** The strongest evidence is not a smaller p-value. It is an independent quantity
> predicting your outcome *in advance*, across conditions. Design for that.

---

## 8. Reporting the best point instead of the sweep

**What happened.** A result at one arbitrary setting appeared to contradict the thesis.
Rather than tune until it disappeared — which would be p-hacking — the parameter was swept
and **the entire curve reported**: 54 configurations across 4 datasets, 2 parameter groups,
9 learning rates spanning four orders of magnitude.

The opposing hypothesis was given its best possible shot and still failed in 0/54. That is
far stronger than a claim made at a single setting.

> **Rule.** When a result contradicts your thesis, tune it to give the *contradiction* its
> best chance. Report the sweep. If the contradiction survives, update your thesis.

---

## 9. Failures accumulating into a finding

Seven proposed methods were falsified. Individually: seven disappointments. Taken together
with a measurement explaining *why* none could work, they became a single structural result
that was more valuable than any of the methods would have been.

> **Rule.** Preserve and document every failure with its diagnosis. A robust, explained
> failure is a finding. Ask what the failures have in common — that is often the paper.
