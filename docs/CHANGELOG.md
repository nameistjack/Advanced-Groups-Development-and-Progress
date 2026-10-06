---
layout: page
title: CHANGELOG
---

# Changelog

All notable changes to AdvancedGroups will be tracked here.

Beta versioning starts at `Beta v0.1.0`. Entries keep exact timestamps until a stable release is confirmed. Major fixes/features bump the minor version (`v0.X.0`); small fixes bump the patch version (`v0.X.Y`).

## Unreleased - 2026-09-24

### Added

- Added stable per-member tactical-ping identification: each persisted group roster position maps to a distinct ping icon that follows the ping through synchronized map, 3D, marker-list, and minimap presentation.
- Added synchronized marker-radius circles and configured strike lines to the minimap for group, private, server, and event markers.
- Added an expanding minimap marker pool up to a guarded ceiling so busy servers do not silently lose markers at the former fixed limit.
- Added Zone Management as a first-class admin tab beside Map, Command, and Config, using the shared map for circle/polygon creation, selection, node dragging, save, and deletion instead of opening it as a Command Center sub-panel.
- Added synchronized zone geometry to the marker-map overlay and minimap, backed by the same authoritative `AGZoneDefinition` collection and each zone's enabled/map-visible/color settings.
- Added a collapsible Zones category to the marker rail with a persistent 2D visibility toggle; Show All and Hide All now include zone geometry.
- Added an optional, independently configured spawn-selection module for newly created characters, with named deployment regions, random deployment, persistent per-player cooldowns, server-authoritative validation, and an original responsive map interface.
- Added cached spawn rows and map marks with one-second local cooldown presentation, avoiding full widget reconstruction during normal menu updates.
- Added dedicated `spawn_config.json` and `spawn_cooldowns.json` storage so spawn definitions and runtime player state remain isolated from group configuration.
- Added visible polygon vertex handles to the zone editor, including click selection, highlighted active nodes, and direct map dragging with live geometry updates.
- Added a complete 27-layout geometry audit that identifies responsive containers, scaled work surfaces, anchored HUD panels, dynamic rows, and intentionally fixed leaf controls.
- Added a UI scaling contract and multi-resolution test matrix covering proportional containers, exact leaf controls, runtime geometry boundaries, and prohibited mixed-coordinate mutations.
- Added an Admin Command Center Flagpoles toggle backed by a server-side vanilla territory registry, including flag state and protection-radius metadata without external dependencies.
- Added a unified circle/polygon zone definition for Safezone, No-Build, PvE, PvP and Visual zones, with priority, visibility, color and friendly-fire policy metadata.
- Added a Command Center Zones overlay toggle with native-map circle and polygon rendering.
- Added permission-checked server CRUD endpoints for creating, updating and deleting zone definitions, including geometry and range validation.
- Added an isolated Admin Center zone manager with collapsible type cards, create/edit/delete forms, circle drawing, polygon vertex drawing, enabled/map visibility controls, priority, color and friendly-fire settings.
- Added authoritative circle safezones whose fixed semantic blocks every damage source for players, vehicles, creatures, and other damageable entities inside the zone.
- Added client and server no-build enforcement for placement holograms, deployment actions, and base-building construction actions.
- Added persistent per-player 2D/3D visibility overrides for individual server and event markers; these controls no longer require admin mode or mutate the server marker definition.
- Added collapsible marker-category cards for Server, Events, Group, Players, and Private markers, with persistent `+`/`-` expansion state and category-level 2D/3D controls.
- Added independent 2D and 3D visibility controls beside individual marker entries and in the marker editor.
- Added card-style group color controls with larger RGB interaction tracks and visible channel values.

### Changed

- Made `FFFF00` the standard color for newly created groups and moved custom per-group color assignment into the admin-selected group inspector with a live hex preview; ordinary group leaders can no longer assign custom colors.
- Restored visible saved-color previews for Group Tier rows by rendering their swatches with a color-capable surface and applying synchronized colors to both swatches and labels.
- Suspended AdvancedGroups gameplay shortcuts whenever chat, another scripted menu, or a standalone UI cursor owns input; chat submission now also consumes an overlapping camera-view action for that frame.
- Matched chat message-body typography to the channel, role, group, and sender widgets and moved the history scrollbar to the left with a dedicated content gutter.
- Moved zone administration into the existing marker popup: zone rows now open a zone mode that reuses the shared name/RGB preview and replaces unused marker controls with type, shape, radius, priority, friendly-fire, enabled/map visibility, map-center, Save, and confirmed Delete actions.
- Removed the Command Center width previously reserved for its hidden legacy online column and moved the shared-map boundary to the end of the active inspector, while retaining the wider Zone workspace.
- Clarified the UI geometry contract around proportional workspaces, hybrid columns, exact control rows, compact design-unit dialogs, and consistent screen-space drag bounds.
- Expanded chat presentation and validation so client channel visibility preferences and chat font size are honored, role/group/channel colors remain independent, unsafe badge delimiters are normalized, and unavailable channels return clear feedback.
- Unified minimap overlays with the full map's synchronized marker categories, per-marker display modes, zone visibility, and rendered colors.
- Stabilized minimap visibility while dead or using another menu and defined the render order as terrain, geometry overlays, marker blips, then player direction.
- Removed the visible Command Center zone-launch buttons; Command remains focused on live group operations while the dedicated Zones tab owns zone administration.
- Unified Config Studio and Command Center around the same compact header, card palette, spacing rhythm, action hierarchy, and `Ready / Loading / Saved / Changed / Confirm / Failed` status vocabulary while retaining distinct configuration and live-operations identities.
- Kept primary actions green, secondary synchronization/navigation actions neutral, and destructive actions red across both admin surfaces; shortened competing help copy and retained webhook fields as disclosure content shown only when integrations are enabled.
- Promoted frequent Save, Sync, Reload, Zones, group inspection, and map operations to persistent surfaces while keeping detailed configuration and zone collections organized inside existing section and collapsible-card structures.
- Unified Config Studio color controls, swatches, live previews, saved values, and server normalization around the same uppercase `RRGGBB` codec without changing or importing layout files.
- Changed Config Studio Save to remain dirty until the authoritative server response confirms the persisted values; Reload and Reset now explicitly replace the draft, while ordinary Sync protects unsaved edits.
- Centralized shared-map boundary placement in the main responsive layout pass and removed duplicate page-switch offsets; the invitation dialog now uses layout anchoring instead of framebuffer-based repositioning every frame.
- Anchored the shared menu close control to the layout's right edge and removed its raw-framebuffer runtime positioning.
- Began the responsive-layout migration: the marker editor now uses a proportional viewport-sized root with exact leaf controls, rendered screen dimensions for centering/drag bounds, and no runtime resizing of its scaled parent.
- Reworked zone selector and action buttons to use independent, consistently scaled labels over compact hit surfaces instead of oversized native DayZ button captions.
- Added an always-visible ZONES action to the Command Center header so the card-based Safezone, No-Build, PvE, PvP and Visual zone manager is directly discoverable without scrolling through selected-group controls.
- Coalesced group-position synchronization to one snapshot per changed group every 500 ms instead of rebroadcasting the complete group for every member packet.
- Limited minimap-blip and expensive map-overlay refresh work to 10 Hz while retaining per-frame compass and marker projection so labels remain anchored during camera turns and map dragging.
- Cached GPS inventory detection for one second, throttled expired-ping scans to once per second, and reused one server player-list snapshot per position broadcast.
- Debounced chat scroll correction so message bursts retain only one pending layout callback instead of accumulating two callbacks per line.
- Restored no-build zones as persistent policy data instead of migrating and clearing them into display-only server markers.
- Added schema recovery for legacy no-build circles that had already been converted into `LEGACY_NOBUILD` map markers.
- Advanced the client/server RPC protocol to 5 and the persisted server configuration schema to 9 for unified zone synchronization.
- Reworked marker and Group RGB sliders to use a clean custom rail, colored fill, visible thumb, and numeric channel value instead of overlapping a native slider skin with the custom fill.
- Changed disabled marker visibility channels to display `-` instead of retaining the `2D`/`3D` label, including aggregate category controls when no marker in that category uses the channel.
- Widened the marker workspace to a 520-unit editor with a 484-unit list and aligned the shared map boundary immediately after it, preventing ultrawide clipping of marker names and controls.
- Changed the marker rail to target a measured physical width derived from the active DayZ UI scale, keeping both 2D/3D columns visible on ultrawide displays where exact layout units are scaled down.
- Simplified the marker toolbar to Show All/Hide All; category visibility now lives on the category cards instead of a second compressed row of duplicate controls.
- Returned marker-map wheel zoom and initial view selection to native `MapWidget` behavior instead of forcing custom scale and center values.
- Reworked Admin Command Center and Config Studio positioning to use authored layout geometry instead of combining layout scaling with conflicting runtime `SetPos`/`SetSize` overrides.
- Converted the Config Studio toolbar to readable Reload, Save, and Sync actions and expanded the Events editing and action controls.
- Improved marker category and row styling with opaque card surfaces and clearer active/inactive visibility colors.

### Fixed

