# Experiment design

## Pre-registration (before the run, not after)

Write these in the experiment file's docstring so they cannot drift:

```
Question       what are we trying to learn?
Hypothesis     what do we expect, and why (mechanism, not intuition)?
Design         why can this experiment DISTINGUISH the competing explanations?
Prediction     signed in advance — which direction, roughly what size?
Falsification  what result makes us abandon or narrow the hypothesis?
Decision rule  what we do for each outcome, fixed now
```

The decision rule is the part that protects you. "If crop area is within noise of chance we
report the stronger negative and move compute to X" — written *before* seeing results — is
what makes the later pivot a scientific decision rather than a rationalisation.

**Sign your predictions.** A prediction with a direction can be wrong. One without a
direction cannot, and is therefore not a prediction.

---

## Controls that earn their cost

| Control | What it rules out | When required |
|---|---|---|
| **Split-half / disjoint-subset** | the subset definition manufacturing the effect | whenever a subset is defined using a related quantity |
| **Held-out or cross-fitted tuning** | tuning bias inflating your own method | any time you search a parameter grid |
| **Incidental-parameter control** | the gain coming from a nuisance knob | any multi-parameter method |
| **Independent-condition replication** | multiplicity, dataset-specific artifacts | before any significance claim |
| **Random / shuffled baseline** | "better than nothing" masquerading as signal | any selection or ranking method |
| **Parameter sweep on a contradiction** | mistaking a bad setting for a property | whenever a run fails or contradicts |

---

## Tuning protocol (asymmetric, and deliberately so)

- **Tune your own method fully.** Judging your proposal at one arbitrary setting is not a
  fair test of it.
- **Do not tune the baseline to look weak.** Leave it at its published configuration. Tuning
  the baseline downward is the mirror error, and it is misconduct.
- **Cross-fit.** With a large grid on finite data, reporting on the data you tuned on buys a
  point or more of pure overfitting. Use k-fold cross-fitting so every unit is scored under
  parameters selected without it.
- **State the multiplicity exposure.** "972 settings × 6 datasets, so 2 results at p < 0.05
  is close to what multiplicity alone would give" is a sentence that belongs in the paper.

**Tuning against your own thesis.** When a result contradicts you, tune it to give the
contradiction its *best* chance and report the full sweep. If it survives, your thesis is
wrong and you update. If it evaporates across the whole sweep, your claim is now much
stronger than one made at a single point. Never tune to make an inconvenient result vanish.

---

## Statistics

- **Paired tests for paired data.** Comparing two methods on the same items is a paired
  design; use McNemar (binary outcomes) or a paired test, not a difference of accuracies.
- **Report the discordant counts** (`+b / −c`), not only the p-value. They show whether an
  effect rests on 4 items or 400.
- **Bootstrap CIs** on any statistic without a clean analytic distribution. State the
  number of resamples.
- **Underpowered is not negative.** n = 4 with p = 0.55 means *no measurable effect*, not
  *no effect*. Say which one you mean.
- **Effect size against the achievable range.** "+0.32 pp against a measured ceiling of
  8–27 pp" is informative; "+0.32 pp, p < 0.05" is not.

---

## Choosing what to run next

Rank candidates by **information per unit of compute**, not by likelihood of a nice result.

The highest-value experiment is usually one of:

- the one that would **falsify your own current claim**;
- the one that decides between two live explanations;
- the one that measures whether the information you need **exists at all**, before you build
  another method to extract it.

That last move is worth naming. After several methods fail, the instinct is to design a
better one. Often the better move is to stop and measure whether the signal those methods
all read actually carries the information — a measurement with no method in the loop. One
such measurement explained seven separate failures at once, which no eighth method would
have done.
