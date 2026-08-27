# Result card template

One per figure or table, stored beside it. It makes the result auditable without re-reading
pixels, and it forces the separation between what was measured and what it means.

```markdown
# <figure or table filename>

**Experiment.** E<N> — <the question this belongs to>
**Produced by.** <script path> @ commit <hash>, run <date>
**Inputs.** <raw output paths, log paths, data version>
**Regenerate with.** <exact command>

---

## What it shows
Axes and units. What a single point / row is. What the error bars or bands are over
(seeds? data resamples? both?). How many runs per point. What is held fixed.

## Why it was made
The question it answers, and which competing explanation it discriminates against.

## The numbers
<table of the underlying values, with uncertainty — so the figure can be checked>

## What the result means
Reading of the data. Include what is NOT visible: null results, conditions where the effect
is absent, and anything within the variance floor.

## Justified conclusion
Only what this data supports, bounded by the conditions actually tested (datasets, scales,
architectures, seeds, method class).

## Alternatives not yet excluded
What could still explain this, and what would exclude it.

## Limits
Negative and inconclusive findings. Power: could this design have detected the effect if it
were real? Known confounds. Protocol deviations.
```

## Discipline

- **Write the card when the figure is made**, not at paper-writing time. The context is gone
  by then, and reconstructing it is where errors enter.
- **Generate the figure and its numbers programmatically from raw outputs**, so the card and
  the document cannot drift from the data.
- **Never silently edit a card** when a later experiment revises the interpretation. Add:

  ```
  > **SUPERSEDED** by E<M>, <date>: <what changed and why>. Retained for the record.
  ```

  The revision history is itself evidence of rigour.
- **Cards exist for negative results too.** Especially for negative results — that is where
  the diagnosis lives, and diagnosed failures accumulate into findings.