- Fixed Zone popup controls inheriting an unintended alignment origin and drawing over the marker icon, visibility, and admin sections; Zone mode now replaces those sections cleanly.
- Fixed respawn spawn selection being bypassed by custom mission `StartingEquipSetup` overrides that omit `super`; death is now tracked independently and the selector is queued when the replacement character connects.
- Fixed chat message bodies wrapping into a clipped second spacer row after channel, role, group, and sender badges; the body now remains inside the visible fixed-height line.
- Fixed the marker editor expanding to 48% of screen width and 82% of screen height despite containing a compact fixed-size control card, and corrected its initial placement to use the same screen coordinate space as dragging.
- Fixed unused chat visibility and font-size settings, normalized player messages to a bounded single-line payload, and added feedback for muted players, disabled Global chat, and Group/Subgroup use without membership.
- Fixed chat history rendering only the first message by replacing conflicting nested auto-flow height rules with deterministic vertical row geometry, explicit scroll-content sizing, and a bounded reusable row pool.
- Fixed the spawn selector being skipped after respawn when `StartingEquipSetup` ran before the replacement player entity received its identity, and deferred client presentation until the respawn UI releases the active menu slot.
- Fixed minimap projection jitter and one-frame offset errors by positioning the minimap before projecting players, markers, zones, and the direction indicator.
- Fixed Group and Subgroup chat delivery depending on stale persisted `member.online` flags; recipients are now resolved from live connected players, every player-facing channel is validated through the shared chat RPC, and Subgroup chat reports when the sender has no subgroup assignment.
- Unified each chat channel's selector, tag, and message-body colour so Global, Group, Subgroup, and Vicinity messages retain their channel identity across the complete UI path.
- Fixed the shared map assigning `AGMainMenu`—a `UIScriptedMenu`—where DayZ requires a `ScriptedWidgetEventHandler`; page activation now assigns the active `AGPageBase` handler while the main menu retains sole widget ownership.
- Replaced unsupported C-style ternary expressions in the spawn menu and cooldown timestamp helpers with Enforce-compatible conditional statements, fixing Mission-module compilation.
- Added visible header status strips to both admin screens, including authoritative save confirmation, loading feedback, validation failures, protected-draft/server-change warnings, and live-operation progress.
- Added two-step confirmation for Command Center group deletion, member removal, group-marker deletion, and zone deletion; changing the active selection cancels the pending destructive confirmation.
- Fixed delayed Settings sync responses overwriting edits made after a request, and fixed Save presenting an unconfirmed request as completed before the server replied.
- Centralized ownership of the shared Group/Markers/Admin `MapWidget` in `AGMainMenu`; page switches now route map input exclusively to the active page instead of reassigning the widget handler and leaking interactions across pages.
- Unified marker preview, saved RPC color, synchronized marker color, 2D map rendering, minimap rendering and 3D rendering around one canonical uppercase `RRGGBB` codec, preventing preview colors from diverging from rendered markers.
- Fixed circle, polygon, and strike-line overlays lagging behind the map during pan or zoom by redrawing immediately whenever the native map center or scale changes, while retaining the idle redraw throttle.
- Wired the marker popup TP action to an admin-only server-authoritative teleport with terrain-bound and surface-height validation.
- Fixed existing marker colors appearing inconsistent with the editor preview: the selected list icon, label, and map marker now follow the live RGB draft, while Cancel restores the last saved color.
- Fixed event markers shuddering and causing severe client/server lag when producers repeatedly replaced the same marker: event updates now retain stable IDs, unchanged producer heartbeats no longer broadcast or fire webhooks, and position-only synchronization updates live marker objects without rebuilding the map page.
- Fixed the marker popup root using physical dimensions while its children used DayZ-scaled layout units; popup width and height now derive from the measured UI scale so the close control, icon strip, Admin Tools, Confirm, Delete, and Cancel controls remain contained.
- Fixed zone-map double-clicks opening the marker popup by explicitly transferring the shared MapWidget handler to Admin/Zones whenever that page or editor opens.
- Increased the marker popup's runtime height to accommodate DayZ's vertically scaled child coordinates, preventing the Admin Tools and footer controls from being clipped; added Studio-style cyan accent rules to zone text inputs.
- Fixed the marker popup's remaining horizontal overflow by moving Private marker onto a second Visibility row; replaced the native close-button caption with a separately scaled `X` label so it cannot collapse into a clipped white bar.
- Fixed the zone editor layout failing to instantiate controls after the name field, which left Type, Shape, Radius, drawing, and save/delete actions invisible and non-interactive; the complete form now uses parser-safe widget declarations.
- Fixed the zone manager rendering as a translucent second page over the active group inspector; Zones now uses an opaque top-level editor surface while retaining its active owner tree for reliable DayZ event routing, and its form fields, buttons, and checkbox captions use explicit contained styling.
- Fixed marker-editor checkbox captions overflowing their Visibility and Admin Tools cards at scaled UI settings by decoupling captions from DayZ's oversized native checkbox text skin; replaced the clipped close caption with a compact, always-contained `X` control.
- Fixed Player Information and group-member health/blood telemetry reading inconsistent health zones, which could leave displayed values stuck at 100%; legitimate zero blood is no longer mistaken for a failed stat read.
- Standardized Config Studio toolbar, Events action, checkbox and footer typography; widened Reload/Save/Sync and the complete Add/Update/Duplicate/Delete row so labels no longer clip or collapse into fragments.
- Replaced the marker popup's inconspicuous close icon with a clearly sized CLOSE action and explicit typography, while retaining the bottom Cancel action and Escape-key dismissal.
- Limited chat identity colors to their channel, role and clan badges; sender names now use a stable accent and message bodies remain neutral instead of inheriting the admin-role color.
- Kept short chat histories anchored at the top with the scrollbar at zero, scrolling to the bottom only when message content is taller than the chat viewport.
- Rebuilt the marker popup visibility controls as a contained card and assigned explicit checkbox typography so 2D, 3D, private, server and strike-line labels cannot overflow their panels.
- Replaced the overflowing Command Center Flagpoles/Zones controls with a contained Admin Map Tools card inside the selected-group scroll surface.
- Fixed 3D labels and map markers wobbling behind camera rotation or map dragging by restoring per-frame screen projection while leaving expensive overlay drawing throttled.
- Fixed migrated legacy zones reappearing after deletion, duplicate/null zone IDs surviving normalization, and admin RPCs accepting geometry outside the active terrain.
- Fixed newly created zones duplicating on a second Save by acknowledging the server-assigned ID, and protected card changes/close actions from silently discarding dirty zone edits.
- Fixed World-module compilation failing on an unmodifiable native `EntityAI` declaration; safezone damage hooks now target concrete moddable player, item, vehicle, infected and animal classes.
- Fixed chat messages being pushed below the viewport while the scrollbar reported the bottom by removing a stale manual WrapSpacer Y offset and leaving history positioning to the scroll container.
- Fixed 3D-only and hidden group markers being omitted from the Admin Command Center map even though they were present in its marker list.
- Fixed the marker list parent retaining its old width and clipping the 2D/edit columns after its child cards were widened.
- Fixed card-list content height being inferred inconsistently from absolute-positioned children, which could leave the scrollbar thumb out of sync with the final marker rows.
- Removed marker-page custom map clamping that prevented reaching the north and east map boundaries while zoomed out.
- Fixed sent and newly received chat messages appearing outside the visible history or disagreeing with the scrollbar position.
- Fixed the Command Center online-members panel overlaying the selected-group inspector and clipping member and marker actions.
- Fixed Command Center filters overlapping group rows and moved Delete/Join fully inside the visible panel.
- Fixed Config Studio toolbar, Events actions, color fields, and Apply/Reset controls being clipped or displaced by mixed coordinate scaling.
- Removed unnecessary Group page scrollbars when their content fits inside the panel.

## Beta v0.4.0 - 2026-09-19

- Advanced the client/server RPC protocol to 3. Admin config responses redact configured webhook URLs, and unchanged fields preserve their server-side secrets.
- Reworked Admin Config Studio sizing around screen-relative margins, proportional columns, live resolution reflow, a stable action footer, and responsive status/log/form sizing.
- Removed the optional external marker bridge; marker storage, visibility filtering, and synchronization now use only the native AdvancedGroups path.
- Extended responsive sizing to the complete UI surface: main pages, admin views, map lists and dialogs, HUD elements, chat, player list, compass, minimap, and invitations now use shared viewport metrics, anchors, clamped proportions, or scrolling according to component type.
- Added a native reusable UI theme with consistent surfaces, cards, fields, lists, typography, hover states, navigation selection, and semantic action colors.
- Added compact two-column Config navigation, automatic collapse of disabled webhook details, and a draggable marker editor whose normalized position persists in client settings.
- Standardized live RGB previews across every editable color surface. Typed six-digit hex values now update RGB sliders and swatches immediately; slider changes update normalized hex text, and marker previews tint the swatch, icon, and label with the exact saved color.

### Fixed
- Hardened the Addon Builder include manifest so `config.cpp` and `$PBOPREFIX$` are explicitly present in packed releases alongside scripts, layouts, assets, XML, CSV, and default configuration data.
- Removed the global `RscMapControl` override so AdvancedGroups no longer changes unrelated map interfaces or competes with other map/admin mods through config load order.
- Replaced the minimap's hard-coded Chernarus pixels-per-metre marker projection with the active MapWidget projection, keeping member, group, private, server, and event blips accurate across terrains, zoom levels, aspect ratios, and UI scales.
- Fixed uncontrolled default typography in the group-invite dialog and corrected the Markers sidebar's proportional height declaration.
- Darkened editable fields and added an explicit Group invite-field surface so empty inputs remain visible and consistent with the rest of the current interface.
- Constrained the existing Markers sidebar to an opaque native panel instead of allowing the map or parent surface to bleed through it.
- Stabilized the Markers, Group, Command Center, and chat-input layouts by removing duplicate physical-pixel resizing from already scaled widgets, constraining marker typography, and correcting an exact/proportional width mismatch in marker-list headers.
- Replaced oversized native RGB slider fills on the Group and marker editors with compact track-and-fill previews while retaining the full slider hit area and live color swatches.
- Restored a visible Group invite input and strengthened the Command Center work-surface background so controls remain readable over the shared map.
- Fixed map player and marker overlays switching between absolute screen coordinates and map-local coordinates while panning or zooming, which made markers appear to change world position.
- Fixed custom Admin tier colors not being applied to the `[SERVER ADMIN]` chat role, sender, and message text.
- Fixed chat messages being clipped after their channel, role, and group badges by removing incompatible physical-pixel resizing, widening the exact-unit chat row, and wrapping long message bodies.
- Removed undeclared ownership-method assumptions from PVE and friendly-fire damage attribution; AdvancedGroups now tracks grenade, trap, and placed-explosive ownership through its own namespaced hooks.
- Narrowed ownership tracking to `ExplosivesBase` and `TrapBase` instead of intercepting every `ItemBase` inventory-location change.
- Fixed Mission-script compilation failures caused by ambiguous JSON escape literals in webhook payload construction.
- Removed an obsolete No-Build admin broadcast call; the existing general-config broadcast already synchronizes those entries.
- Fixed the marker editor color swatch being forced fully transparent even while its icon and label used the selected RGB value.

