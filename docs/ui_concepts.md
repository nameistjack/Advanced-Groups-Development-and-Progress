---
layout: page
title: ui_concepts
---

# AdvancedGroups Documentation

AdvancedGroups is a DayZ group, marker, chat, and server-information mod. This page documents the current UI direction, marker model, server config behavior, and storage layout so the GitHub repository has a usable wiki-style reference instead of loose concept notes.

Current documentation status: Beta v0.4.0 plus unreleased development work, verified 2026-10-06. Future documentation and changelog updates should keep the beta version label until a stable release is confirmed.

## User Interface

AdvancedGroups uses a tactical sidebar model: navigation stays on the left, the active page owns the center/right workspace, and map-based pages use the shared map behind or beside the page content. The marker panel is sized for ultrawide use, with a fixed left navigation rail, a wider marker list column, and the map viewport beginning to the right of that content panel.

```text
+---+--------------------------+----------------------------------------------+
|   |                          |                                              |
| i |  INFO / GROUP / MARKERS  |                                              |
|   |--------------------------|                                              |
| g |  Active page content     |                                              |
|   |  Lists, controls, forms  |                                              |
| m |--------------------------|                 SHARED MAP                   |
|   |  Marker / admin tools    |             or content workspace             |
| s |                          |                                              |
|   |                          |                                              |
| a |                          |                                              |
|   |                          |                                              |
+---+--------------------------+----------------------------------------------+
```

The main pages are:

- **Info**: server information, tabbed text pages, Discord/website buttons, player stats placeholders, and a native 3D player preview.
- **Group**: group overview, members, invites, ranks, and group map context.
- **Public Groups**: player-visible service/group directory for admin-created public teams such as Moderators, Taxi, Medic, Traders, or Event Staff.
- **Map / Markers**: shared map, server/group/private/event marker lists, marker placement, 2D/3D controls, and marker popup editing.
- **Admin Command Center**: admin-only group, member, marker, zone, and tactical map workspace.
- **Admin Config Studio**: admin-only server config, UI defaults, chat/GPS rules, permissions, and maintenance workspace.

Player-facing navigation should stay lean. Admin-only configuration belongs in the admin suite, not in separate partial player settings tabs. The older HUD, Appearance, Layout, Server Config, Admin Groups, and Zones tab concepts should be treated as WIP material to fold into the two production admin pages.

| Page                 | Scope        | Persistence target                                      |
|----------------------|--------------|---------------------------------------------------------|
| Info                 | Player       | `$profile:AdvancedGroups/server_info.json` text/link data |
| Group                | Player       | group JSON files                                        |
| Public Groups        | Player/Admin | group JSON plus admin visibility flags                  |
| Map / Markers        | Player/Admin | group JSON, private marker JSON, `server_config.json`   |
| Admin Command Center | Admin        | group JSON, private marker JSON, `server_config.json`   |
| Admin Config Studio  | Admin        | `server_config.json`, admin permission config, info config |

## Info Page

The info page is designed as the server-facing welcome/wiki surface inside the mod. It has a large content area, server-configured info tabs, and a right sidebar for player information.

```text
+------------------------------------------------------------------------------+
| AdvancedGroups                                      Server title / status area  |
| Welcome to the server                                                        |
|==============================================================================|
|                                                                              |
| [ Welcome ] [ Rules ] [ Guides ]                                             |
|                                                                              |
| +---------------------------------------------------+  +---------------------+ |
| | MainScroll / rich server text                     |  | Player Information  | |
| |                                                   |  | Name                | |
| | - server rules                                    |  |                     | |
| | - crafting notes                                  |  | Health        --    | |
| | - base building info                              |  | Blood         --    | |
| | - event schedule                                  |  | Water         --    | |
| |                                                   |  | Food          --    | |
| |                                                   |  |                     | |
| |                                                   |  | Pos: X:0 Z:0        | |
| +---------------------------------------------------+  |                     | |
|                                                        | +-----------------+ | |
|                                                        | | 3D player       | | |
|                                                        | | preview         | | |
|                                                        | +-----------------+ | |
|                                                        +---------------------+ |
|                                                                              |
| [ EXIT ]                                             [ WEBSITE ] [ DISCORD ]  |
+------------------------------------------------------------------------------+
```

The 3D preview uses DayZ's native `PlayerPreviewWidget`, binds to the local player, updates the item in hands, and supports drag rotation plus mouse-wheel zoom.

## Roadmap


## Public Groups Concept

Public Groups are admin-created, server-facing groups for public service roles rather than normal private player clans. Examples include Moderators, Taxi, Medic, Mechanics, Traders, Event Staff, Police, or other server departments that players should be able to discover quickly.

The concept should replace the need to overload normal group names such as Admin or VIP. Normal players should not be able to create groups with reserved service/category names, while admins can create and curate public groups through the Admin Command Center.

### Player-Facing Public Groups Tab

Add a small player-facing tab with a player/group icon in the main navigation. The page title should read `Active Public Groups`.

