<img src="docs/logo.png" alt="BedrockGuard" width="160" align="right">

# BedrockGuard

Anti-cheat for **Bedrock players** on Java servers running Geyser/Floodgate.

Java anti-cheats (Grim, Vulcan…) are built around the Java client. Bedrock players arrive through Geyser with
different movement, touch controls and timing, so those anti-cheats either exempt them or flag them by mistake.
BedrockGuard analyses **only Floodgate players** and works alongside your existing Java anti-cheat.

> **Status:** version 1.1.0 (combat detection) on all stores.

## Checks

| Check | Detects |
|---|---|
| Timer | Game running faster than real time |
| Speed | Horizontal movement above the vanilla limit (Speed effect and ice taken into account) |
| Fly | Vertical movement that ignores gravity, hovering in mid-air |
| Reach | Hits beyond 3 blocks, with ping compensation |
| Nuker | Several blocks broken in the same tick, or too many per second |
| Scaffold | Blocks placed in mid-air, several per tick, or too many per second |
| AutoClicker | Click rate above a human limit |
| BadPackets | Values no legitimate client can send |
| Velocity | Knockback ignored or reduced (AntiKnockback) |
| KillAura | Several targets hit in the same tick, hits on a target far outside the player's view, and (experimental) hits on a hidden verification bot |

Each check raises a violation level (VL) that decays over time. You get staff alerts in game, an optional Discord
webhook, and a configurable punishment command when a check reaches its maximum VL.

The combat checks (Velocity, KillAura) **only alert** by default: they never cancel a hit and do not count towards
the punishment until you set `punish: true` for them. Their alerts show the measured values and the player's last
hits, and `/bguard stats` gives alerts per hour of play for each input mode (touch, controller, keyboard and mouse),
so you can check them on your own server first.

## What it cannot see

BedrockGuard only sees what Geyser forwards to the server, as Java packets:

- Touch players hit wherever they tap, not where the camera points. The angle check therefore only catches hits far
  outside the field of view (behind the player). An aura that hits inside the field of view without turning the
  camera, at a legal distance and on one target at a time, is only caught by the verification bot.
- A moderately enlarged hitbox (Hitbox cheat) is not detected; Reach still caps the distance.
- No fine aim analysis (aimbot, smoothed aim): touch and controller aim are too different from a mouse to tell
  apart without false positives.
- Click rates above 20 per second cannot be measured: Geyser forwards one swing per tick.
- Velocity does not judge a hit when the player jumps or hits back right after it (jump reset and sprint hits
  legitimately reduce knockback), or when the player rides, swims, climbs or stands against a wall. A cheat that
  cancels knockback while the player walks backward looks like a normal knockback.
- Behind a proxy (Geyser on Velocity or BungeeCord), the real ping and the live input mode are unknown: Velocity
  waits longer before judging, and `/bguard info` shows "cautious mode".

## Requirements

- Paper or Folia **1.21 → 26.x**, Java 21+ (tested on Paper 1.21.11, Paper 26.3 and Folia 26.3)
- [Floodgate](https://geysermc.org/download) and [PacketEvents](https://modrinth.com/plugin/packetevents) 2.14+
  installed as plugins
- Geyser on the server, or on a proxy: tested with **Velocity** 4.2 (modern forwarding) and **BungeeCord** build 2104
  (ip_forward), Geyser + Floodgate on the proxy and Floodgate on the game server
- Keep Geyser's `forward-player-ping` at its default `false`: with `true`, Paper kicks real Bedrock clients
  ("keepalive response without matching challenge"). With Geyser on the game server, BedrockGuard reads the real
  Bedrock ping through the Geyser API
- Timer and hover detection need **1.21.2+**: older versions don't send the end-of-tick packet they rely on

Alerts, commands and logs are available in English and French.

## Editions

- **Free**: every check, up to **5 Bedrock players** analysed at the same time, available on [SpigotMC](https://www.spigotmc.org/resources/bedrockguard-free.139718/) and [CurseForge](https://www.curseforge.com/minecraft/bukkit-plugins/bedrockguard-free)
- **Full**: no player limit, [available on Payhip](https://payhip.com/b/PlDW4)

## Documentation

- [Configuration](docs/configuration.md)
- [Commands and permissions](docs/commands.md)
- [Developer API](docs/api.md)

## Support

Report bugs and false positives in [Issues](../../issues). The source code of the anti-cheat is not public.

## Also by the author

[BedrockAssist](https://github.com/TheMax98000/BedrockAssist) (free): native Bedrock menus for what Geyser cannot translate
(links, commands, enchantments, anvil, villagers), [available on SpigotMC](https://www.spigotmc.org/resources/bedrockassist.139746/).
