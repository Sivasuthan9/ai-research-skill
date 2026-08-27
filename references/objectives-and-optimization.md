# Objectives, optimization, and tuning

## Write the objective down

In symbols, completely, including every term that is actually in the code: regularisers,
auxiliary losses and their weights, temperature, normalisation, stop-gradients, label
smoothing, masking, and the reduction (`mean` vs. `sum` changes the effective learning rate,
and per-token vs. per-sequence averaging changes what is being optimised).

Then ask the question that matters:

> **What behaviour is optimal under this objective?**

Not the intended behaviour — the optimal one. Wherever those differ, you have found either a
bug, a research object, or an explanation for something surprising. Models do not do what you
meant; they do what the loss rewards, using the cheapest available route.

### Objective ≠ metric ≠ construct

Three distinct things, routinely conflated:

- the **objective** you optimise (differentiable, computed on training data);
- the **metric** you report (often non-differentiable, computed elsewhere);
- the **construct** you claim to be studying (capability, robustness, quality, alignment).

Every gap between them is a place your conclusion can fail. State the chain explicitly, and
note where you are *assuming* rather than *demonstrating* that one stands for the next.

**Goodhart's law is a mechanism, not an aphorism.** A proxy is valid because of a correlation
observed on the pre-intervention distribution. Optimising against the proxy moves you off
that distribution, which is precisely the condition under which the correlation was
established. Expect proxy–construct divergence to grow with optimization pressure, and
measure it rather than assuming it away: hold out an independent measure of the construct
and watch whether it tracks the proxy as optimization proceeds.

This is the general form of reward hacking, benchmark overfitting, and metric gaming. It is
not a pathology of any one setup; it is what optimization does.

### Designing an objective

If you are proposing a loss, be able to answer:

- What does each term reward, and what is the optimal solution for each in isolation?
- What does it *fail* to penalise? Absence of a term is a design decision.
- What are its degenerate solutions, and what prevents them? (Collapse, trivial constants,
  shortcut features, degenerate scale.) Name the mechanism that rules each out — often it is
  an implementation detail like a stop-gradient or a normalisation, not the loss itself.
- Is it well-conditioned? Are terms on comparable scales, and does the weighting need to be
  tuned per-setting? A loss that requires careful per-dataset weighting is a weaker
  contribution than one that does not, and you should say so.
- Is it identifiable — could two very different solutions achieve the same loss? If so, the
  optimizer, not the objective, is choosing your model.

---

## Optimization is a confound before it is a contribution

An enormous fraction of reported architectural, objective, and algorithmic effects in deep
learning are optimization effects in disguise. Treat this as the default hypothesis.

**Effective learning rate.** After accounting for batch size, loss reduction, warmup,
normalisation layers, weight decay coupling, and gradient clipping, two runs nominally at the
same learning rate can be far apart. Most "this method is unstable" findings are a learning
rate chosen without thought. **Before concluding anything from a diverged or collapsed run,
sweep the parameter that most plausibly caused it.** A configuration error reported as a
property of a method is one of the most damaging mistakes available.

**Optimizer dynamics.** Adaptive methods, momentum, EMA, state resets, and clipping change
what the update rule *is*. Analyses that assume a plain gradient step may not describe the
system you are running. Read the implementation.

**Schedule and budget.** Comparisons at unequal training length, or with schedules tuned for
one arm, are not comparisons. A method that "converges faster" under a schedule shaped for it
has demonstrated nothing.

**Initialisation and seed.** Seed-to-seed variation in deep learning routinely exceeds the
size of published improvements. A single run cannot distinguish a real effect from a draw.

**Precision and numerics.** Reduced-precision matmul paths, mixed precision, and accumulation
order change results — and can cap the accuracy of a measurement below the effect you are
trying to detect. When the *number itself* is the evidence (verifying an identity, a
gradient, a bound, a conservation property), use double precision and disable reduced-precision
paths, then verify your verification. Watch for catastrophic cancellation in any expression
where two near-equal quantities are subtracted; find the algebraically stable form.

---

