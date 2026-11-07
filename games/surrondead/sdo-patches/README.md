# SDO patches (AGPL-3.0-or-later)

Patches against [DatGuySnowfox/SDO](https://github.com/DatGuySnowfox/SDO) pin
`5d9a73d2b05b888abf64990eb003b6446abdf0d9` (Hugo’s `D:\coop-test\SDO`).

Hugo blanket OK (2026-11-06): apply/build/retest on **test copy** without further owner asks.
**Never** live Steam. Prefer upstream PR to DatGuySnowfox once proven.

## Patches

| File | What |
| --- | --- |
| `0001-zombie-proxy-smallest-levers.patch` | Type-check `FindObject` fallback; opt-in `enable_zombie_proxies.flag` (default off); SEH actor clear; max 12 Roamer-only proxies; one-shot `SYNC_DIAG` entity counts |
| `BUILDING-STATUS.md` | README vs source: BuildingComponent hook already present — no rehome patch until 3c fails |

## Apply on Windows test copy

```powershell
cd D:\coop-test\SDO
git rev-parse HEAD   # 5d9a73d…
git apply --check D:\coop-test\tools\0001-zombie-proxy-smallest-levers.patch
git apply D:\coop-test\tools\0001-zombie-proxy-smallest-levers.patch
# VsDevCmd x64; gen_import_lib.ps1 if needed; xmake build
.\scripts\deploy.ps1 -Win64 "D:\coop-test\SurrounDead\SurrounDead\Binaries\Win64" -SkipBuild
```

## Enable zombie proxies (after deploy)

```powershell
New-Item -ItemType File -Force "$env:APPDATA\SDO\enable_zombie_proxies.flag"
```

Default remains **off**. Look for `SYNC_DIAG:` in `%APPDATA%\SDO\debug.log` once entities arrive.

## Resume brief

Store: `internal/phase3a-windows-resume-brief.md` (when Windows worker is back).
