---
layout: page
title: ENGINEERING_REFERENCE_AUDIT
---

# AdvancedGroups Engineering Architecture Audit

This document records architecture decisions derived from AdvancedGroups requirements, defects found during implementation, DayZ engine constraints, and direct testing of this codebase. It is a roadmap for improving AdvancedGroups without introducing runtime dependencies or weakening ownership of its code, assets, data formats, and visual identity.

## Evaluation rules

- Derive changes from a documented AdvancedGroups requirement, defect, or measured limitation.
- Keep every AdvancedGroups symbol and persisted format namespaced.
- Prefer small, testable subsystems over a larger manager class.
- Verify all client requests again on the server.
- Preserve backward compatibility and migration paths for released configuration.
- Adopt a pattern only when it simplifies AdvancedGroups or closes a demonstrated reliability gap.

## Highest-priority opportunity: damage attribution

The current AdvancedGroups fallback stores a weak `PlayerBase` reference on traps and explosives. It is safe when the player object is destroyed, but attribution is lost after disconnect, object replacement, or restart.

A stronger design should store attribution metadata rather than relying only on a runtime object pointer:

- Placer/thrower plain identity ID
- Remote activator identity ID
- Last direct damager identity ID where relevant
- Optional display name snapshot for diagnostics only
- Attribution source/reason tag
- Timestamp or lifecycle generation where useful

Resolution should follow an explicit precedence chain:

1. Direct player source
2. Player at the source hierarchy root
3. Stored remote activator
4. Stored last damager
5. Stored placer/thrower
6. Bounded hierarchy traversal
7. Unknown source, fail open

Return a small outcome object rather than only a player pointer. It should contain the resolved online player when available, stored identity fields, source class, and a resolution tag such as `direct_player`, `hierarchy_root`, `remote_activator`, `stored_placer`, or `unknown`.

Benefits:

- Better diagnostics when damage is unexpectedly allowed or blocked
- Correct handling of remotely triggered explosives
- A path to support launched explosives and special ammunition
- Less reliance on an online `PlayerBase` instance
- Clear test cases for every attribution route

Guardrails:

- Do not keep strong references to player entities.
- Never block environmental or AI damage when attribution is uncertain.
- Keep metadata server-authoritative.
- Namespace every field and method.
- Avoid persisting entity pointers; persist identity strings only if persistence is actually required.

## Modular service architecture

AdvancedGroups currently places substantial responsibility in `AGServerManager`. Splitting that responsibility into focused services gives each subsystem clearer client/server ownership and makes this codebase easier to test and maintain.

Candidate services:

- Group service
- Permission service
- Marker service
- Event-marker service
- Configuration service
- Damage-policy service
- Webhook service
- Audit-log service
- Admin-status service

Each service should own its state, RPC handlers, validation, lifecycle, persistence, and log category. `AGServerManager` should coordinate startup and cross-service workflows rather than implement every feature directly.

Acceptance criteria:

- Feature-specific logic can be located without searching one large manager.
- Client-only UI services are never initialized on a dedicated server.
- Server-only state is never created on ordinary clients.
- Subsystems can reload independently where safe.

## Permission model

Replace coarse admin checks for sensitive operations with named capabilities while retaining a super-admin path.

Suggested capability structure:

```text
AdminStudio:Open
AdminStudio:ReadConfig
AdminStudio:WriteConfig
Groups:View
Groups:Edit
Groups:Disband
Markers:EditServer
Markers:EditEvents
Logs:View
Webhooks:Edit
Maintenance:Reload
```

Rules:

- Check capabilities server-side on every mutating RPC.
- Use the client permission snapshot only to hide or disable controls.
- Log denied operations with actor, capability, and operation type—but never secrets.
- Support immediate permission refresh for connected administrators.
- Keep permission identifiers stable once released.

## RPC boundaries and contracts

- Give every RPC one clear request or response purpose.
- Validate sender identity, authorization, payload length, ranges, and collection counts before mutation.
- Separate DTO/wire models from live server objects.
- Include protocol/schema versions where payloads are likely to evolve.
- Return structured success/failure results for admin mutations instead of relying only on notifications.
- Rate-limit expensive reads and all mutations.
- Keep unknown-RPC and malformed-payload logging bounded to avoid log flooding.

## Configuration and persistence

Useful patterns for AdvancedGroups:

