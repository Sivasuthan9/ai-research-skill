# First-principles reasoning in AI/ML

Most proposals in machine learning are **recombinations**: take a component from paper A,
attach it to the architecture from paper B, evaluate on the benchmark from paper C. This is
cheap to generate and almost never produces understanding, because nothing in the process
required anyone to know *why* the combination should work.

First-principles reasoning is the alternative: decompose the system into things that are
actually true, and derive what must follow.

---

## The five substrates

Every claim in machine learning is ultimately a claim about one or more of these. Before
proposing anything, ask what you actually know about each.

### 1. The data — what information is present?

- What process generated it? Sampling, labelling, curation, filtering — each is a
  transformation with its own biases.
- What is the **information content** relevant to your task? A method cannot extract
  information the input does not contain. This is a measurable quantity, and measuring it is
  often more valuable than building another extractor.
- What is **spuriously correlated** with the target? Anything predictive but not causal is a
  shortcut the model will prefer if it is easier to compute.
- What is the label actually a label *of*? Annotator judgment, a proxy outcome, an automated
  heuristic, and ground truth are four different things.

### 2. The model — what function class is this?

- What can it represent? What can it *not*? Expressivity arguments are cheap to construct
  and often decisive.
- What is its **inductive bias** — not what it can represent, but what it finds easy. Locality,
  equivariance, sequence position, smoothness, low-rank structure, attention's
  permutation-equivariance-plus-positional-encoding.
- Where is capacity actually spent? Parameter count is a poor proxy; effective capacity
  depends on architecture, data, and optimization together.
- Which of the model's properties does your claim depend on? If your claim would hold for
  *any* function approximator, say so — that is a stronger and different claim than one
  about a specific architecture.

### 3. The objective — what is literally being minimised?

- Write the loss down. In symbols. Including the regularisers, the temperature, the
  normalisation, the stop-gradients, and the reduction (mean vs. sum changes effective
  learning rate).
- Ask: **what is the optimal behaviour under this objective?** Not the intended behaviour —
  the optimal one. The gap between them is where most surprising model behaviour lives.
- Which quantity is *optimised* and which is *reported*? They are frequently computed on
  different inputs, with different normalisation, or at different granularity. That gap is
  itself a research object.
- What does the objective *not* penalise? Absence of a term is a design decision.

### 4. The optimizer — which solution do you get?

- Many parameter settings achieve the same training loss. The optimizer, schedule,
  initialisation, and batch composition select among them. This is **underspecification**:
  models with identical validation performance can encode entirely different decision rules
  and behave arbitrarily differently under shift.
- What does the update rule actually do? Adaptive methods, gradient clipping, weight decay
  coupling, EMA, and optimizer-state resets change the effective dynamics in ways the naive
  "gradient step" model does not capture.
- What is the effective learning rate after batch size, loss reduction, warmup, and
  normalisation layers are accounted for? Most "instability" findings are this quantity.

### 5. The evaluation — what does the number mean?

- What is literally measured, on what population, under what protocol?
- What construct do you claim it stands for? The **inferential gap** between the measurement
  and the claim determines how much validity evidence you owe. Accuracy on a fixed test set
  supports "accuracy on this distribution"; it supports "reasoning ability" only with a great
  deal more work.
- Would the measurement move for reasons unrelated to your hypothesis — prompt format,
  tokenisation, normalisation, decoding, seed, evaluation harness version?

---

## Deriving what to do

Once the substrates are written down, the next experiment is usually implied. Useful moves:

**Ask what must be true.** If the phenomenon is real, what else must hold? Predictions that
follow necessarily from a mechanism are more valuable than the phenomenon itself, because
they can fail.

**Ask what the mechanism forbids.** A mechanism that forbids nothing explains nothing. Find
the configuration where your account predicts the effect *disappears* or *reverses*, and test
there. That is a far sharper test than another instance where it appears.

**Ask whether the information exists.** Before building method N+1 to extract a signal, run
a measurement — with no method in the loop — of whether the signal is there. Upper bounds,
mutual-information estimates, oracle probes, and label-permutation controls all serve. When
several methods have failed, this is almost always the highest-value experiment.

**Ask what a trivial solution would achieve.** Constant predictor, majority class, nearest
neighbour, linear probe, random baseline, retrieval-only, the smallest model. If a trivial
solution gets most of the way, the interesting quantity is the remainder, not the total.

**Ask what changes when you scale it down.** A phenomenon reproducible at small scale can be
studied 100× more thoroughly. Establishing the smallest system that exhibits the phenomenon
is one of the most productive early moves in an AI/ML project, and it also tests whether you
understand what causes it.

**Ask where the assumption breaks.** Take the theory or intuition you are relying on, find
the regime where its assumption is violated, and go there deliberately.

---

## Reasoning discipline

- **Separate the claim from the implementation.** "Method M improves accuracy" and "the
  mechanism in M improves accuracy" are different claims requiring different evidence. Most
  papers demonstrate the first and assert the second.
- **Prefer the simpler explanation, and test it first.** Before attributing behaviour to a
  sophisticated cause, rule out: a bug, a leak, a shortcut, a learning rate, a normalisation
  difference, an evaluation harness discrepancy, and seed variance. This ordering is not
  cynicism; it is where the base rate actually is.
- **Give the alternative account its strongest form.** Argue the opposing hypothesis as well
  as its best proponent would, then test *that*, not a weakened version.
- **A control beats an argument.** If a quantity could be contaminated by a dependence, no
  verbal argument establishes that it is not. Design the dependence away, or report the
  result as confounded.
- **Distinguish "we did not observe X" from "X does not occur."** These are different
  sentences and the second requires power you probably do not have.

---

## Where new ideas actually come from

Not from recombination lotteries. Reliably, from:

- a **contradiction** between what a theory predicts and what a system does;
- an **assumption** that everyone inherits and nobody has tested;
- a **measurement nobody has made**, usually because it is unglamorous;
- a **failure mode with no explanation** — a systematic error that current accounts do not
  cover;
- a **quantity that is optimised but never reported**, or reported but never optimised;
- a **construct** that a widely used metric is presumed to measure, but has never been shown
  to measure;
- **transferring a mechanism, not a method**, from another field — after checking whether
  the conditions that made it work there hold here.

See `novelty-and-ideas.md` for how to turn these into a concrete, checkable research idea.
