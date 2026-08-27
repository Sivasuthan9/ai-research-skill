# Models, inductive bias, and representation claims

## An architecture is a hypothesis

Every architectural choice is an assertion about the structure of the problem: convolution
asserts translation-relevant locality; attention asserts that relevance is content-dependent
and long-range; recurrence asserts sequential dependence; a graph network asserts a known
relational structure; a normalisation layer asserts something about scale.

So when you choose or propose an architecture, state **what structural assumption it
encodes** and why the problem has that structure. "It is state of the art" is not an answer;
it explains the choice sociologically, not scientifically.

### Questions worth answering before you commit

- **Representational capacity.** What can this class express, and what can it not? Where a
  clean expressivity argument exists, it is cheap and often decisive.
- **Inductive bias.** Not what it *can* represent — what it finds *easy*. Under-constrained
  models converge to whichever sufficient solution is easiest to reach, which is why
  shortcuts win.
- **Effective capacity.** Parameter count is a poor proxy. Depth, width, normalisation,
  regularisation, data scale, and training length interact; two models with equal parameters
  can differ by an order of magnitude in what they actually fit.
- **Scale sensitivity.** Does the property you care about exist at the scale you can afford
  to study? If it emerges only at scale, say so and design accordingly; if it exists at small
  scale, study it there, thoroughly.
- **What your claim actually needs.** If your claim would hold for any function
  approximator, it is a claim about the objective or the data, not the architecture — a
  stronger and different claim. Say which one you are making.

---

## Comparing models fairly

Model comparison is where most unfair experiments live. The comparison is only informative if
the models were given **equivalent opportunity**.

- **Equal tuning budget.** Search budget, search space quality, and per-model tuning are part
  of the method. A comparison where you tuned yours and inherited theirs measures your
  compute, not your idea.
- **Nuisance parameters at their own optimum on every arm.** Learning rate, weight decay,
  batch size, and schedule optima differ by architecture. Fixing them to *your* optimum is a
  systematic bias in your favour, and it is a common, invisible one.
- **Matched budget along the axis you claim.** State whether you are matching parameters,
  FLOPs, wall-clock, memory, or training tokens. These give different rankings, and the
  choice must be justified by the claim ("cheaper at equal accuracy" vs. "better at equal
  parameters" are different papers).
- **Matched data and preprocessing**, unless data is the variable.
- **Same evaluation protocol**, including decoding, prompt format, normalisation, and metric
  implementation.
- **Never tune a baseline downward.** Leave it at its published configuration or better.
  Tuning a baseline to look weak is misconduct, not a shortcut.

If you cannot afford a fair comparison, run the comparison you can afford and **state the
asymmetry explicitly** rather than presenting it as fair.

---

## Representation claims need representation evidence

Claims of the form "the model learns / encodes / represents X" are among the most common and
least supported in the field. Accuracy on a downstream task is weak evidence for them.

To make such a claim you need to rule out the standard alternatives:

- **The probe learned it, not the model.** A sufficiently expressive probe can extract
  structure from nearly anything. Control with: a probe on random features, a probe on an
  untrained network of the same architecture, a probe on a shuffled-label version, and
  probe-complexity controls. Report probe capacity and selectivity, not just probe accuracy.
- **The information is in the input, trivially.** Compare against a probe on raw inputs or
  simple features. If those suffice, the model's representation is not the finding.
- **Decodability is not use.** That information is present does not mean the model uses it.
  Establishing use requires intervention: ablate, patch, or steer the representation and
  measure the behavioural consequence.
- **Correlation is not localisation.** A component correlating with a concept is not evidence
  that the component implements it.
- **The measure is doing the work.** Similarity and alignment measures between
  representations are sensitive to normalisation, dimensionality, and preprocessing, and
  different measures disagree. Report the choice, and check that your conclusion survives an
  alternative measure.

**Sanity-check any interpretability method before drawing conclusions from it.** The
canonical check: if the explanation is largely unchanged when the model's weights are
randomised, or when the labels are randomised, it is not explaining the model. Methods that
produce visually convincing output while being insensitive to the thing they claim to explain
are common. Run the randomisation controls first.

---

## Understanding a released model or implementation

For any model or codebase your analysis depends on, read the implementation, not just the
description. Recurring discrepancies that change what the correct experiment is:

- a component described as trained is actually **frozen** (or vice versa);
- the quantity **optimised** and the quantity **reported** are computed on different inputs
  or with different normalisation;
- an optimizer, buffer, or state is **reset** in a way that changes the effective update rule;
- a configuration flag silently **couples** two conditions everyone treats as independent;
- preprocessing, augmentation, or normalisation differs from the paper's description;
- the released checkpoint was trained with a recipe that differs from the released config.

None of these are visible from the abstract. Each can invalidate an entire analysis built on
the paper's description. Verify the load-bearing facts against the code, and record what you
verified.

---

## When your result is about the model at all

Before attributing anything to the architecture, rule out — in this order, because this is
where the base rate is:

1. a bug or preprocessing difference;
2. leakage or contamination;
3. the effective learning rate / schedule / initialisation;
4. tuning budget asymmetry;
5. seed variance;
6. evaluation protocol differences;
7. a nuisance hyperparameter interacting with the architecture change.

Only what survives all seven is a property of the model.
