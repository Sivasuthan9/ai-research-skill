# Experiment header template

Put this in the experiment script's module docstring, written **before** the run. It travels
with the code, so the pre-registration cannot drift.

```python
r"""R<N> -- <one-line question this experiment answers>

WHY THIS EXISTS
<What earlier result raises this question? Why is it the right next step rather than a
convenient one? Name the specific finding that motivates it.>

HYPOTHESIS AND MECHANISM
<What we expect, and the physical/mathematical reason. Not "it might help" -- the
mechanism that would make it help.>

WHY THIS DESIGN CAN DISTINGUISH EXPLANATIONS
<What competing explanation does this rule out? Which control does that work?>

PRE-REGISTERED PREDICTION (signed before running)
  P1  <directional, ideally with rough magnitude>
  P2  <secondary prediction, if any>

FALSIFICATION
<The result that makes us abandon or narrow this. Be specific and quantitative.>

DECISION RULE
<What we do for each outcome, fixed now:
   if P1 holds  -> ...
   if P1 fails  -> ...>

PROTOCOL NOTES
<Controls used; what is tuned and what is deliberately not; how significance is
computed; any deviation from the standard setup and why.>
"""
```

## Worked example

```python
r"""R29 -- does the augmentation metadata see what the model's response cannot?

WHY THIS EXISTS
R22 bounds statistics of the class posterior; R27 extends that to the view features. Both
are functions of the MODEL'S RESPONSE. The augmentation parameters are a third source that
no method in this family uses and our bound does not cover: we GENERATE the crop box, so it
is known exactly, is label-free, and is not a function of the model at all.

HYPOTHESIS AND MECHANISM
Views are crops down to 8% of image area, which frequently contain no object. Prior work
measured that augmentation un-calibrates the model almost purely through OVERCONFIDENCE, so
such a crop is often CONFIDENTLY WRONG -- an entropy selector KEEPS it. Crop area knows the
view saw 8% of the image regardless of confidence. The two signals should be complementary.

PRE-REGISTERED
  P1  crop area has controlled AUC > 0.5 on reducible errors, where entropy is ~0.50
  P2  |Spearman(area, -entropy)| < 0.3 -- new information, not re-encoded confidence

FALSIFICATION
P1 fails if AUC is within noise of 0.50. That outcome is a STRONGER negative result and is
reported as such.

DECISION RULE
If P1 holds -> pivot to P3 (does it convert to accuracy?) and replicate across backbones.
If P1 fails -> report the stronger impossibility and move compute to the MEMO measurement.

PROTOCOL NOTES
Split-half control as in R28: half A alone defines the subset, statistics measured on the
disjoint half B. View 0 excluded -- it is not a random crop and its box is degenerate.
"""
```

Note what this header did in practice: because the decision rule was fixed in advance, the
later pivot was a scientific decision rather than a rationalisation — and because the
falsification clause named the alternative outcome as *also* publishable, there was no
incentive to see the result one way.
