# BedrockGuard — Developer API

BedrockGuard only analyses Bedrock players (Geyser/Floodgate). Its API lets your plugin react to violations,
cancel them, or read violation levels (VL).

## Setup

The API ships inside the plugin jar. Put `bedrockguard-<version>.jar` in your project (for example under `libs/`)
and compile against it without shading it:

```kotlin
// build.gradle.kts
dependencies {
    compileOnly(files("libs/bedrockguard-1.0.0.jar"))
}
```

```xml
<!-- pom.xml -->
<dependency>
  <groupId>fr.mcplugins</groupId>
  <artifactId>bedrockguard</artifactId>
  <version>1.0.0</version>
  <scope>system</scope>
  <systemPath>${project.basedir}/libs/bedrockguard-1.0.0.jar</systemPath>
</dependency>
```

```yaml
# plugin.yml
softdepend: [BedrockGuard]   # or depend, if your plugin cannot run without it
```

With `softdepend`, check `Bukkit.getPluginManager().isPluginEnabled("BedrockGuard")` before you touch the API.

## `ViolationEvent`

Fired each time a Bedrock player fails a check, **before** any consequence. Cancelling it skips the VL increase,
the staff alert, the Discord message and the punishment. The protective action of the check itself (cancelled
hit, block break or placement, movement setback) still happens.

| Method | Description |
|---|---|
| `getPlayer()` | The Bedrock player |
| `getCheck()` | `Timer`, `Speed`, `Fly`, `Reach`, `Nuker`, `Scaffold`, `AutoClicker` or `BadPackets` |
| `getDetail()` | Human-readable detail, as shown in the alert |
| `getViolationLevel()` | VL of this check after this violation |
| `setCancelled(boolean)` | Cancel the violation |

The event is synchronous: it runs on the player's thread (main thread on Paper, the player's region thread on
Folia).

## `BedrockGuardApi`

| Method | Description |
|---|---|
| `isTracked(Player)` | `true` if BedrockGuard analyses this player (Bedrock player; within the limit on the free version) |
| `getViolationLevel(Player, String check)` | Current VL of a check, decay included; `0` if none |
| `getViolationLevels(Player)` | Current VL of every failed check (copy, sorted by check name) |

These methods can be called from any thread.

## Example

Ignore movement checks while a player is in a minigame with launch pads, and log the others:

```java
import fr.mcplugins.bedrockguard.BedrockGuardApi;
import fr.mcplugins.bedrockguard.ViolationEvent;
import java.util.Set;
import java.util.UUID;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;

public final class MinigameBridge implements Listener {

    private final Set<UUID> inLaunchPadGame;

    public MinigameBridge(Set<UUID> inLaunchPadGame) {
        this.inLaunchPadGame = inLaunchPadGame;
    }

    @EventHandler(ignoreCancelled = true)
    public void onViolation(ViolationEvent event) {
        boolean movement = event.getCheck().equals("Speed") || event.getCheck().equals("Fly");
        if (movement && inLaunchPadGame.contains(event.getPlayer().getUniqueId())) {
            event.setCancelled(true);
            return;
        }
        double total = BedrockGuardApi.getViolationLevels(event.getPlayer()).values().stream()
                .mapToDouble(Double::doubleValue).sum();
        event.getPlayer().getServer().getLogger().info(event.getPlayer().getName() + " total VL: " + total);
    }
}
```
