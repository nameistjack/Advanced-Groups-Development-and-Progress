---
layout: page
title: UI_DESIGN_SYSTEM
---

# AdvancedGroups UI Design System

Status: Beta v0.4.0 plus unreleased development work, verified 2026-10-06.

The UI uses a native component system implemented by `AGUITheme` and `AGUILayoutMetrics`. It does not require another addon or any external UI assets.

## Visual tokens

- Deep neutral surfaces separate the UI from the game world without an opaque full-screen backdrop.
- Raised cards use a lighter neutral surface to establish hierarchy.
- Cyan is reserved for selection, navigation, and section identity.
- Green identifies confirm, save, apply, accept, and create actions.
- Amber identifies reset and reload actions.
- Red identifies close, discard, kick, decline, and delete actions.
- Primary and muted text colors replace unrelated per-layout label colors.
- Lists use a consistent high-contrast dark background.
- Form controls retain a light editable surface so they remain clearly distinguishable from labels.

## Components

`AGUITheme.ApplyTree` styles a layout and its descendants consistently. Reusable state helpers cover navigation selection and pointer hover without overriding selected tabs.

`AGUILayoutMetrics` supplies viewport dimensions, safe margins, clamped proportions, compact/wide checks, and dialog centering. Layout controllers use these metrics instead of repeating 1920 by 1080 assumptions.

Long or dense content follows these rules:

- only one Admin Config section is visible at once;
- disabled webhook integration details collapse automatically;
- compact Admin Config navigation changes to two columns;
- Group and Command panels scroll vertically;
- marker and player lists live inside bounded scroll regions;
- secondary maps and summaries may be hidden when space is insufficient.

## Movable dialogs

The marker editor has a dedicated title drag surface. Its normalized viewport position is saved in the client settings, clamped to the visible screen, and restored across sessions and resolution changes.

## Reusable dynamic content

Marker rows, marker headers, icon buttons, player rows, chat rows, event entries, and navigation controls are produced from reusable layouts or controller-driven collections. The surrounding containers determine available space while the reusable items retain readable control and typography sizes.

## Editable colors

Editable colors use six-digit `RRGGBB` values and an opaque alpha channel for on-screen previews. Every color editor follows the same synchronization contract:

- moving an R, G, or B slider updates the hex value and preview immediately;
- entering a valid hex value updates all three sliders and the preview immediately;
- incomplete or invalid text does not replace the last valid preview;
- saved values are normalized to uppercase, without a `#` prefix;
- marker editing previews the exact color on the swatch, selected icon, and marker label;
- Config Studio previews cover chat identity tags, UI defaults, Event markers, and every group-tier color;
- the Group page previews the group color with the same RGB-to-hex mapping.

All previews use `ARGB(255, r, g, b)`, matching the opaque color used by map markers, 3D markers, group tags, compass text, and player-list text.

## Asset policy

The design system intentionally uses engine-native panels, color tokens, and the existing AdvancedGroups icon set. This avoids texture scaling artifacts, extra PAA packaging, and visual dependencies while still providing consistent card hierarchy and semantic states.
