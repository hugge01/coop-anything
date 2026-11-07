# SurrounDead — working notes

Owner goal: play SurrounDead in co-op with Steam friends. Claim:
https://github.com/cranebeaker/coop-anything/issues/334

**Do not touch** the live Steam install or saves. Work only on a test copy (steps below).
Private hashes/paths with usernames stay in `local/`.

---

## Phase 0 triage (2026-11-06) — summary

Path: **revive**, not build-from-zero. Furthest open attempt is SDO. Official co-op not shipping.
Full Phase 0 detail is folded into the sections below; session log at the end.

---

## 1. Game identity (rule 5)

| Field | Value | Confidence |
|---|---|---|
| Title | SurrounDead | observed |
| Year / EA | Early Access from 2022-06-24 | observed |
| Dev / pub | Zurvivor / PlaySurrounDead | observed |
| Store | Steam app **1645820**, Windows 64-bit | observed |
| Public version | Patch **0.8.0 – Pandemic** (stable, 2026-11-05); UE **5.3 → 5.6** | observed |
| Hugo OS | Windows 10 Pro — `NT 10.0.19045.0` | observed (Windows worker 2026-11-06) |
| Hugo Steam path | `<game dir>` = Steam `steamapps\common\SurrounDead` (Windows) | observed |
| Friends | Steam (platforms = Steam) | observed (Hugo) |
| Steam build id | **26471822** (`TargetBuildID` same) | observed (appmanifest via Windows worker) |
| Branch (BetaKey) | **none** in appmanifest (default/stable line; language `english`) | observed |
| SizeOnDisk | 2000186958 bytes (~1.86 GiB); depot 1645821 manifest 6403181274628414723 | observed |
| Main exe | `<game dir>\SurrounDead\Binaries\Win64\SurrounDead-Win64-Shipping.exe` (168432128 bytes); launcher `SurrounDead.exe` also at top level | observed |
| SHA-256 of shipping exe | `C2E211B4FA93C9FBFA9E5296DA94DAF22D2950EE3133B48DA6EB17EF4FF15118` | observed (read-only hash of **live** exe via Windows worker; live install not modified) |
| `Engine\Build\Build.version` | **absent** | observed |
| Engine CL string | **unknown** — need string dump on a test copy; SDO names UE **5.6.1** for Sep experimental; Patch 0.8.0 notes say UE 5.6 | partial |
| Saves folder | `%LOCALAPPDATA%\SurrounDead\Saved` exists | observed |

Sources: Steam store; SteamDB app + patchnotes 26467602; Hugo’s answers; SDO README; FullerK55 NOTES; Windows worker report `internal/surrondead-steam-identity.md` (agent bc-52d42668-18f3-5802-9f8f-214bb9e0f163 on worker Hugo Windows SurrounDead).

---

## 2. Hugo’s Phase 0 answers (2026-11-06)

