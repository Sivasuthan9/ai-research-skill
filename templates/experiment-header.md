# Experiment header template

Put this in the experiment script's module docstring, written **before** the run. It travels
with the code, so the pre-registration cannot drift.

```python
r"""E<N> -- <one-line question this experiment answers>

WHY THIS EXISTS
<Which earlier result raises this question? Why is this the right next step rather than a
convenient one? Name the specific finding that motivates it. If this is the cheapest
experiment that could falsify the current claim, say so.>

HYPOTHESIS AND MECHANISM
<What we expect, and the mechanism -- in terms of data / model / objective / optimizer /
evaluation. Not "it might help": the reason it would help. State what the mechanism
FORBIDS, since that is what makes it testable.>

COMPETING EXPLANATIONS THIS MUST RULE OUT
  A  <nuisance parameter: which one, and how the design excludes it>
  B  <data artifact / leakage / shortcut>
  C  <variance: what the measured variance floor is>
  D  <evaluation protocol / tuning budget>
<For each, what result would be impossible if that explanation were the true one?>

PRE-REGISTERED PREDICTION (signed, before running)
  P1  <directional, ideally with rough magnitude>
  P2  <secondary prediction -- ideally about a shape, ordering, or interaction, which is
       harder to satisfy by accident than a single number>

FALSIFICATION
<The result that makes us abandon or narrow this. Specific and quantitative. Note whether
the design has the power to detect the predicted effect at all.>

DECISION RULE
<Fixed now, for each outcome:
   if P1 holds  -> ...
   if P1 fails  -> ...            (name why this outcome is also reportable)
   if ambiguous -> ...>

PROTOCOL
<Scientific variable; nuisance parameters re-optimised per arm; parameters fixed and the
scope limit that implies; controls used and which alternative each eliminates; seeds and
what uncertainty is computed over; baselines and their tuning budget; metric implementation;
any deviation from the standard setup and why.>

PROVENANCE
<Data version; code commit; output path namespaced by every varying condition; where raw
outputs and logs are written.>
"""
```

## Why each part is load-bearing

- **Written before the run**, so it cannot be reshaped by the result.
- **The mechanism must forbid something** — otherwise the experiment cannot discriminate.
- **The competing explanations are listed explicitly**, so "my method works" cannot be the
  only conclusion the design supports.
- **The prediction is signed**, so it can be wrong.
- **The decision rule names the negative outcome as reportable**, which removes the incentive
  to see the result one way and makes a later pivot a scientific decision rather than a
  rationalisation.
- **The protocol records what was re-optimised per arm** — the single most common omission,
  and the one that separates an attribution from a story.

## After the run

Append, in the same file:

```
OUTCOME       what happened, with the artifact path
VERDICT       P1 held / failed / underpowered-inconclusive
DIAGNOSIS     if it failed or surprised: which of A-D, or a real finding, and how you know
DECISION      what the pre-registered rule dictated, and what was done
DEVIATIONS    anything done differently from the protocol above, and why
```

Never edit the pre-registration to match the outcome. If it must be revised, mark the
revision and treat everything downstream as exploratory.
