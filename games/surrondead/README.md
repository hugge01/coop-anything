# SurrounDead (2022, Early Access)

| | |
|---|---|
| Engine | Unreal Engine 5.6 (0.8.x; was 5.3) |
| Versions tested | none yet on this fork — public research against Patch 0.8.0 notes and mod READMEs only |
| Projects | [SDO (SurrounDead Online)](https://github.com/DatGuySnowfox/SDO) (directory primary); listen-server experiments [SurroundeadMP / SDMP](https://github.com/FullerK55/SurroundeadMP), [Koopz8/SurrounDeadMP](https://github.com/Koopz8/SurrounDeadMP), [Seebrasse345/SurrounDeadMP](https://github.com/Seebrasse345/SurrounDeadMP) |
| QA level | none |
| Last updated | 2026-11-06 |

## Where things stand

Friends cannot play official co-op today (Steam lists single-player; co-op is still a planned future feature). The furthest open fan project is **SDO**: a UE4SS C++ mod plus a Node.js dedicated server. On UE 5.6 the author verified two clients moving and seeing each other with clothing/weapons; zombies stay invisible to clients, and combat/vehicles need re-testing after Patch 0.8.0. This fork’s path is to **revive SDO**, not start a third architecture.

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
- SDO bypasses unfinished engine session code: clients talk to a Node gateway (TCP 31000 / HTTP 31001) that owns world state.

## What we changed

Nothing in-game yet. Research only: identity, directory entries, networking decision (SDO dedicated), UE4SS digest pins, test-copy steps for the owner’s PC.

## QA

- Level: **none**. No clients launched from this environment (no game install here).

## How to play

Not ready. When SDO is built against the owner’s 0.8.0 test copy, follow upstream SDO README (UE4SS `experimental-latest`, Node 22+, xmake build, ticketed join). Do not install into the live Steam folder.

## Known issues

Inherited from SDO (author): invisible zombies on clients; building hooks stale on 5.6; vehicles untested; combat/death not re-verified on 5.6; setup is manual. Patch 0.8.0 stable may need a fresh name/offset pass after SDO’s September experimental work.

## Next steps

1. Owner records Steam `buildid` + SHA-256 of the exe on a **test copy** (steps in NOTES.md).
2. With owner OK, pin-download UE4SS experimental into the copy and smoke-boot.
3. Build/deploy SDO; two-client smoke on 0.8.0; attack the zombie-proxy and 0.8.0 rename gaps.