## Tuning as a scientific procedure

Tuning is not a chore before the experiment. It *is* part of the experiment, and it is where
most unfair comparisons are created.

### Separate the roles of hyperparameters

For each study, classify every hyperparameter:

- **Scientific** — the one whose effect you are measuring. Your independent variable.
- **Nuisance** — must be tuned *within each arm* so the comparison is fair. Anything whose
  optimum plausibly depends on the scientific variable is a nuisance parameter, not a fixed
  one. Learning rate is almost always nuisance.
- **Fixed** — held constant to bound cost. **Fixing a parameter narrows your conclusion to
  that value**, and the more it interacts with the scientific variable, the more damage
  fixing it does. State what you fixed and accept the corresponding scope limit.

The core rule follows directly: **to measure the effect of a scientific variable, let every
nuisance parameter reach its own optimum separately on each arm.** Anything less confounds
your variable with the tuning it happened to receive.

### Tuning protocol

- **Tune your own method fully.** Judging a proposal at one arbitrary setting is not a fair
  test of it either.
- **Give the baseline equal opportunity** — the same budget, a search space of comparable
  quality, and its published configuration as a floor. Never tune a baseline downward.
- **Select on validation data, report on test data.** Reporting on the split you tuned on
  buys pure overfitting, and with a large grid on finite data it can buy a great deal. Where
  data is scarce, use nested cross-validation or cross-fitting so every scored unit was
  evaluated under parameters selected without it.
- **Report the budget.** Number of trials, search space, search algorithm, and the selection
  criterion. Results at unreported budgets are uninterpretable — a method's performance is a
  function of search budget, and comparing single best trials across unequal budgets is
  meaningless.
- **State the multiplicity exposure.** If you evaluated hundreds of configurations across
  several datasets, a few results at p < 0.05 are close to what chance alone produces. Say so
  yourself, before a reviewer does.
- **Prefer exploration over exploitation while you are still learning.** Early studies should
  buy *structural insight* — which parameters matter, which interact, where the sensible
  ranges are — rather than the best validation number. The best trial is a by-product; the
  understanding is the point.
- **Read the training curves, not just the final metric.** Overfitting, instability,
  step-to-step variance, and premature convergence are visible there and invisible in a
  single number.
- **Re-run the chosen configuration.** Before adopting a change, retrain the winner across
  seeds. The best trial of a search is upward-biased by selection; that bias is often the
  entire reported improvement.

### Tuning against your own thesis

When a result contradicts your hypothesis, do **not** tune until it disappears — that is
p-hacking. Do the opposite: tune the contradiction to give it its *best possible chance*, and
report the whole sweep.

- If the contradiction survives its best configuration, your thesis is wrong. Update.
- If it evaporates across the entire sweep, your claim is now far stronger than one made at a
  single point.

Either way you learn something, and neither outcome required you to be lucky.

---

## Scale and scaling laws

If your claim concerns scale, the scaling study is the experiment and it deserves the same
rigour as any other.

- **Only scale may vary.** Architecture, optimizer, schedule, data composition, tokeniser,
  and preprocessing must be held fixed across the sweep, or you are fitting a law to a moving
  target. State exactly what was held constant.
- **Report the fitting procedure**: the functional form, the loss used to fit it, the
  optimiser and its settings, parameter uncertainty, and the sensitivity of the fit to those
  choices. Small differences in fitting propagate into enormous differences when extrapolated
  across orders of magnitude.
- **Report the range fitted and the range extrapolated**, separately. A law fitted on small
  models and extrapolated far beyond them is a conjecture; label it as one.
- **Check regional sensitivity**: refit on subranges and see whether the exponents move. If
  they do, the functional form is not capturing the phenomenon.
- **Triangulate.** A scaling conclusion supported by two or three methodologically
  independent approaches that agree is far more credible than one fit, however clean.
- **Beware precision artifacts**: rounding of reported losses, tolerance settings in the
  fitting routine, and averaging vs. summing in the fit objective have all produced published
  disagreements that turned out to be numerical rather than scientific.
