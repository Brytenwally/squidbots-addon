# SquidBots Lite for SkyFire (5.4.8)

A World of Warcraft **5.4.8 (Mists of Pandaria)** addon for [SkyFire-playerbots](https://github.com/Brytenwally/SkyFire-playerbots): your bots, without a window in the way.

A thin bar of buttons, the bots' roles drawn straight onto the party frames you already have, and **My alts** to log your other characters in as bots.

Port of the 3.3.5 [SquidBots Lite](https://github.com/Zyth45/squidbots-addon) for CoA Bots. Every command it sends was checked against what SkyFire-playerbots understands (its `COMMANDS.md`); features that need commands SkyFire-playerbots does not have were removed (see [Not in this version](#not-in-this-version)).

![The SquidBots Lite bar in game](images/bar.png)

## The bar

**Follow**, **Stay**, **Attack my target**, **Active / Passive**, **Dungeon**, **Regroup**, **Alts** and **Bots**, one click each.

- **Active / Passive**: passive bots only fight back when attacked (`passive` / `aggressive`). Any other order wakes them.
- **Dungeon**: bots stay with you, step out of damaging ground effects and open the fight right away (`co -wait for attack,+avoid aoe`, then `follow`).
- **Regroup**: every bot teleports next to you (`summon`).
- **Revive** shows up at the end of the bar while a bot is dead: dead bots come back to life next to you (`summon`).
- **Bots**: My alts, and group orders: best gear (`autogear`), learn new spells (`maintenance`), sell junk and repair (`sell`), take the quests nearby (`accept`).

Right-click the bar for profiles: **Farm** (hunt, loot, eat), **Strict follow**, **Dungeon mode** / **Leave dungeon mode**, best gear, sell, and **Orders by role**: tank attacks my target, damage attacks my target, healers follow me or stay here, ranged follow me (`@tank`, `@dps`, `@heal`, `@ranged`).

Shift-drag a button to move the bar; Ctrl + mouse wheel resizes it.

## Key bindings

Follow, Stay, Attack, Passive, Dungeon, Regroup, Revive and My alts can each get a key: Esc > Key Bindings > SquidBots Lite. The keys work with the bar hidden.

## On your party frames

Each member gets a role icon on its party frame (and on the raid-style frames, `/sbl raidframes off` to stop). The role is the member's party role, which SkyFire-playerbots sets from the bot's specialization when it joins the group. Click the icon for that bot alone: come to me, stay here, attack my target, best gear, learn new spells, sell and repair, its quests, make it the leader, remove from group. A tank adds **Mark the pulls with a skull** (`rti skull`); a healer adds **Heal only** (`co +heal,-healer dps`) and **Heal, and deal damage when nobody needs it** (`healer dps`).

A bot with no party role yet is asked `co ?` once to learn it; the question and the answer stay out of your chat. Only members known as bots are asked (your alts, bots listed by `.playerbots list`, members that already answered like a bot), never a real player; `/sbl roles off` turns it off.

SkyFire-playerbots bots answer every `co` / `nc` order with their strategies; the addon hides those answers for the orders it sends itself.

## My alts

The **Alts** button (or **Bots > My alts**, or `/sbl alts`) lists your other characters, logged-in ones first, with their level, class and specialization. **Log in** brings one online as a bot (`.playerbots add`) and invites it to your group; **Summon** brings it next to you, since it logs in where it logged out; **Quests** shows the quests it is on; **Log out** sends it home (`.playerbots remove`). Who is logged in comes from `.playerbots list`.

**Quests** (also in a bot's menu on its party frame, or `/sbl quests <name>`; your target when no name) whispers the bot `quests all` and opens its quest log in a small window: the quests in progress with their objectives (`3/8 Kobold Vermin`), then the ones ready to turn in, and the total. Hover a quest for its tooltip, shift-click to link it in chat. The bot's answer stays out of your chat. A `quests all` (or `quests incompleted` / `completed` / `summary`) you type yourself, to one bot or the group, opens in the window too and stays in the chat. Needs SkyFire-playerbots with the `quests all` command.

- The server cannot list an account's characters to a player, so each character notes itself when you play it with the addon on: log in once on each of your characters. Right-click a card to forget a deleted character.
- `.playerbots add / remove / list` are **GM commands** on SkyFire: My alts needs GM rights on the realm.
- **Teams**: save your group, or the logged-in alts, under a name, then log the whole team in or out in one click (one a second; a sixth member turns your party into a raid).

![My alts](images/my-alts.png)

## GM mode

`/sbl gm` adds GM items to the menus: **List the active bots** (`.playerbots list`), and on a bot's menu **Switch role** (`.playerbots init <name> tank|healer|dps`, which also re-gears it) and **Log out** (`.playerbots remove`).

## Commands

```
/sbl                 show or hide the bar
/sbl show | hide     show or hide it, said outright
/sbl scale <0.5-2>   bar size (1 = normal; /sbl scale alone says the current one)
/sbl lang fr | en    French or English
/sbl alts            your other characters, played as bots
/sbl quests [name]   a bot's quest log (your target when no name)
/sbl roles on | off  ask bots without a party role for it
/sbl raidframes on | off  role icons on the raid-style frames
/sbl gm              GM mode
/sbl reset           put the bar back in the middle
```

The bar's size, place and shown/hidden state are kept per character; the language and other choices stay account-wide.

## Install

Copy the `SquidBotsLite` folder into `World of Warcraft/Interface/AddOns` (5.4.8 client), or unzip the latest file in `release/` there.

## Not in this version

These features of the CoA version call commands SkyFire-playerbots does not have, so they were removed. They can come back once the commands exist on the server:

| Feature | Commands it needs |
| --- | --- |
| Asking for bots (lfg offers, raid of 10 / 25, regear) | `lfg bot`, `.playerbots coa ...` |
| Bot sheet (gear, bags, bank, quests, talents, mounts, stats) | `c`, `e`, `ue`, `u`, `t`, `s`, `bank`, `drop`, `quests all`, `talents ...`, `mount ?`, `stats` |
| Missions (gathering alts) | `mission ...` |
| Dungeon clear panel | `.dc ...` (mod-dungeon-clear) |
| Specializations panel, Balance the group | `talents spec list`, `talents spec <name>` (GM mode keeps a role switch through `.playerbots init`) |
| Tank auto pull on / off | `nc +coa auto pull` |
| Formations | `formation ...` |
| Give me the lead, Heal only me | `give leader`, `focus heal ...` |
| Melee: back to me | the `@melee` filter |
| Gathering everything / skill-ups only, learn from a trainer | `nc +gather all`, `trainer learn` |
| Linked accounts, players logging their alts in | `.playerbots bot ...`, `.playerbots account ...` |
| Low mana / Pulling / Rez alerts, version check | messages SkyFire-playerbots bots do not say |

## License

GPL-2.0, see [LICENSE](LICENSE).
