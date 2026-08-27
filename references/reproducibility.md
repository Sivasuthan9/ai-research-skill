# Reproducibility, gates, and research engineering

Research conclusions are only as trustworthy as the runs that produced them. This file covers
both the scientific gates (what must be true before a result means anything) and the
engineering discipline that keeps runs trustworthy.

---

## Three distinct things

| Term | Meaning | What it establishes |
|---|---|---|
| **Reproducibility** | same data, same code → same result | your own pipeline is deterministic and correctly recorded |
| **Replicability** | new data from the same distribution → same conclusion | the finding is not an artifact of one sample |
| **Robustness** | same data, independent reimplementation → same conclusion | the finding is not an artifact of your code |

Claiming one and demonstrating another is a common and consequential error. Say which you
have.

---

## Gates — pass these before anything downstream is interpretable

Record the result of each. Refuse to build on a failed gate.

| Gate | Check | Why |
|---|---|---|
| **Determinism** | the same input and seed produce the same output to a stated tolerance | without it, no later difference means anything |
| **Data pipeline** | loaders, class ordering, tokenisation, splits; counts printed by the code; a known number reproduced | a silently permuted label map reads as chance and looks like a finding |
| **Implementation facts** | every load-bearing claim about a method verified against the *released code*, not the paper | papers routinely omit or misstate what the code does |
| **Baseline reproduction** | published numbers reproduced within a stated tolerance | if you cannot reproduce what you are studying, you are studying something else |
| **Negative control** | shuffled labels or randomised inputs give chance performance | a pipeline that "works" on noise has a leak or a bug |
| **Variance floor** | repeat the same configuration across seeds and measure the spread | you cannot interpret any difference smaller than this |

The variance-floor gate is the one most often skipped and the most often decisive. Measure it
early: it tells you which effects your study is capable of detecting at all.

---

## Read the code, not just the paper

For any method your work depends on, read the implementation. Discrepancies between described
and implemented behaviour are common and frequently change what the correct experiment is:

- a component described as trained is actually frozen, or vice versa;
- the quantity optimised and the quantity reported are computed on different inputs or with
  different normalisation;
- an optimizer or buffer state is reset in a way that changes the effective update rule;
- a configuration flag couples two conditions that everyone treats as independent;
- preprocessing or augmentation differs from the description;
- the released checkpoint was produced by a different recipe than the released config.

Record which facts you verified against code, and which you took from the paper.

---

## Provenance: every number traceable to a run

Each recorded result carries: the code version (commit hash, and whether the tree was dirty),
the exact config, the seed, the data version, the environment (library versions, hardware,
driver, precision settings), the command, the start/end time, and the paths to raw outputs and
logs.

If a number in your write-up cannot be traced to such a record, it is not a measurement. Do
not use it.

---

## Namespacing and overwriting

**Every output path includes every condition that distinguishes the run** — dataset, model,
objective, seed, variant, and any swept parameter. Derive the tag once, next to argument
parsing, so it cannot drift between scripts.

The failure this prevents: a result file keyed on fewer variables than the experiment varies,
so a later run silently overwrites an earlier one with no error, and two conditions are
quietly merged into one. This is invisible in every plot you subsequently make.

Related discipline:

- Prefer failing loudly over overwriting silently: refuse to write to an existing path unless
  explicitly told to.
- When files of ambiguous provenance appear, quarantine, regenerate, and verify against
  recorded values rather than guessing which is which.
- When results are regenerated, **check them against previously recorded values**. An exact
  match is simultaneously a correctness check and a determinism proof.
- Sync code as a directory, never file-by-file — path bugs from partial syncs are silent.

---

## Verify before you depend

- **Verify a patch landed** before starting anything long that depends on it. A change applied
  inside a compound command that exited early never landed; grep for it.
- **Verify the code being run is the code you think it is** — check the commit hash inside the
  run, not outside it.
- **Verify a launch actually launched.** A command that timed out may still have started the
  job; relaunching duplicates it and corrupts throughput and results. Check before relaunching.
- **Verify process management assumptions.** Killing a wrapper can leave the child running; a
  pattern-matching process check can match its own command line; queues that wait on each
  other by process-name matching deadlock when the watcher matches the watched. Prefer
  explicit PIDs, sentinel files, and sequential chains.
- **Re-verify every gate after any infrastructure change** — new hardware, new library
  version, new precision setting.

---

## Numerical precision

- Use double precision, and disable reduced-precision matmul paths, whenever the number
  *itself* is the evidence — verifying an identity, a gradient, a bound, or a conservation
  property. Otherwise you may be measuring the arithmetic rather than the mathematics.
- Watch for catastrophic cancellation wherever near-equal quantities are subtracted; find the
  algebraically stable form.
- Beware silent dtype retention: constructing a tensor from an existing array usually keeps
  that array's dtype regardless of any global default you set.
- Non-determinism from nondeterministic kernels, atomics, and reduction order is real; either
  disable it or record the resulting tolerance explicitly.

---

## Cost structure

Find the quantity that is constant across your experiments and compute it once — cached
features from a frozen component, precomputed retrievals, tokenised corpora, fixed evaluation
subsets. Recognising one such invariant early can convert each subsequent experiment from a
training run into a cheap operation, which determines how much science you can actually do.
Look for it before the sweep, not after.

---

## What to release

The standard to hold yourself to, whether or not the venue requires it: code, exact configs,
seeds, data preparation scripts, split definitions, environment specification, evaluation
code, the exact prompts where applicable, raw outputs, and the analysis scripts that turn raw
outputs into the figures and tables in the paper.

State clearly what you could not release and why. An honest statement of a restriction is
worth more than a vague claim of availability.
