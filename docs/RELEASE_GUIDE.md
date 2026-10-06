---
layout: page
title: RELEASE_GUIDE
---

# AdvancedGroups Beta v0.4.0 Release Guide

Install the same build on server and clients. The current development build uses RPC protocol version 8. Server integrations should check `AGPublicAPI.SupportsAPIVersion(2)` and use a stable, unique owner ID for Event markers.

Release numbering follows `VERSIONING.md`. Open a new changelog section for later work instead of appending it to an already published version.

## Upgrade and rollback

1. Stop the server and back up `$profile:AdvancedGroups/`.
2. Replace client and server mod files together.
3. Keep existing profile JSON; never overwrite it with the packaged example config.
4. Start the server and inspect the AdvancedGroups log for migrations, recovery, rejected saves, or protocol mismatches.
5. Verify groups, Event rules, runtime markers, private markers, server information, and admin IDs.

To roll back, stop the server, restore the matching older client/server build, and restore its profile backup. Do not run an older build against newer migrated schemas unless compatibility is documented.

## Persistent data

Authoritative runtime data is under `$profile:AdvancedGroups/`: `server_config.json`, `server_info.json`, `event_markers.json`, `event_runtime_markers.json`, `spawn_config.json`, `spawn_cooldowns.json`, `groups/`, `groups_index.json`, and `private_markers/`. Do not edit these while the server runs.

## Clean package

`Documentation/` is repository material and must remain outside the shipped mod/PBO payload. Also exclude source-control folders, editor state, server profiles, backups, temporary JSON, logs, test missions, and credentials. Build from a clean revision, inspect the packed file list, confirm `CfgMods.version`, test a clean client/server pair, record PBO hashes, and archive the exact source revision.

Run `powershell -ExecutionPolicy Bypass -File tools/release-audit.ps1` before building. Use `-Strict` to make warnings fail the release.
