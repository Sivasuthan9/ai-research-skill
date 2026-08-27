# Genuine novelty: generating ideas, and verifying they are new

Two distinct jobs. Generation without verification produces confident reinvention;
verification without generation produces nothing to verify.

---

## Part 1 — Generating ideas that are actually new

Recombination ("transformer + diffusion + RL for task T") is the default mode and is almost
never novel in any sense that matters, because the space is small, obvious, and already
being searched exhaustively by everyone else. Ideas with real novelty come from a different
place: **a mechanism nobody has articulated, or a measurement nobody has made.**

### Generators that reliably work

**Invert the question.** Instead of "how do we make M better at T", ask "what would have to
be true for M to be *unable* to do T?" Impossibility reasoning produces sharp, testable
claims and often reveals that the entire class of attempted solutions was misdirected.

**Attack the assumption everyone inherits.** Identify a practice adopted because it worked
in some original setting, and ask whether the conditions that justified it still hold. Data
scale, model scale, architecture, objective, and evaluation have all changed since most
standard practices were established.

**Find the quantity that is never measured.** Fields develop blind spots around quantities
that are hard or unglamorous to measure. If everyone reasons about quantity Q and nobody
measures it, measure it. This is one of the highest-yield moves available.

**Exploit an asymmetry between what is optimised and what is reported.** Whenever these are
different objects — different inputs, granularity, normalisation, or population — the gap
is a research object, and it is often larger than the effects people study.

**Take a failure and demand a mechanism.** Every unexplained failure is a claim that
something about your model of the system is wrong. Chase it until you have a mechanism that
predicts the failure *and predicts something else you can check*.

**Transfer a mechanism, not a method.** Importing an algorithm from another field is
recombination. Importing the *reason* the algorithm works — and checking whether the
conditions that made it work there hold here — is science. The check is the contribution;
often the answer is no, and that is itself a finding.

**Change the level of abstraction.** A phenomenon described at the level of accuracy may
have a clean description at the level of representations, gradients, data statistics, or
information content. Moving to the level where the phenomenon is simple is frequently the
whole insight.

**Look for the case that should not work but does — or works but should not.** Anomalies are
where mechanisms are exposed.

### Discipline on generated ideas

For each candidate, before it earns compute, write:

- the **mechanism** (why it would work, in terms of data / model / objective / optimizer /
  evaluation);
- the **risky prediction** it makes — something that could come out the other way;
- the **cheapest experiment that could kill it**;
- the **closest existing work**, named, having actually searched;
- what remains true and interesting **if the idea fails**.

Generate several competing ideas and compare them on information-per-compute, not on
appeal. Kill freely. Ideas are cheap; compute and credibility are not.

---

## Part 2 — Verifying novelty

"It sounds different" is not evidence. Neither is "I have not seen it." Neither is a model's
recollection: **do the search, retrieve the sources, read them.**

### Search dimensions

- **Exact terminology**, and the synonyms other communities use for the same object.
- **Mathematically equivalent formulations** — the same operation under a different name, a
  different parameterisation, or a different derivation.
- **The mechanism, not the framing.** Many methods implement one idea in different language.
  Search for what the method *does*, described neutrally.
- **Adjacent fields.** Statistics, information theory, signal processing, econometrics,
  operations research, psychometrics, ecology, and control theory re-derive the same
  estimators, divergences, diversity indices, and concentration measures constantly.
- **Classical literature.** Neural-network ideas are frequently decades old under other
  names. Check pre-2012 work explicitly; search engines bias recent.
- **Recent preprints**, not only published venues — the gap between arXiv and proceedings is
  where collisions happen.
- **Negative results and workshop papers**, where "we tried this and it did not work" lives.

Record what you searched, where, and what you found. A novelty claim with no recorded search
is an assertion.

### The question to answer

> Has essentially **this exact scientific idea** — this mechanism, this claim, this
> measurement — already been proposed, derived, or experimentally demonstrated?

If the answer is uncertain, **label the claim uncertain**. Never resolve uncertainty in your
own favour.

### Tier the claim

Separating what is new from what is standard is a strength, not a concession.

| Tier | Meaning |
|---|---|
| **Genuinely new** | not found anywhere after a documented search; derived and verified here |
| **New but elementary** | new as far as we can tell, but follows quickly from standard tools |
| **Known result, new evidence** | the claim existed; we are the first to test or measure it properly |
| **Standard tool, new application** | the technique is classical; applying it *here* is the contribution |
| **Unification** | previously separate results shown to be instances of one thing |
| **Refutation / correction** | an accepted claim shown to be wrong, or to hold only conditionally |
| **Explicit non-claims** | what we are *not* claiming, stated so reviewers need not infer it |

An explicit non-claims list ("we do not claim X is achievable; we do not claim no method can
do Y — we claim methods of class Z cannot, and we measured that") pre-empts the most damaging
referee objections and costs nothing but honesty.

### If you were scooped

Say so, immediately and plainly, and re-scope. Options that remain genuinely valuable:

- an independent replication (rarer and more valuable than it should be);
- the mechanism they demonstrated but did not explain;
- the conditions under which their result does *not* hold;
- a stronger or simpler proof, or a tighter bound;
- the measurement they assumed rather than made.

Discovering the collision early is cheap. Discovering it in review is not.

### Awkward-fit honesty

If your best analysis characterises something you later show does not matter, say so and
reposition it as the *explanation* rather than the headline. Hiding the tension is worse: a
sharp reviewer will find it, and then everything else looks suspect too.
