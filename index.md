---
layout: home
title: Overview
---

This is the public development hub for Advanced Groups, an independently
designed and implemented DayZ group, map, marker, chat, navigation, zone, spawn,
and administration framework.

No source code from another group, map, or administration mod was copied to
build Advanced Groups. The project has its own namespaced scripts, RPC protocol,
persistence formats, interface controllers, configuration tools, and public API.
Common gameplay concepts such as groups, ranks, maps, and markers are implemented
specifically for this project.

## Start here

- [Development progress]({{ site.baseurl }}/progress/) - current status and the complete changelog.
- [Documentation wiki]({{ site.baseurl }}/wiki/) - linked index of all published technical pages.
- [UI design system]({{ site.baseurl }}/docs/UI_DESIGN_SYSTEM/) - visual tokens, components, and behavior.
- [Architecture and feature direction]({{ site.baseurl }}/docs/ui_concepts/) - system and product design.
- [Marker integration]({{ site.baseurl }}/docs/MARKER_GUIDE/) - supported public API for server-side integrations.
- [Spawn module]({{ site.baseurl }}/docs/SPAWN_MODULE/) - optional server-authoritative spawn selection.

## Project principles

- Server-authoritative validation and persistence.
- Namespaced inputs, RPC identifiers, classes, and stored data.
- No required runtime dependency on another map, group, or admin mod.
- Responsive native DayZ layouts with reusable project-owned components.
- Explicit schema, protocol, public API, migration, and release contracts.
- Compatibility through documented interfaces instead of access to internal data.

## Publication boundary

This site publishes development documentation only. Game source, server
configuration, credentials, packaged assets, and the Steam Workshop listing are
not included in the generated site.
