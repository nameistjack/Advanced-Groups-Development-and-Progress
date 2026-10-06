---
layout: page
title: UI_LAYOUT_COVERAGE
---

# UI Layout Coverage

Status: Beta v0.4.0 plus unreleased development work, verified 2026-10-06.

AdvancedGroups uses adaptive outer containers and stable, readable control sizes. Controls are not uniformly stretched with the screen. Full-screen surfaces divide available space proportionally; compact HUD elements remain pixel-sized but are anchored and clamped to the current viewport; long forms use scrolling or compact breakpoints.

## Shared sizing rules

- The reference canvas is 1920 by 1080, but runtime geometry comes from the actual viewport.
- Horizontal and vertical margins are clamped to prevent both edge crowding and excessive empty space.
- The main navigation rail remains a stable touch target.
- Content/map divisions use clamped proportions instead of fixed 1920-pixel coordinates.
- Fixed-size dialogs are centered at runtime and clamped to the viewport.
- Long vertical content uses scroll containers.
- Reusable rows, icons, markers, and blips retain fixed readable sizes inside responsive parents.

## Layout coverage

| Layout | Runtime strategy |
|---|---|
| `ag_main_menu.layout` | Full viewport root; adaptive content/map split and close-button placement. |
| `ag_page_info.layout` | Full remaining viewport; proportional content regions and dynamically distributed tabs. |
| `ag_page_group.layout` | Adaptive content-column centering; scrollable manage/create panels on short displays. |
| `ag_page_markers.layout` | Clamped list-column width, responsive scroll height, remaining width assigned to the map. |
| `ag_page_marker_popup.layout` | Runtime-centered and viewport-clamped modal. |
| `ag_page_admin.layout` | Full-width compact mode; split map mode on wide displays; scrollable list and detail columns. |
| `ag_page_settings.layout` | Screen-relative margins, proportional columns, compact breakpoints, live reflow, and adaptive log/status regions. |
| `ag_top_button.layout` | Stable reusable navigation control inside the adaptive rail. |
| `ag_marker_icon_button.layout` | Stable reusable control inside a scrolling strip. |
| `ag_marker_list_header.layout` | Fixed readable row inside the adaptive marker-list scroll container. |
| `ag_marker_list_entry.layout` | Fixed readable row inside the adaptive marker-list scroll container. |
| `ag_marker_zone_panel.layout` | Contextual zone state and editing controls beside the map. |
| `ag_spawn_menu.layout` | Responsive spawn-selection shell with list, actions, status, and map context. |
| `ag_zone_card_entry.layout` | Fixed-height reusable zone row inside a scrolling category. |
| `ag_zone_card_header.layout` | Fixed-height collapsible zone-category header. |
| `ag_zone_editor.layout` | Dedicated scaled zone-management work surface beside the shared map. |
| `hud_root.layout` | Full viewport root. |
| `compass.layout` | Top-center anchor; width clamped to viewport and cells redistributed; outer cells hidden at very narrow widths. |
| `minimap.layout` | Top-right runtime anchor; selected size capped to viewport while preserving 16:9 aspect ratio. |
| `minimap_blip.layout` | Constant map-symbol size positioned relative to the responsive minimap. |
| `playerlist.layout` | Safe-margin anchor; height derived from member count and viewport; scrollable member region. |
| `playerlist_member_row.layout` | Stable readable row inside the player-list container. |
| `infopanel.layout` | Compact anchored HUD component; currently intentionally hidden by its controller. |
| `ag_chat_root.layout` | Bottom-left anchor; width and history height clamped to viewport. |
| `ag_chat_input.layout` | Input width aligned with responsive chat width. |
| `ag_chat_item.layout` | Content-sized row inside the responsive chat history. |
| `map_marker.layout` | Screen projection performed by its controller; constant icon/label size for legibility. |
| `marker_label.layout` | Screen-projected 3D label with fixed readable typography. |
| `invite_menu.layout` | Runtime-centered modal on initialization, display, and resolution changes. |

## Breakpoint expectations

- Wide: 1700 pixels and above. Maps and optional summaries can coexist with editor panels.
- Standard: 1500 to 1699 pixels. Core split views remain available with reduced secondary space.
- Compact: below 1500 pixels or below 850 pixels high. Secondary map/summary regions may be hidden, and long editor columns scroll.

The release audit validates that every layout remains referenced. Visual acceptance still requires one in-game pass at representative 16:9, 16:10, ultrawide, and compact resolutions because font metrics and UI-scale settings are applied by the engine at runtime.
