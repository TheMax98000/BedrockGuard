# Commands and permissions

| Command | Description |
|---|---|
| `/bguard alerts` | Toggle staff alerts for yourself |
| `/bguard info <player>` | Device, input mode, ping, current VL and last 5 hits (target, distance, angle, ping) of a Bedrock player |
| `/bguard stats` | Alerts per hour of play for each input mode, since the server started |
| `/bguard check <player>` | Send the verification bot behind a player now (needs `checks.killaura.verification-bot.enabled`) |
| `/bguard exempt <player> [seconds]` | Exempt a player from movement checks (default: 60 s) |
| `/bguard reload` | Reload `config.yml` (Timer settings apply to players at their next join) |

| Permission | Default | Description |
|---|---|---|
| `bedrockguard.admin` | op | Use `/bguard` |
| `bedrockguard.alerts` | op | Receive alerts in game |
