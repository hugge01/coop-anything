# SurrounDead — Phase 0 triage notes

Started 2026-11-06. Following AGENTS.md and playbook/triage.md. Owner goal: play SurrounDead
in co-op with friends. Steam install exists on Hugo's PC; **not touched** this session.

Claim issue: https://github.com/cranebeaker/coop-anything/issues/334

## 1. Game identity (rule 5)

| Field | Value | Confidence |
|---|---|---|
| Title | SurrounDead | observed (Steam store) |
| Year / EA start | Early Access from 2022-06-24 | observed |
| Developer / publisher | Zurvivor / PlaySurrounDead | observed |
| Store | Steam, app id **1645820**, Windows 64-bit | observed |
| Current public version | Patch **0.8.0 – Pandemic** (stable, announced 2026-11-05) | observed (Steam / SteamDB) |
| Steam build id on Hugo's PC | **unknown** — need his install / appmanifest | unknown |
| SHA-256 of main exe | **unknown** — need a copy of his install (do not hash the live Steam folder until he agrees) | unknown |
| Engine | Unreal Engine **5.6** on 0.8.x (upgraded from 5.3); SDO README names **UE 5.6.1** for the Sep 2026 experimental bump | observed (patch notes + SDO); exact CL string on Hugo's build unknown |
| OS target | Windows 10/11 x64 (Steam store requirements) | observed |
| Hugo's OS / install path | **unknown** | unknown |

Sources:

- https://store.steampowered.com/app/1645820/SurrounDead/
- https://steamdb.info/app/1645820/
- https://steamdb.info/patchnotes/26467602/ (Patch 0.8.0 notes: UE 5.3 → 5.6)
- https://github.com/DatGuySnowfox/SDO (README status section)

Private install facts (paths, hashes, logs) go in `local/` only when collected.

## 2. Directory / hub coverage

Checked 2026-11-06 on hugge01/coop-anything (synced with upstream main):

- **DIRECTORY.md / `data/directory.yaml`:** no SurrounDead / surrondead entry.
- **`games/`:** no `games/surrondead/` before this branch.
- **Issues / PRs on hugge01 fork:** issues disabled on the fork.
- **Upstream `cranebeaker/coop-anything`:** no prior issue/PR for SurrounDead before claim #334.
- **Claim:** opened #334 so others can find this work.

## 3. Existing co-op / multiplayer attempts (research skim for triage)

### Official

- Steam store lists **single-player only**. Planned features still include “Online multiplayer survival with player run servers” and “Finally, in the future co-op.”
- Community comments and roadmap talk put multiplayer/co-op in a late phase (often called Phase 5); Phase 1 / 0.8 content is not that.
- Older Steam news (2023-era) said MP/co-op was “actively worked on” with a Trello board; **no shipping official MP** as of this triage.

Sources: Steam store planned features; Steam community discussion on Patch 0.8 info; historical Steam news post about MP work.

### Primary revive candidate: DatGuySnowfox/SDO (AGPL-3.0)

- https://github.com/DatGuySnowfox/SDO — “SurrounDead Online”
- Architecture: **UE4SS C++ mod** in the game client + **Node.js dedicated server** owning world state; optional WPF launcher + Cloudflare Worker server directory.
- Last push observed: **2026-09-27**. Mid-port from UE 5.3 → 5.6 after the experimental 0.8 bump.
- Author’s own status (README): two clients on 5.6 can connect, spawn, see each other move, clothing/weapon in hand, some montages, time of day. Combat / zombies-as-visible / vehicles / building / death-respawn **not re-verified** on 5.6; zombie proxy rendering **disabled** (invisible zombies). Vehicle sync built but never live-tested.
- Setup is not one-click; needs UE4SS **experimental-latest** (not v3.0.1 tag), Node 22+, xmake/MSVC to build.
- Author posted the project on Reddit 2026-09-24: https://www.reddit.com/r/SurrounDead/comments/1wpau62/unofficial_coop_mod_for_surroundead_open_source/

### Native Unreal listen-server experiments (thinner)

These try to turn on the game’s **leftover SteamSockets / replication** scaffolding instead of an external sync server.