### Changed
- Moved project documentation into the dedicated, version-controlled `Documentation/` directory so release packaging can keep non-runtime material separate from the mod payload.
- Established responsive screen-sizing, breakpoint, proportional-space, reusable-component, and interaction-state requirements for the Admin Config Studio redesign.
- Added a broader engineering roadmap covering capability permissions, service boundaries, identifier-based damage attribution, persistence, RPC validation, webhook routing, and release checks.

### Added
- Added Admin Config Studio screenshot audit and acceptance criteria.
- Added an engineering architecture audit derived from AdvancedGroups requirements, observed defects, engine constraints, and direct project testing.
- Added a Semantic Versioning policy that separates release, RPC protocol, public API, and persisted-schema versions.

## Beta v0.3.0 - 2026-09-14

### Fixed
- Removed the unreachable legacy No-Build, standalone HUD, Appearance, and Layout pages, their orphaned layouts, the dead Command Center route, and unused RPC handlers while reserving former RPC offsets for compatibility.
- Hardened primary JSON saves by writing and parsing a temporary file before replacing the live server, server-info, or Event configuration, preventing an invalid serialization from destroying the active config.
- Fixed the Command Center search field being decorative; it now filters the received group list immediately and reports loading, empty, and no-match states.
- Fixed Config Studio controls overlapping on narrower displays: the summary column now collapses before it can cover the editor, compact Chat fields reflow vertically, header actions remain right-aligned, long inputs size to the available panel width, and all section buttons follow the resized navigation rail.
- Fixed the Permissions page placing the webhook toggle over the webhook URL input and normalized spacing across all webhook rows.
- Fixed the compact Events editor treating a newly entered classname as a replacement for the first configured rule; unknown classnames now append while matching classnames update in place, preserving every other event entry.
- Fixed unreadable `event_markers.json` files being overwritten with defaults during startup without recovery; the raw file is now timestamp-backed-up before default recreation, and admin saves also preserve a pre-save backup.
- Fixed global PVE and group-friendly-fire protection using the post-hit callback; damage is now vetoed at DayZ's authoritative calculation hook before it is applied, with player attribution for weapons, grenades, traps, placed explosives, and driven vehicles.
- Fixed unsaved Admin Config Studio edits being silently lost when closing through the X button, Escape, map/group hotkeys, or page-level close actions; closing now presents a keep-editing/discard choice.
- Fixed server-populated Admin Config Studio fields being able to trigger false unsaved-change state while the form was loading.
- Fixed the group-creation button silently doing nothing when the clan tag field was left blank — it required a tag to even attempt creation, which is exactly what made "normal" (untagged) groups impossible to create. Validation is now independent per field and the tag is properly optional.
- Fixed the group color picker: the manual hex-entry box did nothing when typed into (only the RGB sliders were ever read), the RGB slider fill bars were permanently hidden regardless of value, and the server stored the raw unnormalized hex string instead of the same normalized form every other color path uses.
- Fixed `BroadcastServerMarkersToAll` re-sending the full marker list to every online player on every single marker mutation; it now coalesces bursts and sends at most once per second.
- Fixed the shared map's custom edge-clamping (`ClampMapToWorldBounds`, duplicated in the Group, No-Build, and Markers pages) mismeasuring its own viewport and snapping the pan back before reaching the real east/south world edges; removed the custom clamp in favor of trusting the native `MapWidget`.
- Fixed admin-panel inputs (search, group name/tag) rendering as solid black with no visible boundary against the dark theme; restored explicit backing plates instead of relying on the native edit-box skin, which does not render with its own contrast.
- Fixed several illegibly small (9-12px) labels in the Admin Command Center.
- Fixed a settings-page bug where the main content panel was colored solid saturated red instead of the intended panel background.
- Cleaned up dozens of sub-pixel/fractional widget coordinates across the admin, settings, and info layouts left over from prior editing drift.

### Changed
- Webhook delivery now uses a bounded asynchronous queue with duplicate suppression, payload limits, HTTPS endpoint validation, one in-flight request, staged retry backoff, terminal failure handling, and delivery/failure/drop counters.
- Versioned the supported server integration contract as Public API v2; group lookups now return detached snapshots, marker-owner inputs are validated, and the legacy server-marker entry point delegates through the owned Event-marker API.
- Added explicit schema versions to server-info, Event, runtime-marker, private-marker, and group-index persistence models so future upgrades can migrate files deliberately.
- Centralized admin RPC authorization and added per-player operation throttles, unknown-call logging, and a client/server protocol handshake with visible mismatch feedback.
- Increased Command Center list-row and small status-text sizing so group, member, marker, and online-player entries remain readable and selectable after safe-zone scaling.
- Moved native client/server RPCs out of the commonly reused low integer ranges into an AdvancedGroups-specific high identifier block to greatly reduce cross-mod RPC collisions. Client and server must be updated together.
- Standardized the main UI canvas on DayZ safe-zone-aware scaling while retaining proportional map/HUD layers and centered, screen-clamped admin workspaces.
- Reworked the dark theme across the group, admin, settings, no-build (+ style popup), marker popup, appearance, layout, main menu, and marker-icon-button layouts: unified panel/well tones, replaced hacky white input backgrounds with a consistent backing-plate + accent-rule treatment.
- Chat channel colors are now fixed per channel: Global chat is sky blue, Group/Team chat is red, Vicinity chat is white. The separate `[GroupTag]` badge still uses the group's own owner-configurable color.
- Replaced the group member list's single health-icon-and-bar with vanilla-style separate health and blood bars, synced per member through the existing position-update RPCs.
- Player list group header now shows only the clan tag (e.g. `[CHL]`) instead of the full group name plus tag.

### Added
- Added a repeatable release audit that checks version alignment, unreferenced layouts, duplicate script filenames, forbidden generated/profile artifacts, reserved null page slots, and whitespace errors.
- Added a release and upgrade guide covering client/server compatibility, config migrations and backups, clean packaging, rollback, and the supported public integration contract.
- Added an explicit Event rule list with selection plus Add, Update, Duplicate, and confirmed Delete actions; edits remain a client-side draft until the administrator saves the configuration.
- Added a Status tab to the Admin Config Studio: live counts for total groups, online players, online members in groups, active server markers, and active event markers.
- Added an Admin Log tab: the last 200 log lines, viewable in-game with a Refresh button, backed by a new in-memory ring buffer in `AGLogger`.
- Added a Group Tiers admin section for editing the Admin/VIP/Tier1/Tier2 category color, size limits, subgroup limits, marker limits, and invite-cap bypass in-game.

## Beta v0.2.1 - 2026-07-20

### Fixed
- Fixed the shared map's custom edge-clamping (`ClampMapToWorldBounds`) mismeasuring its own viewport and snapping the pan back before reaching the real east/south world edges; removed the custom clamp in favor of the native `MapWidget`.
- Fixed `enablePvPProtection` being a config field and admin checkbox with no actual enforcement anywhere — it now works via a new damage hook.

### Added
- Added `blockGroupFriendlyFire` (default on): blocks damage between members of the same group, independent of the PVE toggle.
- Wired `enablePvPProtection` as a real global PVE switch: when enabled, no player can damage another player; AI and environment damage are unaffected.
- Replaced every remaining raw hex-text color field with a proper RGB slider picker (swatch + R/G/B sliders driving the hex box): Group Tiers colors (Admin/VIP/Tier1/Tier2), Admin/VIP chat tag color, Event marker color, and the three UI-default colors (player marker, compass, player list).

### Removed
- Removed the Zones/No-Build admin page from the menu — it had no sidebar tab and was unreachable in-game; its functionality now lives in the marker popup's admin panel. The underlying files are left in place but no longer load.

## Beta v0.1.0 - 2026-07-04 21:14 CEST

### Added
- Added `$profile:AdvancedGroups/event_runtime_markers.json` as a separate persistent store for trusted API-created lifecycle event markers.
- Added `AGPublicAPI.PlaceEventMarker`, `AGPublicAPI.RemoveEventMarkers`, and `AGPublicAPI.FindEventMarker` so third-party server mods can create and clean up Events-category markers without editing AdvancedGroups internals.

### Changed
- Routed trusted `AGPublicAPI.PlaceServerMarker` compatibility calls through the Events marker store so API-owned lifecycle markers appear under the Events category instead of normal admin server markers.

## Beta v0.1.0 - 2026-07-04 21:01 CEST

### Fixed
- Fixed a server marker helper compile-risk by storing newly created `AGServerMarker` instances in a strong `ref` local.
- Fixed the server-marker RPC reader to create synced marker structs with a strong `ref` local and removed a duplicate position assignment.
- Broadcast admin-created server markers through the explicit server-marker sync path so map/list state updates consistently with API and event markers.
- Preserved existing hidden event-marker entries when editing the first visible Admin Config Studio event entry so saving the Events page does not drop additional classname rules.

## Beta v0.1.0 - 2026-07-04 20:55 CEST

### Added
- Added `AGPublicAPI` as the preferred server-side integration facade for third-party mods, covering group lookups, group/private marker placement, admin server markers, trusted server-owned global markers, group category assignment, and group color updates.
- Updated mod creator documentation to use `AGPublicAPI` and document trusted server-owned marker placement without a live admin player.

## Beta v0.1.0 - 2026-07-04 16:32 CEST

### Changed
- Replaced marker-list row delete affordances with the new pen edit icon so group, private, and admin server markers open the edit popup first; deletion remains inside the popup.

## Beta v0.1.0 - 2026-07-04 15:56 CEST

### Changed
- Split the Admin Config Studio Events entry editor into separate Classname, Label, Type, Color, Display, and Radius edit boxes instead of requiring pipe-delimited text.

## Beta v0.1.0 - 2026-07-04 15:43 CEST

### Added
- Wired Event Markers through the player marker stack with an Events list group, 2D map/minimap/3D visibility handling, `[E]` labels, and a `K` shortcut cycle mode for Events Only.
- Added an Admin Config Studio Events section for enabling classname-triggered markers, tuning sync/max-active limits, and editing classname marker entries without recompiling.

