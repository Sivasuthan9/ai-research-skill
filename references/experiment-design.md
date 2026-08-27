# Experiment design

An experiment is not a computation you schedule. It is a question posed to the world in a
form where the world can answer "no."

**There is no standard set of experiments.** What to run follows from the hypothesis, the
competing explanations, and the constraints — derived each time. This file gives the
reasoning; the selection is yours to justify.

---

## Pre-register, in the experiment file itself

Written before the run, in the script's docstring, so it travels with the code and cannot
drift (`templates/experiment-header.md`):

```
Question       what are we trying to learn?
Hypothesis     what do we expect, and by what mechanism?
Design         why can THIS experiment distinguish the competing explanations?
Prediction     signed, ideally sized, before running
Falsification  what result makes us abandon or narrow the hypothesis?
Decision rule  what we do for each outcome — fixed now
Protocol       what is tuned, what is fixed, controls used, how uncertainty is computed
```

The decision rule is the part that protects you. Fixing in advance what you will do for each
outcome is what makes a later pivot a scientific decision rather than a rationalisation, and
naming the negative outcome as *also reportable* removes the incentive to see the result one
way.

---

## The central design question

> **Which competing explanation does this experiment eliminate?**

If the answer is "none — it shows my method works," it is a demonstration, not an experiment.
Demonstrations have their place, but they are not evidence about a mechanism.

Design backwards from the alternatives:

1. List the explanations that could produce the observation you expect
   (`hypotheses.md` has the standard set: nuisance parameter, data artifact, variance,
   evaluation protocol, tuning budget, bug).
2. For each, ask what result would be *impossible* if that explanation were true.
3. Build the experiment that produces that discrimination.

An experiment consistent with every explanation on your list is uninformative regardless of
its cost.

---

## Isolating the scientific variable

Change one thing, and let everything that interacts with it re-optimise on both sides.

- The **scientific variable** is what you are measuring the effect of.
- **Nuisance variables** must reach their own optimum within each arm, or their tuning is
  confounded with your variable (`objectives-and-optimization.md`).
- **Fixed variables** bound your conclusion to the values you fixed. Say so.

The naive version of "change one thing" — holding nuisance parameters at a single shared
value — is the most common source of false architectural and algorithmic findings in the
field, because the shared value is usually the optimum for one arm and not the other.

---

## Controls: choose the ones that rule out *your* alternatives

Not a checklist to satisfy. For each control, name the specific alternative explanation it
eliminates; if you cannot, do not run it.

| Control | Eliminates | Consider when |
|---|---|---|
| **Negative control** (shuffled labels, randomised inputs, ablated modality) | a pipeline that "works" on nothing; leaks and bugs | always cheap, always informative — this is the floor |
| **Trivial baseline** (majority, constant, nearest-neighbour, linear probe, retrieval-only) | "better than nothing" masquerading as signal | any new task or metric |
| **Disjoint-subset / split-half** | the subset definition manufacturing the effect | whenever a subset is selected using a quantity related to the measurement |
| **Nuisance-parameter control** (best-with-mechanism-off vs. best-with-mechanism-on) | the gain coming from an incidental knob | any multi-component or multi-parameter method |
| **Held-out or cross-fitted selection** | tuning bias inflating your own method | any time you search a grid |
| **Independent-condition replication** (another dataset, backbone, scale, split) | multiplicity and setting-specific artifacts | before any significance claim |
| **Randomisation sanity check** (random weights, random labels, untrained model) | an analysis method that would produce the same output regardless of the model | any probing or interpretability claim |
| **Parameter sweep on a contradiction** | mistaking a bad setting for a property | whenever a run fails or contradicts you |
| **Matched-budget control** | the effect coming from compute, data, or search rather than the idea | any efficiency or capability comparison |

A general principle worth keeping: **a control beats an argument.** If a quantity could be
contaminated by a dependence, no verbal reasoning establishes that it is not. Design the
dependence away or report the result as confounded.

---

## Power, before you run

Ask: *if the effect I predict is real, would this design detect it?*

- What effect size do I expect, from the mechanism?
- What is my measurement noise — seed variance, data-sampling variance, evaluation variance?
- Given both, how many seeds / examples / conditions do I need?

Running an experiment that cannot resolve the effect you predicted wastes the compute *and*
produces an uninterpretable null. If the answer is "I cannot afford enough power," that is a
finding about the study design: change the question, change the scale, or reduce the noise
(paired designs, shared seeds, variance reduction, larger eval sets) rather than proceeding
and reporting a meaningless null.

Also ask the reverse: **is the effect I am chasing larger than the noise floor of the field's
standard protocol?** If not, the honest contribution may be to measure and report that noise
floor.

---

## Sweeps, not points

A result at one arbitrary setting is an anecdote about that setting.

- Report the **whole sweep**, not the best point. Curves and orderings are far harder to
  satisfy by accident than single numbers.
- Where a result contradicts you, sweep it to give the contradiction its *best* chance
  (`objectives-and-optimization.md`).
- Where a result supports you, sweep it to find where it *stops* holding. The boundary is
  usually more informative than the effect.

---

## Choosing what to run next

Rank candidates by **information per unit of compute**, not by likelihood of a pleasant
result. The highest-value experiment is usually one of:

- the one that would **falsify your own current claim**;
- the one that **decides between two live explanations** you cannot currently separate;
- the one that measures whether the information you need **exists at all**, with no method in
  the loop — especially after several methods have failed. A single such measurement can
  explain many failures simultaneously, which no additional method can;
- the one that establishes the **smallest system** in which the phenomenon appears, because
  it multiplies every future experiment you can afford;
- the one that **checks a load-bearing assumption** you have been taking on trust.

Deprioritise: experiments whose outcome you can already predict confidently; experiments
that would not change any decision; and experiments that add another point to a trend you
have already established.

---

## Cost structure — find it early

The amount of science you can do is set by what you can avoid recomputing. Early in a
project, deliberately look for the quantity that is **constant across your experiments** and
compute it once: frozen-encoder features, cached activations, precomputed retrievals,
tokenised corpora, fixed evaluation subsets, shared checkpoints.

Recognising one such invariant can turn each subsequent experiment from a training run into
a matrix multiplication, which is the difference between six experiments and six hundred.
Look for it before you start the sweep, not after.

Corollaries: run the cheapest discriminative experiment first; stage expensive studies behind
cheap ones that could kill the hypothesis; and prefer designs where a negative result arrives
early.

---

## Before launching anything long

- The pre-registration is written and the decision rule is fixed.
- The reproducibility gates pass (`reproducibility.md`).
- A short smoke run has completed end-to-end and produced a parseable result.
- Every output path is namespaced by every condition that distinguishes the run.
- You have verified that the code being run is the code you think it is.
