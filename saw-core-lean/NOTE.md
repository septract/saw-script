# NOTE — branch `saw-core-lean-p6-wip`

This branch is **parked**. Do not develop on top of it.

## What it is

A snapshot of the third (and last) iteration of the universe-
polymorphism approach: "P6", an attempt to close the residual
Prop/Type gap that P4 v2 left open. The plan was to
universe-polymorphize the SAWCore-side inductive *parameters*,
not just their result sorts, so that `Eq`'s `(t : sort 1)` arg
could be instantiated at `Prop` or `Type 0` independently per
call site.

This branch carries the first attempt:
`7a51eb079 WIP: P6a first attempt — regressions beyond agent's
validated patterns`. The approach landed regressions in places
the design hadn't validated, and the cluster of failing
`Eq`-family elaboration errors didn't shrink. Documented in
`saw-core-lean/doc/2026-04-22_p6-prop-type-investigation.md`.

## Why it's parked

The P6 investigation doc is the deliverable that justified the
**specialization** pivot
(`saw-core-lean/doc/2026-04-23_specialization-approach.md`). Once
specialization was in scope, P6's incremental fixes to a Prelude
translation that wasn't going to be emitted anyway became dead
work.

The investigation doc itself remains useful — it's the single
clearest explanation of why Lean's non-cumulativity makes any
attempt to translate the SAWCore Prelude as one library
fundamentally hard. Future maintainers should read it before
proposing a Prelude-as-library architecture.

## When to revisit

Same answer as `saw-core-lean-p4-wip`: only if a real user term
forces support for genuinely-universe-polymorphic SAWCore. P6
went further than P4 v2 in realizing that the right fix is at the
inductive-declaration site (not just at use sites), so if
resurrection happens, P6's commits are likely a better starting
point than P4's.

The earlier context — what makes this hard, what didn't work, why
specialization sidesteps it — is in:

- `saw-core-lean/doc/2026-04-22_universe-problem.md`
- `saw-core-lean/doc/2026-04-22_universe-internal-investigation.md`
- `saw-core-lean/doc/2026-04-22_universe-external-research.md`
- `saw-core-lean/doc/2026-04-22_p6-prop-type-investigation.md`

Read those first.