```text
+------------------------------------------------------------------------------+
| Active Public Groups                                      status: 4 online     |
|==============================================================================|
|                                                                              |
| [icon] Moderators                         Online: 2                           |
|        Alice, Blake                                                           |
|                                                                              |
| [icon] Medics                             Online: 1                           |
|        Cara                                                                  |
|                                                                              |
| [icon] Taxi Service                       Online: 0                           |
|        No active members                                                       |
|                                                                              |
| [icon] Event Staff                        Online: 1                           |
|        Devon                                                                 |
|                                                                              |
+------------------------------------------------------------------------------+
```

The player page should be read-only. It should not expose private group management actions, ranks, SteamIDs, invites, marker management, or admin controls. Its job is discovery: who can help, which service groups are active, and whether contacting that service makes sense right now.

### Admin Controls

Admins should be able to mark a group as public from the Admin Command Center. Public group management belongs beside the existing admin group details, because admins already inspect groups, categories, online members, and group identity there.

Recommended admin controls:

- `Public Group` toggle: exposes the group in the player-facing Public Groups page.
- `Public Label`: optional display name override, for example `Taxi Service` instead of the internal group name.
- `Public Icon`: reuse the marker/icon catalog, for example `player.paa`, `hospital.paa`, `sedan-car-model.paa`, `shield.paa`, or `star.paa`.
- `Public Color`: tint for the group row/icon.
- `Show public group name`: when enabled, players see the public group row.
- `Show public player names`: when enabled, players also see online member names under that group.
- `Show online count`: when enabled, players see `Online: N` even if names are hidden.

Visibility modes should be explicit:

```text
Hidden                 -> not listed
Group name only        -> "Medics", online count optional
Group + online players -> "Medics" plus visible online member names
```

This lets admins advertise official server services without exposing staff rosters if they do not want to. For example, a server could show `Moderators Online: 2` without listing moderator names, while allowing `Taxi Service` to list available drivers.

### Data Model Direction

Public Groups can reuse existing group records with additional server-owned metadata. This keeps membership, online status, category, color, and persistence centralized in group JSON while avoiding a separate public roster system.

Suggested fields:

```c
bool   isPublicGroup;
bool   publicVisible;
string publicName;
string publicIcon;
string publicColor;
bool   publicShowGroupName;
bool   publicShowPlayerNames;
bool   publicShowOnlineCount;
bool   publicShowAsOfflineWhenEmpty;
```

The server should sync a filtered public roster to clients instead of sending full group internals. The public roster payload should include only the public display fields, online count, and optionally online player names, depending on admin toggles.

### UX Rules

- Public Groups are not normal player-created clans.
- Reserved labels such as Admin, VIP, Moderator, Staff, Owner, Support, Medic, Taxi, and Trader should be protected from normal group creation if the server wants them for public services.
- Admins can still assign categories like Admin or VIP internally; public visibility is a separate presentation flag.
- The Public Groups page should remain compact and readable during gameplay, with no edit controls for players.
- If no public groups are active, show a quiet empty state such as `No public groups are currently active.`
- If names are hidden, do not leak them in tooltips, row metadata, search, or chat-style hover text.


These concepts extend AdvancedGroups around admin workflow, map presentation, manual visibility, and clean state sync. They should stay centered on groups, markers, public service teams, and server-owner configuration.

### Manual Public Group Visibility

Public Groups should use simple manual visibility. Admins turn a public group on or off once, and that state remains until an admin changes it. This keeps `Taxi Service`, `Medics`, `Event Staff`, and similar service groups predictable without extra timing logic.

Suggested fields:

```c
bool isPublicGroup;
bool publicVisible;
bool publicShowAsOfflineWhenEmpty;
```

UI concept:

```text
Public Visibility
[x] Public Group
[x] Visible To Players
[x] Show Group Name
[ ] Show Player Names
[x] Show Online Count
[ ] Show Offline Row When Empty
```

When `Visible To Players` is off, the group is not listed at all. When it is on but no members are online, the group can either be hidden or shown as an offline row, depending on `Show Offline Row When Empty`. The player-facing Public Groups tab should never expose hidden roster details.

### Map Legend And Player Filters

AdvancedGroups should add a compact map legend for server marker categories, public groups, and zone/radius overlays. A server-defined legend plus client-side display toggles fits the marker-heavy UI well.

Server-configured legend entries:

```c
string label;
string icon;
string color;
bool showOnPlayerMap;
bool showOnAdminMap;
```

Client display preferences:

```c
bool showServerMarkers;
bool showGroupMarkers;
bool showPrivateMarkers;
bool showPublicGroups;
bool showEventMarkers;
bool showZoneRadii;
bool showMarkerLabels;
```

These should live in a lightweight player settings panel or map filter drawer, while the defaults remain server-owned in Admin Config Studio. The player should be allowed to hide clutter, but not reveal anything the server did not sync.

### Public Group Presence States

Public Groups can be more expressive than `Online: N`. Add a small status field controlled by admins or group leaders with permission:

```text
Available
Busy
On Call
Off Duty
Event Active
```

This gives service teams a clean roleplay/admin signal without exposing private group internals. In the Public Groups tab, rows should sort by active status first, then online count, then display name.

### Admin Preview And Reload Workflow

AdvancedGroups should keep server-owner maintenance actions inside Admin Config Studio:

```text
[Reload Server Config] [Reload Groups] [Reload Webhooks] [Validate Config] [Save Backup]
```

