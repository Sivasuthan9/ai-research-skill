# ai-research-skill: rigorous-research

A Claude skill for conducting **AI/ML research as a science**.

It equips an agent to reason from first principles, form mechanism-level hypotheses, make
falsifiable predictions, design experiments that discriminate between explanations, evaluate
honestly, diagnose its own failures, and write up claims that survive hostile review — while
making fabricated evidence structurally difficult to produce.

---

## Philosophy

**Every research action must have a scientific reason.** Not convenience, not convention, not
availability, not the hope of a better number.

Four commitments follow from that:

**1. First principles over pattern matching.** Before proposing anything, decompose the
system into things that are actually true — what the *data* contains, what the *model* can
represent and finds easy, what the *objective* literally rewards, what the *optimizer*
selects among equally-good solutions, and what the *evaluation* measures versus what it is
claimed to stand for. A proposal earns compute only when you can say which of these it
changes and why that produces the predicted effect. "It might help" is not a mechanism.

**2. The goal is a true result, not a strong one.** A robust negative result with a mechanism
is worth more than a positive result that dissolves under a control. Experiments are designed
so that both outcomes teach something, and the decision rule for each is fixed before the run.

**3. Judgment, not recipes.** There is no canonical set of experiments, no default dataset, no
mandatory ablation table. The skill supplies the reasoning — what each control eliminates,
what each design choice bounds, where the field's failure modes actually live — and the agent
derives the study the problem demands, and justifies it.

**4. The research must be real.** An AI agent's characteristic failure is not sloppiness but
fluency: a well-formed report citing a paper that does not exist, or reporting a number from a
run that never executed. Every number traces to an artifact; every citation to a retrieved
source; every novelty claim to an executed search. If it cannot be verified, the skill says so
rather than inventing it.

---

## The workflow

Not a pipeline. A loop, re-entered wherever the evidence demands, with the current phase
stated explicitly.

```
        problem  →  gap  →  first-principles model  →  hypothesis  →  prediction
                                                                          ↓
   paper  ←  finding  ←  iteration  ←  falsification / validation  ←  experiment
```

| Phase | What it produces |
|---|---|
| **Problem** | a phenomenon we do not understand, stated so it can be disagreed with; defined success *and* failure |
| **Gap** | an unexplained phenomenon, violated assumption, theory–practice contradiction, systematic failure, untested mechanism, missing measurement, or invalid construct — never "nobody tried M on D" |
| **First-principles model** | the mechanism-level account: what must be true for the phenomenon to occur, and what that implies is measurable |
| **Hypothesis** | several competing ones, each with a mechanism, assumptions, and what it forbids — including the dull alternatives (nuisance parameter, leakage, variance, protocol, bug) |
| **Prediction** | signed and sized, before the run; ideally about a shape, ordering, or interaction rather than a single number |
| **Experiment** | a design that *discriminates* between the live explanations, with the scientific variable isolated and nuisance parameters re-optimised on every arm |
| **Falsification / validation** | the outcome taken at face value; failures checked against configuration errors, successes against nuisance parameters, leaks, shortcuts, and variance |
| **Iteration** | honest updating, then the next experiment chosen by information per unit of compute — usually the one most likely to falsify the current claim |
| **Finding** | what was discovered, why it is true, under what assumptions, which alternatives were eliminated, and what is genuinely new |
| **Paper** | every claim mapped to its evidence and scoped to exactly the conditions tested |

Throughout, a **Skeptical Reviewer** role — ideally a separate agent, since a critic sharing
your context inherits your blind spots — attempts to falsify each consequential decision, and
must be able to change the plan.

---

## Scope

Domain-general AI/ML research: supervised, self-supervised, and generative modelling,
representation learning, optimization, evaluation methodology, theory, and foundation-model
research. The reasoning is about the *scientific objects* — data, models, representations,
objectives, optimizers, metrics — rather than about any one subfield, architecture, or task.

Coverage includes data provenance and the leakage taxonomy, splits and contamination,
inductive bias and representation claims, objective design and Goodhart effects, optimization
confounds and tuning protocol, scaling-law methodology, experiment design and controls,
ablation and attribution, validity and statistics, model-based judges and prompt sensitivity,
theory hygiene and non-vacuity, reproducibility gates and research engineering, failure
diagnosis, and novelty verification.

