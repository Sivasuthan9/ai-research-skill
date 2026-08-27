# Research plan template

Written before compute is spent, revised as evidence arrives, and kept under version control
so the revision history is visible. Anything you cannot fill in is a thing to find out, not a
thing to invent.

```markdown
# <Project title>                                        v<N>, <date>

## 1. Phenomenon
What do we observe that we do not understand? State it so someone could disagree.
[Observed] ... / [Known] ...

## 2. Question
The precise question. A specific answer must be possible, and a wrong answer recognisable.

## 3. State of knowledge
What is established, by whom, with what evidence. Distinguish "settled", "contested",
"assumed but untested", and "not yet read". Every entry cites a retrieved source.

## 4. Why this is not already known
The novelty search actually executed: queries, communities searched, closest work found,
and why it does not answer this question.  → references/novelty-and-ideas.md

## 5. First-principles account
  Data        what information is present / absent; what is spuriously correlated
  Model       function class, inductive bias, what it makes easy
  Objective   what is literally minimised; what is optimal under it
  Optimizer   which of the equally-good solutions is selected, and by what
  Evaluation  what is measured; what construct it is claimed to stand for
Which of these does the project change, and by what mechanism?

## 6. Competing hypotheses
  H1  <mechanism> — assumptions — prediction (signed) — falsifier
  H2  <nuisance-parameter account>
  H3  <data / leakage / shortcut account>
  H4  <variance account>
  H5  <evaluation-protocol account>
Which experiment discriminates among these?

## 7. What would count as
  Discovery:  ...
  Failure:    ...          (and what we do in that case)

## 8. Measurement feasibility
  Expected effect size (from the mechanism):  ...
  Measured variance floor:                    ...   [Observed / to be measured]
  Seeds / conditions needed to resolve it:    ...
  If infeasible: what changes — scale, question, or noise reduction?

## 9. Scientific objects
  Data        chosen because ... ; provenance ... ; leakage risks ... ; splits ...
  Model       chosen because ... ; what its inductive bias asserts about this problem
  Objective   chosen because ... ; degenerate solutions and what rules them out
  Metric      chosen because ... ; construct it stands for ; validity evidence owed
  Baselines   trivial ... ; strongest current ... ; matched-budget ...
Each with the alternative rejected and why.

## 10. Gates before any headline experiment
  [ ] determinism        [ ] data pipeline verified    [ ] implementation facts vs. code
  [ ] baseline reproduced  [ ] negative control at chance  [ ] variance floor measured

## 11. Experiment sequence
  E1  question — what it discriminates — cost — kills which hypothesis
  E2  ...
Ordered by information per unit of compute; cheapest potentially-fatal experiment first.

## 12. Cost structure
The quantity constant across experiments that can be computed once: ...

## 13. Scope of any resulting claim
Datasets, scales, architectures, seeds, method class. Stated now, so it cannot expand later.

## 14. Reviewer's standing objections
The strongest criticisms, and their current status: answered / narrowed / open.

## 15. Open threads
Carried into limitations if unresolved.
```

## Revising the plan

Revise it when evidence demands, and record **why** in the decision log. A plan that never
changes was not being tested; a plan that changes without a recorded reason is being
rationalised.