`Validate Config` should run non-destructive checks and report missing marker colors, invalid webhook URLs, reserved public group names, duplicate group tags, broken icon paths, and unsafe radius values. This fits the current beta direction better than adding many separate admin pages.

### Event Callback Surface

AdvancedGroups should eventually expose small server-side callback hooks so integrations can react to group, marker, public group, and zone events without reaching into storage classes.

Useful hooks:

```c
void OnAGGroupCreated(string groupId, string ownerId);
void OnAGGroupDisbanded(string groupId);
void OnAGPublicGroupPresenceChanged(string groupId, string status);
void OnAGMarkerPlaced(string groupId, int markerId, int markerType);
void OnAGPlayerEnteredServerMarkerZone(PlayerBase player, int markerId);
void OnAGPlayerExitedServerMarkerZone(PlayerBase player, int markerId);
```

This should build on `AGPublicAPI` and may later add a callback registry. The important concept is clean integration through an AdvancedGroups-owned surface.


## Marker Types

AdvancedGroups currently separates marker ownership into three visible categories:

```text
+----------------+-------------------------+------------------------------+
| Marker Type    | Stored On               | Visible To                   |
+----------------+-------------------------+------------------------------+
| Server marker  | server_config.json      | All players, admin managed   |
| Group marker   | group JSON file         | Members of that group        |
| Private marker | private_markers/SteamID | Owning player only           |
+----------------+-------------------------+------------------------------+
```

Server markers are configured and persisted in `$profile:AdvancedGroups/server_config.json`.

Group markers are saved inside the group's persisted JSON under `$profile:AdvancedGroups/groups/`.

Private markers are now server-owned and saved per player:

```text
$profile:AdvancedGroups/
|-- server_config.json
|-- server_info.json
|-- groups/
|   |-- <groupId>.json
|-- private_markers/
|   |-- <steamId>.json
```

Private markers are synced back to the owning player with a dedicated private-marker sync RPC. The client keeps a local cache in `AGClientSettings.privateMarkers` for rendering, but the server is the source of truth.

### Icon Catalog And Config Colors

Every icon in `gui/icons/` is exposed through `AGMarkerType`, `AG_MarkerIcon()`, and `AG_MarkerTypeName()`. The default RGB tint for each type is seeded in `AGServerConfig.markerTypes` and saved in `server_config.json` as hex RGB. Existing configs are normalized by `AGConfigLoader`, which appends missing marker type color entries without overwriting existing admin-edited colors.

Info panel widgets can reuse the same catalog:

```text
----------------------+---------------------------------------------+
| Need                 | Use                                         |
+----------------------+---------------------------------------------+
| Icon path            | AG_MarkerIcon(AGMarkerType.<TYPE>)          |
| Display name         | AG_MarkerTypeName(AGMarkerType.<TYPE>)      |
| Config tint          | serverCfg.GetMarkerTypeConfig(<TYPE>).color |
| Runtime ARGB         | AG_HexToARGB(config.color)                  |
+----------------------+---------------------------------------------+
```

Footer link icons on the Info page use:

```text
Website -> AGMarkerType.GLOBE   -> globe.paa
Discord -> AGMarkerType.DISCORD -> discord.paa
Donate  -> AGMarkerType.PAYPAL  -> paypal.paa
```

