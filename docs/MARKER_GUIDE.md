---
layout: page
title: MARKER_GUIDE
---

# AdvancedGroups Marker Integration Guide

Status: Beta v0.4.0 plus unreleased development work, verified 2026-10-06.

This guide is for DayZ mod creators who want to place AdvancedGroups markers from server-side script. It documents the current working integration surface only. Server-owner classname-triggered event markers are configured separately in `EVENT_MARKER_CONFIG.md`; `AG_Event_1` trigger objects are still roadmap items and are intentionally not used here.

## Requirements

Add AdvancedGroups as a dependency of your mod so its classes are available before your scripts run.

```cpp
class CfgPatches {
	class MyServerMod {
		requiredAddons[] = {"DZ_Characters", "DZ_Data", "DZ_Scripts", "AdvancedGroups"};
	};
};
```

Call the marker helpers on the server. The preferred supported surface is `AGPublicAPI`.

The current integration contract is API version 2. Check it during integration startup:

```c
if (!AGPublicAPI.SupportsAPIVersion(2)) {
	Print("Required AdvancedGroups public API is unavailable");
	return;
}
```

Group lookup methods return detached snapshots. Reading or modifying a returned object never mutates live AdvancedGroups state; use the provided mutation methods for changes that require persistence and synchronization.

```c
bool ready = AGPublicAPI.IsReady();
```

Do not write directly into group JSON, private marker JSON, or `server_config.json` from another mod. The public API routes through AdvancedGroups server logic so persistence, validation, permissions, and sync stay centralized.

## Marker Types

Use `AGMarkerType` for marker icons. Common choices:

```c
AGMarkerType.GENERIC
AGMarkerType.RALLY
AGMarkerType.DANGER
AGMarkerType.LOOT
AGMarkerType.BASE
AGMarkerType.PING
AGMarkerType.CAR
AGMarkerType.SKULL
AGMarkerType.HOSPITAL
AGMarkerType.SAFEZONE
AGMarkerType.HELICOPTER
AGMarkerType.POLICE
AGMarkerType.STAR
AGMarkerType.RUNWAY
AGMarkerType.SERVER_ZONE
```

Colors are six-character RGB hex strings such as `"FFD700"`, `"FF3333"`, or `"00FF66"`. Do not include `#`.

## Visibility

Group and private markers use `visibilityState`:

```text
0 = hidden
1 = 2D only
2 = 2D + 3D
3 = 3D only
```

Server markers use `displayMode`:

```text
AG_SVRMARKER_SHOW_ALL  = 2D + 3D
AG_SVRMARKER_SHOW_3D   = 3D only
AG_SVRMARKER_SHOW_2D   = 2D only
AG_SVRMARKER_SHOW_NONE = hidden
```

## Group Markers

Use group markers when the marker should be visible to a player's group.

```c
bool ok = AGPublicAPI.PlaceGroupMarker(
	player,
	"Rally Point",
	AGMarkerType.RALLY,
	player.GetPosition(),
	0,
	"00FF66",
	2
);
```

This call:

- Requires `player` to have an identity.
- Requires `player` to be in a group.
- Uses AdvancedGroups group marker permissions.
- Respects the group's marker limit.
- Saves to the group JSON.
- Broadcasts updated group markers to group members.
- Sends the marker webhook if marker webhooks are enabled.

If it returns `false`, the player is probably not in a group, lacks permission, or the group marker limit is reached.

## Private Markers

Use private markers when the marker should be visible only to one player.

```c
bool ok = AGPublicAPI.PlacePrivateMarker(
	player,
	"Personal Stash",
	AGMarkerType.LOOT,
	"7500 0 7500".ToVector(),
	0,
	"FFD700",
	2
);
```

This call:

- Requires `player` to have an identity.
- Saves to `$profile:AdvancedGroups/private_markers/<steamId>.json`.
- Syncs the updated private marker list back to that player.
- Uses `cfg.maxMarkersPerGroup` as the current private marker limit.

Private markers do not require the player to be in a group.

## Server And Event Markers

Use `PlaceAdminServerMarker` when the action is performed by a live admin player and should be managed as a normal admin server marker.

```c
bool ok = AGPublicAPI.PlaceAdminServerMarker(
	admin,
	"Event Zone",
	AGMarkerType.DANGER,
	"5000 0 5000".ToVector(),
	"FF3333",
	AG_SVRMARKER_SHOW_ALL,
	250.0,
	"FF3333",
	true,
	"FFAA00"
);
```

Parameters:

