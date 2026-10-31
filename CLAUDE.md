# CLAUDE.md

Notes for Claude sessions working on SquidBots Lite. **Keep this file up to
date** with anything future sessions need, in the same commit as the change.

## What this is

A WoW **5.4.8 (MoP)** addon (`SquidBotsLite/`, one file `SquidBotsLite.lua`)
for **SkyFire-playerbots** (`Brytenwally/SkyFire-playerbots`, module of
`Brytenwally/SkyFire_548`). Ported from the 3.3.5 CoA Bots version; the CoA
original's features without SkyFire commands were removed (README, "Not in this
version"). `Brytenwally/mod-playerbots` is a 3.3.5 reference only.

## Rules

- Commit and push straight to **`main`**. No feature branches or PRs unless the
  owner asks.
- **Every command the addon sends must exist in SkyFire-playerbots.** Check
  `COMMANDS.md` there and the code: chat orders in
  `PlayerbotAI::HandleChatCommand`, strategy names in
  `engine/BotStrategyEngine.cpp` (`co`/`nc`, comma lists ok), filters in
  `PlayerbotAI::MatchesRoleFilter` (`@tank @dps @heal @ranged`), GM commands in
  `PlayerbotScripts.cpp` (`.playerbots add|remove|list|init|...`, all GM-only).
  When the server gains a command, a removed feature can come back.

- **Owner's rule: the addon must not change the server.** Only use commands
  SkyFire-playerbots already has for the bots; never add server commands just
  for the addon (a `.playerbots alts` for My alts was added and reverted).
  Deleted characters are forgotten by hand (right-click a card in My alts).

## Server behaviour the addon relies on

- Bots whisper `co: +a, -b, ...` / `nc: ...` back after **every** co/nc order,
  even in party chat; the addon hides them for `QUIET_SECONDS` after its own.
  The combat list shows the role: `tank assist` / `save mana` / `dps assist`.
- Bots set their party role from their spec when grouped, so roles come from
  `UnitGroupRolesAssigned`; `co ?` is only a fallback for known bots.
- `summon` resurrects a dead bot next to you (there is no `release`).
- `.playerbots add` prints `Bot 'X' logged in.` (no auto-group: the addon
  invites; bots accept invites). `remove` prints `Bot 'X' logged out.`.
  `list` prints `Active bots: N` then `  Name (GUID n)` lines.
- `quests all` answers `--- Incompleted quests ---`, one line per quest (quest
  link, then ` - 3/8 objective, ...`), `--- Completed quests ---`, ...,
  `--- Summary ---`, `Total: n / 25 (...)` (the end marker). The Quests panel
  hides and shows those lines (`IsQuestAnswer`, `OnQuestLine`); a list typed by
  hand opens the panel too and stays in chat (`CaptureTypedList`). Quest lines
  start with the link: `Can't take [quest]` whispers are not list lines.
- Compare whisper senders/targets with `SameName` (no `-Realm` part, any case):
  bots get a virtual realm, so the client can name them `Name-Realm`. If the
  Quests window gets no answer, it prints the unexpected sender's exact name.
- My alts' logged-in state: `.playerbots list`, the add/remove answers
  (`'X' is not an active bot.`, `Could not add bot 'X': That character is
  already an active bot.` correct a stale state), or a group member that is
  **connected** (a party survives a server restart with offline bots in it).
- The server cannot list an account's characters: each character records itself
  in `SquidBotsLiteDB.altInfo` at login, and My alts lists those.

## Checking

No WoW client in the cloud container. Syntax and undefined names:

```sh
apt-get install -y lua5.1 lua-check
luac5.1 -p SquidBotsLite/SquidBotsLite.lua
luacheck SquidBotsLite/SquidBotsLite.lua --std lua51 --no-max-line-length --no-unused-args --globals <WoW API names used>
```

5.4.8 API notes: `IsInGroup`/`IsInRaid`/`GetNumGroupMembers`/`GetNumSubgroupMembers`,
`UnitIsGroupLeader`, `GROUP_ROSTER_UPDATE`, `GetSpecialization`; no `SetShown`
reliance, `SetBackdrop` is native (no BackdropTemplate). Role icons use
`UI-LFG-ICON-PORTRAITROLES` (64x64). Strings come in English and French; add both.
