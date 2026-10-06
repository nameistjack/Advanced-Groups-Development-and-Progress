---
layout: page
title: UI_LAYOUT_AUDIT
---

# UI layout audit

This inventory applies the scaling contract to every shipped layout. A file is
not made proportional merely to increase the migration count: fixed row and
icon templates must remain exact so their hit targets and list rhythm do not
stretch with aspect ratio.

## Classification

| Layout | Role | Geometry policy |
|---|---|---|
| `helper/map_marker.layout` | projected map label | fixed leaf; script projection allowed |
| `hud/ag_chat_input.layout` | viewport overlay | proportional anchored container |
| `hud/ag_chat_item.layout` | dynamic chat row | fixed row width, content-sized height |
| `hud/ag_chat_root.layout` | chat history window | anchored floating panel; dynamic content height only |
| `hud/ag_main_menu.layout` | full-screen root | proportional viewport container |
| `hud/ag_marker_icon_button.layout` | icon template | fixed leaf |
| `hud/ag_marker_list_entry.layout` | list row template | fixed-height dynamic row |
| `hud/ag_marker_list_header.layout` | category card header | fixed-height dynamic row |
| `hud/ag_marker_zone_panel.layout` | contextual zone controls | fixed card beside map editor |
| `hud/ag_page_admin.layout` | command work surface | scaled design panel beside shared map |
| `hud/ag_page_group.layout` | content page | proportional parent page |
| `hud/ag_page_info.layout` | full-page information | proportional parent page |
| `hud/ag_page_marker_popup.layout` | draggable editor | compact exact-size design island; exact leaves; screen-space drag bounds |
| `hud/ag_page_markers.layout` | responsive map rail | measured rail width; fixed-height cards |
| `hud/ag_page_settings.layout` | configuration work surface | scaled design panel with internal columns |
| `hud/ag_spawn_menu.layout` | spawn-selection surface | responsive list/map split |
| `hud/ag_top_button.layout` | navigation icon | fixed leaf |
| `hud/ag_zone_card_entry.layout` | zone list row | fixed-height dynamic row |
| `hud/ag_zone_card_header.layout` | zone category header | fixed-height dynamic row |
| `hud/ag_zone_editor.layout` | zone work surface | scaled design panel beside shared map |
| `hud/compass.layout` | HUD strip | viewport-centered, width-clamped overlay |
| `hud/hud_root.layout` | HUD root | proportional viewport container |
| `hud/infopanel.layout` | notification row | fixed-height, width-clamped overlay |
| `hud/marker_label.layout` | projected 3D label | fixed leaf; script projection allowed |
| `hud/minimap.layout` | HUD instrument | fixed-aspect, viewport-anchored overlay |
| `hud/minimap_blip.layout` | minimap point | fixed leaf |
| `hud/playerlist.layout` | HUD list | fixed-width, dynamic-height anchored panel |
| `hud/playerlist_member_row.layout` | player row | fixed-height dynamic row |
| `menu/invite_menu.layout` | modal dialog | layout-centered fixed dialog |

## Runtime geometry ownership

- `AGMainMenu` alone owns the sidebar, content rail, and shared-map boundary.
- Page scripts may resize dynamic list content and rows, but must not resize
  their scaled top-level page from framebuffer dimensions.
- Admin, Settings, and Zone panels retain one authored design coordinate space;
  their shared-map boundary must be derived from that same space.
- HUD scripts may clamp an entire fixed-aspect instrument to the viewport.
- Projection code may position map/minimap/3D marker leaves every frame.
- Layout anchors own centering. No per-frame centering loop is permitted for a
  static dialog.

## Verification status

- Full-screen roots: audited.
- Marker rail and marker editor: migrated.
- Shared map boundary: consolidated under `AGMainMenu`.
- Invitation dialog: layout-centered; redundant per-frame mutation removed.
- Dynamic rows/icons/projected labels: retained as exact geometry by design.
- Admin, Settings, Zone, chat, and HUD panels: classified for focused viewport
  tests at every resolution listed in `UI_SCALING.md`.
