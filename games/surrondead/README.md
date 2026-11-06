# SurrounDead (2022, Early Access)

| | |
|---|---|
| Engine | Unreal Engine 5.6 (0.8.x; was 5.3) |
| Versions tested | Hugo Steam build **26471822** (Win10 Pro); test copy + pinned UE4SS; SDO @ `5d9a73d` (`main.dll` SHA `6E83B911…8854`); Node **25.2.1** two-client smoke on copy (ports 42200/42201) |
| Projects | [SDO (SurrounDead Online)](https://github.com/DatGuySnowfox/SDO) (directory primary); listen-server experiments [SurroundeadMP / SDMP](https://github.com/FullerK55/SurroundeadMP), [Koopz8/SurrounDeadMP](https://github.com/Koopz8/SurrounDeadMP), [Seebrasse345/SurrounDeadMP](https://github.com/Seebrasse345/SurrounDeadMP) |
| QA level | none (manual two-client smoke only — **not** automated) |
| Last updated | 2026-11-06 |

## Where things stand

Friends cannot play official co-op today (Steam lists single-player; co-op is still a planned future feature). The furthest open fan project is **SDO**: a UE4SS C++ mod plus a Node.js dedicated server. On Hugo’s build **26471822** test copy we confirmed two copy clients can auth to a local Node host, latch session, show proxies, and sync clothing equip (still on FirstJoin Barber; free move / zombies / combat not tried). Author’s earlier 5.6 notes still apply for invisible zombies and unverified combat/vehicles. Path: **revive SDO**, not a third architecture.

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

Nothing in the live game. On a **test copy**: pinned UE4SS experimental; SDO cloned @ `5d9a73d`, built with xmake 3.1.2 (`gen_import_lib.ps1` then `xmake build`), deployed via `deploy.ps1` into copy `ue4ss\Mods\SDO\` only. Ran a local Node host + two copy clients for smoke (torn down after).

## QA

- Level: **none** — one **manual** two-client smoke on the owner’s Windows test copy (2026-11-06). **Not** an automated harness; do not set directory `qa: automated`.
- Asserted (logs): both clients UE4SS+SDO loaded; auth + session latch; health `players:2`; proxy spawn; clothing equip sync. Partial: world reached via auto `ContinueGame`, but FirstJoin Barber blocked free-move check. Unknown / not tried: on-screen peer after Barber, weapon-in-hand visual, zombies, combat, vehicles.
- Caveat: smoke used real `%LOCALAPPDATA%\SurrounDead\Saved` (out-of-tree backup exists). Default ticket TTL **120s** is tight for dual boot — mint fresh tickets immediately before launch.
- Details: `games/surrondead/NOTES.md` §10c; store report `internal/phase2-node-smoke-report.md`.

## How to play

Not ready for friends. Local revive path: UE4SS experimental on a **test copy**, build SDO `main.dll`, Node 22+ server on **42200/42201**, ticketed join launching the **copy** exe with `-sdo_*` (never `steam://run` / live Steam folder).

## Known issues

Inherited from SDO (author): invisible zombies on clients; building hooks stale on 5.6; vehicles untested; combat/death not re-verified on 5.6; setup is manual. Our smoke: FirstJoin Barber on new `playerId`s; ticket TTL surprises; shared `%APPDATA%\SDO` logs across dual instances.

## Next steps

Owner/coordinator deciding next. Likely: dismiss Barber + free-move peer check; then zombie-proxy / 0.8.0 rename work; longer combat/vehicles session when OK’d.