1. Friends on Steam; install path Windows; OS later confirmed Win10 Pro 19045.
2. Install path given.
3. Earlier soft OK to test-copy + hash; identity hash done read-only on live exe. **Re-ask before creating a test copy or downloading UE4SS/SDO** (coordinator: still waiting on explicit OK).
4. Networking track: **agent decides** (see §5).
5. AGPL-SDO-only vs other mods: “idk” → **recommend AGPL SDO only** (Discord malware warning on other “co-op mods”). Soft recommendation; hard veto only if he objects. **Accepted 2026-11-06** (Hugo: “ok”).
6. Draft PR: soft OK (“ok idk”) → opened draft [hugge01/coop-anything#1](https://github.com/hugge01/coop-anything/pull/1). Hugo confirmed “ok” again after the link.

---

## 3. Directory / hub coverage

- Before this work: no `DIRECTORY.md` / `data/directory.yaml` / `games/surrondead` entry.
- Claim #334 on `cranebeaker/coop-anything`.
- hugge01 fork: issues disabled; work on branch `cursor/game-surrondead-7487` (playbook slug `games/surrondead/`).

---

## 4. Existing co-op attempts (Phase 1 research)

### Searched (2026-11-06)

- This repo directory, games/, upstream issues/PRs (claim only).
- Web: “SurrounDead co-op/multiplayer mod”, engine + coop.
- GitHub repos: DatGuySnowfox/SDO, Seebrasse345/SurrounDeadMP + SurrounDead-MP-Fix, FullerK55/SurroundeadMP, Koopz8/SurrounDeadMP, plus noise (icons, KR patch, cheats).
- Reddit r/SurrounDead (SDO announcement 2026-09-24; experimental 0.8 bug threads).
- Steam store + community news / Patch 0.8 notes.
- Nexus game page (modding tutorial / single-player mods; no verified polished co-op listing in triage search).
- UE4SS-RE/RE-UE4SS issues mentioning SurrounDead (#1204 StaticConstructObject on 5.6).
- **Not done:** Discord export of official or SDO channels (needs Hugo + discord-export.md; no tokens in chat). Archive.org deep crawl. Friend playtest of any mod.

### Official

- Store: single-player. Planned: “Online multiplayer survival with player run servers” and future co-op.
- Community: MP/co-op treated as late roadmap (often “Phase 5”); not in 0.8.0.
- Historical Steam news claimed MP work + Trello; still **no shipping official MP**.

### A. DatGuySnowfox/SDO — SurrounDead Online (primary)

| | |
|---|---|
| URL | https://github.com/DatGuySnowfox/SDO |
| License | **AGPL-3.0-or-later** |
| Author | DatGuySnowfox (Reddit: u/AnOrangeTrafficCone) |
| Last push | **2026-09-27** (master); frozen branch `v0.7-ue5.3` |
| Releases | none (build from source) |
| Issues | #1 open “Make a discord server” only — **bug list lives in README** |
| Architecture | UE4SS **C++** client mod + **Node.js dedicated server** (SQLite world) + optional WPF launcher + Cloudflare directory worker |
| Ports | game TCP **31000**, HTTP tickets **31001** |

**What really syncs (author’s README, trust that over press):**

| Scope | 5.3 | Verified on 5.6 (author, ~2026-09-26) |
|---|---|---|
| Players visible / move | yes | yes |
| Clothing / drawn weapon / some montages | yes | yes (clothing+weapon; armour/CC not re-proven) |
| Time of day | yes | yes |
| Weapon fire / NVG / inventory / damage / death-respawn | yes | **unverified** on 5.6 |
| Ground items / building | yes / partial | building hook **broken** (`SpawnBuild` moved to `BuildingComponent`) |
| Zombies (server sim) | yes | server sim + native spawner suppress still on; **proxy render DISABLED** (invisible) |
| Vehicles | code exists | **never live-tested** |
| Story / quests | n/a / unknown | unknown |
| Anti-cheat | none | none — trusted friends only |

**Biggest open bugs / revive work (from SDO README “Contributing”):**

1. Full two-client gameplay verification on current build (gates everything).
2. Two unresolved offsets (`JigsawItem_DataAsset` torso / equip-socket).
3. Rehome `SpawnBuild` / `Svr_SpawnBuild` onto `BuildingComponent`; verify `EquipActorToSocket` display-name lookup.
4. **Zombie proxy rendering** (disabled after five crash attempts).
5. Verify head-look + attachment toggle-off fixes.
6. First-ever vehicle sync test.
7. Re-extract zombie archetype stats after each game update.
8. Launcher auto-install of the mod.

**0.8.0 stable vs SDO’s Sep experimental 5.6:** SDO last pushed 2026-09-27 against experimental 0.8 / UE 5.6. Stable 0.8.0 shipped **2026-11-05** with skeleton/animation changes, infection system, new POIs, etc. Expect another name/offset pass (`research/check_names.py`, regenerate CXXHeaderDump). **Unknown** whether two-client still works on today’s stable until tested on a copy.

Reddit: https://www.reddit.com/r/SurrounDead/comments/1wpau62/unofficial_coop_mod_for_surroundead_open_source/

### B. FullerK55/SurroundeadMP — SDMP (listen-server research)

| | |
|---|---|
| URL | https://github.com/FullerK55/SurroundeadMP |
| License | **none stated** |
| Last push | **2026-10-26** (newer than SDO) |
| Architecture | UE4SS **Lua** listen-server on leftover SteamSockets / IpNetDriver; Radmin/VPN + `-nosteam` direct launch |

**How far it got (author NOTES, verified on UE 5.3, 2026-10-25):**

- `open PersistentLevel?listen` works; New Game streams `LongdownValley` without dropping listen.
- SteamSockets join: Steam-level connections appear but **Unreal Hello/Login handshake stalls**.
- IP path works with **direct exe launch** (`-nosteam`); Steam launch breaks IpNetDriver bind.
- Two-PC over Radmin: joiner gets a body; host crash fixed via `ExecuteInGameThread`; death/respawn partially fixed; clothing partial; **movement snaps** (~10/s of 30–70 cm) open.
- Launcher **downloads master zip from GitHub on every launch** — treat as untrusted auto-update; do **not** point friends at this until license + pin + 0.8.0 proof.

Game plumbing documented (valuable for any path): SteamSockets primary net driver, OnlineSubsystemSteam, Jigsaw `BP_JigMultiplayer` RPCs, EasyMultiSave, Blueprint-only gameplay, missing `BP_MPGameMode` asset, spawn via `SD_GameInstance` / `Survival_SpawnPlayer`.

### C. Seebrasse345/SurrounDeadMP + MP-Fix

UE4SS installer + Lua/Blueprint spawn fixes for listen join (camera-only clients). Last code push **2025-12**. README still UE5.3-era. Superseded as a starting point by FullerK55’s deeper NOTES.

### D. Koopz8/SurrounDeadMP

“Nothing playable yet” (2026-10-23). Findings: SteamSockets configured; replication census; no session UI. Good corroboration of leftover netcode; not a product.

### E. SYMPLYSCK/SurrounSync — Steam P2P co-op (2026-11-07 research)

| | |
|---|---|
| URL | https://github.com/SYMPLYSCK/SurrounSync |
| License | **Source-available proprietary** (© 2026 Jayden Munroe) — play + read OK; **no copy/modify/redistribute/fork** without written permission. UE4SS stays MIT. |
| Author | SYMPLYSCK / Reddit OP cited by Hugo: Primary-Memory-661 |
| Latest release | **v1.1.0** (2026-11-07); installer `SurrounSyncSetup.exe` |
| Installer SHA-256 (published) | `859C78C64159BEAD03B85847FB6ECE6EF2FB750FCEACD3FA90C579B0DA2156A3` |
| Game version | **0.8.0 Pandemic** — normal Steam branch **or** Experimental; all clients same branch. **0.7.1 previous does not work** |
| Architecture | Each client stays SP; UE4SS **Lua** syncs state via link files; `SurrounSyncBridge.exe` does **Steam P2P** (game’s `steam_api64.dll`). Host AI authority; guests get zombie/AI copies. Optional **Realms** (Cloudflare Worker + D1/R2). Optional SyncFX (off by default). |
| Players | Up to **4** per session; Realms up to **16** members |

**Author-claimed sync (README / ARCHITECTURE — not our QA):** players, combat, zombies/AI copies, loot/containers/drops, time/weather/doors/building/cars/airdrops, chat, map pings, minimap + big-map player dots (1.1), voice experimental. Traders/quests **not** synced. Zombies only hear other players’ gunshots/explosions.

**Bugs / pain (labeled):**

- **Author release notes** (1.0–1.1 *claim* fixes): crawler visuals, double-hit swings, storage put-ins lost, 1-damage guns, building duplicates/move desync, car copies with 3–4 players, long-session memory leak (~2.6 GB / half-second frames), autosave/join freezes (older 0.14.x era).
- **Hugo / Reddit “Multiplayer Mod RELEASED!” themes** (thread body **not** re-fetched — Reddit API blocked from this environment): zombie visibility/types, infestations, loot/drops, autosave lag, vehicles, building desync, FPS. Discord mods (per Hugo): public code, **distinct from the Sep malware scare**; unsigned installer trips Defender.

**Compare write-up:** store `internal/surrounsync-vs-sdo.md`. **Eval brief (no download until coordinator places):** `internal/phase3e-surrounsync-eval-brief.md`.

### Noise / security

- Official Discord warning (~2026-09-06, quoted on Reddit) about a circulated “SurrounDead Co-op Mod” as malware / paid scam. **Not** the AGPL SDO GitHub. Hugo: Discord mods treat **SurrounSync** as distinct + public-source. Still: pin hashes, unsigned installer → Defender expected, no unread installers, no auto-updating launchers from random zips.
- Cheat-named repos and icon packs ignored.

---

## 5. Networking track decision (evidence)

**Updated 2026-11-07 — dual-track, SurrounSync preferred for friends-soon (Hugo blanket OK, no ask):**

1. **Primary “play with friends soon”:** black-box **SurrounSync 1.1.0** on a **SEPARATE** test copy (e.g. `D:\coop-test-surrounsync`). Never live Steam. **Never mix** SurrounSync + SDO on the same Win64 tree. Pin installer SHA above. Brief: `internal/phase3e-surrounsync-eval-brief.md`. **Do not download until coordinator places Windows.**
2. **Secondary AGPL research:** keep SDO Phase 3a+ on `D:\coop-test` if still running — zombie opt-in / directory contribution. Do **not** cancel 3a for this research; it is no longer the friends-soon critical path.
3. **All-rights-reserved:** do not fork SurrounSync into our patches; document bugs only.
4. SDMP remains research-only (no license; 5.3; zip auto-fetch).

**Why SurrounSync first for friends:** Steam P2P, no port forward, installer, author-claimed visible zombies/map/cars/building on **0.8** — matches Hugo’s goal faster than finishing SDO zombie proxies. **Why keep SDO:** AGPL forkable; our verified mutual-visibility smoke; cannot reuse SurrounSync code.

---

## 6. UE4SS pins (do not download until Hugo OKs)

SDO requires rolling **`experimental-latest`**, not tag `v3.0.1` (no UE 5.6 pattern scan).

Observed on GitHub Releases API for tag `experimental-latest` (2026-11-06):

| Asset | Size | GitHub `digest` (pin) |
|---|---|---|
| `UE4SS_v3.0.1-1212-gd4aebbf0.zip` | 8 756 935 | `sha256:8196a66237dcd116051ce101659c7a73f0807cba8818dc2f798017f80a566039` |
| `zDEV-UE4SS_v3.0.1-1212-gd4aebbf0.zip` | 43 948 166 | `sha256:b01ff962c5dd4e33f37befcfa85059b8ac5e9fbd70c5baa6708bd6380de38c1e` |
| `zCustomGameConfigs.zip` | 423 121 | `sha256:0f1f60064f022dda9f1e6df16a27d5ca9bd34d6918dc0cc4646f24bacb7f65bf` |

URL base: `https://github.com/UE4SS-RE/RE-UE4SS/releases/download/experimental-latest/`

Notes:

- Tag is rolling; **re-check digest before any future download** — asset names change.
- Layout after v3.0.1: `dwmapi.dll` next to exe; `UE4SS.dll` + settings + `Mods/` under `ue4ss\`.
- UE4SS issue [#1204](https://github.com/UE4SS-RE/RE-UE4SS/issues/1204): SurrounDead 5.6 may need a custom `UE4SS_Signatures/StaticConstructObject.lua`; reporter then hit a main-menu crash. SDO’s author got past this on experimental 5.6 — capture their working UE4SS build id when we build SDO.
- FullerK55 recorded working experimental `v3.0.1-1183-gcdbc0b66` on **5.3** — different pin; do not mix.

**Ask Hugo before downloading** these zips (or Node 22 / VS Build Tools) onto his PC.

---

## 7. Test copy + hash steps (Hugo’s Windows PC)

Cloud agents **cannot** see his disk. Either he runs these, or a later self-hosted/local worker on his machine does.

### A. Record Steam build (read-only)

1. Open `<steam>\steamapps\appmanifest_1645820.acf` in Notepad.
2. Copy `buildid` and `ScheduledAutoUpdate` / size fields into `local/identity.md` (or paste to the agent).
3. In Steam → SurrounDead → Properties → Betas: note whether **None / Previous / experimental**.

### B. Test copy (never modify Steam folder)

In PowerShell (adjust drive if needed):

```powershell
$src = "C:\Program Files (x86)\Steam\steamapps\common\SurrounDead"
$dst = "D:\coop-test\SurrounDead"   # pick a non-Steam path with space
robocopy $src $dst /E /COPY:DAT /R:1 /W:1 /XD .git
```

Optional integrity spot-check: compare file counts; do **not** delete or write inside `$src`.

### C. Hash the exe (on the copy)

```powershell
Get-FileHash "$dst\SurrounDead\Binaries\Win64\SurrounDead-Win64-Shipping.exe" -Algorithm SHA256
```

Paste the hash into `local/identity.md`. Confirm exe name if the path differs.

### D. Save backup (before any mod)

```powershell
$saves = "$env:LOCALAPPDATA\SurrounDead\Saved"
Copy-Item $saves "D:\coop-test\SurrounDead-Saved-backup-$(Get-Date -Format yyyyMMdd)" -Recurse
```

SDO warns it can corrupt saves; only point mods at disposable saves.

### E. Dual-instance / Steam notes (later)

- For SDO: follow upstream README (UE4SS experimental → build `main.dll` → Node server → tickets). Prefer launching the **copy’s** exe with `steam_appid.txt` containing `1645820` if Steam relaunches the live install (unverified for this game — test carefully).
- Do **not** install UE4SS into the live Steam `Win64` folder.

---

## 8. Engine / source (for co-op)

- UE 5.6 Shipping, Blueprint-heavy, no meaningful game DLL (FullerK55 / Koopz).
- One classic `.pak` (v11, Oodle); Nexus tutorial cited AES for FModel on an older line — **re-verify key/version on 0.8.0 copy** before trusting.
- Marketplace: Jigsaw Inventory (MP-aware RPCs), SmartAI, EasyMultiSave, UltraDynamicSky, Narrative, Prefabricator, DLSS/FSR.
- Net: `SteamSocketsNetDriver` primary, `IpNetDriver` fallback; Steam OSS on; P2P relay flagged. `BP_MPGameMode` referenced in config but **missing from pak**.
- Saves: EasyMultiSave under `%LOCALAPPDATA%\SurrounDead\Saved`.

Playbook family: UE4/5 + UE4SS (`playbook/engine-families.md`).

---

## 9. Observations / derived / guesses / unknowns

### Observations

- Directory was empty for this game; claim #334 filed.
- SDO AGPL, mid-5.6 port, last commit 2026-09-27; bug tracker ≈ README.
- SDMP got two-PC bodies on 5.3 via IP/Radmin; SteamSockets handshake fails; no license; auto-updating bat.
- Stable 0.8.0 (2026-11-05) is newer than both mods’ last proven targets.
- Hugo OK’d test copy + draft PR (soft); friends on Steam.

### Derived

- Revive **SDO dedicated** as primary; keep SDMP as research.
- AGPL-only soft recommendation.
- Next concrete work needs build id + exe hash from his PC, then Hugo OK to download UE4SS experimental + clone SDO.

### Guesses

- Build **26471822** is the public 0.8.0 stable line (matches SteamDB patch timing; no BetaKey) — treat as 0.8.0 until contradicted.
- Stable 0.8.0 animation/skeleton changes will break some SDO name lookups again.
- Official MP remains too far out to wait for.

### Unknowns

- UE CL / engine string in the shipping exe (`Build.version` missing).
- Confirmed hit/death combat outside safezone; vehicle enter/sync on **26471822**.
- Whether co-op peers appear on the in-game **map** / compass (Hugo wants yes; untested here).
- Reliable per-process Saved isolation (LOCALAPPDATA override via ProcessStartInfo failed in 2d).
- Discord bug channels (not exported).
- Anti-cheat / SteamStub quirks beyond dual-instance copy smokes.

---

## 10. Phase 2 workspace (Windows) — 2026-11-06

Done by [Phase 2 Windows workspace](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48) on Hugo Windows SurrounDead. Full report: `internal/phase2-windows-workspace-report.md`. Live Steam untouched.

| Item | Result |
|---|---|
| Saved backup | out-of-tree backup created (36 files) |
| Test copy | SHA-256 matches live; buildid 26471822 |
| UE4SS | `UE4SS_v3.0.1-1212-gd4aebbf0.zip` digest **matched**; installed into **copy** Win64 only (`dwmapi.dll` + `ue4ss\`) + `steam_appid.txt` = 1645820 |
| SDO clone | `master` @ `5d9a73d2b05b888abf64990eb003b6446abdf0d9` |
| SDO build (later) | **OK** — see §10b |

Private absolute paths stay in `local/identity.md`.

## 10b. Phase 2b — xmake build + deploy (2026-11-06)

Same agent [bc-f4712bbb…](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48). Report: `internal/phase2-xmake-build-report.md`. Live Steam untouched; game not launched.

| Item | Result |
|---|---|
| xmake | v3.1.2 via `winget install Xmake-io.Xmake` → `C:\Program Files\xmake\xmake.exe` |
| Build | First `xmake build` → **LNK1181** missing `UE4SS.lib`; `scripts/gen_import_lib.ps1` → stub lib; rebuild OK → `D:\coop-test\SDO\build\out\main.dll` |
| `main.dll` | 819712 bytes; SHA-256 `6E83B911F7D786FDE25124F93030CEF68AA6326F884F081D7053DE4CB2278854` |
| Deploy | `deploy.ps1 -Win64 <copy Win64> -SkipBuild` → copy `ue4ss\Mods\SDO\` (`enabled.txt` + `dlls\main.dll`); live Steam still clean |
| Build log | `D:\coop-test\tools\xmake-build.log` (private path) |

## 10c. Phase 2c — Node + two-client smoke (2026-11-06)

Same agent [bc-f4712bbb…](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48). Report: `internal/phase2-node-smoke-report.md`. **Not** an automated QA suite — one manual two-client smoke with log evidence.

| Item | Result |
|---|---|
| Node | **v25.2.1** / npm 11.13.0; `npm install` OK in `D:\coop-test\SDO\server` |
| Ports | Gateway **42200** / HTTP **42201**; `SDO_PUBLIC_HOST=127.0.0.1`; directory blank |
| Launch | **Copy** Shipping exe + `-sdo_host=127.0.0.1 -sdo_port=42200 -sdo_ticket=…` only; never `steam://run` |
| Auth / session | Both clients `authenticated` / `joined` / `session latched` (first attempt hit **120s ticket TTL** → `expired_ticket`; fresh tickets OK) |
| Health | `hostOnline:true`, **`players:2`**; two ESTABLISHED TCP clients on 42200 |
| Proxies / clothing | `proxy spawned` ×2; `equip-clothing` ok; equipment frames exchanged. Free WASD **not** exercised (FirstJoin **Barber** UI up) |
| Weapon-in-hand visual | **unknown** while on Barber |
| Zombies / combat / vehicles | **not tried** |
| Saves | SDO auto `ContinueGame` → used real `%LOCALAPPDATA%\SurrounDead\Saved` (backup still at `D:\coop-test\SurrounDead-Saved-backup-20261106`) |
| Live Steam | untouched; clients + Node torn down after smoke |

## 10d. Phase 2d — deeper in-game (2026-11-06)

Same agent [bc-f4712bbb…](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48). Report: `internal/phase2-deeper-ingame-report.md`. Manual deeper smoke — **not** automated QA.

| Item | Result |
|---|---|
| Node / ports | v25.2.1; **42200** / **42201**; TTL raised to 600000 ms; tickets `deep-a`/`deep-b` |
| Launch | Copy Shipping + `-sdo_*` only; never Steam |
| Saved isolation | **Partial fail** — intended `D:\coop-test\appdata-smoke-{a,b}\Local`; `ProcessStartInfo` LOCALAPPDATA override did not stick; real `%LOCALAPPDATA%\SurrounDead\Saved` still received writes (`UserSettings.ini`, CrashReportClient). Backup still present. |
| Teleport | `%APPDATA%\SDO\teleport.flag` → `check_teleport_trigger` (`mod.cpp`); shared APPDATA ⇒ one consumer per write; sequential writes with offsets. Safezone barber `115013.6 133181.6 1185.0` (+ offsets). |
| Barber + WASD | **PASS** — Barber dismissed; free-play HUD; WASD with caveat (Esc opened pause and blocked some input) |
| Mutual visibility | **PASS** — **Hugo** both see each other; agent shot `after-tp-A.png` shows peer; logs `proxy spawned` ×2; health `players:2` |
| Combat | **PARTIAL** — LMB bursts; ammo drained to `000/000`; no clear damage/death in server log (safezone likely limited PvE) |
| Zombies | **Confirms known SDO gap** — none visible on shots; `spawn_zombie_actor` returns `nullptr` / proxy render disabled in `entity_manager.cpp` |
| Vehicles | **Not tried** successfully (56 entities broadcast; no enter prompt; skipped further hunt) |
| Live Steam | clean; clients + Node torn down |

## 11. Zombie visibility research (2026-11-06)

Hugo priority: zombie visibility toward “perfect co-op” feel, **plus players visible on each other’s map** (party/map markers — Hugo ask 2026-11-06). Full zombie plan: store `internal/zombie-visibility-fix-plan.md`. Sources = SDO README / AGPL source comments / Session 59 investigation log / Reddit announcement / GitHub issues (only #1 Discord ask — **no** zombie issue threads). No SurrounDead binary RE this turn; no Windows placement.

**Map markers:** not checked in Phase 2c/2d smokes (**unknown** whether SDO already shows co-op peers on the in-game map). Game dump has `BP_PlayerMarker` / marker widgets; SDO README does **not** list map-marker sync as verified. Treat as a **wanted feature** for “perfect co-op” after zombie proxies (or a quick Windows check: open map with two clients and note PASS/FAIL).

**Cars / place / drop (Hugo ask — from SDO README + `host-agent.js` / `entity_manager.cpp` @ `5d9a73d`, not game binary RE):**

| Feature | How SDO intends it | Status on 5.6 / our copy |
| --- | --- | --- |
| **Drop items to each other** | Client `ItemDropRequest` → host creates `GroundItem`, broadcasts `EntitySpawn`+`EntityState` (persisted). Peer `ItemPickupRequest` despawns. TTL prune + max-count. Dropper’s local native pickup **claimed** (`find_and_claim_native_pickup`) so they don’t see a duplicate. Host trusts client itemId (equipped drops won’t match inventory bookkeeping). | Author: worked on **5.3**; “not yet re-verified on 5.6.” **We have not smoke-tested.** |
| **Place / build objects** | Client `InteractionRequest` BUILD → host validates distance/itemId → `PlacedStructure` entity; clients spawn via `BuildActorClass` on the DataAsset. | **README still says broken** (pawn lookup). **Code @ `5d9a73d` already resolves** `SpawnBuild`/`Svr_SpawnBuild` off `BuildingComponent` via `obj_prop` (`mod.cpp` ~9254–9280) and author notes “NOT YET LIVE-VERIFIED.” Treat as **needs live placement smoke**, not a known missing rehome — README Contributing #3 may be stale. |
| **Cars / vehicles** | Once per session host `_broadcastVehicles()` (~56 zones from world-data); clients **adopt** nearest already-loaded level vehicle (`find_native_vehicle_near`) — deliberately no spawn (avoids zombie crash class). Health default 100. | Code complete; author **never live-tested**. Our 2d: broadcast seen, enter **not tried**. |

Full ranked wanted list: store `internal/perfect-coop-wanted-backlog.md`.

### Observations (cited)

- README **Broken/disabled**: `spawn_zombie_actor` returns `nullptr` on purpose after five live crashes / three guessed fixes; server sim + native spawner suppress stay on ([README](https://github.com/DatGuySnowfox/SDO/blob/master/README.md)).
- Same author on Reddit: invisible to clients; can still hurt; rendering crashes never root-caused ([thread](https://www.reddit.com/r/SurrounDead/comments/1wpau62/unofficial_coop_mod_for_surroundead_open_source/)).
- `entity_manager.cpp` + Session 59: controller UnPossess was a red herring; crashes persisted at same addresses without controller code; leading theory = untyped `FindObject` → bad `UClass*` cache and/or dangling actor on roam teleport. Hardening partially landed (`resolve_class_by_name` FindFirstOf-primary; SEH teleport) but spawn still `#if 0` / early zombie skips.
- Our Phase 2d: confirms invisible + disabled spawn path on build 26471822.

### Guesses (author / ours — labeled in plan)

Ranked hypotheses A–E in the plan (FindObject type, dangling actor, AI lifetime, 0.8 archetype rename, all-clients broadcast lag).

### Proposed fix order (smallest lever first)

1. Type-check FindObject fallback (never cache non-UClass).  
2. Stronger teleport invalidation after SEH.  
3. Flag-gate `enable_zombie_proxies.flag` (default off).  
4. Cap / near-player spawn before full flood.  
5. Re-enable Roamer-only under flag with heavy logging — **ask coordinator before Windows build/deploy**.  
6. Then Dog/Boss, combat, server relevance.  
7. Hugo blanket OK (2026-11-06): no further owner asks — default to AGPL patches in-hub + apply on `D:\coop-test\SDO`; upstream PR when proven.

### Other SDO-documented sync gaps (not invented)

Vehicles never live-tested; building hook stale on 5.6; combat/death unverified on 5.6; server inventory restore incomplete; ticket TTL 120s; concurrent character creation blocker; no anti-cheat.

### Wanted / perfect-co-op backlog (ranked)

1. Visible zombies (Phase 3a in flight)  
2. Peers on the **map** (check in 3a; feature if FAIL)  
3. Shared **drops/pickups** (smoke after zombies)  
4. **Building** rehome + test  
5. **Vehicles** enter/drive sync test  
6. Combat feel; then friend howto  

Detail: `internal/perfect-coop-wanted-backlog.md`.

## 12. Zombie proxy patches prepared (2026-11-06)

Hugo: keep improving (~10h AFK). Cloud implemented smallest levers as AGPL patch (no Windows placement from this agent):

- Path: `games/surrondead/sdo-patches/0001-zombie-proxy-smallest-levers.patch` (against SDO `5d9a73d`; includes SYNC_DIAG)
- Changes: FindObject non-class reject; `enable_zombie_proxies.flag` opt-in (default off); clear actor on teleport SEH; max 12 Roamer-only; one-shot entity-count `SYNC_DIAG`
- Building: no rehome patch yet — see `sdo-patches/BUILDING-STATUS.md`
- Windows briefs: `internal/phase3-zombie-proxy-build-brief.md` + resume `internal/phase3a-windows-resume-brief.md` (machine was offline mid-3a)

## 13. Next phase

Hugo blanket OK (Project `preferences.md`): no owner asks. **Friends-soon primary = SurrounSync Phase 3e** on separate test copy after coordinator places brief (no download yet). SDO 3a remains AGPL secondary if Windows still running it.

Queued briefs (coordinator place/resume only, no self-place):

1. **3e SurrounSync eval (primary)** — `internal/phase3e-surrounsync-eval-brief.md` (compare: `internal/surrounsync-vs-sdo.md`)
2. 3a zombie report fold when it lands — then 3b–3d on SDO tree only if still pursuing AGPL path
3. 3b drops / 3c building / 3d vehicles — SDO secondary
4. Map markers — may be moot if SurrounSync minimap PASSes

Do **not** claim QA `automated` or “perfect”.

---

## 14. Session log

| When | What |
|---|---|
| 2026-11-06 | Phase 0 triage; claim #334; initial branch later renamed to `cursor/game-surrondead-7487`. |
| 2026-11-06 | Hugo answers Phase 0 Qs (path, test copy OK, soft draft PR, agent picks net). |
| 2026-11-06 | Phase 1: SDO README/bug list, UE4SS digest pins, FullerK55/Koopz/Seebrasse deep read, networking decision = SDO dedicated, directory + README, draft PR. No downloads, no install touch. |
| 2026-11-06 | Hugo asked agent to read install from his PC; clarified he is on **Windows**. Only connected self-hosted worker is **Hugo's MacBook Pro** (darwin) — cannot see `C:\Program Files (x86)\Steam\...`. Blocked until a Windows worker is online or he pastes §7 output. |
| 2026-11-06 | Hugo asked to connect this Windows PC instead. Yes: run Cursor CLI `agent worker start` on the Windows box (docs: https://cursor.com/docs/cloud-agent/self-hosted/my-machines). Agent cannot start that remotely; waiting for Windows worker to appear. |
| 2026-11-06 | Windows worker **Hugo Windows SurrounDead** connected (`72bfb90e-501e-4bc1-a08d-56617b8d6d75`). Two Task probes ([bc-513446ff…](bc-513446ff-fab7-5b47-a754-b1784799e4fa), [bc-4bd178b2…](bc-4bd178b2-60bc-57bd-a134-7a80c80e7f89)) ignored `workerId` and ran on Linux cloud. Asked coordinator for real placement; no more retries from this agent until then. |
| 2026-11-06 | Coordinator-placed [Windows identity worker](bc-52d42668-18f3-5802-9f8f-214bb9e0f163) SUCCESS: buildid 26471822, exe SHA-256 C2E211B4…F15118, Win10 Pro 19045, Saved exists. Folded into NOTES + local/identity. No test copy / UE4SS / SDO yet — re-ask Hugo. |
| 2026-11-06 | Hugo: **ok to both** (test copy + pinned UE4SS/SDO on copy only). Brief for Windows placement: `internal/phase2-windows-workspace-brief.md`. This agent will not self-place on Windows. |
| 2026-11-06 | Coordinator placed [Phase 2 Windows workspace](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48) on Hugo Windows SurrounDead. Standing by for `internal/phase2-windows-workspace-report.md`. |
| 2026-11-06 | Phase 2 report SUCCESS-partial: test copy + UE4SS pin + SDO clone; **xmake missing** (MSVC present). Waiting Hugo OK to install xmake (or alt build path). |
| 2026-11-06 | Hugo: **ok** to install xmake. Brief `internal/phase2-xmake-build-brief.md`; waiting coordinator Windows placement. |
| 2026-11-06 | Coordinator resumed [Phase 2 Windows workspace](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48) with xmake-build brief (no new placement). Standing by for `internal/phase2-xmake-build-report.md`. |
| 2026-11-06 | Phase 2b SUCCESS: xmake 3.1.2; `main.dll` built+deployed to copy only (SHA 6E83B911…). Folded into §10b + PR. Waiting Hugo OK for Node server + two-client smoke. |
| 2026-11-06 | Hugo: **OK** Node dedicated server + two-client smoke (test copy only). Wrote `internal/phase2-node-smoke-brief.md` (ports 42200/42201, npm, copy exe only — no live Steam). Asking coordinator to resume [bc-f4712bbb…](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48). |
| 2026-11-06 | Phase 2c SUCCESS (manual smoke, not automated): Node 25.2.1; both copy clients auth+session; health `players:2`; clothing equip sync; FirstJoin Barber only; ContinueGame used real LOCALAPPDATA Saved (backup exists); live Steam untouched; torn down. Folded into §10c + PR. Coordinator asking Hugo what’s next. No Windows placement from this agent. |
| 2026-11-06 | Hugo wants deeper in-game checks. Wrote `internal/phase2-deeper-ingame-brief.md` (WASD past Barber, combat, zombie gap, vehicles if safe, Saved isolation). Asking coordinator to resume [bc-f4712bbb…](bc-f4712bbb-71f8-5814-bc23-4ea1fd46bb48). |
| 2026-11-06 | Phase 2d done: teleport.flag; Hugo mutual visibility PASS; Barber+WASD PASS; combat partial; zombies confirm known gap; vehicles not tried; Saved isolation partial fail (real LOCALAPPDATA still written); live Steam clean; torn down. Folded §10d + PR. Coordinator asking Hugo next (friend howto / zombie fix / pause). No Windows placement. |
| 2026-11-06 | Hugo priority = zombie visibility. Researched SDO README/issues/PRs/source comments (no binary RE). Wrote `internal/zombie-visibility-fix-plan.md` + §11; patch sketch flag-gated; ask before Windows build. PR #1 updated. |
| 2026-11-06 | Hugo AFK ~10h: keep improving. Landed AGPL patch `sdo-patches/0001-…` + Phase 3a Windows brief. Asking coordinator to resume bc-f4712bbb for apply/build/flag-on retest (copy only). |
| 2026-11-06 | Hugo: also want players visible **on the map**. Noted as wanted/unknown in NOTES + howto + Phase 3a checklist (not verified in 2d). |
| 2026-11-06 | Hugo asks how cars / placing / dropping work. Documented from SDO README+code: drops=shared GroundItems (5.3 ok, 5.6 unverified); build=broken on 5.6; vehicles=code never live-tested. |
| 2026-11-06 | Coordinator pinged 3a for map markers. Expanded wanted backlog (`internal/perfect-coop-wanted-backlog.md` + NOTES ranked list) from host-agent protocol details while waiting on Phase 3a report. |
| 2026-11-06 | Hugo: keep iterating while AFK. Corrected building status (README vs `mod.cpp` BuildingComponent hook already present, unverified). Drafted `phase3b-drop-pickup-brief.md` + `map-marker-research-notes.md`. Still waiting Phase 3a report. |
| 2026-11-06 | Hugo: **approve everything / no asking** (chat stops if asked). Autonomous path: patches + Windows via coordinator only. |
| 2026-11-06 | Windows worker offline/asleep mid-3a. Offline: refreshed 0001 (+SYNC_DIAG), BUILDING-STATUS.md, phase3a-windows-resume-brief, 3b–3d briefs already queued. No Hugo asks. |
| 2026-11-07 | Hugo pasted SurrounSync Reddit + README (SYMPLYSCK, v1.1.0). Published-source only (no binary RE; Reddit API blocked). Wrote `internal/surrounsync-vs-sdo.md` + Phase 3e brief. Decision: **SurrounSync SEPARATE test-copy = friends-soon primary**; SDO 3a = AGPL secondary; never mix Win64 installs; no download until coordinator places. Directory + NOTES/README/PR updated. |
| 2026-11-07 | Coordinator placed **new** SurrounSync eval worker on Hugo Windows for Phase 3e brief. Standing by for `internal/phase3e-surrounsync-eval-report.md`. SDO 3a on bc-f4712bbb remains secondary. No self-place. |