## Beta v0.1.0 - 2026-07-04 14:39 CEST

### Added
- Added disabled-by-default classname-triggered Event Markers through `$profile:AdvancedGroups/event_markers.json`, with runtime-only generated server markers, gold defaults, radius support, duplicate-position protection, and read-only negative marker IDs.
- Added `EVENT_MARKER_CONFIG.md` and updated the marker integration guide to separate server-owner event marker config from third-party marker API usage.

## Beta v0.1.0 - 2026-07-04 14:28 CEST

### Fixed
- Polished the Admin Config Studio left rail and active editor panel by compacting section tabs, checkboxes, labels, and edit-box text, widening the runtime editor column, and reducing cramped panel/button sizing in the Workbench preview path.

## Beta v0.1.0 - 2026-07-04 13:02 CEST

### Fixed
- Polished Admin Command Center text scaling by hiding the clipped header hint, forcing compact exact text sizes on group/member/online/marker lists, aligning selected-group edit box backings at runtime, and shortening long group, member, marker, and SteamID display strings.

## Beta v0.1.0 - 2026-07-04 12:48 CEST

### Added
- Added the mod-creator marker guidance now maintained in `MARKER_GUIDE.md`, covering third-party placement of group, private, and admin server markers through the supported server surface.

## Beta v0.1.0 - 2026-07-04 12:38 CEST

### Changed
- Added the Events marker category concept with gold defaults, vanilla world-event presets, `AG_Event_1` through `AG_Event_10` custom trigger anchors, and a separate `event_markers.json` config direction.
- Extended the Events marker concept to include the `K` 3D marker shortcut cycle, player map filters, admin marker bulk controls, compact labels, and webhook-eligible event marker lifecycle logs.
- Renamed the future concept section in `ui_concepts.md` to `Roadmap`.

## Beta v0.1.0 - 2026-07-04 12:32 CEST

### Changed
- Started versioned beta tracking with `Beta v0.1.0` while keeping exact timestamps for each beta note.
- Simplified the Public Groups roadmap to manual admin-controlled on/off visibility.

## Beta - 2026-07-04 12:27 CEST

### Changed
- Refined the Public Groups roadmap with original plans for manual visibility, map legends, player filters, public presence states, admin validation/reload tools, and future callback hooks.

## Beta - 2026-07-04 12:13 CEST

### Fixed
- Removed normal text labels from Admin Command Center action buttons so the controls render as centered icons, with only the destructive delete confirmation showing temporary confirm text.
- Tightened Admin Command Center member and group-marker list row sizing so list text no longer scales oversized during runtime layout resizing.

## Beta - 2026-07-04 11:55 CEST

### Fixed
- Blocked reserved server category names such as Admin, VIP, Staff, Owner, and Support from normal group creation and admin renames, with client/server feedback for rejected names.
- Recomputed marker RGB hex directly from sliders when confirming marker placement so the placed marker color matches the popup preview.
- Added explicit AdvancedGroups chat channel/message colors and applied configured Admin/VIP role colors to role tags, sender names, and messages.
- Added icons and tighter text sizing to Admin Command Center action buttons so dense admin controls are easier to scan without oversized labels.

## Beta - 2026-07-04 11:43 CEST

### Fixed
- Widened the Admin Config Studio left section rail and reduced/centered section tab text so Workbench preview labels no longer clip.

## Beta - 2026-07-04 11:42 CEST

### Fixed
- Widened the Admin Config Studio webhook URL edit boxes in both the raw Workbench preview layout and runtime section positioning so full Discord webhook URLs are easier to edit.

## Beta - 2026-07-04 11:19 CEST

### Changed
- Set the raw Admin Config Studio layout defaults to a webhook-only Workbench preview so the Discord webhook menu can be inspected before packing or compiling.

## Beta - 2026-07-04 11:18 CEST

### Fixed
- Added dark backing panels behind every edit box across the AdvancedGroups HUD layouts so text inputs stay readable against translucent admin, group, marker, no-build, layout, and chat surfaces.

## Beta - 2026-07-04 10:40 CEST

### Fixed
- Removed the obsolete webhook token field from the Admin Config Studio layout, client UI binding, and checked-in server config template so Workshop-facing config only exposes full webhook URL fields.
- Verified all event-specific webhook URL fields remain enabled in the Permissions / Integrations tab while backend migration still accepts older token-based configs.

## Beta - 2026-07-04 10:07 CEST

### Fixed
- Sorted Admin Config Studio value fields by active section tab at runtime so each tab starts its own fields near the top of the editor panel instead of inheriting the long master-layout positions.

## Beta - 2026-07-04 09:20 CEST

### Removed
- Removed obsolete standalone admin/settings menu backend classes and their old menu layouts now that Admin Command Center and Admin Config Studio live inside the main AdvancedGroups menu.
- Removed an unused text-tab layout and verified the remaining layout files are still referenced by active UI code.

## Beta - 2026-07-04 09:11 CEST

### Fixed
- Rebuilt the Admin Command Center left workspace into separate groups, selected-group, and online-member columns so those panels no longer overlap at high UI scale.
- Shortened Command Center group, member, online-member, and marker rows and hid the map-center debug text so scaled text cannot spill across the selected-group controls.
- Added a delayed server refresh after admin group rename/retag saves so the edited name and tag resync back into the group list and selected-group details.

## Beta - 2026-07-04 09:00 CEST

### Fixed
- Reworked Admin Command Center into a true split view where the admin workspace uses the left half of the screen and the tactical map starts at the right half, reducing text/list overlap at high UI scale.
- Raised shared map marker widget sorting and reapplied stored marker colors during marker updates so placed map markers keep their selected tint instead of falling back to plain white.

## Beta - 2026-07-04 08:50 CEST

### Fixed
- Expanded the Admin Config Studio workspace for the growing server config surface, widened the editor and preview columns, and pinned Apply/Reset controls to the bottom of the editor panel so Chat/Moderation fields no longer overlap the action buttons.

## Beta - 2026-07-04 08:44 CEST

### Fixed
- Made server config marker type entries readable by adding generated marker type names beside their numeric IDs.
- Hardened marker type config normalization so duplicate marker type rows from manually merged or backup-copied configs are collapsed instead of kept.

## Beta - 2026-07-04 08:36 CEST

### Changed
- Added server-controlled 3D player name-tag modes for player names, player icons, and group shortname prefixes, each supporting `0` everywhere, `1` safezones only, and `2` disabled.
- Rendered group shortnames in front of player names on 3D teammate labels and colors those labels with the group's configured clan color when the prefix is enabled.
- Synced configured no-build zones to clients for safezone-only name-tag visibility checks.

### Fixed
- Reduced 3D label overhead by caching loaded icon paths in the fixed label pool instead of reloading icon textures every frame.

## Beta - 2026-07-04 08:16 CEST

### Fixed
- Stopped fresh or re-saved server configs from writing the obsolete `webhookToken` field now that Discord webhooks are stored as full URLs.

## Beta - 2026-07-04 08:08 CEST

### Fixed
- Made the Admin Command Center map serve its tactical purpose by drawing selected-group members, selected-group markers including death markers, and the admin player's current location on the shared map.
- Refreshed admin group-detail member positions from live server player positions before sending them to the Command Center so online members do not show stale or zero map coordinates.
- Removed the misleading group-sync chat line that said the group name had joined whenever the client received group data, and replaced connection status feedback with player-name based group notifications for members coming online or going offline.

## Beta - 2026-07-04 07:58 CEST

### Changed
- Matched the Admin Config Studio backdrop closer to the  admin-menu approach by using a dim full-page overlay, non-alpha-inheriting opaque panels, and `rover_sim_black` panel styling instead of a transparent map-backed surface.
- Added resolution-aware bounds for Admin Config Studio and Admin Command Center so their main panels center/fit from the current screen size and the Command Center map starts after the live panel width.

## Beta - 2026-07-04 07:32 CEST

### Fixed
- Fixed Admin Command Center rename controls by binding handlers directly to the group lists and action buttons, making the name/tag inputs and save button visibly styled, and pushing a fresh admin group list from the server immediately after successful renames.

## Beta - 2026-07-04 07:20 CEST

### Fixed
- Fixed Admin Command Center group rename/retag sync by removing the immediate stale client refresh, trimming submitted values, updating the visible selected-group labels on save, and resending the admin group list after the server applies a rename.

## Beta - 2026-07-04 07:15 CEST

### Fixed
- Forced Admin Config Studio back onto an opaque dimmed admin surface instead of the shared map backdrop, and replaced the Command Center member action button style so the controls render as dark buttons instead of white blocks.

## Beta - 2026-07-04 07:06 CEST

### Fixed
- Added more vertical separation to the Admin Command Center group/member lists and member action controls to prevent list text, filters, and buttons from overlapping at high UI scale.
- Darkened the Admin Config Studio root and panel backing so the shared map reads as a dimmed frosted background instead of a transparent page.

## Beta - 2026-07-03 19:04 CEST

### Fixed
- Reduced Admin Command Center text/button overlap by compacting group and member row labels, shrinking dense list text, and splitting member action controls into fixed-width rows inside the selected-group panel.

## Beta - 2026-07-03 19:02 CEST

### Changed
- Restored a visible frosted backing for Admin Config Studio panels over the shared map backdrop so the page reads as blurred/dimmed glass instead of fully transparent floating text.

## Beta - 2026-07-03 18:55 CEST

### Changed
- Added a server-synced player stats RPC so the Info page can show multiplayer health and blood percentages without calling client-forbidden health natives.

## Beta - 2026-07-03 18:50 CEST

### Changed
- Put the Admin Config Studio over the shared map backdrop, lowered panel opacity for a stronger glass effect, and replaced the left section arrow offset with centered button backgrounds for cleaner alignment.

## Beta - 2026-07-03 18:47 CEST

### Fixed
- Fixed an Admin Command Center crash while receiving group details by stopping member, online-member, and marker list rows from writing into undeclared hidden listbox columns; rank data is now cached separately for admin actions.

## Beta - 2026-07-03 18:32 CEST

