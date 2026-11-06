# Building sync — README vs source @ `5d9a73d`

## Observation

SDO README “Broken / disabled” still says `SpawnBuild` / `Svr_SpawnBuild` are looked
up on the **pawn** (moved to `BuildingComponent` on 5.6).

In source pin `5d9a73d`, `mod.cpp` already:

- Resolves `BuildingComponent` via `obj_prop(pawn, "BuildingComponent")`
- Hooks `SpawnBuild` + `Svr_SpawnBuild` on that component
- Author comment: **NOT YET LIVE-VERIFIED**

5.6 dump confirms both functions on `BuildingComponent.hpp`. Pawn property type is
`UBP_JigComponent_C* BP_JigMultiplayer` (rename of class, property name kept).

## Derived

No AGPL “rehome” patch until Phase **3c** live smoke proves hooks never fire.
If 3c FAIL with `SpawnBuild=NOT FOUND` or never `handle_build_hook`, then write `000x-building-…`.

## Wanted test

See store `internal/phase3c-building-smoke-brief.md`.
