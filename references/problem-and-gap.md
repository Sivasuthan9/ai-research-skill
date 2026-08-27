# Choosing a problem, and finding a real gap

## The problem statement

Before anything else, write down — precisely, in a form someone could disagree with:

- **The phenomenon.** What do we observe that we do not understand? (Not: what method could
  I build?)
- **The question.** Stated so that a specific answer is possible and a wrong answer is
  recognisable.
- **The state of knowledge.** What is established, by whom, with what evidence, and what
  remains genuinely open. Distinguish "unknown" from "you have not read it yet."
- **The constraints.** Compute, data access, time, tooling. These are scientific facts about
  your study, and they bound what you can legitimately conclude.
- **Success.** What result would constitute a discovery? Write it before starting.
- **Failure.** What result would mean the direction is not worth pursuing? Write that too.
  A project with no defined failure condition never terminates and never concludes.

If the question cannot be stated without naming a method you intend to build, you have a
proposal, not a problem.

---

## Is the problem worth working on?

Two conditions, both required:

**It matters.** Answering it changes what someone would do, believe, or build. A result that
changes nothing is not important however difficult it was.

**You have an attack.** Importance without a viable line of approach produces nothing.
Hamming's formulation is exactly right: it is not the consequence that makes a problem
important, it is that you have a reasonable attack on it. The productive move is to keep
important problems in view and wait for an attack to become available — often a new
measurement, a new tool, a new dataset, or an argument you did not previously have.

Useful further filters:

- **Would the negative result be publishable?** If only one outcome is interesting, you are
  not running an experiment; you are hoping. Design the question so that both outcomes teach
  something.
- **Is the effect you are chasing larger than the noise you can achieve?** In deep learning,
  seed-to-seed variation frequently exceeds published improvements. If your achievable
  measurement precision cannot resolve the effect size you expect, the study is
  unfalsifiable before it starts. Decide this *first*.
- **Can you make it small?** The smallest system that exhibits the phenomenon is where the
  science gets done.
- **Is anyone else's answer going to arrive first, and does that matter?** Sometimes it does
  not — a better *explanation* of a known effect is a contribution.

---

## What is not a gap

- "Nobody has applied method M to dataset D." Combinatorics is not a research question.
- "Accuracy on benchmark B is only 78%." That is a number, not a phenomenon. Ask *what kind
  of errors* make up the remaining 22%, and whether they are reducible at all.
- "Method M is popular but has no theory." Only a gap if the missing theory would change a
  decision. Otherwise it is a formalisation exercise.
- "This has not been done at scale S." Sometimes genuinely valuable — but only if you can say
  what qualitatively changes at S, and how you would detect it.
- "Approach A is more principled." Aesthetic preference is not evidence.

---

## What a real gap looks like

| Type of gap | What makes it real | What it demands |
|---|---|---|
| **Unexplained phenomenon** | reproducible, and existing accounts do not predict it | a mechanism that predicts it *and* something else |
| **Violated assumption** | a widely relied-upon assumption is measurably false in practice | measure the violation; show what it changes |
| **Theory–practice contradiction** | theory predicts X, systems reliably do not-X | locate which assumption fails, not merely that one does |
| **Systematic failure mode** | errors cluster along an identifiable axis, not randomly | characterise the axis; show it is not a data artifact |
| **Untested mechanism** | everyone credits component C, nobody isolated it | an attribution experiment that can exonerate C |
| **Missing measurement** | a quantity everyone reasons about is never measured | measure it; establish that the measurement is valid |
| **Invalid construct** | a metric is presumed to measure a construct, unvalidated | show the metric and construct come apart |
| **Impossibility / ceiling** | a whole class of methods fails, and it is not obvious why | a bound or measurement explaining *why none can work* |

The last row is worth emphasising for AI/ML specifically. A well-supported statement of the
form *"methods of class Z cannot do W, and here is the measurement showing why"* is often
more useful to the field than another member of class Z.

---

## Finding the gap in practice

1. **Read for the residue.** In every paper that matters to your area, note explicitly what
   remains unexplained, what the limitations section concedes, and what the appendix
   quietly reveals. Collect these across papers; the recurring ones are the field's real
   open problems.
2. **Look where the claim outruns the evidence.** Find the sentence in a well-known paper
   that the experiments do not actually support. Testing it properly is a contribution.
3. **Look for the inherited assumption.** Trace a standard practice back to its origin. Often
   the original justification applied to a setting that no longer holds — a different scale,
   dataset, architecture, or objective. Re-testing it is cheap and frequently surprising.
4. **Look at disagreements.** Where two credible papers reach opposite conclusions, one of
   them has an unstated condition. Finding it is a real result.
5. **Look at what is never ablated.** The components everyone copies without testing.
6. **Look at your own failures.** A method that fails for a reason you cannot articulate is
   pointing at something you do not understand. That is the gap.

---

## Before committing

Write, and be prepared to defend:

- the phenomenon, question, and the mechanism-level account you currently believe;
- what would count as discovery and what as failure;
- the smallest system in which you can study it;
- the measurement precision you can achieve versus the effect size you expect;
- **why this is not already known** — with the search actually executed
  (`novelty-and-ideas.md`), not assumed;
- what you will do if the first result is negative.

Then hand it to the Skeptical Reviewer before spending compute.