### Fixed
- Fixed map/Admin Config Studio opening crashes caused by the Info page reading player health and blood with client-forbidden `Object::GetHealth` calls during multiplayer menu initialization.

## Beta - 2026-07-03 18:27 CEST

### Changed
- Gave Admin Config Studio action buttons visible `DayZDefaultButton` backgrounds and scaled their icons up to match the stronger Info page button treatment.

## Beta - 2026-07-03 18:18 CEST

### Fixed
- Made the Admin Config Studio glass treatment visible by removing the opaque root sheet, reducing panel opacity, and adding actual backing panels behind permission webhook/Admin SteamID edit boxes so editable fields no longer look like plain text.

## Beta - 2026-07-03 18:05 CEST

### Changed
- Restyled the Admin Command Center panels toward a translucent map-glass treatment instead of opaque dark blocks, while making editable search/name/tag boxes brighter so customization fields read as actual inputs.

## Beta - 2026-07-02 09:15 CEST

### Fixed
- Compacted the Admin Command Center layout for high UI-scale resolutions by narrowing the command workspace, reducing exact text sizes, shortening dense action labels, docking online members inside the command workspace, and pushing the shared map farther right to avoid panel overlap.

## Beta - 2026-07-02 09:02 CEST

### Changed
- Rebuilt the Admin Command Center selected-group workspace so group name/tag editing, member controls, marker oversight, and online member visibility have dedicated visible controls.

### Fixed
- Wired the Admin Command Center rename/retag action to the visible name and tag fields, added client feedback for empty names, and blocked server-side duplicate group names or tags during admin renames.
- Added a right-side online member list for the selected group so admins can see which members are currently online while using the map context.

## Beta - 2026-07-01 22:46 CEST

### Fixed
- Prevented the Admin Command Center from being clipped under the shared map by forcing its root, header, group list, and selected-group panels to a high-sort fixed work area when the page opens.

## Beta - 2026-07-01 22:41 CEST

### Fixed
- Fixed Admin Command Center group loading by sending its admin RPCs through the local player target instead of a null target, and added visible empty/access-denied feedback when the server returns no admin group list.

## Beta - 2026-07-01 21:45 CEST

### Fixed
- Hardened marker RGB color apply handling so group and private marker create/edit requests validate the submitted `RRGGBB` value server-side, preserve the previous color on invalid input, and notify players when a group marker edit is blocked by permissions.

## Beta - 2026-07-01 21:27 CEST

### Fixed
- Fixed a Mission compile parser error in webhook JSON sanitizing by removing unsafe backslash string literals from `AGWebhook.c`.

## Beta - 2026-07-01 21:21 CEST

### Changed
- Extended Discord webhook routing with optional full-URL destinations for global chat, vicinity chat, group chat, subgroup chat, and all marker placement events while keeping blank event URLs on the default webhook.

## Beta - 2026-07-01 21:17 CEST

### Changed
- Migrated Discord webhook configuration to full webhook URLs with optional per-event destinations for group created, group disbanded, member joined, member left, and member kicked events; deprecated split webhook tokens are folded into the default URL and cleared during config normalization.

## Beta - 2026-07-01 21:04 CEST

### Fixed
- Hardened Discord webhook delivery by respecting the group-event logging toggle, preserving optional webhook tokens through config migration/Admin Studio saves, parsing full webhook URLs into a stable REST base and endpoint, and escaping quotes/backslashes in webhook JSON payloads.

## Beta - 2026-07-01 21:01 CEST

### Changed
- Added Admin Config Studio editing for role chat tag identities so Admin, VIP, and custom tags can update their SteamID, display tag, and hex color from the Chat section.

## Beta - 2026-07-01 20:55 CEST

### Fixed
- Applied the Group page clan color to visible group chat lines, including the group channel tag, sender name, message text, and clan tag.

## Beta - 2026-06-30 20:24 CEST

### Changed
- Wired the new update icon into reload, refresh, and reset controls, keeping the cloud icon reserved for sync.
- Wired the new trash icon into marker row delete, marker popup delete, zone delete, admin marker delete, and server-marker removal controls.

## Beta - 2026-06-30 20:15 CEST

### Changed
- Replaced text-heavy reload, sync, save, accept, OK, confirm, and apply controls with icon buttons using the new cloud, save, thumbs-up, and check assets, with distinct tints for different action types.

## Beta - 2026-06-30 19:23 CEST

### Changed
- Switched RGB color controls to native filled slider rendering across marker, group, appearance, zone, and admin color pickers to avoid custom fill panels drifting under DayZ UI scaling.
- Added horizontal map overscroll allowance on shared map pages so zoomed-out players can drag left/right far enough to inspect the west and east edges instead of snapping back to center immediately.

### Fixed
- Rendered group, private, and server marker icons through the colored AdvancedGroups map overlay on the Markers page so marker icons use the saved RGB color instead of appearing white on the native map layer.

## Beta - 2026-06-30 19:05 CEST

### Fixed
- Hardened map-menu focus handling by focusing the AdvancedGroups menu root, excluding the vanilla `map` input while open, and debouncing map-close shortcuts briefly after opening.
- Hid clipped RGB numeric value labels across marker popup, Group color, Appearance, Zones, and Admin Config color controls; color selection still uses the sliders plus hex/swatch previews.

### Changed
- Reclassified earlier June 29-30 map-key, slider-label, marker-header, and map-clamp changelog entries that were implementation attempts but were not fully fixed in-game.

## Beta - 2026-06-30 18:59 CEST

### Changed
- Attempted to rework the marker popup and Group page RGB slider rows with shorter bars and wider value fields; this was later superseded by hiding the clipped numeric labels.
- Changed shared map clamping to measure the visible map parent viewport instead of the full MapWidget surface; east/south edge behavior still requires in-game validation.
- Added a post-vanilla-map fallback for replacing DayZ's native map with the AdvancedGroups map when vanilla opens first; this was later supplemented with active input excludes.

## Beta - 2026-06-30 18:35 CEST

### Changed
- Attempted to pull the marker popup RGB slider values inside the color panel and shorten the slider bars; this did not fully resolve DayZ font/layout clipping.

## Beta - 2026-06-30 18:28 CEST

### Changed
- Added edit-box focus guards to suppress AdvancedGroups keyboard shortcuts while typing in marker names, chat text, group fields, zone fields, layout values, or admin config values.
- Attempted to let the `M` map opener replace DayZ's native map menu when the vanilla map catches the key first; this was later supplemented with active input excludes.

## Beta - 2026-06-30 18:22 CEST

### Changed
- Attempted to realign the Group page color picker so the RGB sliders, value labels, hex readout, and apply button fit inside the left panel; the visible value labels were later hidden due to clipping.

## Beta - 2026-06-30 18:17 CEST

### Fixed
- Preserved the current marker-map zoom and pan position across marker refreshes, deletes, edits, and 2D/3D visibility toggles so map updates no longer reset the user's view.

## Beta - 2026-06-30 18:11 CEST

### Changed
- Relaxed the shared map zoom clamp from 98% to the real world bounds in an attempt to keep the south and east sides reachable while preventing off-map texture bleed.

## Beta - 2026-06-30 18:07 CEST

### Changed
- Added a mission `M` key event path with a short suppression guard; this did not fully resolve the map-open conflict by itself.
- Tightened the marker list header and row layout to reduce clipping of Server, Group, Players, and Private category labels inside the left marker panel.
- Slimmed and realigned the marker popup RGB slider bars and value readouts; the value readouts were later hidden due to continued clipping.

## Beta - 2026-06-30 17:55 CEST

### Changed
- Added a raw `M` key fallback for opening the AdvancedGroups map when the native map input action is not reported to the mod.

## Beta - 2026-06-29 22:38 CEST

### Fixed
- Restyled the Group page member and nearby-player lists with the dark server-browser list style so native red scroll thumbs no longer appear as bright vertical bars in the left panel.
- Removed the duplicate local-player group map marker so the yellow `You` marker no longer overlaps the player's normal group-member marker label.
- Compact the Group page roster rank text and narrowed 2D map marker label bounds to reduce clipping and label collisions.

## Beta - 2026-06-29 22:35 CEST

### Fixed
- Tightened the Info tab header by resizing static tab slots, centering smaller tab text, and abbreviating long server-info page titles so tabs no longer smear together or clip off-screen.

## Beta - 2026-06-29 22:32 CEST

### Changed
- Wired DayZ's native map toggle input to the AdvancedGroups map page as an initial attempt to make `M` open and close the AG map instead of relying only on the custom AG map action.

## Beta - 2026-06-29 22:26 CEST

### Fixed
- Fixed the Mission compile error from assigning the chat input menu as a widget event handler while keeping chat history wheel scrolling on the menu override.

## Beta - 2026-06-29 22:20 CEST

### Fixed
- Enabled the mouse cursor while the custom chat input is open and routed mouse-wheel input to the chat history so players can scroll up and down before sending a message.
- Stopped new chat messages from forcing the history back to the bottom while a player has intentionally scrolled upward to read older messages.

## Beta - 2026-06-29 22:18 CEST

### Added
- Added admin-only server-marker editing and drag-moving from the Markers map page, including label, icon type, RGB color, radius, strike-line state, display mode, and position persistence to `server_config.json`.

### Fixed
- Routed server-marker popup delete through the admin server-marker remove RPC instead of the group marker delete path.

## Beta - 2026-06-29 22:06 CEST

### Fixed
- Hid server-marker row and bulk `2D`/`3D` controls unless admin mode is active, matching the admin-only server-marker display-mode RPC instead of showing inert buttons to normal players.
- Updated server-marker `2D`/`3D` toggles to apply the local row state immediately while the persisted server sync is in flight.
- Restored the map close shortcut by also listening for DayZ's native `UAMapToggle` input while the AdvancedGroups menu owns focus.

### Changed
- Reworked shared-map clamping to use the actual map parent viewport when the proportional MapWidget reports a tiny screen rect; later testing still showed east/south reachability issues.

## Beta - 2026-06-29 21:53 CEST

