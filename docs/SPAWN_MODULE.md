---
layout: page
title: SPAWN_MODULE
---

# Spawn Module

The optional AdvancedGroups spawn module opens a server-authoritative location selector whenever DayZ creates a fresh player character. Existing living characters are not moved when they reconnect.

## Configuration

The server creates `$profile:AdvancedGroups/spawn_config.json` on first start. The packaged `spawn_config.json` is an example and is not read directly at runtime.

- `enabled`: Enables the module. It defaults to `false` so installing AdvancedGroups cannot unexpectedly alter player spawning.
- `forceSelection`: Prevents closing the selector until a valid location is chosen.
- `allowRandom`: Enables the random-deployment action.
- `randomIgnoresCooldowns`: Allows random deployment even when named choices are cooling down.
- `points`: Named deployment regions.

Each point has a stable `id`, player-facing `name`, map `center`, random placement radius, cooldown in seconds, and an optional array of candidate `positions`. When candidate positions are present, the server chooses one and applies the configured random radius around it. Otherwise it uses the center. Final terrain height and world bounds are validated by the server.

Example:

```json
{
    "id": "chernogorsk",
    "name": "Chernogorsk",
    "enabled": true,
    "center": [6580.0, 0.0, 2440.0],
    "randomRadius": 250.0,
    "cooldownSeconds": 900,
    "positions": []
}
```

Cooldown state is stored separately in `$profile:AdvancedGroups/spawn_cooldowns.json` and survives restarts. Clients only submit a stable spawn ID; the server rechecks pending-respawn state, availability, cooldown, destination bounds, and final placement.

The existing AdvancedGroups server-config reload action also reloads the spawn configuration.

## Rollout

Configure points for the active terrain before changing `enabled` to `true`. The packaged coordinates are Chernarus examples and should not be used unchanged on another terrain.
