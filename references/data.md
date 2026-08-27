# Data as a scientific object

Data is not infrastructure. It is the evidence base, and every property of it is a property
of your conclusion. More AI/ML results have been invalidated by data problems than by
modelling ones.

Choosing a dataset because it is downloadable, popular, or already on disk is not a
scientific decision. **If no defensible dataset exists for your question, say so and stop.**
"I could not find a scientifically appropriate dataset — here is what I looked for and what
is missing" is a legitimate and useful output. Quietly substituting a weak one is not.

---

## Interrogating a dataset before using it

Reason about each; drop the ones your question makes irrelevant, and say why.

**Provenance and generation.** Who collected it, from where, when, under what selection?
What was filtered out, and by what rule? Filtering is a transformation with consequences —
deduplication, quality classifiers, safety filters, and length cutoffs all reshape the
distribution.

**What the label is.** Human annotation (with what agreement?), a proxy outcome, an
automated heuristic, a model's output, or true ground truth. These support very different
claims. A model-generated label caps your conclusion at "agrees with that model."

**Construct match.** Does the task as operationalised measure the thing your question is
about? A benchmark named after a capability is not evidence that it measures that
capability.

**Known biases and shortcuts.** What is predictive but not causal? Backgrounds, artifacts,
annotation templates, answer-position statistics, lexical overlap, formatting, source
distribution. Assume shortcuts exist until you have checked; models exploit the *easiest*
sufficient cue, not the intended one.

**Statistical adequacy.** Is n large enough to resolve the effect you expect? Compute this
before running, not after. An underpowered dataset makes both positive and negative results
uninterpretable.

**The standard protocol.** What splits, preprocessing, and metric does prior work use? Match
it exactly for comparability, or deviate deliberately and state the deviation. Silent
protocol drift makes your numbers incomparable to every number you cite.

**Legitimate scope.** Could your conclusion generalise from this data at all? Name the
population your claim covers.

---

## Splits and leakage

Leakage — information about the evaluation target reaching the model or the model-selection
process — is the single most common cause of irreproducible ML results, and it is usually
invisible in the paper. Reason through each of these categories:

**No independent test set at all**
- No held-out split; performance reported on training data.
- The test set used repeatedly for model selection, early stopping, or architecture choice.
  A test set consulted many times is a validation set, and reporting it as a test set
  overstates performance.

**Training-time contamination**
- Preprocessing fit on the full dataset: normalisation statistics, vocabulary, imputation,
  feature selection, dimensionality reduction, resampling. All of these must be fit on
  training folds only.
- Duplicates or near-duplicates spanning the split — extremely common in scraped corpora and
  image datasets. Deduplicate *before* splitting, and check near-duplicates, not just exact.
- Grouped data split at the wrong granularity: multiple records from the same patient,
  speaker, document, user, image, or session on both sides. Split by the group, not the row.
- Temporal leakage: random splits on time-ordered data let the model see the future. Split by
  time, and check that features do not encode future information.

**Feature leakage**
- A feature that is a proxy for the label, or that would be unavailable at prediction time.
- A feature causally downstream of the outcome.
- Selection on the outcome: the sampling of examples depends on the target.

**Evaluation-population leakage**
- Test distribution not matching the population the claim is about.
- Train/test drawn under different conditions so that a trivial cue separates them.

**Pretraining contamination (foundation models)**
- The benchmark may be in the pretraining corpus. This is not hypothetical; it is the default
  assumption for any public benchmark predating the model.
- Check with the tools available: n-gram overlap against the corpus if you have access,
  canary strings, memorisation probes, perturbed or paraphrased variants of the benchmark,
  and post-cutoff held-out data. Report what you did and what you could not rule out.
- A benchmark released after the model's data cutoff is the cleanest evidence available;
  say so explicitly when you use one.

**Where the leak most often hides:** in preprocessing code that ran before the split, and in
"just one more" glance at the test set.

---

## Distribution shift and external validity

I.i.d. test performance answers a narrow question: performance on data drawn like the
training data. It says little about behaviour anywhere else, and models with *identical*
i.i.d. performance can encode entirely different decision rules that diverge sharply under
shift.

If your claim is about robustness, generalisation, or capability rather than about one
distribution, you owe evidence beyond an i.i.d. split:

- **held-out conditions** — a different source, domain, time period, population, or
  collection process;
- **targeted stress tests** derived from your mechanism: construct the shift that your
  hypothesis says should break the model, and check that it does;
- **shortcut probes** — decorrelate the suspected spurious cue and re-measure; if performance
  collapses, the model was using it;
- **negative controls** — shuffled labels, randomised inputs, ablated modality. These
  establish the floor. A pipeline that "works" with shuffled labels has a bug or a leak.

Be precise afterwards: a result under one shift supports a claim about that shift, not about
robustness in general.

---

## Data for training vs. data for evidence

Keep the two roles separate and say which is which.

- **Training/development data** may be examined freely, and should be — error analysis on
  development data is where mechanisms are found.
- **Evidence data** is consulted once, under a protocol fixed in advance. Every additional
  look costs you validity, and if you look many times you must say so and treat the results
  as exploratory.

When a dataset is used for both across a project, the honest report states how many times
the evaluation set was consulted and what decisions were made after seeing it.

---

## Document what you used

Enough that someone could reconstruct your evidence base: exact version or snapshot,
download date and source, preprocessing code, split definition and the seed or rule that
produced it, deduplication procedure, filtering rules, final counts per split and per class,
and any examples dropped and why. Counts should be *printed by the pipeline*, not recalled.

Verify the pipeline before trusting it: a silently permuted label map, an off-by-one in
tokenisation, or a mis-ordered class list reads as a surprising scientific result and is
one of the most expensive ways to waste a month.
