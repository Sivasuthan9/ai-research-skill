# Theory in AI/ML research

Theory earns its place when it **changes a decision**: predicts something you would not
otherwise have predicted, forbids something, explains a discrepancy, or tells you what to
measure. Formalism that decorates an empirical result adds risk without adding evidence.

Before writing mathematics, answer: *what does this let me conclude that I could not conclude
without it?* If the answer is "it makes the paper look rigorous," delete it.

---

## Mathiness — the failure mode to avoid

Mathematics used to impress rather than clarify: symbols that are never used, theorems whose
assumptions do not hold in the setting studied, notation that obscures where the real
assumption lives, and formal statements loosely connected to the empirical claims they
supposedly support.

Diagnostics for your own writing:

- Can you state, in one sentence, **which step of the proof does the work**? If not, you do
  not yet understand your own result.
- Does every symbol appear in a load-bearing role?
- Would the empirical section be unchanged if the theory were deleted? If yes, they are not
  connected, and you must either connect them or present them as separate contributions.
- Are the theorem's assumptions *stated near the claim*, or buried where a reader will carry
  the conclusion away without them?

---

## Assumptions are the whole game

A theorem is a conditional. The interesting question is never whether the proof is valid; it
is whether the antecedent holds in the systems you care about.

For each assumption, state:

- **what it says**, plainly;
- **why it is needed** — which step breaks without it;
- **whether it holds in practice**, and how you know. "Commonly assumed" is not evidence;
- **what happens when it fails** — does the conclusion degrade gracefully, or collapse?
- **whether you can measure the violation.** Measuring how badly a standard assumption is
  violated in real systems is frequently a better contribution than the theorem.

Assumptions that routinely fail in deep learning and are routinely assumed anyway:
independence or exchangeability of examples, i.i.d. train/test, convexity, infinite width or
depth limits, gradient flow rather than discrete steps, exact optimisation, unlimited data,
well-specified models, and independence between the tuning procedure and the evaluation.

**Naming which assumption your competing account violates is often the sharpest scientific
move available** — sharper than proving a new theorem.

---

## Bounds

- **Is it non-vacuous** in the regime you care about? A bound exceeding the trivial range, or
  requiring more data than exists, states a fact about the proof technique, not about the
  system.
- **Is it tight, and where?** Report where it is tight and where it is loose. Plot it against
  the empirical quantity across the range you can measure.
- **Which direction does it constrain?** Upper bounds forbid; lower bounds guarantee. Be
  precise about which you have, because they support opposite kinds of claims.
- **Does it depend on quantities you can compute?** A bound in terms of unmeasurable
  quantities cannot be checked and cannot guide a decision.
- **Impossibility results are valuable and under-supplied.** A rigorous statement that no
  method of a given class can achieve X — with the assumptions that define the class stated
  precisely — is often worth more than another method in that class. Be exact about the class:
  the common failure is proving something about a narrow class and stating it about all
  methods.

---

## Connecting theory to experiment

The two should constrain each other. Useful patterns:

- **Theory predicts, experiment tests.** Derive a quantitative, signed prediction from the
  theory and test it in a regime where alternatives predict something different. This is the
  strongest use of theory.
- **Experiment reveals, theory explains.** After an empirical finding, a mechanism-level
  account that *also* predicts something new — which you then test — is a real contribution.
  An account that only explains what you already saw is post-hoc storytelling.
- **Theory bounds the search.** A result showing that a whole family cannot work redirects
  effort. Say so plainly and act on it.
- **Experiment measures the assumption.** Take a theoretical requirement people acknowledge
  but never measure, and measure it.

Anti-pattern: proving something in a simplified setting, then presenting empirical results
from a setting where the simplification does not hold, with no discussion of the gap. If you
must do this, name the gap explicitly and say what would be needed to close it.

---

## Proof hygiene

- **Verify numerically before believing analytically, and vice versa.** Implement the
  quantity, check the identity or bound at small scale in double precision, and check that
  it fails where the assumptions fail. A theorem that appears to fail numerically is more
  often a precision problem than a wrong theorem — and more often a wrong theorem than one
  hopes. Check both.
- **Check edge cases**: zero, one, degenerate dimensions, boundary values of parameters,
  and the limits where terms cancel.
- **Watch for catastrophic cancellation** in any expression subtracting near-equal
  quantities; use the algebraically stable form.
- **Do not present an unverified derivation as established.** Label it Derived only if you
  have actually checked it; otherwise it is Hypothesised.
- **Re-derive rather than recall.** Standard results are frequently misremembered in a way
  that changes their conditions. Retrieve the statement from a source.

---

## Honest positioning

If your analysis turns out to characterise something you later show does not matter, say so
and reposition it as the *explanation* rather than the headline. Reviewers find such tensions,
and an unacknowledged one makes everything else in the paper look suspect.