- Separate configuration definitions from runtime state.
- Validate and normalize immediately after deserialization.
- Keep indexes by stable ID and normalized name when frequent lookup requires them.
- Write temporary files, parse/verify them, then replace live files.
- Maintain timestamped backups with an explicit retention limit.
- Discover modular configuration files deterministically and sort paths before loading.
- Preserve disabled definitions for editing but omit them from active runtime collections.
- Mark dependent caches stale after reload instead of rebuilding everything immediately.
- Report validation problems with file, field, and rejected value category.

## Definition/runtime separation

Complex features such as event markers and future zones should distinguish:

- Persisted definition: administrator-authored configuration
- Validated definition: normalized configuration ready for activation
- Runtime instance: active world state and cached calculations
- Client snapshot: minimal serializable representation for display

This prevents UI edits from mutating live world state before Apply and makes reload/rollback behavior easier to reason about.

## Bounded runtime processing

For systems that inspect many players, markers, actors, or hosted events:

- Maintain stable maps for direct lookup.
- Keep a separate ordered key list only when incremental iteration is needed.
- Process a bounded number of entries per tick using a cursor.
- Coalesce dirty-state broadcasts.
- Rebuild expensive geometry or display caches only when marked stale.
- Avoid full-player/full-marker scans for each individual mutation.

## Webhook architecture

AdvancedGroups already has a bounded queue, duplicate suppression, retry behavior, and counters. It can be improved further through:

- Typed message objects serialized through `JsonSerializer`
- Multiple named endpoint profiles
- Per-endpoint event subscriptions
- Optional group/category filters
- Privacy controls for player names, coordinates, and group names
- Endpoint-specific embed colors or presentation settings
- Runtime reload that clears stale REST contexts safely
- Queue metrics exposed in Admin Status
- A test action that uses a harmless typed test event

Security requirements:

- Never log endpoint credentials.
- Mask URLs in every client response and UI field unless explicitly revealed.
- Do not send secrets back to clients that lack write capability.
- Apply payload and queue limits before allocation where possible.

## Logging and diagnostics

- Use categories and levels consistently.
- Include UTC timestamps for persisted logs.
- Rotate logs by session or bounded size.
- Keep an in-memory ring buffer for the UI rather than reading unbounded files repeatedly.
- Permission-check remote log retrieval.
- Attach resolution tags to damage-policy decisions.
- Add correlation IDs to multi-step admin mutations where practical.
- Redact webhook credentials, tokens, and sensitive identifiers from ordinary logs.

## Admin UI system

The detailed UI recommendations live in `ADMIN_CONFIG_UI_AUDIT.md`. Architecturally, the important lesson is to create a small AdvancedGroups design system:

- Central widget styles and interaction states
- Reusable row prefabs
- Dynamic size-to-content form hosts
- Shared dropdown, tooltip, confirmation, validation, and collapsible-section controllers
- List/detail editors for collections
- Window-owned headers and footers
- Localized display strings

## Integration boundaries

AdvancedGroups should expose stable, minimal interfaces rather than invite direct access to internal arrays or JSON files.

- Keep public API models detached from live state.
- Version the public API contract.
- Validate every external input.
- Provide callbacks/events only for stable lifecycle moments.
- Avoid compile-time coupling to optional integrations unless protected by explicit defines.
- Prefer adapter files for optional integrations so core services remain independent.

## Release and compatibility checks

Extend the release audit to detect:

- Calls to undefined or unowned extension methods
- Unnamespaced members added to vanilla classes
- RPC identifiers outside reserved ranges
- Duplicate RPC identifiers
- Secrets or webhook URL patterns in tracked files
- Layout controls referenced by scripts but absent from layouts
- Layout controls that are never bound or used
- Config fields written but never read, or exposed in UI without enforcement
- Modded overrides that omit `super` without an explicit allowlist reason
- Client/server protocol version mismatches

## Recommended implementation order

1. Add release-audit checks for unowned calls, secrets, RPC duplication, and missing widgets.
2. Replace transient damage ownership with namespaced identity metadata and resolution outcomes.
3. Split permission and damage policy logic out of the central server manager.
4. Introduce capability-based authorization for Admin Config Studio operations.
5. Build the reusable Admin UI design system described in the UI audit.
6. Convert collection editors to list/detail workflows.
7. Introduce typed webhook payloads and endpoint profiles.
8. Separate persisted definitions from runtime instances for event-driven features.
9. Add bounded tick processing and cache-stale signaling where profiling demonstrates value.

## Non-goals

- Reproducing another product's appearance or branding
- Importing third-party proprietary assets or source
- Adding required runtime dependencies
- Expanding AdvancedGroups into a general-purpose admin tool
- Adopting complexity without a concrete AdvancedGroups use case
