# Evidence, provenance, and anti-fabrication

The purpose of this file is one guarantee: **when someone uses this skill, the research the
agent did was real.** Not plausible-looking. Real.

An AI agent's characteristic failure is not sloppiness — it is *fluency*. It is entirely
possible to produce a well-structured, confident, correctly-formatted research report in
which a citation points to a paper that does not exist, a number came from no run, a dataset
statistic was reconstructed from memory, and a "reproduction" was never executed. Such output
is worse than no output: it looks like evidence, propagates, and is expensive to detect.

Every rule below exists to make that impossible rather than merely discouraged.

---

## The provenance rule

**Nothing enters a claim without an artifact behind it.**

| Claim type | Required artifact |
|---|---|
| A measured number | The run that produced it: config, seed, code version, log path, raw output file |
| A comparison to prior work | Either their number *with the source retrieved and the protocol checked*, or your own re-run |
| A citation | The retrieved source. Title, authors, venue, year verified against the actual document |
| A quote or a reported finding | The passage, located in the retrieved source |
| A definition or theorem statement | The statement as given by an authoritative source, with its assumptions |
| A property of a dataset, model, or method | Verified against the data card, released code, or paper — retrieved, not recalled |
| A dataset statistic (counts, class balance, length) | Printed by your pipeline, not remembered |
| "This is novel" | The executed search: queries, sources searched, what was found |
| "X is standard practice" | A source, or explicit downgrade to "in my experience / commonly seen" |

If the artifact does not exist, the claim does not get written.

---

## Prohibited absolutely

- **Inventing a citation.** Including plausible-sounding author/venue/year combinations,
  DOIs, or arXiv IDs. If you cannot retrieve it, you do not cite it.
- **Reporting a number from a run that did not execute.** Including "expected", "typical", or
  "representative" values placed where a measurement belongs.
- **Simulating results.** If compute, data, or tooling is unavailable, report the blocker and
  stop. Do not generate an illustrative table.
- **Recalling dataset or model properties as fact.** Sizes, licences, splits, cutoffs, and
  architectures drift and are misremembered. Retrieve them.
- **Presenting an unverified derivation as established.** Label it Hypothesised until checked.
- **Filling a gap in the literature review with something that sounds right.** Say the gap is
  unresolved.
- **Rounding an uncertainty away.** Reporting a mean without its spread when you have the runs
  to compute one.
- **Silently changing a protocol, metric, or split** between what you compared against and
  what you ran.

---

## Scaffolding, when you need it

Sometimes you must write structure before results exist. That is fine, provided the structure
cannot be mistaken for evidence:

- Mark every unfilled slot inline: `PLACEHOLDER — NOT MEASURED`.
- Never populate a placeholder with a number, even a rough one.
- Keep a `PLACEHOLDERS.md` (or a section) listing every one, so none survives to the final
  document.
- Before delivering anything, grep for the placeholder marker and confirm zero remain — or
  state explicitly which remain and why.

---

## The evidence ledger

Maintain one file, appended to as the project runs (`templates/evidence-ledger.md`). One row
per fact your conclusions rest on:

```
ID | Claim | Label | Source / artifact | Verified how | Date | Notes
```

`Label` is the evidential status:

| Label | Meaning |
|---|---|
| **Known** | verified against an external source you actually retrieved |
| **Observed** | measured in our own runs, with the artifact retained |
| **Derived** | follows mathematically from stated assumptions, checked |
| **Hypothesised** | proposed, not yet tested |
| **Speculative** | plausible, weakly supported |
| **Unknown** | insufficient evidence |

Two properties make this worth the effort:

1. **The paper can be checked against it.** Every claim in the write-up should trace to a
   ledger row. Claims with no row are the ones to delete or downgrade.
2. **It makes downgrades visible.** When a Known turns out to be misremembered, or an Observed
   turns out to come from a run with a bug, the ledger shows everything downstream that must
   be revisited.

---

## Verification protocol for external sources

1. **Retrieve the actual document.** Not a search snippet, not an abstract listing, not a
   summary of it, and not your recollection of it.
2. **Confirm the identifiers** — title, authors, venue, year — against the document itself.
3. **Locate the specific claim** in the text. If it is not there, do not attribute it.
4. **Read enough context to preserve the conditions.** A finding detached from its setup is a
   different finding (`reading-papers.md`).
5. **Record what you checked** and what you did not. "Abstract only" is a legitimate note; it
   just limits what the citation can support.
6. **Note disagreement between sources** rather than silently picking one.

If retrieval fails: say so. `[Unverified — could not retrieve source]` in the draft is
correct behaviour, and it is far better than a confident sentence with no basis.

---

## Verification protocol for your own results

- The result exists as a file on disk, produced by a script you can name.
- The script's pre-registration header matches the analysis you are reporting.
- Re-running reproduces it to a stated tolerance.
- The number in the write-up was **read from the artifact**, not transcribed from memory or a
  chat message. Prefer generating tables and figures programmatically from raw outputs, so
  the document cannot drift from the data.
- The uncertainty was computed, not estimated.
- Any post-hoc analysis is labelled as post-hoc.

---

## Correcting yourself

When you find your own error, correct it plainly and immediately, in one or two sentences,
and continue. Do not bury it, do not soften it into ambiguity, do not over-apologise, and do
not quietly delete the affected claim without saying that you did.

Then propagate: check the evidence ledger for everything that depended on the wrong fact.

Corrections often make results *stronger* — an invalid defence replaced by a real control, or
a hasty falsification overturned on replication. When that happens, say so too.

---

## Language discipline

Bound every claim by the conditions actually tested, and keep these distinctions:

- "measured at chance" ≠ "proved impossible";
- "no measurable effect at n = 4" ≠ "no effect";
- "significant on 2 of 12 conditions, neither replicating" ≠ "significant";
- "improves on our benchmark suite" ≠ "improves generally";
- "the probe decodes it" ≠ "the model uses it";
- "correlates with" ≠ "causes";
- "we did not run X" is a sentence you must be willing to write.

State the number of datasets, seeds, scales, and architectures, and the class of methods the
conclusion covers. If a sentence would survive deleting those bounds, it is overclaiming.