```text
+-----------------------+-------------------+----------------------------------------------+---------+
| Enum                  | UI Name           | Icon                                         | RGB     |
+-----------------------+-------------------+----------------------------------------------+---------+
| GENERIC               | Marker            | circle.paa                                   | FFFFFF  |
| RALLY                 | Rally             | house-flag.paa                               | 00FF66  |
| DANGER                | Danger            | radioactive.paa                              | FF2222  |
| LOOT                  | Loot              | locked-chest.paa                             | FFD700  |
| BASE                  | Base              | home.paa                                     | 4488FF  |
| PING                  | Ping              | ping.paa                                     | 00D6FF  |
| CAMP                  | Camp              | camp.paa                                     | FFAA00  |
| CAR                   | Vehicle           | sedan-car-model.paa                          | B8B8B8  |
| SKULL                 | Kill              | skull.paa                                    | FF3333  |
| FLAG                  | Flag              | house-flag.paa                               | 00E676  |
| HOSPITAL              | Medical           | hospital.paa                                 | 37D7FF  |
| SAFEZONE              | Safe              | shield.paa                                   | 52FF8F  |
| SNIPER                | Sniper            | target.paa                                   | FFFFFF  |
| AMMO                  | Ammo              | magazine.paa                                 | FFE066  |
| WATER                 | Water             | water-tap.paa                                | 2EA8FF  |
| BEAR                  | Bear              | bear-side-view-silhouette.paa                | C28A52  |
| CANCEL                | Cancel            | cancel.paa                                   | FF4444  |
| CASTLE                | Castle            | castle.paa                                   | C7C7C7  |
| COW                   | Cow               | cow.paa                                      | FFFFFF  |
| DINO                  | Dino              | dino.paa                                     | 7ED957  |
| DRAGON                | Dragon            | dragon.paa                                   | FF5A36  |
| EVIL                  | Evil              | evil.paa                                     | B000FF  |
| GAS_MASK              | Gas Mask          | gas-mask.paa                                 | A8FF60  |
| HEALTH                | Health            | health-capsule.paa                           | 37E06F  |
| HELICOPTER            | Heli              | helicopter.paa                               | 9AD7FF  |
| GARAGE                | Garage            | home-garage.paa                              | 80B3FF  |
| MONEY                 | Money             | money.paa                                    | 58D26A  |
| PARKING               | Parking           | parking.paa                                  | 4AA3FF  |
| PATH                  | Path              | path-distance.paa                            | F2F2F2  |
| PIRATE                | Pirate            | pirate-captain.paa                           | D8D8D8  |
| PLAYER                | Player            | player.paa                                   | 00FFAA  |
| POLICE                | Police            | police-officer-head.paa                      | 4DA3FF  |
| PROMOTION             | Promo             | marketing.paa                                | FF66CC  |
| REPORT                | Report            | report.paa                                   | FFCC33  |
| ARROW                 | Arrow             | right-arrow.paa                              | FFFFFF  |
| ROBBER                | Robber            | robber.paa                                   | DDDDDD  |
| ROBOT                 | Robot             | robot.paa                                    | 7DEBFF  |
| SAILBOAT              | Boat              | sailboat.paa                                 | 66D9FF  |
| SETTINGS              | Settings          | settings.paa                                 | CFCFCF  |
| SIGNAL                | Signal            | signal-tower.paa                             | FFFFFF  |
| SKULL_BONES           | Bones             | skull-crossed-bones.paa                      | FF3333  |
| SKULL_CRACK           | Crack             | skull-crack.paa                              | FF3333  |
| SKULL_CROSS           | Cross             | skull-crossed-bones.paa                      | FF3333  |
| SPACESHIP             | Ship              | spaceship.paa                                | 9B7DFF  |
| STAR                  | Star              | star.paa                                     | FFE65C  |
| STRONGBOX             | Strongbox         | strongbox.paa                                | FFD47A  |
| TICKET                | Ticket            | ticket.paa                                   | FF8AD8  |
| TRADER                | Trader            | trader.paa                                   | 00FF66  |
| TRIANGLE              | Triangle          | triangle.paa                                 | FFEE00  |
| TRIANGLE_TARGET       | Target            | triangle-target.paa                          | FFEE00  |
| MILITARY              | Military          | military-fort.paa                            | FF4444  |
| WOLF                  | Wolf              | wolf.paa                                     | D0D0D0  |
| ARMOR                 | Armor             | armor.paa                                    | B0C7FF  |
| DISCORD               | Discord           | discord.paa                                  | 5865F2  |
| ENDLESS               | Endless           | endless.paa                                  | 9B7DFF  |
| FIREFOX               | Firefox           | firefox.paa                                  | FF7139  |
| GLOBE                 | Globe             | globe.paa                                    | 3CA8FF  |
| GUN                   | Gun               | gun.paa                                      | DADADA  |
| HELMET                | Helmet            | helmet.paa                                   | B8C6A0  |
| HOUSE_FLAG            | House Flag        | house-flag.paa                               | 00E676  |
| LION                  | Lion              | lion.paa                                     | FFB84D  |
| MAGAZINE              | Magazine          | magazine.paa                                 | FFE066  |
| MARKETING             | Marketing         | marketing.paa                                | FF66CC  |
| MILITARY_BASE         | Military Base     | military-base.paa                            | FF4444  |
| PAYPAL                | PayPal            | paypal.paa                                   | 0070BA  |
| PORT                  | Port              | port.paa                                     | 4DD8FF  |
| RUNWAY                | Runway            | runway.paa                                   | F0F0F0  |
| SELLER_STORE          | Seller Store      | seller-store.paa                             | 00FF66  |
| TIGER                 | Tiger             | tiger.paa                                    | FF8C1A  |
| WATER_TAP             | Water Tap         | water-tap.paa                                | 2EA8FF  |
+-----------------------+-------------------+----------------------------------------------+---------+
```

### Event Marker Category

Add a server marker category named `Events` with default color `FFD700` (gold). This category is for short-lived world events that players may want to find quickly without mixing them into normal admin server markers.

Default vanilla event presets:

```text
+--------------------+----------------------+----------------------------+--------+
| Event Preset       | Suggested Icon       | Default Label              | Color  |
+--------------------+----------------------+----------------------------+--------+
| Heli crash event   | helicopter.paa       | Heli Crash                 | FFD700 |
| Static train       | train/port-style icon | Static Train               | FFD700 |
| Airplane crash     | runway.paa           | Airplane Crash             | FFD700 |
| Police stop event  | police-officer-head.paa | Police Stop             | FFD700 |
+--------------------+----------------------+----------------------------+--------+
```

If a dedicated train icon is not available, use `port.paa`, `runway.paa`, or another transport-style icon until a proper train icon is added. Event markers should support the same 2D/3D display state, label color, radius, and optional strike-fill behavior as server markers.

### Event Object Trigger Concept

Custom events should be triggerable by invisible or empty AdvancedGroups event objects. The mod should define placeholder classes from `AG_Event_1` through `AG_Event_10`. When one of these objects spawns on the map, the server creates an Events-category marker at that object's position.

The placeholder objects should be intentionally plain:

