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
| Hugo Steam path | `<game dir>` = `…\Steam\steamapps\common\SurrounDead` (Windows) | observed (Hugo) |
| Friends | Steam (platforms = Steam) | observed (Hugo) |
| Steam build id on Hugo’s PC | **unknown** — read `appmanifest_1645820.acf` on his machine | unknown |
| Branch (stable / Previous / experimental) | **unknown** — ask after 0.8.0 drop | unknown |
| Main exe (typical) | `<game dir>\SurrounDead\Binaries\Win64\SurrounDead-Win64-Shipping.exe` | observed (SDO / FullerK55 / Nexus tutorial); confirm on copy |
| SHA-256 of main exe | **unknown** — hash the **test copy** only | unknown |
| Engine CL string | **unknown** until string dump on copy; SDO names UE **5.6.1** for Sep experimental | partial |

Sources: Steam store; SteamDB app + patchnotes 26467602; Hugo’s answers; SDO README; FullerK55 NOTES.

---

## 2. Hugo’s Phase 0 answers (2026-11-06)

1. Friends on Steam; install path Windows as above (OS version not stated).
2. Install path given.
3. **OK** to make a test copy and hash the exe; never touch live install/saves.
4. Networking track: **agent decides** (see §5).
5. AGPL-SDO-only vs other mods: “idk” → **recommend AGPL SDO only** (Discord malware warning on other “co-op mods”). Soft recommendation; hard veto only if he objects.
6. Draft PR: soft OK (“ok idk”) → open draft after NOTES/write-up ready; show him the PR link.

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

### Noise / security

- Official Discord warning (~2026-09-06, quoted on Reddit) about a circulated “SurrounDead Co-op Mod” as malware / paid scam. **Not** the AGPL SDO GitHub. Still: pin hashes, no unread installers, no auto-updating launchers from random zips.
- Cheat-named repos and icon packs ignored.

---

## 5. Networking track decision (evidence)

**Primary: revive SDO’s dedicated Node.js host-authority server + UE4SS C++ client.**

Reasons:

1. **Only project with author-verified two-client sync on UE 5.6** — matches Hugo’s likely 0.8.0 engine. SDMP’s real two-PC sessions were on **5.3**.
2. **AGPL-3.0-or-later** — clear revive/fork license; network clause understood. SDMP has no stated license.
3. **World authority already designed** (server owns zombies/time; client spawners suppressed). Survival co-op needs that more than raw pawn replication.
4. SDMP’s SteamSockets path is **broken** (handshake stall); IP path needs `-nosteam` + VPN and fights Steam launch — worse UX for “friends on Steam.”
5. SDMP’s every-launch GitHub zip pull is a **security footgun** given the malware scare.

**Secondary (research only, not shipping path):** reuse FullerK55/Koopz findings (level streaming, spawn/possess, game-thread rules, Jigsaw RPCs) when fixing SDO. Do not start a third architecture unless SDO is proven dead on 0.8.0.

**Recommendation to Hugo:** stick to **AGPL SDO + pinned UE4SS** only. Soft; say no if you want SDMP evaluated on a 5.3 Previous-branch copy instead.

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

- His Steam library is on **0.8.0 stable** after Nov 5 (unverified).
- Stable 0.8.0 animation/skeleton changes will break some SDO name lookups again.
- Official MP remains too far out to wait for.

### Unknowns

- Exact build id, branch, exe SHA-256, UE CL string on his disk.
- Whether SDO two-client still works on 0.8.0 stable.
- Whether UE4SS `1212-gd4aebbf0` boots 0.8.0 without custom signatures (#1204).
- Discord bug channels (not exported).
- Anti-cheat / SteamStub quirks on dual instance — unchecked.

---

## 10. Next phase

1. Hugo (or local worker): run §7 → fill `local/identity.md`.
2. Hugo OK → download pinned UE4SS experimental into the **test copy** only; smoke-boot with console.
3. Clone SDO (AGPL), build with xmake/MSVC, deploy per README; stand up Node 22 server on LAN.
4. Two-client smoke on 0.8.0; file failures against SDO README bug list (especially zombies + combat).
5. Phase 5 revive work starts at zombie proxies / 0.8.0 name audit — not a greenfield sync layer.

---

## 11. Session log

| When | What |
|---|---|
| 2026-11-06 | Phase 0 triage; claim #334; initial branch later renamed to `cursor/game-surrondead-7487`. |
| 2026-11-06 | Hugo answers Phase 0 Qs (path, test copy OK, soft draft PR, agent picks net). |
| 2026-11-06 | Phase 1: SDO README/bug list, UE4SS digest pins, FullerK55/Koopz/Seebrasse deep read, networking decision = SDO dedicated, directory + README, draft PR. No downloads, no install touch. |
