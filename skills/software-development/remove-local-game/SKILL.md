---
name: remove-local-game
description: Use when removing local games and their mods.
---

# Remove a local game

## Inputs and boundaries
- Require the exact game and the user's intended storage scope. Treat an explicit removal request as authorization only for identified game-specific files.
- Apply the latest user correction before deleting. If saves must remain, locate all save copies and script-extender companions, including archived Wine prefixes and emulator save directories.

## Procedure
1. Inspect the game root, mod-manager instance settings, downloads, launcher entries and per-game configuration. Resolve home-directory symlinks; do not assume different path spellings imply separate installs.
2. Identify whether Wine/Proton prefixes are shared. Use launcher configurations to establish consumers. Delete a dedicated prefix only after confirming it has no saves to retain or unrelated consumers; remove only game-specific data and registry sections from shared prefixes.
3. Make an explicit deletion manifest. Exclude retained saves and unrelated games. Snapshot retained save files with content hashes before destructive work; this addresses the concrete risk of irreversible save loss.
4. Check running processes and their WINEPREFIX/STEAM_COMPAT_DATA_PATH. A wineserver for an unrelated prefix is not a blocker and must not be killed.
5. Remove identified game/mod files, stale launcher entries, per-game icons/configuration, and handlers used solely by the removed mod instance. Preserve shared runtimes and icon-theme assets. Keep launcher JSON changes limited to the identified application IDs and verify unrelated records are unchanged.
6. Verify each deletion target is absent, retained saves match the snapshot, unrelated game directories/launcher records remain, and a fresh scoped scan contains only intended save remnants. Rebuild desktop MIME caches if handlers were removed.

## Practical cautions
- Bound filesystem exploration and print reduced results. Large download/trash trees can timeout a recursive scan; a failed scan is not evidence of no matches.
- Disk free-space changes can include concurrent downloads or snapshots. Report approximately reclaimed space, not an exact deletion total inferred solely from global free-space deltas.
- Finish with what was removed, what was retained and verified, and the precise retained-save locations. Do not claim a whole-computer scan if only scoped locations were inspected.
