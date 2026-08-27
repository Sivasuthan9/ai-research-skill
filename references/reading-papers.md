# Reading a paper to support a decision

An abstract tells you what the authors want you to conclude. It is a map, not evidence.

Read at the depth the decision requires: a passing reference needs a citation check; a paper
your entire design rests on needs the full pass and often the code.

---

## The full pass

1. **Problem formulation.** What exactly is optimised or claimed, in symbols. Often the
   formalisation is narrower than the prose.
2. **Assumptions.** All of them, including the ones stated only in passing, and the ones
   implicit in the experimental setup.
3. **Method as implemented.** Not as described. Read the released code where available; the
   two differ often enough that assuming otherwise is a real risk
   (`reproducibility.md`).
4. **Experimental protocol.** Datasets, splits, preprocessing, baselines and their
   configurations, tuning budget per arm, metric implementation, number of seeds, what is held
   fixed. This is where a result is made fair or unfair.
5. **Ablations and limitations.** Usually the most informative pages. Note what was *not*
   ablated.
6. **Appendix.** Where the load-bearing caveat generally lives — full sweeps, failure cases,
   hyperparameters, and the honest version of the headline.
7. **Claim vs. evidence.** Separate what the authors *claim* from what their experiments
   *support*. These differ more often than not; the gap is where your contribution can live.
8. **What remains unexplained.** Collect it. Across many papers, the recurring residue is the
   field's real open problem.

---

## Questions that decide whether to trust a result

- Were baselines given equal tuning budget, or inherited from another paper's table?
- Is the comparison protocol identical, or are numbers copied across incompatible setups?
- How many seeds, and is the reported improvement larger than seed variance?
- Was the test set used for selection? How many configurations were evaluated?
- Could the effect be a nuisance parameter, a capacity increase, or extra compute?
- For foundation models: could the benchmark be in the pretraining data?
- Does the ablation re-optimise nuisance parameters on both arms?
- Does the theory's setting match the experimental setting?
- Is the conclusion bounded by the conditions tested, or stated generally?

A "no" to several of these does not make the paper wrong — but it changes what you may build
on top of it, and you should record which parts you are treating as established.

---

## Engage the strongest competing account head-on

Do not cite past a paper that contradicts you. Take its strongest form, accept what it
establishes, and identify precisely where it is vulnerable:

- an assumption the implementation violates;
- a proof step that assumes the conclusion;
- a model of a mechanism that does not match the code;
- an approximation used outside its regime;
- a claim about quantity A silently applied to quantity B;
- a theoretical requirement (independence, i.i.d., convergence) acknowledged but never
  measured;
- a protocol difference that explains the disagreement.

A rival's own figure or appendix, read carefully, is often the best evidence *for* your
account — and citing it that way is far stronger than asserting the same thing yourself.

---

## Reading a literature, not a paper

For a research direction rather than a single decision:

- **Trace the lineage.** Follow a standard practice back to the paper that introduced it, and
  check whether its justification still applies at today's scale, data, and architectures.
- **Find the disagreements.** Where credible papers conflict, one has an unstated condition.
  Locating it is a contribution.
- **Note what is never ablated** across the whole literature. Those are the untested
  inherited assumptions.
- **Read the negative results and the workshop papers.** They are where "we tried the obvious
  thing and it failed" is recorded, and they save months.
- **Read across communities.** The same mechanism appears under different names in
  statistics, information theory, signal processing, control, and psychometrics.

---

## Honesty when reading

- Do not distort prior work to make yours look better. Reviewers know that literature.
- Accept what a rival establishes, explicitly, before saying where it stops.
- If a paper anticipated your idea, say so and re-scope. Discovering that early is cheap;
  discovering it in review is not.
- **Never cite from memory.** Retrieve the paper, verify the identifiers, and locate the claim
  in the text before attributing it (`evidence-and-honesty.md`).
