---
layout: page
title: UI_SCALING
---

# UI scaling contract

AdvancedGroups uses a single responsive coordinate model across every menu and HUD surface.

## Coordinate rules

1. Full-screen roots and workspace containers use proportional size flags (`hexactsize 0`, `vexactsize 0`). Compact dialogs may use an exact-size design-unit root when every child belongs to the same design coordinate system.
2. Columns and large cards are proportional to their parent whenever they must grow with the viewport.
3. Exact layout units are reserved for leaf controls, padding, icons, and fixed row heights.
4. Runtime code may read `GetScreenSize` and `GetScreenPos` for hit testing, projection, and bounds checks.
5. Runtime code must not feed framebuffer dimensions into `SetPos` or `SetSize` on a widget inside a scaled layout tree.
6. A parent and its children must never be sized in different coordinate systems.
7. Responsive placement belongs in layout anchors (`left_ref`, `right_ref`, `center_ref`) before script code.
8. Runtime movement must use rendered screen dimensions consistently for bounds while keeping the moved widget's position flags exact.
9. Prefer hybrid page geometry: proportional outer regions and columns, exact header/row/control heights, and proportional widths for fields that must consume remaining space.
10. A fixed design-unit panel and the shared-map boundary beside it must use the same scaled layout coordinate space. Do not calculate one from framebuffer pixels and the other from authored units.
11. When screen-space positioning is required for a draggable widget, use its rendered size for bounds and the screen-space form of `SetPos`; persist normalized viewport coordinates rather than raw pixels.

## Allowed runtime geometry

- map and 3D-marker projection;
- canvas drawing;
- scroll-content height based on dynamic row count;
- slider fill and thumb movement inside a fixed authored rail;
- draggable popup position using rendered popup dimensions;
- map viewport boundaries expressed in the same scaled design space as their parent.

## Component sizing patterns

- **Full workspace:** proportional root, proportional major columns, exact navigation rail and header heights.
- **Scrollable editor:** proportional card height with exact rows and a content panel whose height is derived only from its children.
- **Compact popup:** one exact-size root, exact internal controls, rendered-size centering and drag bounds.
- **Dynamic list:** proportional viewport, exact-height reusable rows, explicit content height.
- **Map overlay:** proportional map/canvas pair with projected leaves positioned in screen space.

## Disallowed runtime geometry

- resizing an exact authored popup root with raw framebuffer dimensions;
- positioning a scaled child with `GetScreenSize()` values;
- clamping a parent while leaving its children in a larger scaled design space;
- correcting overflow with per-resolution magic numbers.

## Required test viewports

- 1280x720;
- 1920x1080;
- 2560x1440;
- 2560x1080 ultrawide;
- 3440x1440 ultrawide;
- small, default, and large DayZ UI scale where available.

At each viewport, verify the global close button, page/map boundary, marker cards, marker popup, zone editor, Config Studio, chat, player list, compass, minimap, and notification toast.
