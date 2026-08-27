# Evidence ledger template

One file for the whole project. Every fact your conclusions rest on gets a row. Every claim in
the final write-up must trace to one; claims with no row get deleted or downgraded.

This is the mechanism that makes fabrication structurally difficult rather than merely
discouraged — see `references/evidence-and-honesty.md`.

```markdown
# Evidence ledger — <project>

| ID | Claim | Label | Source / artifact | Verified how | Date | Notes |
|----|-------|-------|-------------------|--------------|------|-------|
| K1 | <external fact> | Known | <full citation + URL/DOI> | retrieved PDF; claim on p.N; identifiers checked | | |
| K2 | <external fact> | Unknown | — | could not retrieve; NOT used in claims | | |
| O1 | <our measurement> | Observed | results/raw/<path>; commit <hash>; seed(s) <...> | re-ran, matched to <tol> | | n=..., ±... over <seeds/splits> |
| D1 | <derived result> | Derived | derivation in <file>; numeric check in <script> | float64 check, residual <...> | | assumptions: ... |
| H1 | <proposed mechanism> | Hypothesised | — | test planned in E<N> | | |
| S1 | <plausible reading> | Speculative | — | not tested | | not to appear as fact |
```

## Labels

| Label | Meaning | May appear in the paper as |
|---|---|---|
| **Known** | verified against a source you actually retrieved | a cited fact |
| **Observed** | measured in our runs, artifact retained | a result, with uncertainty |
| **Derived** | follows from stated assumptions, and checked | a proposition, with assumptions |
| **Hypothesised** | proposed, not yet tested | an explicitly labelled hypothesis |
| **Speculative** | plausible, weakly supported | discussion only, marked as such |
| **Unknown** | insufficient evidence | "we could not determine ..." |

## Rules

- **A row is created when the artifact exists**, not when the claim is made.
- **Never upgrade a label without new evidence.** Downgrades happen and must be recorded.
- **When a row is downgraded or removed**, find everything downstream that depended on it. The
  ledger exists largely to make this possible.
- **Unverifiable is a valid state.** `Unknown` with a note on what you tried is honest and
  useful; a confident sentence with no basis is not.
- **Record what you checked and what you did not.** "Abstract only" limits what a citation can
  support, and saying so costs nothing.

## Final pass before writing up

1. Every claim in the draft maps to a row.
2. Every `Observed` row's number was re-read from its artifact, not from memory.
3. Every `Known` row's source was retrieved and its identifiers verified.
4. Every `Speculative` and `Hypothesised` item in the draft is labelled as such in the prose.
5. No `PLACEHOLDER — NOT MEASURED` markers remain.
