---
layout: page
title: EVENT_MARKER_CONFIG
---

# AdvancedGroups Event Marker Config

Status: Beta v0.4.0 plus unreleased development work, verified 2026-10-06.

Event markers let server owners create read-only global markers when configured world objects spawn. This is configured by classname and does not require another mod to call the AdvancedGroups marker API.

The config is created automatically at:

```text
$profile:AdvancedGroups/event_markers.json
```

The default file is disabled so existing servers do not suddenly show extra markers.

## Basic Config

```json
{
	"configVersion": 2,
	"enabled": true,
	"syncIntervalSeconds": 15.0,
	"maxActiveMarkers": 120,
	"entries": [
		{
			"className": "Wreck_Mi8_Crashed",
			"label": "Heli Crash",
			"markerType": 24,
			"color": "FFD700",
			"displayMode": 0,
			"circleRadius": 150.0,
			"circleColor": "FFD700",
			"drawStrikeLines": false,
			"strikeColor": "FFD700",
			"removeWhenObjectDeleted": true,
			"positionMergeDistance": 8.0
		}
	]
}
```

## Fields

- `enabled`: master on/off switch for event markers.
- `syncIntervalSeconds`: how often marker changes are broadcast. Values are clamped from 5 to 300 seconds.
- `maxActiveMarkers`: safety cap for generated markers. Values are clamped from 1 to 500.
- `className`: exact object classname to watch for.
- `label`: marker label shown to players.
- `markerType`: `AGMarkerType` numeric value.
- `color`: six-character RGB hex color, without `#`.
- `displayMode`: server marker visibility mode.
- `circleRadius`: optional map radius in meters.
- `circleColor`: optional radius color.
- `drawStrikeLines`: optional radius fill/strike lines.
- `strikeColor`: optional strike-line color.
- `removeWhenObjectDeleted`: removes the marker when the watched object is deleted.
- `positionMergeDistance`: prevents duplicate markers for the same classname near the same position.

## Display Modes

```text
0 = 2D + 3D
1 = 3D only
2 = 2D only
3 = hidden
```

## Marker Type Examples

```text
24 = HELICOPTER
31 = POLICE
44 = STAR
66 = RUNWAY
70 = SERVER_ZONE
```

## Safety Notes

Event markers are generated at runtime and are not written into `server_config.json`.

Generated markers use reserved negative marker IDs and are treated as read-only in the marker UI. Admin-created server markers keep using the existing positive-ID config flow.

The first implementation listens for object lifecycle events. If a server owner enables the feature while the server is already running, existing world objects may need to respawn or the server may need a restart before all configured event objects are detected.