```text
admin            PlayerBase with SteamID in server_config.json adminIds
label            marker label
markerType       AGMarkerType icon enum
pos              world position
hexColor         text/icon color, RGB hex
displayMode      server marker display mode
circleRadius     optional map radius
circleColor      optional radius color, defaults to hexColor when blank
drawStrikeLines  optional radius strike fill
strikeColor      optional strike-line color, defaults to circleColor when blank
```

This call:

- Requires `admin` to be listed in `server_config.json` `adminIds`.
- Saves to `server_config.json`.
- Broadcasts server marker updates to players.
- Supports radius and strike-line data.
- Sends the marker webhook if marker webhooks are enabled.

Use `PlaceEventMarker` or `PlaceServerMarker` for trusted server-side lifecycle markers from another mod. These markers are stored separately in `$profile:AdvancedGroups/event_runtime_markers.json` and appear under the AdvancedGroups Events marker category.

```c
bool ok = AGPublicAPI.PlaceEventMarker(
	"MyServerMod",
	"My Server Mod",
	"Automated Event",
	AGMarkerType.STAR,
	"6000 0 6000".ToVector(),
	"FFD700",
	AG_SVRMARKER_SHOW_ALL,
	300.0,
	"FFD700",
	true,
	"FFAA00",
	true
);
```

The final `replaceExisting` argument keeps one active marker per owner/label pair. That is the safest default for landing events, boss events, convoy events, and other lifecycle markers.

Remove event markers when the event ends:

```c
int removed = AGPublicAPI.RemoveEventMarkers("MyServerMod", "Automated Event");
```

Find an existing marker id without editing JSON:

```c
int markerId = AGPublicAPI.FindEventMarker("MyServerMod", "Automated Event");
```

`PlaceServerMarker` remains as a compatibility alias for trusted server-owned event markers, but new integrations should prefer the clearer `PlaceEventMarker` name.

## Common Recipes

Place a marker for a player's group at an objective:

```c
void MarkObjectiveForGroup(PlayerBase player, vector pos)
{
	if (!GetGame().IsServer()) return;
	AGPublicAPI.PlaceGroupMarker(player, "Objective", AGMarkerType.FLAG, pos, 0, "00E676", 2);
}
```

Place a private death/stash marker:

```c
void MarkPrivateLocation(PlayerBase player, vector pos)
{
	if (!GetGame().IsServer()) return;
	AGPublicAPI.PlacePrivateMarker(player, "My Cache", AGMarkerType.STRONGBOX, pos, 0, "FFD47A", 2);
}
```

Place an admin-created radius marker:

```c
void MarkAdminEvent(PlayerBase admin, vector pos)
{
	if (!GetGame().IsServer()) return;
	AGPublicAPI.PlaceAdminServerMarker(admin, "Admin Event", AGMarkerType.STAR, pos, "FFD700", AG_SVRMARKER_SHOW_ALL, 300.0, "FFD700", true, "FFAA00");
}
```

Use terrain height before placing a marker:

```c
vector SnapToSurface(vector pos)
{
	pos[1] = GetGame().SurfaceY(pos[0], pos[2]);
	return pos;
}
```

## Reading Group State

You can read group ownership before deciding what marker to place.

```c
string steamId = player.GetIdentity().GetId();
AGGroupData group = AGPublicAPI.GetGroupForPlayer(steamId);

if (group) {
	AGPublicAPI.PlaceGroupMarker(player, "Group Target", AGMarkerType.TRIANGLE_TARGET, player.GetPosition(), 0, "FFEE00", 2);
} else {
	AGPublicAPI.PlacePrivateMarker(player, "Solo Target", AGMarkerType.TRIANGLE_TARGET, player.GetPosition(), 0, "FFEE00", 2);
}
```

## Best Practices

- Run marker placement from server-side code.
- For player-owned group/private/admin actions, pass a real `PlayerBase` with identity.
- Use `AGPublicAPI.PlaceGroupMarker` for group visibility, `AGPublicAPI.PlacePrivateMarker` for one-player visibility, `AGPublicAPI.PlaceAdminServerMarker` for admin-created global visibility, and `AGPublicAPI.PlaceEventMarker` for trusted server-owned lifecycle markers that should appear under Events.
- Keep labels short; they render in 2D and 3D contexts.
- Use valid RGB hex strings without `#`.
- Check the boolean return value and avoid retry loops every frame.
- Prefer surface-snapped positions for world objects.
- Avoid direct JSON edits; use `AGPublicAPI` so sync and persistence stay correct.

## Current Limitations

- Server-owner event markers can be configured by classname in `event_markers.json`, but `AG_Event_1` through `AG_Event_10` helper trigger objects are still roadmap items.
- The current public API exposes lifecycle create/find/remove helpers for event markers. Dedicated third-party marker update callbacks are still future work.
