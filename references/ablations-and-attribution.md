# Ablations and attribution

An ablation exists to answer one question: **what is actually responsible for the effect?**

Most published ablations do not answer it. They remove components one at a time, at fixed
hyperparameters, and report that performance drops — which is consistent with the mechanism
being responsible and equally consistent with several duller explanations.

---

## The attribution problem

You observe: system with component C outperforms system without C.

Candidate explanations, all compatible with that observation:

1. **The mechanism.** C does what you claim it does.
2. **A nuisance parameter.** Removing C changed the optimum of something else — effective
   learning rate, capacity, regularisation strength, sequence length, number of retained
   items — and the un-reoptimised arm is simply mistuned.
3. **Capacity or compute.** C added parameters, FLOPs, training steps, or data passes.
4. **Tuning budget.** The full system received more search than the ablated one.
5. **Variance.** The difference is within seed noise.
6. **Interaction.** C only matters in the presence of D, and single-component removal cannot
   see that.
7. **Implementation.** Removing C changed something incidental — an initialisation, a
   normalisation, a code path.

An ablation is only evidence for (1) if it rules out (2)–(7).

---

## Designing an ablation that attributes

**Re-optimise the nuisance parameters on both arms.** The load-bearing control: compare *best
configuration with the mechanism off* against *best configuration with the mechanism on*,
letting every incidental parameter float to its own optimum on each side. Whatever difference
survives is attributable to the mechanism. Without this, you are comparing your tuned system
to a mistuned one.

**Match the budget along the axis you are not claiming.** If C adds parameters, compare
against a parameter-matched alternative use of those parameters. If C adds compute, match
compute. Otherwise "C helps" reduces to "more helps."

**Give the ablated arm equal search.** Same budget, same space, same algorithm.

**Ablate across conditions.** A component that matters on one dataset, backbone, or scale and
not others has an interaction, and the interaction is the finding. Reporting only the
condition where it helps is cherry-picking.

**Report uncertainty.** An ablation table of point estimates without seed variance cannot
distinguish a real component from a draw. If the ablation deltas are smaller than the seed
spread, say so plainly.

**Look for interactions deliberately.** One-at-a-time removal cannot detect them. Where the
components plausibly interact and the budget allows, vary the important ones factorially — or
at least test the specific pairs your mechanism predicts should interact, since that
prediction is itself a test of the mechanism.

**Replace, do not only remove.** Removing a component changes two things: the mechanism *and*
the model. Substituting a mechanism-free equivalent of the same size and cost (a random
version, a shuffled version, an identity, a simpler alternative) isolates the mechanism.

**Ablate in both directions.** *Removal from the full system* and *addition to the minimal
system* frequently disagree. When they do, you have an interaction, and reporting only the
favourable direction misrepresents the result.

---

## Attribution beyond ablation

**Vary the mechanism's strength.** If C is responsible, its effect should scale with its
strength (weight, rank, count, temperature). A dose–response relationship is much stronger
evidence than an on/off comparison, and it can fail.

**Predict where it should *not* help.** Your mechanism implies conditions under which C is
unnecessary or harmful. Test there. A component that helps everywhere equally is suspicious —
that is what "more capacity" looks like.

**Intervene rather than observe.** Where possible, manipulate the mechanism directly —
patching, steering, freezing, or corrupting the specific pathway — and measure the
behavioural consequence. Correlational evidence that a component tracks a phenomenon does not
establish that it causes it.

**Find the mediating quantity.** If C works through mechanism M, then M should be measurable
and should move when C is added. Measuring the intermediate is far more convincing than the
endpoint, and it can be wrong.

---

## Reporting

- State what was held fixed and what was re-optimised on each arm. This is the single most
  important sentence in an ablation table's caption and it is almost always missing.
- Report seeds and uncertainty per row.
- Report the ablations that did **not** show what you expected. Their absence is what makes
  readers distrust ablation tables generally.
- If a component's contribution turns out to be a nuisance parameter, say so. That is a
  useful, publishable finding — it corrects the field's understanding of a widely copied
  component.
- Bound the claim: "on these datasets, at this scale, with these nuisance parameters
  re-optimised, component C contributes X."

---

## The honest default

Before crediting any mechanism for any gain, assume the gain is a nuisance parameter until
the re-optimisation control says otherwise. In practice this assumption is correct more often
than not, and the discipline of testing it is what separates an attribution from a story.