```text
AG_Event_1
AG_Event_2
AG_Event_3
AG_Event_4
AG_Event_5
AG_Event_6
AG_Event_7
AG_Event_8
AG_Event_9
AG_Event_10
```

These objects are not meant to be gameplay loot. They are map/event anchors for server owners and event systems. Their config should make them non-intrusive: no inventory purpose, no player-facing value, and no required visible model beyond whatever DayZ needs for a valid spawned object.

### Event Marker Config

Event marker behavior should live in a separate config file instead of crowding `server_config.json`.

Implemented beta path:

```text
$profile:AdvancedGroups/event_markers.json
```

Current beta config shape:

```c
bool enabled;
float syncIntervalSeconds;
int maxActiveMarkers;
ref array<ref AGEventMarkerEntry> entries;
```

Current beta entry fields:

```c
string className;
string label;
int markerType;
string color;
int displayMode;
float circleRadius;
string circleColor;
bool drawStrikeLines;
string strikeColor;
bool removeWhenObjectDeleted;
float positionMergeDistance;
```

The current implementation creates runtime-only generated server markers from configured classnames. These markers use reserved negative IDs, are synced through the server marker payload, and are kept out of `server_config.json`. The `AG_Event_1` through `AG_Event_10` helper objects remain a roadmap convenience layer on top of the classname config.

Default custom trigger presets:

```text
+--------------+------------------+----------------+----------------+--------+
| Preset ID    | Trigger Class    | Label          | Icon           | Color  |
+--------------+------------------+----------------+----------------+--------+
| custom_1     | AG_Event_1       | Server Event 1 | star.paa       | FFD700 |
| custom_2     | AG_Event_2       | Server Event 2 | star.paa       | FFD700 |
| ...          | ...              | ...            | ...            | ...    |
| custom_10    | AG_Event_10      | Server Event 10| star.paa       | FFD700 |
+--------------+------------------+----------------+----------------+--------+
```

Runtime rules:

- Object spawn creates or refreshes one marker keyed by object identity and trigger class.
- Object deletion removes the marker when `removeMarkerWhenObjectDeleted` is enabled.
- Duplicate objects of the same trigger class are allowed; each spawned object can create its own marker.
- Admins can disable a preset without deleting the object class from the mod.
- Event markers should sync as server-owned markers and should never become group/private markers.
- The Admin Command Center should show event markers in a separate `Events` filter or section so admins can inspect, focus, or delete them without searching the general server marker list.
- Event marker creation and removal should be eligible for webhook logging through the existing webhook routing model.
- Event marker labels should be compact by default because world events can spawn close together.

## Marker Visibility

Markers support 2D and 3D display states:

```text
+-------+-------------+
| State | Meaning     |
+-------+-------------+
| 0     | Hidden      |
| 1     | 2D only     |
| 2     | 2D + 3D     |
| 3     | 3D only     |
+-------+-------------+
```

The marker list supports category-level 2D/3D toggles so all server, group, private, or event markers in that section can be changed at once. Individual marker rows also expose 2D, 3D, and delete controls where permissions allow it.

The `K` shortcut should remain the quick 3D marker filter cycle and include Events as its own stop:

```text
All 3D markers
Disabled
Server only
Group only
Private only
Events only
```

Events are deliberately separate from normal server-only markers in the shortcut cycle. A player should be able to hide ordinary admin map markers while still checking active world events, or isolate event markers during high-traffic moments.

The 2D map uses DayZ's native `MapWidget.AddUserMark()` path for server, group, private, and event markers. This keeps icons/text visible and avoids custom overlay drift. Player/self overlays can still use scripted widgets where useful.

### Player Name Tag Visibility

Player 3D name tags need a server-side visibility mode so server owners can control how much identification is exposed in normal gameplay.

```text
+-------+---------------+--------------------------------------------------+
| Mode  | UI Label      | Behavior                                         |
+-------+---------------+--------------------------------------------------+
| 0     | Everywhere    | Show configured player name/rank tag everywhere  |
| 1     | Safezone Only | Show name/rank tag only inside safezone/zone area |
| 2     | Disabled      | Hide player name/rank tag entirely               |
+-------+---------------+--------------------------------------------------+
```

This should live in Admin Config Studio under `UI Defaults`, because it affects server-wide presentation and player identification. The 3D marker can still retain icon and distance settings separately, allowing a server to show only an icon/distance while hiding the name.

## Server Marker Zones

Server markers are the preferred home for admin-authored map features. A server owner should be able to create one object and choose whether it behaves as:

```text
+--------------------+---------------------------------------------------------+
| Server marker part | Purpose                                                 |
+--------------------+---------------------------------------------------------+
| icon + label        | Normal 2D/3D point-of-interest marker                   |
| color               | Text/icon color for normal point markers                |
| circleRadius        | Optional map radius, used for zones/no-build boundaries |
| circleColor         | Optional radius outline color; falls back to marker color |
| drawStrikeLines     | Optional diagonal strike fill inside the circle         |
| strikeColor         | Optional strike-line color; falls back to circle color  |
| displayMode         | All, 3D only, 2D only, or hidden                        |
+--------------------+---------------------------------------------------------+
```

This replaces the old “No Build” mental model with a richer server-marker package:

```text
Old:
  No Build page -> name + radius + x/z -> list-only admin zone

New:
  Server Marker -> icon + label + text RGB + circle RGB + line RGB + radius + strike fill + 2D/3D state
```

The map rendering uses a lightweight canvas overlay over the shared `MapWidget` for circle and strike-line drawing. Zone shapes are drawn as geometry, not marker icons. Normal marker icons and labels are only used by standard server/group/private/event marker rendering.

Zone definitions should store gameplay data such as shape type, center, radius, polygon vertices, priority, color, draw flags, and no-build radius. Map UI should draw circles and polygons on a dedicated overlay layer, while gameplay checks use zone trigger-style logic such as circular radius checks and point-in-polygon checks for polygon zones.

AdvancedGroups should keep this split:

```text
Server marker data
        |
        +-- normal marker icon/label for point markers
        |
        +-- shape overlay data for radius/polygon zones
        |
        +-- gameplay rule checks for no-build / future zone behavior
```

Circle radius and strike-line overlays should not require separate marker icons. They should be separate render layers and config fields attached to a server marker or future zone object. In the current beta editor, `color` controls the zone label/icon text color, `circleColor` controls the radius outline, and `strikeColor` controls diagonal strike fill.

### Admin Suite Direction

The production admin suite is derived from AdvancedGroups workflows: live group operations belong in one workspace and persistent configuration belongs in another. This produces two focused admin workspaces instead of many partial tabs:

```text
+-----------------------+------------------------------------------------------+
| Admin Page            | Primary Job                                          |
+-----------------------+------------------------------------------------------+
| Admin Command Center  | Groups, members, markers, zones, and tactical map    |
| Admin Config Studio   | Server config, UI defaults, permissions, maintenance |
+-----------------------+------------------------------------------------------+
```

The earlier partial admin/config/settings tabs should stay hidden until rebuilt into these workspaces. The admin suite should use the full screen width, large map/preview regions, dense but readable tables, and clear action zones. Admin-only settings are not player personalization; they are server-owner defaults and rules.

#### Admin Command Center

The Command Center combines group management and map/marker oversight into one page. A selected group drives the middle inspector and right tactical map. Marker tools live here because admins need immediate map context when inspecting group markers, placing server markers, adding temporary server markers, or editing zones.

```text
+================================================================================================+
| ADMIN COMMAND CENTER                                               [Reload] [Sync] [Close]      |
+========================+=========================================+=============================+
| GROUPS                 | SELECTED GROUP                          | TACTICAL MAP                 |
| Search [____________]  | Void [void]                  Tier: Admin | +-------------------------+ |
| [ ] Tag                | Members 4/20   Markers 8/25  Active 2h  | |                         | |
| [ ] Group name         | Created 2026-06-20  Last 2026-06-21     | |  group members          | |
| [ ] Member name        |                                         | |  group markers          | |
| [ ] SteamID            | Name [Void____________] Tag [void____]  | |  server markers         | |
|                        | [Rename] [Set Category] [Join Support]  | |  temp markers           | |
| +--------------------+ |                                         | |  zones / radii          | |
| | Void        4  8   | | MEMBERS                                 | |                         | |
| | Traders     2  3   | | +-------------------------------------+ | |                         | |
| | Admins      1  5   | | | Name        SteamID       Rank  HP  | | |                         | |
| | ...                | | | Lemmy       7656...      Lead  92  | | |                         | |
| +--------------------+ | | Maverick    7656...      Off   76  | | +-------------------------+ |
|                        | +-------------------------------------+ | Filters: [Group] [Server]  |
| [Refresh Groups]       | [Copy ID] [Promote] [Demote] [Leader]   |          [Temp] [Zones]    |
|                        | [Kick] [Focus Member]                   | Tools: [Add Server Marker] |
|                        |                                         |        [Add Temp Marker]   |
|                        | GROUP MARKERS                           |        [Add Zone]          |
|                        | +-------------------------------------+ |        [Edit Selected]     |
|                        | | Icon Label       2D 3D Dist Creator | |        [Delete Selected]   |
|                        | | Base North       Y  Y  1.2k Lemmy   | | Cursor X/Z: 5847 / 9943   |
|                        | +-------------------------------------+ | Selected: Base North       |
|                        | [Edit] [Delete] [Move] [Focus Marker]  |                             |
|                        |                                         |                             |
|                        | DANGER ZONE                             |                             |
|                        | [Delete Group] requires confirmation    |                             |
+========================+=========================================+=============================+
```

Required Command Center capabilities:

- Overview of all groups on the server.
- Search by group tag, group name, member name, and SteamID.
- Inspect each group without leaving the page.
- See all group members with name, SteamID, rank, online state, health, and distance.
- Copy SteamID from the selected member row.
- Promote, demote, kick, or set a member as leader.
- Rename group name and tag.
- Assign group category/level.
- Join a group for support.
- Delete a group with a confirmation step.
- See all group markers and focus/edit/delete them.
- See group members, group markers, server markers, temporary server markers, and zones on the tactical map.
- Add static server markers in-game.
- Add temporary server markers in-game with expiry.
- Add/edit radius zones with separate text, circle, and strike-line colors.

#### Admin Config Studio