| Project | Last push (observed) | Notes |
|---|---|---|
| [Seebrasse345/SurrounDeadMP](https://github.com/Seebrasse345/SurrounDeadMP) + [SurrounDead-MP-Fix](https://github.com/Seebrasse345/SurrounDead-MP-Fix) | 2025-12 | UE4SS installer + Lua/Blueprint spawn/possession fixes for `open …?listen` / join. README still describes UE5.3-era pain (camera-only clients). |
| [Koopz8/SurrounDeadMP](https://github.com/Koopz8/SurrounDeadMP) | 2026-10-23 | Explicitly “nothing playable yet.” Findings doc: SteamSockets net driver + OnlineSubsystemSteam already configured; `BP_MPGameMode` referenced but **missing from pak**; Jigsaw inventory has substantial replication markers; no session/lobby UI. Recon was against a **5.3** build. |
| [FullerK55/SurroundeadMP](https://github.com/FullerK55/SurroundeadMP) | 2026-10-26 | Fan-made UE4SS MP; description says private use among friends. README empty via API at triage time — treat as unverified. |

### Other noise (do not install blindly)

- Official Discord warning (quoted in the Reddit thread, ~2026-09-06) about a circulated **“SurrounDead Co-op Mod”** allegedly malware / paid scam. That is **not** the same as the open AGPL SDO repo; still: only pin official GitHub sources, hash-check downloads, never run unread installers.
- Misc GitHub hits (icons, Korean patch, save managers, cheat-named repos) — not co-op frameworks.
- Nexus has a SurrounDead game page and single-player style mods / UE4SS modding tutorials; no verified polished co-op listing found in triage search.

## 4. Triage answers (playbook checklist)

1. **Which exact game?** SurrounDead, Steam 1645820, Early Access, current public line Patch 0.8.0 (UE 5.6). Exact build id + exe hash for Hugo’s PC: unknown.
2. **Already done?** Not in the directory. Furthest public attempt is **SDO** (partial, mid-5.6 port, last commit ~6 weeks before this note). Official co-op not available.
3. **Engine?** Unreal Engine 5.6 family. Playbook `engine-families.md` UE4/5 row applies: **UE4SS** hooks; many UE games still ship netcode. Sibling evidence: leftover SteamSockets + Jigsaw MP replication (Koopz8 findings); SDO chose an external host-authority Node server instead.
4. **How much source?** Closed binary + Blueprint-heavy content (no game DLL per Koopz8). Marketplace plugins (Jigsaw Inventory, SmartAI, EasyMultiSave, UltraDynamicSky, …). Modding via UE4SS + pak/`~mods` overrides. SDO also ships large research dumps (CXXHeaderDump, Kismet/Blueprint work).
5. **Any multiplayer?** Official: no. Engine: SteamSockets net driver + Steam OSS configured; incomplete game-mode/session layer. SDO: external dedicated server sync.
6. **Shipping check?** Mods can ship without game files (UE4SS + own server). No evidence of anti-cheat (BattlEye/EAC) in public store/tech lists; still verify on the install copy before claiming. Stay off any future official online service when it appears.

## 5. Decision (evidence only)

**Path: revive / finish what exists — primary target SDO.**

Reasons:

- Usable open-source co-op attempt with substantial sync already proven on 5.3 and partially re-proven on 5.6.
- Matches playbook “abandoned or stalled mod” revive path more than build-from-zero.
- Native listen-server experiments are useful **research** (net driver already present) but are not as far as SDO for “play with friends.”

Secondary track to keep in NOTES during Phase 1: whether finishing SDO’s Node sync is still better than completing the unfinished SteamSockets game-mode path (Koopz8/Seebrasse). Do not start a third architecture without evidence SDO is dead.

Recommended next phase after Hugo answers identity questions: **Phase 1 research** (mine SDO Discord/Reddit/issue history, pin UE4SS build hashes, map SDO bug list against 0.8.0 stable) then **Phase 2 workspace** on a **copy** of his install — never the live Steam folder.

## 6. Observations / derived / guesses / unknowns

### Observations

- No directory or games/ coverage before this branch.
- Claim issue #334 filed on cranebeaker/coop-anything.
- Patch 0.8.0 stable (UE 5.6) went out **2026-11-05**, one day before this triage; SDO’s last push is 2026-09-27 against the experimental 5.6 line.
- Steam store still markets single-player; co-op remains a planned future feature.

### Derived

- Revive SDO first; treat listen-server repos as architecture alternatives / evidence of leftover netcode.
- Any work on Hugo’s machine must use a test copy + save backup (`%LOCALAPPDATA%\SurrounDead\Saved` per SDO README).

### Guesses (unverified)

- Hugo’s Steam install is likely already on 0.8.0 stable after the Nov 5 update (or still on Previous / experimental — must ask).
- SDO may need another offset/name pass for differences between Sep experimental 5.6 and Nov stable 0.8.0.
- Official MP, if it ever ships, could obsolete fan mods; no date exists to plan around.

### Unknowns

- Hugo’s OS, Steam library path, current branch (stable / Previous / experimental), Steam build id, exe SHA-256.
- Whether SDO still two-client-works on **today’s** 0.8.0 build (not re-tested here; no game install in this agent environment).
- Whether FullerK55/SurroundeadMP is a fork of SDO, of Seebrasse, or something else.
- Anti-cheat / Steam DRM quirks on dual-instance testing — unchecked on disk.

## 7. Questions for Hugo (block Phase 2)

1. Confirm OS (Windows 10/11?) and whether friends are also on Steam Windows.
2. Steam library path to SurrounDead (report as `<game dir>`, no username) — or run hashing yourself and paste SHA-256 + build id from `appmanifest_1645820.acf`.
3. OK to make a **test copy** of the install for tools (UE4SS, SDO) while leaving the Steam folder and saves alone?
4. Preferred friends model: small private session (SDO dedicated server you host) vs hoping for Steam P2P listen-server later?
5. Aware of the Discord malware warning about *other* co-op downloads — OK to stick only to GitHub AGPL SDO + pinned hashes?
6. OK to open a **draft** PR on hugge01/coop-anything with these notes + a directory entry once you’ve seen the diff?

## 8. Session log

| When | What |
|---|---|
| 2026-11-06 | `git pull` on hugge01/coop-anything main — already up to date. |
| 2026-11-06 | Grep directory/games — no prior SurrounDead work. |
| 2026-11-06 | Upstream issue/PR search — none; claim #334 opened. |
| 2026-11-06 | Public identity + SDO / listen-server survey from Steam, SteamDB, GitHub, Reddit. |
| 2026-11-06 | Branch `game/surrondead`; this NOTES.md. No install, no downloads, no non-draft PR. |