### Fixed
- Added an in-menu input fallback so the AdvancedGroups map/menu can close from its own Map or Group keybind while it owns UI focus.
- Reversed the marker-map mouse wheel zoom direction to match common map behavior and reclamped the map after wheel zooms.
- Updated compass and minimap heading to follow the current camera direction while keeping vehicle position centering for minimap navigation.
- Delayed custom chat auto-scroll until after new chat rows recalculate their layout so incoming messages stay visible at the bottom.
- Widened and shortened 3D marker labels so long marker or player names no longer hide the distance text.

### Changed
- Attempted to correct shared-map world-bound zoom clamping so zooming out recenters against the full terrain; later testing still showed east-side reachability issues.

## Beta - 2026-06-29 21:42 CEST

### Changed
- Compacted the marker list page, row template, and header controls to reduce overflow of the 2D, 3D, and delete buttons inside the visible left map column.

## Beta - 2026-06-28 21:28 CEST

### Added
- Added an admin-only Server Marker toggle to the marker popup so admins can explicitly create persistent server markers, including radius and strike-line settings, without relying only on the active map tab.

### Fixed
- Reworked 2D map strike-line rendering into a denser crosshatch overlay so server marker zones with strike lines are visibly drawn on map-backed pages.
- Adjusted marker popup RGB slider input so click positioning still allows native slider drag/change behavior to update the live values, hex readout, and preview.
- Expanded and spaced the marker popup admin controls so Server Marker, Strike Lines, Radius, TP, Private Marker, and action buttons no longer overlap.

## Beta - 2026-06-28 19:58 CEST

### Fixed
- Rebuilt the Admin Config Studio layout as a static DayZ layout with visible header, section rail, active editor panel, summary panel, and all script-bound controls so Workbench and in-game rendering no longer show an empty page.
- Gave the Admin Config Studio root explicit pixel dimensions so Workbench and runtime page repair no longer collapse the section, editor, and summary panels into the same corner.
- Expanded compact one-line Config Studio widget declarations into normal multiline DayZ layout blocks so Workbench consistently reads each widget position and size.

## Beta - 2026-06-28 19:28 CEST

### Fixed
- Stopped the shared map self-position marker from using an embedded canvas that could stretch into a large tinted rectangle across map-backed tabs.
- Cleared and hid the shared map overlay canvas by default when switching pages, only showing it for pages that actually draw map overlays.

## Beta - 2026-06-28 19:17 CEST

### Fixed
- Made the marker popup RGB sliders visibly render over the map by adding a stronger color-panel backing, thicker color bars, and a live hex color readout.

## Beta - 2026-06-28 14:12 CEST

### Fixed
- Contained the Group tab controls inside the left panel so the invite field, color picker, and action rail no longer overlap the shared map.
- Removed hidden multiline feature-summary text from the Group tab layout so DayZ cannot render literal newline escape text over the map.

## Beta - 2026-06-28 14:01 CEST

### Fixed
- Fixed chat identity tags such as server admin and VIP replacing the group tag instead of rendering alongside it.
- Added an admin chat-tag fallback for SteamIDs listed in `adminIds` when no explicit `chatIdentities` entry exists.

## Beta - 2026-06-28 13:38 CEST

### Changed
- Replaced the Group tab raw clan-color hex input with an RGB slider color picker, live swatch, numeric channel values, and Apply action.

### Fixed
- Confirmed the Group tab color apply path is wired through `C_SET_COLOR` and hardened the server-side group color setter with hex validation before saving and broadcasting.

## Beta - 2026-06-28 13:32 CEST

### Fixed
- Hardened Admin Config Studio show-time layout repair so the page root, header, section rail, editor panel, summary panel, and static controls are forced visible when the Config tab opens.
- Hid stale shared-map layers before showing non-map full-root pages so Config Studio cannot open behind the map surface.

## Beta - 2026-06-28 13:20 CEST

### Changed
- Changed group invites so the client can send a selected nearby player or a typed DayZ player name while the server resolves the target SteamID privately.
- Updated the Group page invite label to make username-based invites clear.

## Beta - 2026-06-28 13:12 CEST

### Fixed
- Synced server markers through the server-marker RPC during player connect so newly joined players receive configured server markers without needing another marker update.
- Restored group invites from a selectable nearby-player list and kept typed SteamID invites as a fallback.
- Blocked creating a group when an existing group already uses the same group name or clan tag.

## Beta - 2026-06-28 08:11 CEST

### Fixed
- Removed the duplicate local-player teammate marker from the Markers map page while keeping the Group tab member marker behavior intact.

## Beta - 2026-06-28 08:06 CEST

### Fixed
- Fixed minimap self-position alignment while driving by using the current vehicle transport position and heading for navigation centering instead of the player seat/proxy position.
- Aligned the minimap self-triangle canvas to the MapWidget transform by placing it over the map surface and deriving the triangle position from MapToScreen.

## Beta - 2026-06-28 07:18 CEST

### Fixed
- Reworked the local player map and minimap navigation indicator to draw a yellow heading triangle on a canvas overlay, avoiding the missing ImageWidget/icon render path.

## Beta - 2026-06-28 07:14 CEST

### Fixed
- Moved the local player navigation arrow away from the external triangle texture and onto the static DayZ arrow image as an initial self-position render pass.
- Fixed shared-map self marker rotation to rotate around the screen Z axis and keep the self arrow yellow above the marker layer.

## Beta - 2026-06-28 07:05 CEST

### Fixed
- Replaced Info page runtime-created tabs with fixed layout tab slots that the script only labels, shows, hides, and binds, matching DayZ UI limitations.
- Hardened Admin Config Studio layering by hiding shared content panels before showing full-root admin pages and giving Studio panels exact heights and sort order.

## Beta - 2026-06-28 06:50 CEST

### Fixed
- Restored Info page tabs when server_info.json exists without any pages by repairing empty page lists to the default server information pages while preserving custom footer links.
- Added a client-side Info page fallback and tab button handlers so tabs rebuild and remain clickable even if server info sync arrives late.

## Beta - 2026-06-28 06:47 CEST

### Fixed
- Fixed Admin Config Studio opening with only the preview column visible by forcing the header, section list, active editor, and summary panels back into place when the page is shown.
- Wired Config Studio edit boxes and checkboxes to the page handler so section changes and value edits update the page state reliably.

## Beta - 2026-06-28 06:44 CEST

### Fixed
- Fixed tactical ping 3D labels using the normal marker height offset, so pings now sit close to the hit position instead of floating above the aim point.
- Added an Admin Command Center show-time map-parent position guard so the shared map stays to the right of the command panels.

## Beta - 2026-06-28 06:35 CEST

### Fixed
- Fixed the Admin Command Center Mission compile error by removing the map-center field/indexing readout and printing the map position directly.

## Beta - 2026-06-28 06:30 CEST

### Fixed
- Fixed the Admin Command Center Mission compile error by replacing the map-center local in Update with a persistent page field.

## Beta - 2026-06-27 23:32 CEST

### Fixed
- Renamed the Admin Command Center map-center local used for the cursor readout to avoid a Mission compile lookup error.

## Beta - 2026-06-27 22:24 CEST

### Fixed
- Fixed the World compile error in private marker saving by using a strong local store reference before repairing and writing private marker JSON.

## Beta - 2026-06-27 22:21 CEST

### Fixed
- Fixed the Admin Command Center mission compile error by giving member and marker position locals unique names in the group details reader.

## Beta - 2026-06-27 21:36 CEST

### Fixed
- Added missing override markers to shared page event handlers and page refresh methods to quiet DayZ script FIX-ME warnings.
- Removed strong ref object arguments from private marker helper methods and typed main-menu tab buttons as ButtonWidget to avoid script safety warnings.

## Beta - 2026-06-27 21:28 CEST

### Fixed
- Removed the Admin Command Center tactical map shortcut rail from the layout because DayZ rendered it over the selected-group inspector despite position changes.
- Kept member and group-marker focus actions in the selected-group inspector while leaving server marker and zone actions in their dedicated map workflows.

## Beta - 2026-06-27 21:25 CEST

### Fixed
- Moved the Admin Command Center tactical map tools to a far-right compact rail so they no longer cover the selected-group inspector.
- Tightened tactical map button widths and labels to fit the smaller right-side rail without text spilling.

## Beta - 2026-06-27 21:19 CEST

### Fixed
- Removed the tactical map helper text from the small Admin Command Center map tools card because DayZ rendered the newline escape literally and clipped the copy.
- Made the map tools card fully solid black so underlying inspector text cannot show through the tactical controls.

## Beta - 2026-06-27 21:17 CEST

### Fixed
- Fixed the Admin Command Center tactical map tools card showing underlying selected-group text by making the card background opaque.
- Shortened the tactical map helper copy so it no longer cuts out of the small map tools panel.

## Beta - 2026-06-27 21:14 CEST

### Added
- Hooked Config Studio Info Buttons to the backend by adding editable Discord, website, and donate link fields that save to server_info.json.

### Changed
- Admin config sync now returns the current server_info.json button links alongside server_config.json values so the in-game editor loads, saves, and pushes the visible section controls to online players.

## Beta - 2026-06-27 21:12 CEST

### Added
- Completed more of the Admin Command Center ASCII layout by adding Sync, tactical map filters, Add Server, Add Temp, Add Zone, Edit Selected, Delete Selected, cursor X/Z, selected marker readout, marker edit/move controls, member/marker headers, and an explicit danger-zone note.
- Wired the new Command Center actions to existing sync, map marker, zone, selected marker focus, and selected group-marker delete workflows.
- Completed more of the Admin Config Studio ASCII layout with Apply Section, Reset Section, unsaved-change state, last reload/save readouts, live rule summary, and UI preview text.

### Changed
- Config Studio now updates its preview/summary panel from the current editor values instead of showing only static helper copy.

## Beta - 2026-06-27 21:00 CEST

### Fixed
- Fixed the Admin Command Center tactical map card snapping into the selected-group inspector by giving the tactical panel explicit left/top alignment.
- Shortened the tactical map control text so it no longer spills into adjacent selected-group fields.

## Beta - 2026-06-27 20:58 CEST

