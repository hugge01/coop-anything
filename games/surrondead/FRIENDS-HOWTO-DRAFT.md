# SurrounDead SDO — friend join howto (DRAFT)

**Status:** draft only. Not play-ready until zombie-proxy (and preferably combat) retests pass on build **26471822**. Do not send friends here yet.

## What this is

Unofficial co-op via [SDO](https://github.com/DatGuySnowfox/SDO) (AGPL): UE4SS C++ mod + Node dedicated server. Trusted friends only — no anti-cheat.

## Host (one PC)

1. Use a **test copy** of the game for modding if you can; never surprise-overwrite the only Steam install without a backup of `Saved`.
2. UE4SS **experimental-latest** (5.6), SDO `main.dll` built and deployed under copy `ue4ss\Mods\SDO\`.
3. Node **22+** in `SDO/server`: `npm install`, `.env` secrets, `npm start`.
4. Ports (current code defaults): game **42200**, HTTP **42201** (older README may say 31000/31001).
5. Mint tickets: `POST http://127.0.0.1:42201/v1/tickets` with `playerId` + `displayName`. Raise `SDO_TICKET_TTL_MS` if boots are slow (default 120s).
6. Launch **copy** `SurrounDead-Win64-Shipping.exe` with  
   `-sdo_host=… -sdo_port=42200 -sdo_ticket=…`  
   **Never** `steam://run` if that would load an unmodded live install by mistake.
7. Forward 42200/42201 (or VPN) for remote friends.

## Friend

1. Same game build as host (Steam **26471822** / 0.8.0 line as tested here).
2. Same UE4SS pin + SDO `main.dll` as host.
3. Fresh ticket from host; launch with `-sdo_*` args.
4. Expect FirstJoin Barber on new `playerId`s; dismiss before free play.

## Known gaps (honest)

- Zombies may be invisible until an opt-in proxy fix is verified (`enable_zombie_proxies.flag` path — experimental).
- Vehicles untested; building broken on 5.6 per SDO README; combat not fully re-proven.
- Gear is mostly local-save; server does not fully restore inventory yet.

## Meetup debug

Write `x y z [yaw]` to `%APPDATA%\SDO\teleport.flag` (one client per write; shared APPDATA).
