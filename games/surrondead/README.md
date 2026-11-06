# SurrounDead (2022, Early Access)

| | |
|---|---|
| Engine | Unreal Engine 5.6 (0.8.x; was 5.3) |
| Versions tested | Hugo Steam build **26471822** (Win10 Pro); test copy + pinned UE4SS; SDO @ `5d9a73d` (`main.dll` SHA `6E83B911…8854`); Node **25.2.1**; manual two-client smokes on copy (ports 42200/42201), including deeper in-world pass |
| Projects | [SDO (SurrounDead Online)](https://github.com/DatGuySnowfox/SDO) (directory primary); listen-server experiments [SurroundeadMP / SDMP](https://github.com/FullerK55/SurroundeadMP), [Koopz8/SurrounDeadMP](https://github.com/Koopz8/SurrounDeadMP), [Seebrasse345/SurrounDeadMP](https://github.com/Seebrasse345/SurrounDeadMP) |
| QA level | none (manual two-client smokes only — **not** automated) |
| Last updated | 2026-11-06 |

## Where things stand

Friends cannot play official co-op today (Steam lists single-player; co-op is still a planned future feature). Furthest open fan project: **SDO** (UE4SS C++ + Node dedicated). On Hugo’s **26471822** test copy, two clients auth, latch session, move past Barber, and **see each other** (Hugo-confirmed). Clothing equip sync works. Zombies stay invisible (SDO disables client zombie proxies — confirmed). Combat/vehicles still weak/untested. Path: **revive SDO**, not a third architecture.

## Existing work

- **[SDO](https://github.com/DatGuySnowfox/SDO)** (AGPL-3.0-or-later, DatGuySnowfox) — host-authority dedicated server. Last push 2026-09-27 (mid-port to UE 5.6 after experimental 0.8). No GitHub releases; build from source. Bug list is the README status tables. [Reddit announcement](https://www.reddit.com/r/SurrounDead/comments/1wpau62/unofficial_coop_mod_for_surroundead_open_source/) (2026-09-24).
- **[SurroundeadMP / SDMP](https://github.com/FullerK55/SurroundeadMP)** — UE4SS Lua listen-server on leftover SteamSockets/IpNetDriver. Reached two-PC join over Radmin on **UE 5.3** (2026-10-26); SteamSockets handshake stalls; no license stated; launcher auto-fetches GitHub zip. Research value high; not the shipping target for 0.8.0.
- **[Koopz8/SurrounDeadMP](https://github.com/Koopz8/SurrounDeadMP)** — recon only (“nothing playable yet”), documents SteamSockets + Jigsaw replication.
- **[Seebrasse345/SurrounDeadMP](https://github.com/Seebrasse345/SurrounDeadMP)** (+ MP-Fix) — older UE5.3 spawn/possession fixes for listen join.
- **Official** — planned multiplayer/co-op; not shipping as of Patch 0.8.0 (2026-11-05).
- Security: official Discord warned about a separate circulated “co-op mod” as malware (~2026-09). Prefer AGPL SDO from GitHub with pinned hashes only.

Working notes: [NOTES.md](NOTES.md). Claim: [cranebeaker/coop-anything#334](https://github.com/cranebeaker/coop-anything/issues/334).

## How the game works (for co-op)

- UE 5.6 Shipping, Blueprint-heavy gameplay, UE4SS as the practical hook.
- SteamSockets net driver and OnlineSubsystemSteam are compiled in; session/game-mode MP UI is incomplete (`BP_MPGameMode` referenced but missing from the pak per community recon).
- Jigsaw Inventory ships substantial replication RPCs; EasyMultiSave for saves under `%LOCALAPPDATA%\SurrounDead\Saved`.
- SDO bypasses unfinished engine session code: clients talk to a Node gateway (TCP **42200** / HTTP **42201** in current `server/src/config.js`; older README still says 31000/31001) that owns world state.

## What we changed

Nothing in the live game. On a **test copy**: pinned UE4SS experimental; SDO cloned @ `5d9a73d`, built with xmake 3.1.2, deployed into copy `ue4ss\Mods\SDO\` only. Local Node host + two copy clients for manual smokes (torn down after). Debug meetups via `%APPDATA%\SDO\teleport.flag` (SDO `check_teleport_trigger`).

## QA

- Level: **none** — **manual** two-client smokes on the owner’s Windows test copy (2026-11-06). **Not** an automated harness; do not set directory `qa: automated`.
- Phase 2c: auth + session latch; health `players:2`; clothing equip sync; stuck on FirstJoin Barber.
- Phase 2d: Barber dismissed + free WASD **PASS**; mutual visibility **PASS** (Hugo); combat **partial** (fired, no confirmed hits in safezone); zombies **confirm known gap** (invisible; `spawn_zombie_actor` disabled); vehicles **not tried**; Saved LOCALAPPDATA isolation **partial fail** (real profile still written).
- Caveats: raise `SDO_TICKET_TTL_MS` for dual boot; mint tickets immediately before launch; Esc opens pause and blocks input automation; teleport.flag is shared APPDATA (one consumer per write).
- Details: `games/surrondead/NOTES.md` §10c–§10d; store reports `internal/phase2-node-smoke-report.md`, `internal/phase2-deeper-ingame-report.md`.

## How to play

Not ready for friends yet (no polished howto). Local revive path: UE4SS experimental on a **test copy**, build SDO `main.dll`, Node 22+ on **42200/42201**, ticketed join of the **copy** exe with `-sdo_*` (never `steam://run` / live Steam). Optional meetup: write `x y z [yaw]` to `%APPDATA%\SDO\teleport.flag` once per client (sequential).

## Known issues

Invisible zombies on clients (`spawn_zombie_actor` returns null — expected until re-enabled). Combat/death not proven outside safezone. Vehicles untested here. Building hooks stale on 5.6 (author). Saved isolation via process `LOCALAPPDATA` override failed on PS 5.1 in our run. Ticket TTL default 120s; shared `%APPDATA%\SDO` across dual instances.

## Next steps

1. Apply `games/surrondead/sdo-patches/0001-zombie-proxy-smallest-levers.patch` on the test-copy SDO tree, rebuild/deploy, retest with `enable_zombie_proxies.flag` (brief: store `internal/phase3-zombie-proxy-build-brief.md`).
2. If Roamer proxies stable: Dog/Boss, then combat; else keep flag off and capture crash evidence.
3. Broader gaps per SDO docs: vehicles, building hook, server inventory restore; friend howto after a stable pass.