### Fixed
- Fixed the Admin Command Center layout panels drawing under or colliding with the shared map by giving the command panels fixed heights and explicit sort order above the map.
- Widened the Command Center header and moved the Reload action out of the title/hint text area.
- Repositioned the tactical map tool card so the selected-group inspector stays in the middle column and the tactical controls sit on the map side.

## Beta - 2026-06-27 20:32 CEST

### Added
- Completed the Admin Config Studio runtime layout as a sectioned server-config editor with General, GPS / Minimap, Chat, UI Defaults, Info Buttons, Permissions, and Maintenance sections.
- Added Admin Command Center map focus controls for selected members and selected group markers.
- Added an admin-only group-marker deletion RPC so Command Center marker oversight can remove markers from the selected group without requiring the admin to join that group first.

### Changed
- Expanded Admin Command Center group detail sync with member online state, map position, and health percentage for richer admin inspection.

## Beta - 2026-06-27 20:28 CEST

### Fixed
- Restored player-facing delete controls for owned private markers and player-created group markers while keeping server-marker deletion admin-only.
- Made marker-list delete visibility match the server permission model: private marker owner, group marker creator, group officer, or active admin mode.

## Beta - 2026-06-21 21:35 CEST

### Fixed
- Fixed Admin Config Studio sections being visual-only by converting the section list into clickable buttons and routing each section to the matching config controls.
- Added direct widget handlers for the Admin Config Studio root and action/section buttons so section clicks do not depend on parent menu bubbling.
- Fixed stacked Admin Config Studio controls by hiding inactive section widgets so General, GPS, Chat, UI Defaults, Permissions, and Maintenance no longer overlap each other.
- Kept the deprecated webhook secret hidden even when the Permissions section is selected.

## Beta - 2026-06-21 20:38 CEST

### Added
- Added server-backed UI defaults to `server_config.json` for compass, playerlist, 3D marker defaults, 3D name/distance/icon toggles, player name tag mode, and default HUD/player marker colors.
- Wired the Admin Config Studio UI defaults through admin config save/load RPCs, general client config sync, config migration, and runtime HUD/3D marker application.

### Changed
- Player name tag mode now affects 3D teammate labels: `0` shows names everywhere, `2` disables names, and `1` is stored/synced for safezone-only but hidden until a safezone source is wired.

## Beta - 2026-06-21 20:22 CEST

### Fixed
- Aligned the Admin Config Studio runtime layout with the concept ASCII by replacing the loose four-panel form with a section selector, active config panel, and preview/summary column while preserving the existing config widget IDs.

## Beta - 2026-06-21 20:14 CEST

### Fixed
- Aligned the Admin Command Center runtime layout with the concept ASCII by keeping the selected-group inspector visible as the middle column and allowing the tactical map tools to render on the map side.
- Changed the group search box from prefilled text to an empty input with a separate label so typed SteamIDs do not append to `Search`.

## Beta - 2026-06-21 19:57 CEST

### Added
- Wired Admin Command Center group rename/retag, support join, selected-member SteamID copy, and selected-group marker inventory through admin RPCs.

### Changed
- Replaced the Command Center's read-only group identity row and marker placeholder with editable group name/tag controls and a real marker list fed by the selected group details response.
- Changed Admin Command Center group deletion to a same-selection confirm click instead of immediate deletion.

## Beta - 2026-06-21 19:14 CEST

### Changed
- Added the first implementation pass for the two-page admin suite by exposing Admin Command Center and Admin Config Studio as the only admin tabs.
- Reworked the group admin layout into an Admin Command Center shell with group search, selected group/member actions, marker oversight copy, and tactical map context.
- Promoted the server config layout into Admin Config Studio and wired inactive group cleanup days plus cleanup interval editing through the admin config RPC.

## Beta - 2026-06-21 19:03 CEST

### Changed
- Updated `ui_concepts.md` with the two-page admin suite direction: Admin Command Center and Admin Config Studio.
- Added detailed ASCII layouts for the combined group/member/marker admin workspace and the combined server config/UI defaults/permissions workspace.
- Documented the server-wide player name tag visibility mode: everywhere, safezone-only, or disabled.

## Beta - 2026-06-21 18:58 CEST

### Changed
- Removed the server logo concept from the server info config, sync payload, and Info page layout so branding assets can live outside AdvancedGroups.

## Beta - 2026-06-21 18:22 CEST

### Added
- Added the Group tab feature summary for map/3D group members, logout positions, member health/distance status, and server-configurable inactive group cleanup.
- Added persisted group activity tracking with `inactiveGroupCleanupDays` and `groupCleanupIntervalSeconds` server config fields for inactive group cleanup.

### Changed
- Reworked the Group tab player list into a full group member status list that shows online/offline state, health, and distance instead of only nearby online players.
- Added offline logout-position rendering to the 3D group member overlay when player 3D markers are enabled.

## Beta - 2026-06-21 18:12 CEST

### Changed
- Removed the current low-value HUD, Appearance, Layout, and Admin Groups sidebar tabs from the main menu navigation while keeping the underlying page code available for future planning.

## Beta - 2026-06-21 17:57 CEST

### Fixed
- Fixed Mission compile failures from unsupported bit-shift operators in color picker and marker color helper code by switching RGB extraction/hex formatting to Enforce-safe arithmetic and hex-pair parsing.

## Beta - 2026-06-21 17:35 CEST

### Fixed
- Standardized RGB color picker slider styling across marker popup, Zones popup, Appearance, and admin menu color controls so DayZ's native slider focus box no longer draws over the custom color bars.

## Beta - 2026-06-21 17:23 CEST

### Changed
- Restored the local player map/minimap marker to a fixed yellow triangle so the player icon is visible consistently.
- Hid the unfinished Server Config tab from the main sidebar and labeled the page as WIP for any remaining admin entry paths.
- Removed visible minimap small/medium/large controls from the layout/settings surfaces until there is a production-ready sizing plan.
- Reworked the Zones editor style controls into an icon/color popup that writes the selected marker icon and RGB color into admin-created server zone markers.

## Beta - 2026-06-21 17:07 CEST

### Added
- Added server-owner install, update, configuration, rollback, and pre-production guidance; current release and upgrade instructions are maintained in `RELEASE_GUIDE.md` and the feature-specific guides.

## Beta - 2026-06-21 15:25 CEST

### Changed
- Reworked the Appearance page from raw hex color inputs into an RGB color picker with selectable color targets, live R/G/B sliders, value readouts, and preview swatches.
- Renamed client page save actions to `SAVE & SYNC` on HUD, Appearance, and Layout so the UI makes the immediate refresh behavior clear.

### Fixed
- Fixed the first-pass Appearance page missing the requested color picker interaction model.
- Fixed the client settings pages showing generic save wording even though saving also refreshes the live HUD state.

## Beta - 2026-06-21 10:40 CEST

### Changed
- Split the settings direction into separate sidebar pages for HUD, Appearance, Layout, Server Config, Admin Groups, and Zones instead of compressing all controls into one layout.
- Added first-pass client HUD, Appearance, and Layout pages with per-tab Save and Reload actions.
- Kept existing admin RPC page indices stable: Server Config remains page 3, Admin Groups remains page 4, and Zones remains page 5.
- Reworked tab visibility so only explicit admin pages are hidden outside admin mode; player-facing HUD, Appearance, and Layout remain available.
- Wired client appearance colors into visible runtime surfaces for compass tint, player-list text/header tint, and self/player marker tint.

## Beta - 2026-06-21 09:52 CEST

### Changed
- Converted the Server Zone Markers list from a plain text list into icon-capable marker rows so zone entries can show the selected marker icon in the admin Zones page.
- Updated map-zone circle scaling to use an aspect-correct map scale basis for more consistent canvas drawing.

### Fixed
- Fixed radius server zones being mirrored into the normal server-marker cache, 3D overlay, and minimap as point icons instead of rendering as shape-only map overlays.
- Fixed the shared map canvas layer being hidden or sorted behind marker widgets on map-backed pages, which could make circles and strike lines disappear.
- Fixed a stale no-build/Zones list widget reference left after the list conversion.

## Beta - 2026-06-21 08:25 CEST

### Changed
- Reworked the Server Zone Markers color controls into separate full-width rows for Text Color, Circle Color, and Line Color.
- Added live color swatches beside each server-zone color field so admins can verify RGB hex values visually.

### Fixed
- Fixed the Mission compile error from storing `RestApi`/`RestContext` as managed `ref` fields. Webhooks now keep native REST handles without trying to manage their private destructor.

## Beta - 2026-06-21 07:11 CEST

### Changed
- Replaced the webhook log-only stub with a real Discord webhook REST POST path using the configured webhook URL.
- Added separate server-zone color fields for marker text, circle outline, and strike-line fill in the in-game Zones editor.
- Extended server marker sync data with `strikeColor` so clients can render strike lines independently from circle/text color.
- Updated server-zone rendering so radius zones use the canvas overlay layer instead of a normal 2D point icon.

### Fixed
- Fixed Discord webhooks not arriving even with a correct URL because AdvancedGroups was only logging webhook events locally.
- Fixed a Mission compile parser error in the webhook JSON sanitizer caused by an unsafe backslash string literal.
- Fixed server zone markers still showing the regular cancel/cross 2D marker icon on the map.
- Fixed the Zones tab exposing the off-map edge strip by applying the shared map world-bound clamp there too.
- Fixed old admin server-marker placement RPCs not sending the expanded server-marker payload shape.

## Beta - 2026-06-20 21:51 CEST

