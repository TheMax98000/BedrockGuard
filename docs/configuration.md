# Configuration

`plugins/BedrockGuard/config.yml`. Apply changes with `/bguard reload`. New keys are added automatically to existing
files when you update the plugin.

## General

| Key | Default | Description |
|---|---|---|
| `language` | `en` | Language of alerts, commands and logs: `en` or `fr` |
| `debug` | `false` | Log every analysed movement and packet (diagnostics only, very verbose) |
| `update-check` | `true` | At startup, ask the GitHub releases of this repository whether a newer version exists, and tell admins (`bedrockguard.admin`) when they join. One request per startup, nothing is sent about your server |
| `violations.decay-per-second` | `0.1` | VL lost per second, for every check |
| `punish.max-vl` | `10` | VL at which the punishment command runs (the VL then resets to 0) |
| `punish.command` | `kick {player} [BedrockGuard] Suspicious behaviour ({check})` | Console command; `{player}` and `{check}` are replaced. Empty = no punishment |
| `discord.webhook-url` | `""` | Discord webhook URL (channel → Edit → Integrations → Webhooks). Empty = disabled |
| `discord.min-interval-seconds` | `30` | Minimum delay between two messages for the same player and check. Punishments are always sent |

## Checks

Every check has `enabled: true|false`. A check with `punish: false` alerts but never counts towards
`punish.max-vl` (default for Velocity and KillAura; the other checks punish).

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
| `checks.reach.mob-extra-distance` | `0.6` | Extra distance allowed on mobs. Positions of mobs a player has hit are rewound like players' (a hit mob flees, and Bedrock shows it late); the margin also covers the first hit |
| `checks.reach.cancel-hit` | `true` | Cancel out-of-reach hits on players (hits on mobs are only flagged) |
| `checks.nuker.max-per-tick` | `1` | Blocks broken in the same tick (a legitimate player breaks one) |
| `checks.nuker.max-per-second` | `20` | Blocks broken per second |
| `checks.scaffold.max-per-tick` | `1` | Blocks placed in the same tick |
| `checks.scaffold.max-per-second` | `10` | Blocks placed per second (fast touch bridging ≈ 6-8/s) |
| `checks.autoclicker.max-cps` | `18` | Clicks per second (arm swings outside block breaking). Geyser forwards at most one swing per tick, so nothing above 20 can be measured |
| `checks.badpackets` | — | Pitch outside [-90, 90] |
| `checks.velocity.punish` | `false` | Count Velocity alerts towards the punishment |
| `checks.velocity.min-ratio` | `0.5` | Alert when the player moves back less than this share of the knockback. A legitimate player pushing towards the attacker still takes about 2/3 of it |
| `checks.velocity.buffer` | `3` | Knockbacks below `min-ratio` (roughly in a row) before an alert |
| `checks.velocity.min-horizontal` | `0.2` | Ignore weaker knockbacks (block/tick): too small to measure |
| `checks.velocity.window-ticks` | `8` | Client ticks watched once the knockback has reached the player (after twice their ping); mobile networks can deliver it several ticks late. Counted in client ticks, not time: a frozen client (lag, app in the background, pause menu) is never judged, and a refused knockback only counts once the player has kept playing for a second |
| `checks.velocity.proxy-window-ms` | `1000` | Assumed delay behind a proxy, where the real ping is unknown (ms) |
| `checks.velocity.max-ping-ms` | `400` | Knockbacks are not checked above this ping (ms) |
| `checks.killaura.punish` | `false` | Count KillAura alerts towards the punishment |
| `checks.killaura.max-angle` | `100` | Largest angle between the view and the target (degrees). Stays wide because touch players hit wherever they tap |
| `checks.killaura.min-distance` | `1.0` | Targets closer than this are ignored (the angle means nothing at contact) |
| `checks.killaura.include-mobs` | `true` | Also check hits on mobs |
| `checks.killaura.multi-target-count` | `2` | Ticks with 2 targets or more hit before an alert… |
| `checks.killaura.multi-target-window-ms` | `5000` | …within this window (ms, at most 10000) |
| `checks.killaura.verification-bot.enabled` | `false` | Experimental: a fake player, shown only to the suspect, appears behind them for a moment. Nobody can hit what they cannot see; a KillAura does |
| `checks.killaura.verification-bot.auto` | `true` | Also send it automatically when the player fights another player |
| `checks.killaura.verification-bot.distance` | `2.0` | Distance behind the player (blocks) |
| `checks.killaura.verification-bot.duration-ticks` | `20` | How long it stays (ticks) |
| `checks.killaura.verification-bot.cooldown-seconds` | `60` | Minimum delay between two automatic checks of the same player |

Nuker and Scaffold cancel the offending break or placement.
