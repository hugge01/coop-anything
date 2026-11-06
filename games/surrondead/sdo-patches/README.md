# SDO patches (AGPL-3.0-or-later)

Patches against [DatGuySnowfox/SDO](https://github.com/DatGuySnowfox/SDO) pin
`5d9a73d2b05b888abf64990eb003b6446abdf0d9` (same as Hugo’s `D:\coop-test\SDO`).

These are **opt-in zombie proxy improvements** from our revive plan
(`internal/zombie-visibility-fix-plan.md`). They do **not** ship game assets.

## Patches

| File | What |
| --- | --- |
| `0001-zombie-proxy-smallest-levers.patch` | Type-check `FindObject` fallback in `resolve_class_by_name`; flag-gate zombie proxies (`enable_zombie_proxies.flag`); clear actor on teleport SEH; cap 12 zombie actors; Roamer-only spawn when enabled |

## Apply on Windows test copy (ask before build)

```powershell
cd D:\coop-test\SDO
git status   # expect clean @ 5d9a73d
git apply --check <path-to>\0001-zombie-proxy-smallest-levers.patch
git apply <path-to>\0001-zombie-proxy-smallest-levers.patch
# then xmake build + deploy.ps1 into COPY Win64 only — never live Steam
```

## Enable at runtime (after deploy)

Default remains **off** (safe). To test:

```powershell
New-Item -ItemType File -Force "$env:APPDATA\SDO\enable_zombie_proxies.flag"
```

Remove the flag to disable without rebuilding.

## License

Derivative of SDO → AGPL-3.0-or-later. If these run on a public gateway, publish the
modified source (network clause). Prefer upstream PR to DatGuySnowfox/SDO once proven.
