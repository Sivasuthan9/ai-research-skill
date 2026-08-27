# Gates — reproduce before you build

Nothing downstream is interpretable until the ground truth is verified. Gate the project on
these and record the results; refuse to proceed if one fails.

| Gate | Check | Why |
|---|---|---|
| **Implementation facts** | every load-bearing claim about the method verified against the *released code*, not the paper | papers routinely omit or misstate what the code does |
| **Data** | every loader; class ordering; a known accuracy reproduced to ~0.01 pp | a silently permuted label map reads as chance and looks like a finding |
| **Reproducibility** | the same input produces the same output to a stated tolerance | without this, no later difference means anything |
| **Baseline reproduction** | published numbers reproduced within a stated tolerance | if you cannot reproduce the thing you are studying, you are studying something else |

## Read the code, not just the paper

Reading a released implementation line by line surfaces facts that change the analysis and
are unremarked in the paper. Real examples from one project:

- a component the paper describes as adapted was actually **frozen** (`no_grad`) — which
  made the whole system exactly analysable;
- the quantity being **optimised** and the quantity being **reported** were computed on
  different inputs — the gap between them turned out to be the largest effect in the paper;
- an optimiser reset every step meant the update was **scale-free sign descent**, not the
  gradient step every analysis assumed;
- a config flag silently tied one experimental condition to the dataset family, perfectly
  **confounding** two variables everyone treats as independent.

None of these are visible from the abstract. Each changed what the correct experiment was.

## Practical

- Record environment, versions, seeds, and hardware alongside results.
- Re-verify a gate after any infrastructure change.
- When results are regenerated, **check them against the previously recorded values**. An
  exact match (0.0e+00) is simultaneously a correctness check and a determinism proof.
