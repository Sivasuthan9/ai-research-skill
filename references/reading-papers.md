# Reading a paper to support a decision

An abstract tells you what the authors want you to conclude. It is a map, not evidence.

## Procedure for a paper that materially shapes your work

1. **Problem formulation** — what exactly is being optimised or claimed, in symbols.
2. **Assumptions** — listed explicitly, including the ones stated only in passing.
3. **Method** — as *implemented*, not as *described*. Read the code where available.
4. **Experiments** — datasets, baselines, protocol, what is held fixed.
5. **Ablations and limitations** — usually the most informative pages.
6. **Appendix** — where the load-bearing caveat generally lives.
7. **Claim vs evidence** — separate what the authors *claim* from what their experiments
   *support*. These differ more often than not.
8. **What remains unexplained** — this is where your contribution can live.

## Engage the strongest competing account head-on

Do not cite past a paper that contradicts you. Take its strongest form, accept what it
establishes, and identify precisely where it is vulnerable:

- an assumption the implementation violates;
- a proof step that assumes the conclusion;
- a model of a mechanism that does not match the code;
- an approximation used outside its regime;
- a claim about quantity A silently applied to quantity B;
- a theoretical requirement (e.g. independence) the authors acknowledge but never measure.

A rival's own figure or appendix, read carefully, is often the best evidence *for* your
account — and citing it that way is far stronger than asserting the same thing yourself.

## Honesty when reading

- Do not distort prior work to make yours look better. Reviewers know that literature.
- Accept what a rival establishes, explicitly, before saying where it stops.
- If a paper anticipated your idea, say so and re-scope. Discovering that early is cheap;
  discovering it in review is not.