The Config Studio combines server config, UI defaults, permissions, and maintenance into one admin page. It should not be a giant scrolling form. Use a narrow section selector, a focused active editor, and a live preview/summary area.

```text
+================================================================================================+
| ADMIN CONFIG STUDIO                                                [Reload] [Save] [Close]      |
+========================+=========================================+=============================+
| SECTIONS               | ACTIVE CONFIG PANEL                      | PREVIEW / SUMMARY           |
|                        |                                         |                             |
| > General              | GENERAL LIMITS                           | Unsaved changes: 3          |
|   GPS / Minimap        | Max group size        [20____]           | Last reload: 19:01          |
|   Chat                 | Max subgroups         [5_____]           | Last save:   19:03          |
|   UI Defaults          | Max markers/group     [10____]           |                             |
|   Info Buttons         | Invite timeout sec    [120___]           | Rule summary:               |
|   Permissions          | Inactive cleanup days [30____]           | - Groups max 20             |
|   Maintenance          | Cleanup interval sec  [300___]           | - Cleanup after 30 days     |
|                        |                                         | - Global chat enabled       |
|                        | [Apply Section] [Reset Section]          | - GPS requires item: no     |
|                        |                                         |                             |
|------------------------+-----------------------------------------+-----------------------------|
| Section Examples       | WHEN SECTION = UI DEFAULTS               | UI PREVIEW                  |
|                        | Player marker color   [RGB picker]       | +-----------------------+   |
| General                | Compass color         [RGB picker]       | | Compass  SW 221       |   |
| GPS / Minimap          | Playerlist color      [RGB picker]       | | [Leader] Name  80m    |   |
| Chat                   | Player name tag mode  [Everywhere v]     | | Ping icon preview     |   |
| UI Defaults            | 3D marker icon        [on/off]           | +-----------------------+   |
| Info Buttons           | 3D marker name        [on/off]           |                             |
| Permissions            | 3D marker distance    [on/off]           |                             |
| Maintenance            |                                         |                             |
+========================+=========================================+=============================+
```

Config Studio sections:

```text
+----------------+-------------------------------------------------------------+
| Section        | In-Game Editable                                            |
+----------------+-------------------------------------------------------------+
| General        | group limits, marker limits, invite timeout, cleanup days   |
| GPS / Minimap  | GPS item rule, vehicle-only rule, vehicle bypass, defaults  |
| Chat           | global chat, moderation toggle, mute system, chat identities|
| UI Defaults    | marker colors, compass/playerlist colors, 3D marker defaults|
| Info Buttons   | 6 buttons, subtext, hyperlink, icon choice, enabled state    |
| Permissions    | assign admin roles, grant/revoke granular permissions       |
| Maintenance    | reload, save, backup, cleanup inactive groups, diagnostics  |
+----------------+-------------------------------------------------------------+
```

Config-file-only items should not clutter the in-game UI:

```text
+-----------------------------+-----------------------------------------------+
| Config-File Only            | Reason                                        |
+-----------------------------+-----------------------------------------------+
| bootstrap admin SteamIDs     | must exist outside in-game permissions        |
| webhook secrets/tokens       | sensitive                                     |
| storage paths                | structural                                    |
| config version/migrations    | internal                                      |
| marker type registry         | structural icon/catalog data                  |
| hard performance limits      | prevents accidental server abuse              |
| large bad-word imports       | easier and safer in JSON                      |
| map/world constants          | rarely changed and risky                      |
| debug/log verbosity          | noisy/dangerous live                          |
| destructive migrations/wipes | require deliberate file-level admin action    |
+-----------------------------+-----------------------------------------------+
```

## Marker Operations

Markers are created and edited from the shared map page.

```text
Double-click empty map area
    -> open create popup at clicked world position

Double-click existing marker
    -> open edit popup for that marker

Drag existing marker and release
    -> move marker to the released world position
```

The edit popup updates the full marker state: label, RGB color, icon type, 2D/3D visibility state, and radius. Group and private marker updates go through RPCs and are saved by the server before the client refreshes its local cache.

The map page clamps the visible viewport to the DayZ world bounds. If the map is zoomed out far enough that off-map terrain would be visible, the page nudges the zoom back in to keep the edge strip hidden.

## Marker Popup

The marker popup is the create/edit surface for group and private markers, and the admin surface for server marker creation.

Features:

- RGB sliders with live values.
- Icon grid with `.paa` icon previews.
- Live icon/name/color preview.
- 3D marker checkbox.
- Private marker selector when the player is in a group.
- Admin-only tools when admin mode is active.
- Double-click map interaction for create/edit to avoid accidental popup opens while panning.

```text
+------------------------------------------------------------+
| Marker                                                [X]  |
| Name [____________________]   [ icon + live preview ]      |
|                                                            |
| Color                         Icon                         |
| R [===========----] 255       [*][*][*][*][*]              |
| G [======---------] 128       [*][*][*][*][*]              |
| B [===============] 255       [*][*][*][*][*]              |
|                                                            |
| [x] 3D Marker        [ ] Private Marker                    |
|                                                            |
| Admin Tools: Radius / Tags / TP                            |
|                                                            |
|                         [CREATE] [DELETE] [CANCEL]         |
+------------------------------------------------------------+
```

