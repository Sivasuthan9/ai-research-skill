# Evaluation

Evaluation is where a measurement becomes a claim. Almost all of the distance between the two
is validity and uncertainty.

---

## Validity: does the number mean what you say it means?

Before arguing about which metric, ask what you are entitled to conclude from any of them.

| Question | Name | What it asks |
|---|---|---|
| Does the measurement cover the relevant cases? | **content** | is the evaluation set representative of the task as defined? |
| Does it capture the abstract thing I claim? | **construct** | does "accuracy on this set" stand for the capability I named? |
| Is the observed difference caused by what I say caused it? | **internal** | are there confounds — leakage, budget, protocol, variance? |
| Does it hold elsewhere? | **external** | other distributions, populations, scales, deployment conditions |
| Does it predict what I care about? | **criterion** | does the benchmark track the downstream outcome? |

**The inferential gap principle:** the further your claim is from what you literally measured,
the more validity evidence you owe. "Accuracy on this test set improved" needs almost none.
"The model reasons better" needs a great deal — convergent evidence across measures,
discriminant evidence against confounded constructs, and evidence that the measure is not
saturated, contaminated, or shortcut-solvable.

The cheapest fix is usually not more evidence: it is **narrowing the claim to what you
measured.** Do that first.

**Benchmarks are constructs with authors.** A benchmark named after a capability is not
evidence that it measures that capability. Ask what it actually contains, how items were
generated, what a shortcut solution would score, whether it is saturated, and whether it is
in the pretraining corpus of the models you are evaluating.

---

## Baselines

A comparison is only as informative as its baseline. Weak baselines are the most common way
to manufacture a result.

- **Include the trivial baselines.** Majority class, constant prediction, nearest neighbour,
  linear probe on features, retrieval-only, the smallest reasonable model, the previous
  generation without your addition. If a trivial baseline gets most of the way, the
  interesting quantity is the remainder — report it that way.
- **Include the strongest current method**, at its best configuration, with its own tuning
  budget. Not its worst reported number, and not an old version because it was easier to run.
- **Include the "just scale it" baseline.** More data, more parameters, more steps, or more
  search, at matched cost. If that matches your method, say so.
- **Re-run baselines under your protocol** where feasible, rather than copying numbers.
  Numbers from different papers are usually not comparable — different splits, preprocessing,
  metric implementations, decoding, or evaluation harnesses. If you must copy, state the
  protocol mismatch explicitly and treat the comparison as indicative.
- **Never tune a baseline downward.** This is misconduct, not an optimisation.

---

## Metrics

- **Report what the metric is, exactly.** Averaging scheme (micro vs. macro — these can flip
  a ranking), normalisation, tie-handling, aggregation across sub-tasks, thresholding, and
  the code that computes it. Metric implementations of the "same" metric differ, and the
  differences are large enough to change conclusions.
- **Report more than one metric where they can disagree**, and say what disagreement would
  mean. A method that wins on one and loses on another is a finding, not a rounding error.
- **Report the distribution, not just the mean.** Per-class, per-group, per-difficulty, and
  worst-case numbers are where the interesting behaviour is. A mean improvement that comes
  entirely from one easy subgroup is a different result from a uniform improvement.
- **Watch for saturation.** Near a ceiling, differences compress and become dominated by
  label noise. Report the estimated ceiling and the headroom, so a small absolute gain can be
  read against what was achievable.
- **Interpret effect size against the achievable range.** "+0.3 points against a measured
  headroom of 8 points" is informative. "+0.3 points, p < 0.05" is not.

---

## Uncertainty and statistics

The purpose is to answer one question: **could this difference plausibly have arisen without
the effect I am claiming?**

**Randomise every source of variation you can, not just the weight seed.** Weight
initialisation, data order, augmentation, dropout, data splits, and the hyperparameter search
itself all contribute variance, and the search contributes an amount comparable to
initialisation. Varying only the weight seed systematically understates uncertainty. Where
resampling the test set is possible, that is often the largest single source and the most
worth including.

**Report central tendency and spread**, and say what the spread is over (seeds? splits?
both?). Error bars whose source is unstated are decoration.

**Use paired analysis for paired data.** Comparing two methods on the same items is a paired
design; use the paired form (McNemar for binary outcomes, a paired test or paired bootstrap
otherwise), not a difference of independent aggregates. Report the discordant counts, not
only a p-value — they reveal whether an effect rests on four items or four hundred.

**Bootstrap** any statistic without a clean analytic distribution, and state the number of
resamples.

**Prefer a decision-relevant statement.** "Method A outperforms B on 82% of randomisations"
communicates more than a p-value against a null nobody believes, and it degrades gracefully
when assumptions are imperfect.

**Underpowered is not negative.** "No measurable effect at this n" is not "no effect." State
which you mean, every time.

**State the multiplicity exposure.** Number of configurations, datasets, metrics, and
checkpoints examined. With a large grid, a few results at p < 0.05 are close to what chance
alone produces — and a reviewer will compute this whether or not you do.

**Replication across an independent condition beats a smaller p-value.** A result that holds
on a second backbone, dataset family, or scale is far more credible than one with more stars
on one condition.

**The strongest evidence is an independent prediction.** A quantity measured with no method in
the loop that predicts your outcome across conditions, in advance, is more persuasive than
any significance test. Design for that when the problem allows it.

---

## Evaluating generative and language models

These settings have failure modes that classification-era intuitions do not cover.

- **Prompt and format sensitivity is large.** Performance can swing by many points on
  wording, ordering, whitespace, and answer formatting, and the best format differs by model.
  A comparison at one prompt is a comparison of prompts as much as of models. Report the
  exact prompts; where feasible report across several formats and show the spread.
- **Implementation differences are large.** Independent implementations of the same benchmark
  produce materially different scores and can reorder models. Do not copy numbers across
  harnesses; re-run everything under one implementation and say which.
- **Scoring choices are load-bearing.** Log-likelihood normalisation (per-token, per-byte,
  unnormalised), answer extraction and parsing, tie-breaking, and stop conditions all change
  results. Document them.
- **Decoding is part of the method.** Temperature, sampling, beam search, and self-consistency
  budgets must be matched or reported as part of the comparison.
- **Contamination is the default assumption** for public benchmarks. Address it explicitly
  (`data.md`).
- **Model-based judges are measurement instruments and must be validated.** They exhibit
  position bias, verbosity bias, self-preference, and sensitivity to formatting. Before
  trusting one: measure agreement with human judgment on a sample, randomise presentation
  order, control for length, avoid using a model to judge its own family without reporting
  it, and report judge version and prompt. An unvalidated judge is not evidence.
- **Report per-item variance and stderr**, not single-run point estimates. Single numbers
  without uncertainty are the norm in this literature and it is a defect, not a convention to
  copy.
- **Do qualitative error analysis.** Read the outputs. Aggregate scores conceal the failure
  modes that constitute the actual finding.

---

## Beyond the aggregate

Before concluding, look at:

- **the errors**, grouped — do they cluster along an identifiable axis?
- **the worst-case slice**, not just the average;
- **behaviour under the shift your mechanism predicts should matter**;
- **whether the improvement is uniform or concentrated**;
- **what a human would say about a sample of outputs.**

An improvement you cannot characterise is an improvement you cannot claim a mechanism for.