### Changed
- Updated the Zones preview to treat circles and strike fills as canvas overlays instead of normal marker icons.
- Documented the server-zone rendering direction: icons/labels, canvas shapes, and gameplay triggers should remain separate concepts.
- Added a Steam Workshop listing draft that clearly labels AdvancedGroups as beta, warns against production public-server use, and discloses AI-assisted development.
- Bumped server config migration target to version 3 for the beta migration pass. Older configs are backed up and migrated before save.
- Deprecated webhook token handling in the in-game admin config flow. The webhook URL remains the active config value, while token payloads are kept empty for compatibility.
- Renamed the bad-word admin editor from CSV wording to a server-config list, matching the existing `server_config.json.badWords` array.
- Added group color editing to the group page so leaders can apply six-digit RGB hex colors through the existing group color RPC.
- Changed the player self marker on the group map and minimap to use a yellow triangle icon instead of the cyan dot/circle treatment.
- Added admin group category assignment controls for `Admin`, `VIP`, `Tier1`, and `Tier2`.
- Added public API documentation snippets for server-side group lookup, marker creation, server marker zones, group categories, and group colors.
- Reworked the group management/create panels into a roster-first layout with a command rail and separate recruitment section, keeping existing widget IDs while changing the UX shape.
- Added neon-tinted action icons to the group controls for promote, demote, invite/join/create, remove, and leave.
- Updated the `K` 3D marker shortcut into a filter cycle: all 3D markers, disabled, server-only, group-only, and private-only.
- Increased marker-list row/header spacing so marker sections are easier to scan.
- Widened the marker side panel and marker row/header layouts for ultrawide screens so names and 2D/3D/delete controls have more room.
- Updated marker edit/update RPCs to send the full editable marker state: label, color, 2D/3D state, icon type, and radius.
- Moved death marker creation to the server-owned death flow so group/private death markers are saved and synced reliably.
- Added server-config normalization on load/save to remove duplicate server markers by marker ID after migration or manual config drift.
- Added map viewport bounds clamping and a zoom guard so the shared map cannot pan or zoom into off-map edge strips.
- Added explicit server config versioning. `server_config.json` is now checked against the current server config version during load; mismatched or legacy configs are backed up and migrated into a current config shape.
- Converted private markers from local-only client settings into server-owned per-player marker files under `$profile:AdvancedGroups/private_markers/<steamId>.json`.
- Updated private marker create, edit, delete, move, and 2D/3D visibility toggles to use server RPCs and server sync.
- Updated solo death markers to create server-owned private markers, while grouped death markers create shared group markers.
- Converted the old UI concept notes into a GitHub/wiki-style AdvancedGroups documentation page.
- Added a server-marker zone model so admin-authored markers can carry radius and optional strike-line overlay data instead of living as separate no-build-only records.
- Refactored the old No Build page into a Zones tool that creates and deletes server markers directly from the in-game admin UI.

### Fixed
- Fixed the admin settings page not fetching config values by sending config request/save/reload RPCs to the player object, matching the mod's `PlayerBase.OnRPC` dispatcher.
- Fixed server-marker zone circles using global map screen coordinates on the shared canvas. Circle and strike-line drawing now subtracts the map widget screen offset before drawing.
- Fixed Zones preview showing the `CANCEL`/cross marker icon on radius zones by removing the normal `AddUserMark()` icon from the shape-only zone preview.
- Fixed solo ping placement by falling back to a server-owned private ping when the player is not in a group.
- Fixed group promote, demote, kick, and invite icon buttons being visual-only by wiring them to the existing group RPC actions.
- Fixed VIP/special category assignment being unreachable from the admin UI by exposing the existing server category setter through RPC.
- Fixed group chat tag color ignoring saved group color by tinting the local group tag from synced `clanColor`.
- Fixed admin config saves carrying forward deprecated webhook tokens by writing and migrating the field as empty.
- Fixed stale admin/menu icon references after icon asset renames by moving the old promotion icon path to the current marketing icon.
- Fixed the group page shared map using unclamped map bounds, which could expose the outside-map strip.
- Fixed config migration duplicating default server markers by clearing constructor-seeded server markers before copying preserved markers from an older config.
- Fixed already-migrated version-2 configs with duplicate server marker IDs by normalizing `serverMarkers` during load and save.
- Fixed marker updates only partially applying by preserving icon type and circle radius during edit-popup updates and visibility toggles.
- Fixed death markers not appearing by creating them server-side instead of relying on a client RPC from the killed player.
- Fixed ultrawide marker list compression by widening the marker panel from 440px to 560px and expanding row text/control spacing.
- Fixed map edge bleed by clamping the marker map viewport to world bounds and preventing zoom levels that expose outside-map strips.
- Fixed old or manually edited server configs being detected only by missing fields; config migrations now use a real version check with legacy safety checks.
- Fixed private markers not being available from the server source of truth for solo players.
- Fixed private marker persistence mismatch where client-local marker edits could diverge from server state.
- Fixed death marker storage so solo death markers are synced back from the server instead of living only in local client settings.
- Fixed server marker label clutter by removing the `[SVR]` prefix from 3D marker labels.
- Fixed marker popup color serialization so RGB slider output is saved as plain `RRGGBB` hex, allowing colors like pink `FF00FF` to apply correctly.
- Fixed 2D marker visibility by restoring native map `AddUserMark()` rendering for group, private, and server markers.
- Fixed server marker name clipping by giving non-admin server marker rows more room when row controls are hidden.
- Fixed map marker placement by supporting right-click placement and normalizing map-click positions to terrain height before saving or moving markers.
- Fixed 3D marker rendering so 2D-only markers no longer render as 3D labels.
- Fixed 3D marker labels to render above marker positions while keeping distance based on player-to-marker location.
- Fixed private marker 2D/3D toggles by tracking each marker-list row's source type instead of relying on a missing tab state.
- Fixed marker popup deletion for private markers by using the marker's actual private/group state.
- Fixed marker icon widgets by selecting the loaded image slot after loading `.paa` textures, preventing colored icon backgrounds from appearing without the actual icon.
- Fixed marker placement input on the shared map by accepting events from the map parent/overlay as well as the map widget itself.
- Fixed the group page missing-map area by enabling the shared map and showing group member positions in blue when online and red when disconnected.
- Fixed marker map rendering so group, private, and server markers are drawn through the native map marker layer, preventing duplicate server labels and making newly-created markers appear reliably on the 2D map.
- Fixed marker list row overflow by clipping/truncating long names and reserving fixed space for 2D/3D/delete controls.
- Fixed marker list section overlap by replacing the fixed marker grid with a stacked marker list layout.
- Rebuilt the marker popup with labeled RGB sliders, live RGB values, icon/color preview, clearer admin controls, and a cancel action.
- Fixed marker popup RGB bars so the selected red, green, and blue values visibly fill behind the native sliders.
- Fixed marker popup panel/button styling so the modal background, action buttons, preview, and RGB fills render over the map instead of appearing as floating text.
- Fixed marker popup controls so a visible close button and Escape close the popup instead of forcing the whole menu flow.
- Fixed marker popup icon selection by replacing the scrollable text list with a fixed icon grid, preventing map zoom/scroll input from leaking through the popup.
- Fixed marker popup input capture by assigning the page handler to every popup child widget, preventing wheel/click input from leaking to the map behind controls.
- Fixed marker list `X` actions so they delete markers instead of silently switching them to hidden `- -` mode.
- Fixed server marker rows so all configured server markers sync from `server_config.json` and render in one dynamic left-panel stack, including 3D-only and 2D-only server markers.
- Fixed popup input shielding so clicks and wheel input over/behind the popup no longer place, edit, or zoom map markers accidentally.
- Fixed broken marker/sidebar icon paths that pointed at missing `.paa` files, causing colored icon backgrounds without visible icons.
- Increased the minimum 3D marker render range to 5000m so server, private, and group marker distance labels remain visible at practical map distances.
- Fixed the info page repeatedly throwing client VM exceptions by removing direct client-side `GetHealth`/stat polling until a server stats sync is added.
- Replaced the map-menu navigation panel with passive 3D-world marker distance labels for server, private, and group markers.
- Fixed 2D server markers not appearing on the map by also rendering them through the native map user-mark layer.
- Fixed shared map bleed-through between tabs by making map and marker overlay visibility page-specific.
- Fixed full-screen AG page roots that used fixed 1860x1080 sizing, which could make the UI behave like a lower fixed-resolution canvas.
- Fixed the global close button layering so it stays available across full-screen tabs.
- Fixed `K` 3D toggle so it toggles both marker labels and player labels, then hides active labels immediately when disabled.
- Fixed stale `data/inputsAdvancedGroups.xml` bindings so the packaged input copy includes `UAAGToggle3D` on `K`.
- Fixed duplicate RPC id collision by moving `S_PLAY_PING_SOUND` to `9117`.
- Fixed manual config sync so `C_REQUEST_SYNC` sends the `isAdmin` value expected by the client.
- Fixed admin kick and rank actions so they target the selected group instead of the admin player's own group.
- Fixed the standalone admin menu to send the updated admin kick/rank RPC payloads.
- Fixed standalone admin menu group-list refresh handling.
- Fixed group deletion so persisted group JSON files are deleted along with index entries.
- Fixed admin-created server markers so placement and removal persist to `server_config.json`.
- Fixed server marker removal sync so the server-marker list/config payload is rebroadcast immediately after deleting a marker.
- Fixed legacy no-build zone config entries by migrating them into server-marker zone entries with radius and strike-line data.
- Fixed Zones panel text clipping by adding exact text sizes and shorter control labels for the server-marker zone editor.

### Added

- Added a shared map canvas overlay for server-marker circles and strike-line zone previews while keeping native map user marks for marker icons and labels.
- Added all current `gui/icons` marker assets to the marker type catalog with default RGB config entries and config-load backfill for old server configs.
- Added icon-only server info footer actions for Website, Discord, and Donate using globe, Discord, and PayPal icons.
- Documented marker enum names, icon asset names, and default RGB colors in `ui_concepts.md` for future config/wiki use.
- Added a `Private Marker` selector to the marker popup so players can choose private vs group marker creation when in a group.
- Added category-level 2D/3D controls to marker list headers for bulk marker visibility toggles.
- Added a native 3D player preview to the server info page, including drag rotation and mouse-wheel zoom.
- Added death marker creation at the local player's death position.
- Added server-side private marker sync RPCs for per-player private marker storage.
- Added `$PBOPREFIX$` with the `AdvancedGroups` package prefix for DayZ Tools packaging.
- Added this changelog.
- Added a full-width in-game server config editor page with save, reload-from-disk, and refresh actions for key `server_config.json` fields.
- Added a refreshed server info panel layout with accent sections, RGB stat values, and lightweight color tags for rich server text.
