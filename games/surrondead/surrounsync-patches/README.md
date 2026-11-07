# SurrounSync patches (hugge01)

Base: **SYMPLYSCK/SurrounSync `v1.1.0`** (`351a1f2`).  
License: all rights reserved — Hugo has **written author OK to modify**. Keep attribution; do not republish as original work.

Working fork notes: https://github.com/hugge01/SurrounSync (MCP-writable; full git push from this cloud token may be denied — apply these patches onto a local clone of upstream v1.1.0).

## 0001 — join FPS / vehicle absorb + soft autosave/weather

File: `0001-v110-join-fps-vehicle-absorb-autosave-weather.patch`

| Change | Why |
| --- | --- |
| `util.lua` absorb unknown group children | Avoid forever one-`FindAllOf`-per-class after game updates (join hitch; Humvee2 lesson) |
| `vehicles.lua` scan learned `Vehicle_*` | New car types sync instead of guest duplicate spawn |
| `config.lua` `checkpoint_min_interval` 90→150 | Soft lever for autosave/ghost hitch (Reddit) |
| `world.lua` `JUMP_CONFIRM` 1.5→2.5 | Soft lever for weather stall flicker (Reddit) |

**Not patched (need 1.1.0 smoke repro):** infestations residual, drop/loot edge cases beyond 1.1 claims, vehicle “launch” glider/enter, join FPS beyond group walks.

Apply on a test copy only (`D:\coop-test-surrounsync`). Never live Steam. Never mix with SDO Win64.