## Death Markers

When a player dies, AdvancedGroups creates a skull marker at the death position from the server-side death event:

- If the player is in a group, the marker is created as a shared group marker.
- If the player is not in a group, the marker is created as a server-owned private marker under that player's SteamID.

This keeps death marker behavior consistent with marker ownership rules while making solo death markers survive reconnects and server restarts. The server writes the marker directly instead of depending on a client RPC from the killed player.

## Server Config Versioning

Server config uses an explicit version check. The current migration target is version 9.

```text
Load server_config.json
        |
        v
Is missing / invalid?
        | yes
        v
Create fresh config and save

        no
        |
        v
configVersion < current version?
or required legacy sections missing?
        | yes
        v
Backup current config
Create fresh current config
Migrate preserved values
Save current config
```

The current server config version is defined in `AGConfigLoader.SERVER_CONFIG_VERSION` and is currently 9. Every save stamps the config with the current version; related persisted stores also carry their own schema version.

Config normalization also runs during load/save. It removes duplicate `serverMarkers` entries that share the same `markerId`, which protects existing servers from older migrations or manual edits that duplicated the default markers. Migration clears constructor-seeded default server markers before copying preserved markers from the old config, so defaults are not appended on every boot.

Migration preserves server-owned custom data where possible:

- group/admin limits
- admin IDs
- webhooks
- chat identities
- bad words and mute settings
- GPS settings
- server markers
- no-build zones
- categories
- marker type colors

Backups are written next to the server config:

```text
$profile:AdvancedGroups/server_config_backup_<timestamp>.json
```

## Runtime Sync Model

```text
Client action
    |
    v
Client RPC
    |
    v
Server validates + writes profile JSON
    |
    v
Server sync RPC
    |
    v
Client state cache + UI refresh
```

Examples:

- group marker creation -> `C_PLACE_MARKER` -> group JSON -> `S_SYNC_MARKERS`
- private marker creation -> `C_PLACE_PRIVATE_MARKER` -> `private_markers/<steamId>.json` -> `S_SYNC_PRIVATE_MARKERS`
- server marker creation -> admin RPC -> `server_config.json` -> `S_SYNC_SERVER_MARKERS`
- event marker creation -> object spawn or admin event preset -> `event_markers.json` runtime entry -> `S_SYNC_SERVER_MARKERS`

## Public API Notes For Other Mods

AdvancedGroups can be used as a dependency by server-side scripts that need group or marker state. Treat `AGPublicAPI` as the preferred integration surface and prefer server-side writes so persistence and sync stay centralized.

For current implementation details and integration examples, see `MARKER_GUIDE.md`. The examples below are a quick API sketch only and do not include roadmap marker concepts.

Read examples:

```c
AGGroupData group = AGPublicAPI.GetGroupForPlayer(steamId);
AGGroupData byId = AGPublicAPI.GetGroupById(groupId);

if (group) {
	Print("Group: " + group.groupName + " tag=" + group.clanTag);
}
```

Marker write examples:

```c
// Group marker owned by the caller's group.
AGPublicAPI.PlaceGroupMarker(player, "Rally Point", AGMarkerType.RALLY, pos, 0, "00FF66", 2);

// Private marker owned by the caller.
AGPublicAPI.PlacePrivateMarker(player, "Stash", AGMarkerType.LOOT, pos, 0, "FFD700", 2);

// Admin server marker visible to everyone.
AGPublicAPI.PlaceAdminServerMarker(admin, "Event Zone", AGMarkerType.DANGER, pos, "FF3333", AG_SVRMARKER_SHOW_ALL, 250.0, "FF3333", true);

// Trusted server-side mod marker visible to everyone, without a live admin player.
AGPublicAPI.PlaceEventMarker("MyServerMod", "My Server Mod", "Scripted Event", AGMarkerType.STAR, pos, "FFD700", AG_SVRMARKER_SHOW_ALL, 300.0, "FFD700", true, "FFAA00", true);
```

Category and color examples:

```c
AGPublicAPI.SetGroupCategory(admin, groupId, "VIP");
AGPublicAPI.SetGroupColor(groupLeader, "FF00FF");
```

Client-side mods should request changes through their own server RPCs before calling `AGPublicAPI`; they should not write AdvancedGroups JSON directly. Future polish can add callback/event registration on top of this facade so third-party mods do not need to know internal storage details.

## Admin Config Notes

Bad-word filtering is stored directly in `server_config.json` as the `badWords` array. The in-game settings page edits that server config list; there is no separate bad-words  file.

Discord-style webhooks only need the webhook URL. `webhookToken` remains as a deprecated compatibility field in older JSON/RPC payloads, but new beta saves and migrations keep it empty.

Group tiers are configured under `categories`. Admins can assign built-in categories such as `Admin`, `VIP`, `Tier1`, and `Tier2` from the group management details panel.

## Development Notes

- Keep server-owned data in `$profile:AdvancedGroups`.
- Keep client settings limited to display preferences and local cache data.
- Prefer native DayZ widgets for core rendering when possible, especially `MapWidget.AddUserMark()` and `PlayerPreviewWidget`.
- Avoid local-only marker writes for gameplay data. Use RPCs so reconnects, deaths, and multiple sessions stay consistent.
