# Engineering discipline for research code

Research conclusions are only as trustworthy as the runs that produced them.

## Namespace every output by every variable that distinguishes it

**What went wrong.** Result files were keyed on dataset but not architecture. A second-model
run silently **overwrote** the first model's results in place, with no error. It then
happened a *second* time, because the fix was pushed to one machine as individual files
rather than as a directory sync.

**Rules.**
- Every output path includes every condition that distinguishes the run (dataset,
  architecture, seed, variant). Derive the tag once, next to argument parsing.
- Prefer failing loudly over overwriting silently.
- When files of ambiguous provenance appear, **quarantine, regenerate, and verify against
  recorded values** rather than guessing which is which.
- Sync code as a **directory**, never file-by-file. `rsync a/b/c.py host:dir/` puts it at
  `dir/c.py`, not `dir/b/c.py` — a silent path bug.

## Verify a patch landed before depending on it

A patch applied inside a compound command that exited early never landed; a long run then
depended on it and wasted an hour. **`grep` for the change before starting anything long.**

## Process management footguns

- `pkill -f <pattern>` on a wrapper leaves the child process running. Verify with `ps`, then
  kill explicit PIDs.
- `pgrep -f X` matches **its own command line** — the "leftover process" may be your own
  check. Verify a different way before acting.
- Queues that wait on each other by pattern-matching process names **deadlock** when the
  watcher matches the watched. Prefer one sequential chain, or a sentinel file.
- A timed-out launch command may still have launched. **Check before relaunching** —
  duplicates silently corrupt throughput and results.

## Precision

- Use float64 and disable reduced-precision matmul when the number *is* the evidence.
- Watch for catastrophic cancellation in any expression of the form `(a − b)/(c − d)` where
  both differences vanish together; find the algebraically stable form.
- `torch.tensor(np_float32_array)` stays float32 — `set_default_dtype` does not override it.

## Cost structure

Find the quantity that is constant across your experiments and compute it once. In one
project, recognising that a frozen encoder made features constant allowed caching them once
per sample; every subsequent experiment became a matrix multiplication instead of a forward
pass. That single observation made 972-setting sweeps and 12-run replications affordable.

Look for this early — it determines how much science you can actually do.

## Parallel compute

Split work by dataset across machines, chain dependent stages with a sentinel file, and
confirm each launch actually started before assuming it did.
