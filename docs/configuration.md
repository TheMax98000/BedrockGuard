# Configuration

`plugins/BedrockGuard/config.yml`. Apply changes with `/bguard reload`. New keys are added automatically to existing
files when you update the plugin.

## General

| Key | Default | Description |
|---|---|---|
| `language` | `en` | Language of alerts, commands and logs: `en` or `fr` |
| `debug` | `false` | Log every analysed movement and packet (diagnostics only, very verbose) |
| `violations.decay-per-second` | `0.1` | VL lost per second, for every check |
| `punish.max-vl` | `10` | VL at which the punishment command runs (the VL then resets to 0) |
| `punish.command` | `kick {player} [BedrockGuard] Suspicious behaviour ({check})` | Console command; `{player}` and `{check}` are replaced. Empty = no punishment |
| `discord.webhook-url` | `""` | Discord webhook URL (channel → Edit → Integrations → Webhooks). Empty = disabled |
| `discord.min-interval-seconds` | `30` | Minimum delay between two messages for the same player and check. Punishments are always sent |

## Checks

Every check has `enabled: true|false`.

| Key | Default | Description |
|---|---|---|
| `checks.timer.max-ahead-ms` | `150` | How far ahead of real time the client may run before an alert (ms) |
| `checks.timer.max-lag-ms` | `2000` | Maximum lag that can be "caught up" at once (ms) |
| `checks.speed.max-per-tick` | `0.7` | Maximum horizontal movement per tick (sprint-jump ≈ 0.62). ×Speed effect, ×2.6 on ice |
| `checks.speed.buffer` | `0.5` | Accumulated excess (blocks) before an alert |
| `checks.speed.setback` | `true` | Teleport the player back to their last safe position |
| `checks.fly.tolerance` | `0.05` | Allowed deviation from vanilla gravity (block/tick) |
| `checks.fly.hover-ticks` | `10` | Client ticks motionless in mid-air before an alert |
| `checks.fly.setback` | `true` | Teleport the player back to their last safe position |
| `checks.reach.max-distance` | `3.0` | Maximum eye → hitbox distance (vanilla survival: 3) |
| `checks.reach.hitbox-margin` | `0.1` | Margin added to the target's hitbox |
| `checks.reach.extra-compensation-ms` | `150` | Compensation window added to the attacker's ping (ms) |
| `checks.reach.cancel-hit` | `true` | Cancel out-of-reach hits |
| `checks.nuker.max-per-tick` | `1` | Blocks broken in the same tick (a legitimate player breaks one) |
| `checks.nuker.max-per-second` | `20` | Blocks broken per second |
| `checks.scaffold.max-per-tick` | `1` | Blocks placed in the same tick |
| `checks.scaffold.max-per-second` | `10` | Blocks placed per second (fast touch bridging ≈ 6-8/s) |
| `checks.autoclicker.max-cps` | `18` | Clicks per second (arm swings outside block breaking). Geyser forwards at most one swing per tick, so nothing above 20 can be measured |
| `checks.badpackets` | — | Pitch outside [-90, 90] |

Nuker and Scaffold cancel the offending break or placement.