**Explicitly discouraged:** benchmark chasing, blind trial-and-error, cherry-picking, weak
baselines, unjustified claims, post-hoc storytelling, metric worship, mathiness, ablation
theatre, and single-run conclusions. Each is listed in the skill with why it is not science
and what to do instead.

---

## Repository layout

```
SKILL.md                    the spine: persona, reality constraint, research loop,
                            non-negotiables, anti-patterns, reviewer protocol, reference map

references/
  first-principles.md            decomposing a problem to mechanism; deriving what to measure
  problem-and-gap.md             choosing a problem worth solving; what a real gap looks like
  novelty-and-ideas.md           generating genuinely new ideas; verifying novelty; tiering claims
  hypotheses.md                  mechanisms, competing hypotheses, signed predictions, falsifiers
  data.md                        provenance, construct match, splits, leakage, contamination, shift
  models-and-representations.md  inductive bias, fair comparison, representation-claim evidence
  objectives-and-optimization.md losses, objective≠metric≠construct, optimization confounds,
                                 tuning protocol, scaling-law methodology
  experiment-design.md           discriminative design, isolation, controls, power, sweeps, cost
  ablations-and-attribution.md   attributing an effect to a mechanism rather than a nuisance
  evaluation.md                  validity, baselines, metrics, uncertainty, generative-model eval
  theory.md                      when theory earns its place; assumptions, vacuity, proof hygiene
  reproducibility.md             gates, determinism, provenance, research engineering discipline
  reading-papers.md              extracting evidence; engaging the strongest rival account
  failure-analysis.md            diagnosing surprises; the recurring AI/ML failure taxonomy
  evidence-and-honesty.md        provenance rules, evidence ledger, anti-fabrication protocol

templates/
  research-plan.md          the plan written before compute is spent
  experiment-header.md      pre-registration that lives in the experiment script's docstring
  decision-log.md           one auditable entry per consequential decision
  evidence-ledger.md        every fact, its label, its artifact, how it was verified
  result-card.md            per-figure documentation: what, why, numbers, limits
```

`SKILL.md` is loaded whenever the skill triggers; the reference files are loaded on demand, so
only the material a given decision needs enters context.

---

## Expected outputs

A project run under this skill leaves an auditable trail, not just a result:

- a **research plan** with the phenomenon, question, first-principles account, competing
  hypotheses, defined success *and* failure conditions, and the executed novelty search;
- a **decision log** where every consequential choice records its evidence, its mechanism, the
  alternative considered, the falsifier, the Reviewer's strongest objection, and the
  resolution — including the decisions the Reviewer changed;
- **pre-registered experiments**, each carrying its question, mechanism, competing
  explanations, signed prediction, falsifier, and a decision rule fixed before the run;
- **raw results namespaced by every varying condition**, with logs for failures as well as
  successes, and figures generated programmatically from those raw outputs;
- **result cards** documenting what each figure shows, why it was made, the underlying
  numbers, what it means, what it does not establish, and its limits;
- an **evidence ledger** in which every load-bearing fact carries an evidential label, its
  artifact or retrieved source, and how it was verified;
- a **falsification log** of the project's own claims that died, how they died, and what
  replaced them;
- a **write-up** in which every claim maps to specific evidence, is bounded by exactly the
  conditions tested, reports full sweeps rather than best points, includes the negative
  results, states the tuning budget and multiplicity exposure, and lists explicit non-claims.

---

## Installation

Copy the repository into your skills directory:

```
~/.claude/skills/rigorous-research/     # personal
.claude/skills/rigorous-research/       # project-scoped
```

so that `SKILL.md` sits at the root of that folder, with `references/` and `templates/`
beside it.

The skill triggers on research activity — starting or steering a project, choosing datasets,
models, objectives or metrics, designing experiments or ablations, assessing novelty,
diagnosing a result, or writing up findings — and can be invoked by name.

---

## License

See [LICENSE](LICENSE).
