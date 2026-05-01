# NOTE — branch `saw-core-lean-p4-wip`

This branch is **parked**. Do not develop on top of it.

## What it is

A snapshot of the second iteration of the universe-polymorphism
approach to translating the SAWCore Prelude as a single
Lean library — "P4 v2" in the chronology in
`saw-core-lean/doc/2026-04-22_universe-problem.md` and
`saw-core-lean/doc/2026-04-22_p4-v2-status.md`.

The architecture: every SAWCore identifier whose source uses
`sort 1` translates to a Lean def with a fresh universe variable
per `sort` occurrence (per-binder freshness), then the surrounding
Lean module quantifies over the union. Inductives whose parameters
mix universes get a `Sort (max 1 u₁ u₂ …)` result sort.

Status when parked: ~100 Lean elaboration errors remained, all
concentrated in the proof-heavy `Eq`/`coerce`/`Pair_fst` cluster
where Lean's non-cumulativity can't paper over SAWCore's `sort 0`
↔ `Prop` traffic. The cluster was demonstrated to be irreducible
in `saw-core-lean/doc/2026-04-22_p6-prop-type-investigation.md`.

## Why it's parked

The whole approach was superseded by the **specialization** pivot
(`saw-core-lean/doc/2026-04-23_specialization-approach.md`). Under
specialization, the SAWCore Prelude is no longer translated as a
universe-polymorphic library at all — `scNormalize` unfolds it
into each user term, eliminating universe pressure at the source.
The P4 machinery this branch adds (`SortVar`, `SortMax1Var`,
`SortMax1Vars`, per-binder fresh universes, universe lists on
`Decl`s) is not reachable from any currently-supported user-facing
command on the post-pivot main branch (`saw-core-lean`).

## When to revisit

A genuinely-universe-polymorphic Cryptol term — one where
`polymorphismResidual` (in `SAWCentral.Prover.Exporter`) refuses
input — is the only forcing function for resurrecting this work.
Such a term would have a Pi-binder at `sort k ≥ 1` after
normalization, which Cryptol's `{a}`-polymorphism does not
produce. If a user lowers a hand-written SAWCore term that does,
the pieces here are the closest existing scaffolding.

Read `saw-core-lean/doc/2026-04-22_p6-prop-type-investigation.md`
**first** before doing any new work — the Prop/Type gap is
fundamental and any new attempt has to address it directly. P4 v2
did not.

## What lives where

- `saw-core-lean/src/Language/Lean/AST.hs` — the universe-
  polymorphic AST extensions (`SortVar`, `SortMax1Var`,
  `SortMax1Vars`).
- `saw-core-lean/src/SAWCoreLean/Term.hs` — `translateSort`'s
  per-call freshening logic.
- `saw-core-lean/src/SAWCoreLean/SAWModule.hs` — the universe-
  collecting walker for translated module-level decls.

The mainline (`saw-core-lean` branch) deliberately keeps the AST
extensions but drops the freshening machinery (`translateSort`
collapses to `Type 0`); resurrection means restoring the
freshening half.
