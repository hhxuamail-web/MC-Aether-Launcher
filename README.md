# Aether Launcher 2.7.0

Aether Launcher is a Windows Minecraft launcher focused on reliable game downloads, modern instance management, Fabric/Forge/NeoForge support, Modrinth content, and flexible Java controls.

Version **2.7.0** combines a complete 927-version download catalog with a PCL-inspired Mod Center, exact compatible-version selection, and pre-install dependency planning.

## New in 2.7.0

- Completed the PCL-overlap audit and replaced every content-download surface with the same independent catalog/detail/version/install architecture.
- Upgraded the modpack market with Minecraft-version, loader, category, and sorting filters plus a dedicated full-page project detail view.
- Modpack details now include gallery media, project description, all compatible pack versions, exact `.mrpack` selection, target runtime, and a one-click new-instance installation panel.
- Unified resource packs, shaders, and data packs into compatible content centers with project details and exact file-version selection.
- Added data pack support with explicit target-world selection and installation into that world's `datapacks/` directory.
- Removed the duplicate Modrinth browser from the instance mod manager; installed mods now use a dedicated management view and new downloads hand off to the Mod Center.
- Added enabled, disabled, and update-available filters to installed mod management.
- Added per-instance Java compatibility status and an automatic Mojang runtime preparation action.
- Added a documented PCL overlap map covering download, instance, launch, account, and settings surfaces.
- Rebuilt download progress around a PCL-style overview with aggregate running-task progress, total speed, downloaded bytes, and each task's current file. Remaining time is intentionally not displayed.
- The floating download window's Collapse action now hides the entire window instead of leaving a progress-only bar. Background work continues and remains available from Download List; a new task shows the window again.
- Updating now offers an opt-in “Delete all old-version user data” installer checkbox. After a second confirmation it removes instances, saves, mods, resource packs, shaders, account data, settings, and download caches. A custom game directory is left untouched.
- Added `Aether-Launcher-2.7.0-full-uninstall.exe`, a dedicated full-uninstall tool that removes the program directory, instances, saves, mods, content, account data, settings, logs, download/update caches, shortcuts, and uninstall registry entries. A custom game directory is detected and listed as a separate target.
- Added **Safe Mode Launch** to the toolbox: temporarily removes all mods and restores them automatically when the game exits.
- Added **World Time Machine**: backs up every world in an instance and restores individual historical snapshots with a pre-restore safety backup.
- Added **One-Click Server Forge**: downloads the official server for a selected Minecraft version and prepares Java, EULA, server properties, and a launch script.
- One-Click Server Forge now selects online or offline authentication and can switch existing servers, fixing the “Invalid session” error when an offline profile joins an online-mode server.
- Added resilient system-proxy fallback: when the configured proxy reports `ERR_PROXY_CONNECTION_FAILED`, tunnel failure, or an unsupported proxy authentication path, the launcher switches to direct networking and remembers that choice for the current period. Version metadata, mirrors, images, and Microsoft sign-in use the same transport.
- Rebuilt game installation around one verified inventory covering the client, libraries, natives, asset index, asset objects, and logging configuration.
- Removed the old standalone “running libraries completed” path. A file only counts as cached after exact-size and SHA-1 verification.
- Added a final full-inventory audit before instance commit and revalidation of expanded legacy virtual assets.
- Both BMCLAPI library routes, `/maven/` and `/libraries/`, are attempted before official Maven fallback.
- A second instance of the same version now reuses every verified client, library, and asset instead of unconditionally downloading the client again.
- A normal “New Instance” flow always defaults to the latest release from the manifest; snapshots are preselected only when installation starts from a specific snapshot entry.
- Changing the selected release, snapshot, or historical version in the wizard updates an automatically generated instance name; user-entered custom names are preserved.
- Download management supports pause, resume, and cancel. Pausing closes the active connection and keeps the `.part` checkpoint; resuming continues from it; cancelling stops the task and removes the unfinished instance directory.
- Removed the “installed/x instances” markers from the version catalog and version selector. The instance library is now the sole authoritative view for existing instances, preventing stale cached counts from misleading users.
- Made the main-process task table the only source of task truth; renderer task snapshots are revisioned, and stale running events can no longer overwrite a completed or failed task.
- Sanitized every task snapshot before IPC, excluding internal callbacks and underscore-prefixed fields so Electron structured cloning always succeeds.
- Added a visible “Download management” action to the compact task dock and hydration of active tasks after renderer reloads.
- Modpack installation now reuses one parent task from instance creation through pack-file installation instead of creating a second nested task that could remain stuck near 10%.
- Download size is now always visible in the task dock, overview, compact task card, and detailed task view; unknown totals display `0 B / calculating` instead of a dash.
- Instances are committed to the library only after all required game files and optional pack content are complete.
- A new instance downloads only missing or invalid clients, shared libraries, assets, natives, and logging configs. Files that pass exact-size and SHA-1 verification are reused, as are Java runtimes; existing instances are never copied as a shortcut.
- Partial instance directories are marked `.aether-installing` and excluded from all instance queries until the `.aether-ready` commit marker is written.
- Replaced the previous downloader with an independent HMCL/PCL-inspired engine; the legacy pipeline no longer handles ordinary file transfers. The former split library-completion path is now replaced by unified inventory verification, download, and final auditing.
- Every file uses one streaming connection and concurrency is applied across files. Clients, assets, and Maven dependencies are speed-tested by category instead of being hard-coded to one mirror.
- Client files prefer the faster measured Mojang endpoint; assets choose between BMCLAPI and the official resource host; Maven and loader dependencies prefer BMCLAPI or a domestic Maven fallback.
- Timeouts, low speed, connection resets, HTTP 429, 404, and SHA-1 mismatches switch to the next candidate and resume through the `.part` file.
- Resumed bytes are counted exactly once, keeping downloaded size, speed, network bytes, and planned total consistent.
- Added persistent native-library cache keyed by loader/profile and native file fingerprints.
- Added a completion marker for expanded legacy asset virtual directories.
- Cached Java version detection by executable path, size, and modification time.
- Reduced the launcher-side command preparation for a Fabric 1.21.4 test instance from about 5.5 seconds on the first run to about 17 milliseconds on the cached run.
- HTTP 429 is handled as a temporary rate limit with `Retry-After`, a pause on new requests to that host, exponential backoff, and resumable source fallback.
- Rebuilt the Mod Library as a dedicated PCL-inspired Mod Center with target-instance compatibility, category filters, sorting, and pagination.
- Added Modrinth search indexes for relevance, downloads, follows, newest releases, and recent updates.
- Added a full project-detail workspace with gallery media, project description, download/follow counts, tags, and external Modrinth access.
- Detail pages list every version compatible with the target Minecraft version and loader, including file name, size, channel, and publish date.
- Users can select an exact compatible version instead of being limited to the latest file.
- Added a pre-install dependency plan that resolves required projects, displays their icons, names, versions, and sizes, and totals the complete download.
- Exact-version installation now preserves the selected root version while resolving each required dependency for the same target instance.
- Mod installation tasks now report the active JAR, source URL, dependency stage, byte progress, and speed.
- Added searchable standard categories including optimization, adventure, world generation, technology, magic, decoration, equipment, food, game mechanics, transportation, storage, social, utility, and libraries.
- Rebuilt the Minecraft download center as a version catalog inspired by the familiar public PCL workflow, without copying PCL implementation code.
- The catalog now includes 103 releases, 763 snapshots, 26 old Beta builds, and 35 old Alpha builds.
- Added Release, Snapshot, Beta, Alpha, and All filters, full-text version search, installed-instance badges, and a version inspector.
- The inspector shows the required Java major, estimated total download, client/library/asset breakdown, release date, existing instances, and current mirror status.
- Added one-click loader choices for Vanilla, Fabric, Forge, and NeoForge directly from the version inspector.
- Added live source measurements and automatic fallback when a mirror metadata endpoint is reset or unavailable.
- Historical versions without a modern `javaVersion` field now infer their required runtime from the version era instead of being rejected as incomplete.
- Download tasks report the exact current file, active source, network bytes, cache bytes, speed, and elapsed time without an ETA.
- Concurrent tasks no longer share a global task reference; metadata and asset-index failures are attributed to the correct instance.
- Resumed `.part` files count their existing bytes toward progress, while incomplete or checksum-mismatched temporary files are discarded safely.
- Rebuilt the instance library as a resource workspace with aggregate statistics, search, type filtering, sorting, compact per-instance actions, and a row-based instance list.
- Added library statistics for total instances, running instances, modded instances, and accumulated play time.
- Rebuilt the game download flow as a three-step wizard: version/loader, runtime, and final review.
- Removed the always-visible create-instance configuration column from the version catalog.
- Each version row now has a direct Download action that starts the same guided flow.
- Redesigned the game-version page as a full-width catalog with per-version Download actions.
- Moved loader, memory, Java, source, and optional-mod configuration into a dedicated Download Instance wizard.
- Added a separate **Download List** button and modal, independent from the version browser.
- Minecraft clients, libraries, assets, logging configs, and loader metadata are downloaded directly from Mojang or the configured mirror into the target instance.
- The version downloader no longer depends on selecting a local archive or local game-version files.
- Removed the always-visible create-instance sidebar from the version catalog.
- The separate Download List provides a live, filterable queue for all running and completed tasks.
- The queue lists every concurrent install task with stage, progress, size, speed, and failure state.
- Creation buttons re-arm immediately after a task is submitted, allowing multiple Minecraft versions to download at the same time.
- Finished tasks remain visible until the user selects “Clear finished”.
- Completed and failed tasks now reject late progress events, preventing a finished task from falling back to an early percentage.
- Instance creation now returns its backend task ID, and the renderer finalizes that exact task when the creation promise resolves, even if a completion IPC event is delayed or missed.
- The top completion toast and the Download List therefore share the same completion state instead of diverging.
- Download details separate total progress, real network bytes, cache-hit bytes, smoothed network speed, elapsed time, source, and a live stage timeline without an ETA.
- Cached files are SHA-1 validated when a checksum is available; corrupt cached files are discarded and downloaded again.
- Network speed uses a smoothed estimate instead of reporting instantaneous parallel-download spikes.
- The task detail view shows whether data is coming from Mojang, a custom mirror, or a BMCLAPI/Aliyun fallback chain.
- Concurrent tasks use isolated task IDs, so progress updates from one version no longer overwrite another.
- Shared libraries, assets, and client files are coalesced by destination, preventing duplicate downloads or `.part` file conflicts.
- Snapshot and non-latest versions now advance explicitly from the download-plan stage into client preparation and client download.
- The asset-index stage reports progress and attempt details and cannot remain silently at the old 7–9% state.
- Legacy libraries that only provide native classifiers no longer generate nonexistent base JAR URLs such as `jinput-platform-2.0.5.jar` or `twitch-platform-5.16.jar`.
- Native classifiers are deduplicated by destination before download.
- Library deduplication now ignores incompatible OS-specific variants before applying loader overrides, preventing a macOS-only LWJGL nightly from removing the Windows/general LWJGL platform library.
- This fixes `NoClassDefFoundError: org/lwjgl/opengl/OpenGLException` on Minecraft versions older than 1.13, including 1.8.1-pre3.
- On Windows, game processes now launch through the sibling `javaw.exe` with hidden-window inheritance disabled, fixing legacy LWJGL 2 versions that previously stayed running without showing a Minecraft window.
- Legacy manifests now receive the required `--userProperties {}` value, preventing clients such as 1.7.10 from parsing the following `--userType` flag as JSON and exiting with `Expected BEGIN_OBJECT but was STRING`.
- Language codes now preserve the exact casing used by the selected client JAR. Legacy clients such as 1.5.2 receive `lang:en_US`, while modern clients continue using lowercase codes such as `en_us`, preventing a null language stream at startup.
- Legacy `map_to_resources` asset indexes are now materialized before launch, fixing the empty language/font resource crash on Minecraft 1.3.2.
- Duplicate manifest entries are merged before building native-library plans, fixing Minecraft 1.13.2 and related versions that downloaded LWJGL but not the matching native JARs.
- On Windows, native libraries are extracted to an ASCII-safe temporary directory and removed after game exit, avoiding Java 8 `UnsatisfiedLinkError` failures when the launcher or game directory contains non-ASCII characters.
- Minecraft clients, libraries, assets, logging files, Java runtimes, and Forge installers now use a mirror-first download order with official-source fallback. No Minecraft game files are bundled in the installer.
- The instance library now uses a card workspace with a recent-instance, pinned-world, and playtime rail; the instance maintenance controls remain at the top of the page.
- The Download List now uses compact task cards with status, stage, and progress visible by default, and expandable network, cache, size, and timeline details.
- All background APIs now return their exact task ID, and successful renderer operations finalize that task immediately. Late progress events can no longer leave a completed repair, mod, resource-pack, shader, modpack, Java, or instance task stuck at an older percentage.
- The task center now includes every background task type instead of filtering to game downloads only.
- Successful task cards remain visible at 100% for five seconds, then close automatically. If another task is still running, the card switches to that active task instead of closing.
- Removed the large AETHER LABS promotional card and vertical generation text from the sidebar; the navigation now leads directly to the compact account control.
- Standardized all launcher and embedded seed-map UI fonts on Microsoft YaHei, except English display labels, which use Times New Roman. The minimum interface font size is now 11.5 px, with larger body copy, controls, headings, and metadata tuned for desktop readability.
- Instance cards now use a dedicated pixel-art Minecraft landscape image instead of a CSS-generated glyph; the shader section description was rewritten in plain language and no longer exposes the internal `shaderpacks` directory name.
- Resource-pack and shader headings now use parallel plain-language copy. Their cards, icons, badges, and hover states inherit the active accent theme instead of retaining fixed blue/purple colors.
- Removed the two technical status pills from the resource-pack and shader headers, and added matching introduction cards to the modpack market and mod library. All four content sections now follow every Aether theme accent.
- Fixed the instance-detail Java button by separating the Java status element from the clickable button ID. The Java selector now opens reliably, and the entire interface was checked for duplicate element IDs.
- JSON metadata requests now have a hard timeout, source status messages, and fallback progression even if the underlying network request never resolves.
- Corrupt or empty cached asset indexes are discarded and fetched again atomically.
- Removed the unfinished Instance Version Transition UI, IPC handlers, state, and documentation.

## New in 2.5.7

- Reduced the minimum staged health-scan animation from five seconds to three seconds.
- The five analysis stages and determinate progress now complete in a shorter, smoother sequence.
- Added a dedicated cleanup animation immediately after confirming safe cleanup.
- Cleanup shows a rotating trash icon, animated striped progress, changing status text, and a percentage while temporary files and logs are removed.
- The cleanup animation remains for at least 0.9 seconds so fast deletions are still visibly acknowledged.
- The health report refreshes directly after cleanup without starting another three-second scan animation.

## New in 2.5.6

- Every instance scan now runs through a themed animation for at least five seconds, even when file analysis finishes immediately.
- The scan shows five stages: reading the instance, analyzing storage, reading mod metadata, checking dependencies/conflicts, and generating recommendations.
- A determinate progress bar, percentage, orbit animation, and changing status text keep the scan state understandable.
- The instance selector and scan button are locked while a scan is running.
- The cleanup confirmation dialog now appears above the health dialog immediately, instead of waiting behind it until the underlying window is closed.
- Confirmation dialogs now use a dedicated top layer across the launcher.

## New in 2.5.5

- Added **Instance Health and Storage Analyzer** as a new full-width toolbox section.
- Calculates a health score and reports disk usage for mods, saves, resource packs, shader packs, logs, configuration, natives, backups, and other files.
- Parses Fabric `fabric.mod.json`, Forge/NeoForge mod TOML metadata, and legacy `mcmod.info` data from installed JAR files.
- Detects duplicate active mod IDs, missing required dependencies, unreadable JAR archives, disabled mods, and mods placed inside vanilla instances.
- Shows the largest files and folders to make cleanup decisions easier.
- Finds abandoned `.part`, `.tmp`, and `.download` files and oversized launcher logs.
- Safe cleanup removes only temporary downloads and launcher-generated log files; saves, mods, resource packs, shaders, and configuration are excluded.
- Reports unsupported or unreadable metadata without deleting user files.

## New in 2.5.4

- The complete Terracotta interface now follows all 16 Aether accent themes instead of using a fixed pink palette.
- Ambient surfaces, cards, buttons, room codes, status tags, loaders, member avatars, and highlights use the active theme variables.
- Added the official Terracotta cube logo from the upstream `burningtnt/Terracotta` repository.
- The logo appears in the multiplayer service badge, startup card, footer, and usage guide.
- Added a built-in Terracotta usage guide with four host/join steps, first-use download information, firewall advice, and troubleshooting notes.
- The guide is accessible from the multiplayer header and the startup card.

## New in 2.5.3

- Added a clear Official Release / Snapshot switch inside the create-instance dialog.
- Replaced the local JAR picker with a connected Modrinth mod library.
- The library searches only for mods compatible with the selected Minecraft version and Fabric, Forge, or NeoForge loader.
- Multiple mods can be selected, removed, and reviewed before the instance is created.
- After the instance is installed, Aether downloads each selected mod and its required dependencies through the existing verified mod installer.
- Changing the Minecraft version or loader clears incompatible mod selections.
- Removed the incomplete Redstone Link 2 option, placeholder configuration screen, simulated node controls, and disabled-room actions.
- Multiplayer now shows a fixed Terracotta service badge instead of a native provider dropdown.

## New in 2.5.2

- The create-instance dialog now closes immediately after validation, while downloads and installation continue through the task dock.
- The Minecraft version selector is grouped into official releases, snapshots, and historical builds instead of showing releases only.
- Fabric, Forge, and NeoForge instances can include up to 100 local mod JAR files directly from the creation dialog.
- Selected mods are copied into the new instance's `mods/` folder after installation and are reported in the completion notice.
- Vanilla instances clearly disable the local-mod picker because they cannot load Forge-style mods.

## New in 2.5.1

- Newly downloaded instances now use their readable display name as the folder name instead of an opaque UUID.
- The default instance name `Minecraft 1.21.4` therefore creates `instances/Minecraft 1.21.4`.
- Duplicate names receive a stable numeric suffix such as ` (2)`.
- Windows-invalid characters, reserved device names, and trailing dots or spaces are sanitized safely.
- Cloned and imported instances also receive readable folder names.
- Existing UUID-based instance folders remain supported and are not moved automatically.

## New in 2.5.0

- Added Modrinth Resource Pack Studio with compatibility filtering, search, pagination, in-app gallery previews, and installation into any Minecraft instance.
- Added Modrinth Shader Atelier with the same compatibility-aware browsing and preview flow, writing shader ZIP files to `shaderpacks/`.
- Resource pack and shader downloads verify the Modrinth SHA-1 hash and file size before installation.
- Added an instance server library that reads and writes Minecraft's real gzip-compressed `servers.dat`.
- Saved servers can be added, renamed by address, launched, pinged, or removed without leaving Aether.
- Server icons, texture-prompt preference, and hidden state are preserved when editing an existing list.
- Changes to a server list are blocked while that instance is running, preventing Minecraft from overwriting in-memory changes.
- Both content centers support vanilla, Fabric, Forge, and NeoForge instances.

## New in 2.4.0

- Added a Java runtime scanner that lists detected major versions, installation sources, and executable paths.
- Added a Minecraft Java server status checker with MOTD, online players, server version, server icon, and measured latency.
- Added a start-log analyzer that sends any selected instance's latest launch log through Aether's crash diagnostics.
- Kept the embedded seed-map explorer as the fourth toolbox tool.

## New in 2.3.2

- Added a game-language selector to every launch.
- The language list is built from the selected Minecraft version's installed asset index, covering all official language packs available in that version.
- Languages are displayed using their native names, regions, and Minecraft language codes.
- The selected language is saved per instance and applied to `options.txt` immediately before launch.
- Language selection can be remembered together with Java, memory, and JVM options.

## Fixed in 2.3.1

- Automatic Java selection now strictly matches the runtime version required by the selected Minecraft profile.
- A globally configured Java installation is ignored for launch when it does not match the profile requirement.
- A compatible local runtime is preferred; otherwise the launcher downloads the official matching Mojang runtime.
- Java 24+ JVM flags are now added only when the selected executable is actually Java 24 or newer.
- Explicitly selecting an incompatible Java now produces a clear error instead of a JVM launch failure.

## New in 2.3.0

- Added a dedicated multiplayer page with Server and Room tabs.
- Added direct server launch through Minecraft Quick Play.
- Added integrated Terracotta hosting and joining, room codes, live member status, reset controls, and fallback direct-connect addresses.
- Added secure on-demand Terracotta installation with the official SHA-256 check.
- Added the Terracotta multiplayer workflow.

## New in 2.2.2

- Unified the background, border, and shadow treatment of all main home-screen cards.
- Kept theme accents and nested interactive controls visually distinct from the shared card surface.

## New in 2.2.1

- Added a monthly play calendar with day selection, month navigation, session times, and duration summaries.
- Added a persistent pinned-world collection on the home screen, with pin and unpin actions in the world manager.
- Game launches and exits are recorded into a local history file for future calendar views.

## Fixed in 2.2.0

- Embedded seed-map zoom controls now use the map's own zoom state instead of scaling the entire embedded web page.
- Zooming in and then out reliably restores the previous map level.
- The seed map is now a dedicated exploration workspace with seed presets, random-seed generation, a current-seed card, copy controls, and clear loading/error states.
- The map canvas is larger in both standard and fullscreen layouts, while source, version, and viewport controls stay together in a compact side rail.
- Fixed the translucent map overlay caused by the third-party zoom-control container stretching across the entire map.
- Added a Biome Query System to the exploration settings, with a unified search field and selectable biome options that open Chunkbase's contextual search with the query already filled.
- Entering the Toolbox resets the previous seed, query text, map overlays, and search results; leaving the Toolbox also clears the state in the background.


## Toolbox and Embedded Seed Map

- Added a dedicated **Toolbox** area for the embedded seed map and seed query.
- Moved world, mod-update, local-import, and repair actions into the **Instance Library** as a dedicated instance-tools section.
- Embedded the Chunkbase seed map directly inside the launcher with Minecraft version selection.
- Added launcher-side and in-map zoom controls backed by the map's native zoom actions.
- Added an in-app fullscreen map mode, including interception of the third-party Expand Map control.
- The launcher still performs seed normalization and passes numeric or text seeds into the embedded map.
- The embedded page runs in an isolated Electron guest with navigation restrictions, disabled Node access, denied permissions, and ad-network blocking.
- An external-browser fallback remains available if the third-party map service is unavailable.

## New in 2.2.0: Five Functional Tools

1. **Mod update checker**: identifies installed JARs by Modrinth SHA1 hash, compares compatible versions, and updates one or all detectable mods while preserving disabled state.
2. **Seed query tool**: reads seeds from local world `level.dat` files, accepts numeric or text seeds, normalizes Java seeds, and opens seed-map queries in the browser.
3. **Instance repair**: revalidates and repairs Vanilla, Fabric, Forge, and NeoForge instances while preserving the current loader and version.
4. **Local mod batch import**: imports multiple local JAR files, skips identical duplicates, and safely renames conflicting files.
5. **World save manager**: lists local worlds, displays extracted seeds, opens the saves folder, and creates timestamped `.aetherworld` backups.

### Mod Update Checker Details

- Identify installed JAR files by their SHA1 hash through Modrinth.
- Compare each recognized project against the latest version compatible with the instance's Minecraft version and loader.
- Show current and latest version names, update individual mods, or update all detectable mods at once.
- Preserve disabled-mod state and restore the previous JAR if an update fails.
- Report files that cannot be recognized or do not have a compatible newer version.

### Instance Safety Tools

- **Instance export/import**: package and move an instance as an `.aetherpack` archive, including mods, saves, configuration, resource packs, and shader packs while excluding logs and Natives.
- **Backup and restore**: create timestamped `.aetherbackup` snapshots, browse them from the launcher, and restore safely with temporary-directory swapping.
- **Crash diagnosis**: automatically analyze non-zero game exits and open an actionable report for memory, Java/JNI, JVM arguments, mod conflicts, graphics, permissions, and missing dependencies.

Archives use path traversal checks and a 16 GB extraction limit. Instance backups are stored under the game root in `backups`.

### Aether 2.x Interface and Download Features

- New Aether 2.x visual system across the title bar, sidebar, hero, cards, settings, dialogs, and pagination.
- Sixteen persistent accent themes: Aurora, Ocean, Nebula, Sakura, Mint, Sunset, Crimson, Mono, Forest, Ice, Ember, Lemon, Cloud, Grape, Coral, and Steel.
- Theme tinting is visible on unselected theme cards as well as active navigation and controls.
- Added NeoForge loader discovery and instance creation.
- Added a NeoForge installer preflight cache so the launcher downloads installer dependencies itself instead of relying on the installer's Java process to reach Maven directly.
- Added a ten-minute NeoForge installer watchdog with clear failure logs.
- Added NeoForge mod searching and installation through Modrinth.
- Added NeoForge dependency detection for Modrinth `.mrpack` modpacks.
- Added full pagination to the Modrinth modpack market and Mod Library, including total counts, page numbers, previous/next controls, active-page highlighting, and automatic scrolling after page changes.
- Kept high-resolution, full-aspect modpack artwork and gallery details.
- Kept Java selection, memory controls, JVM arguments, Microsoft login, instance cloning, mirror fallback, resume, and SHA1 validation.

## Highlights

- Download vanilla Minecraft releases and snapshots.
- Launch directly into a saved multiplayer server with Quick Play.
- Create and join Terracotta rooms from the launcher with room codes, member status, and reset controls.
- Create Vanilla, Fabric, Forge, and NeoForge instances.
- Select Fabric, Forge, or NeoForge loader versions during instance creation.
- Search and install compatible Modrinth mods, including required dependencies.
- Browse and install Modrinth modpacks with Vanilla, Fabric, Forge, or NeoForge dependencies.
- Preview mod and modpack galleries with high-resolution imagery.
- Choose Java, memory, and JVM arguments before every launch.
- Detect Java 8, 11, 17, 21, and 25 installations, or download a matching Mojang runtime.
- Use Microsoft device-code login or an offline player profile.
- Clone existing instances without re-downloading shared version files.
- Choose Mojang official, BMCLAPI, or a custom mirror.

## Multiplayer Center

- **Servers** starts any selected instance and passes `--quickPlayMultiplayer` when the game supports Quick Play.
- **Rooms** integrates the official Terracotta local HTTP API instead of opening a separate website.
- On first use, Aether downloads the official Terracotta Windows x64 package, verifies its published SHA-256, and starts it in hidden integration mode.
- Hosts and guests can create rooms, join with invitation codes, reset interrupted sessions, inspect members, and copy fallback direct-connect addresses.
- Terracotta only listens on `127.0.0.1`; room state and invitation codes stay in the local Terracotta process.
- Aether only exposes multiplayer integrations that can create or join real sessions.

## Download Sources

- **BMCLAPI China Mirror** is recommended for mainland China. Metadata, clients, libraries, and asset objects all prefer `bmclapi2`.
- **Mojang Official** uses official metadata and file endpoints.
- **Custom Mirror** accepts a base URL compatible with the BMCLAPI directory layout.

Downloads use single-stream file transfers, file-level concurrency, stall detection, `.part` resume, category-aware source fallback, and SHA1 verification. NeoForge installer dependencies are pre-downloaded with the same resilient pipeline before the official installer runs.

## NeoForge

NeoForge support is a real installer flow, not a placeholder profile:

1. Aether resolves the matching NeoForge artifact version.
2. It downloads the official NeoForge installer.
3. It reads `version.json` and `install_profile.json` from the installer.
4. It pre-downloads all required Maven artifacts into the game libraries folder.
5. It runs the official NeoForge installer with a timeout watchdog.
6. It verifies that the generated NeoForge profile exists.
7. The resulting instance can be launched and can receive NeoForge mods from Modrinth.

Tested combination: Minecraft 26.3, NeoForge 26.3.0.99, Java 25.

## Mod Library and Modpacks

1. Open **Download Center -> Mod Library**.
2. Select a Fabric, Forge, or NeoForge instance.
3. Search for compatible content.
4. Use the pagination controls to browse results by page.
5. Select **Preview** for details and gallery images, or **Install** to add the mod.
6. Required Modrinth dependencies are resolved automatically.

The Modpack Market supports Vanilla, Fabric, Forge, and NeoForge packs. Quilt modpacks are not supported yet.

## Java and Loaders

- Java can be selected globally, per instance, or in the launch dialog.
- A custom Java executable can be chosen with the file picker.
- Memory and JVM arguments can be overridden at launch.
- Java 24/25 automatically receive `--sun-misc-unsafe-memory-access=allow` when required by the game profile.

| Loader | Status |
| --- | --- |
| Vanilla | Supported |
| Fabric | Supported |
| Forge | Supported through the official Forge installer |
| NeoForge | Supported through the official NeoForge installer with dependency preflight |
| Quilt | Not supported yet |

## Instance and Game Locations

By default:

- Launcher data: `%APPDATA%\Aether Launcher`
- Game files: `%APPDATA%\Aether Launcher\game`
- Instances: `%APPDATA%\Aether Launcher\game\instances`
- Cached libraries: `%APPDATA%\Aether Launcher\game\libraries`
- Microsoft account token: `%APPDATA%\Aether Launcher\account.enc`

You can change the game directory in Settings.

## Quick Start

1. Run `Aether-Launcher-2.7.0-setup.exe` and follow the installer.
2. Optionally sign in with Microsoft.
3. Open **Download Center**, pick a Minecraft version and loader, then select loader version, Java, and memory.
4. Select **Download and Create**.
5. Open **Instances** and select **Launch Game**.

The executable is unsigned. Windows SmartScreen may require **More info -> Run anyway** if you trust the build.

## Verification

The following v2.2-v2.5 flows were exercised on Windows:

- Modrinth search returned 18,000+ compatible resource packs and 700+ shaders for Minecraft 1.21.4.
- Faithful and Complementary Shaders compatibility lookup returned valid `.zip` files for Minecraft 1.21.4.
- Resource-pack and shader installation paths, downloaded size, and SHA-1 hash were verified in an isolated instance.
- `servers.dat` round-trips passed for empty lists, plain servers, host-and-port addresses, and entries with icons.
- Server add/read/delete integration was verified against an isolated Minecraft instance.
- Readable folder creation, duplicate suffixes, path sanitization, and clone naming were verified in isolated instances.
- Snapshot selection and the expanded create dialog were visually inspected in the packaged application.
- The create-instance Modrinth library returned compatibility-filtered results and supported multi-select.
- A real Sodium download from the create-instance mod library was installed into an isolated Fabric 1.21.4 instance and cleaned afterward.
- The fixed Terracotta provider badge was visually inspected after removing the Redstone selector.
- All 16 themes were audited for a distinct Terracotta accent color, themed page background, themed primary button, and successfully loaded logo.
- The usage guide modal and responsive mobile layout were visually inspected.
- The instance health scanner detected duplicate IDs, missing dependencies, broken JARs, disabled mods, temporary files, and disposable log bytes in an isolated instance.
- Safe cleanup removed exactly the generated temporary and launcher-log files while leaving every test mod in place.
- The new toolbox card and populated health report were visually inspected at the default desktop size.
- Automated timing measured the minimum health scan at approximately 5.23 seconds.
- The cleanup confirmation was verified to open at `z-index: 140` while the health window remained open underneath.
- Version 2.5.7 automated timing measured the shorter scan at approximately 3.23 seconds.
- The animated safe-cleanup state was visually inspected before, during, and after progress updates.
- Snapshot `26.4-snapshot-7` and Minecraft `1.20.6` asset indexes loaded concurrently (5,402 and 3,862 assets respectively).
- Minecraft 1.2.1, 1.5.2, and 1.7.6 download plans were verified to exclude nonexistent base native JARs and successfully download the correct Windows classifiers.
- A completed task was verified to remain at 100% after receiving a deliberately late 10% progress event.
- The Download List was verified to force a successful task to 100% even if its stored progress value is still an early value.
- A deliberately corrupt cached file was detected by SHA-1 and replaced with a valid downloaded copy.
- Minecraft 1.5.2, 1.7.6, 1.8.1-pre3, and 1.12.2 launch classpaths were verified to contain their matching LWJGL core and utility JARs.
- All 927 versions in the current official Java version manifest passed client/library/Java/download-plan preflight checks.
- The rebuilt engine completed the full preflight in 18 seconds with zero failures across 103 releases, 763 snapshots, 26 Beta builds, and 35 Alpha builds.
- Minecraft Alpha `a1.2.6` and Beta `b1.7.3` were downloaded concurrently into readable version-named folders, completed at 100% independently, and produced Java 8 launch commands.
- The Mod Center returned 1,133 optimization projects for Fabric 1.21.4, with Sodium listed first when sorted by downloads.
- Sodium returned nine compatible versions for Fabric 1.21.4, and an exact version could be selected before installation.
- Iris Shaders resolved to a two-file plan containing Iris and its required Sodium dependency.
- A real isolated installation of the selected Iris version downloaded both JARs, reported each current file and dependency stage, completed at 100%, and was cleaned up after verification.
- Forge 1.20.1 adventure filtering returned compatible modpacks with selectable versions.
- Faithful 64x, Complementary Shaders, and VeinMiner were installed through their respective centers into isolated targets; the data pack used a temporary world and all test files were removed.
- Per-instance Java preparation resolved Java 21 for both test instances.
- A stale-progress regression delivered `running: 10%` after `success: 100%`; the stored and displayed state remained `success: 100%`.
- Main-process task revisions remained monotonic through creation, progress, completion, and a rejected post-completion update.
- A parent-task regression confirmed that a complete modpack installation produces exactly one task; game preparation maps into 2%–20%, then continues in the same task.
- A real empty-directory `a1.2.6` installation completed all 478 files and 49,704,608 bytes at 100%, then passed its full final audit and entered the instance library.
- A second `a1.2.6` instance reused all 47,833,553 verified shared bytes with zero network transfer, and remained hidden until task completion.
- Same-size/corrupt client content and an incorrectly sized library cache were both detected and downloaded again; both BMCLAPI `/maven` and `/libraries` library routes passed.
- The task UI was visually verified with both running tasks that have known totals and tasks whose totals are still being calculated; byte numbers remained continuously visible.
- Downloading instances remain hidden until completion, and network bytes, cached bytes, and planned totals are reported separately without counting local cache reads as network traffic.
- The task snapshot contained no internal callback fields; structured-clone regression tests passed at every emitted revision.
- On the same network, the Minecraft 1.20.1 client took 60.49 seconds at 0.36 MB/s from BMCLAPI but 21.64 seconds at 1.01 MB/s from Mojang. Automatic source selection correctly chose Mojang.
- The new engine downloaded the 23,028,853-byte client through the official source and passed SHA-1; creating a complete instance with shared caches took 3.49 seconds.
- A rate-limit regression returned HTTP 429 twice, backed off, resumed the same file, and produced the correct result.
- A resume regression confirmed that the reported byte total matches the file size exactly and is not counted twice.
- Actual mirror-backed downloads and launches were verified across 28 era-spanning versions from `rd-132211` and Classic through Alpha, Beta, 1.7.10, 1.13.2, 1.20.5, 1.21.5, 26.3, and `26.4-snapshot-7`.
- The existing Minecraft 1.5.2 instance that previously crashed with a null language reader was relaunched after the casing fix and verified to reach its title screen.
- A real 1.5.2 repair task was executed and verified to return a task ID whose final shared state was `success / 100% / 完成`, matching the completion notification.
- Two concurrent downloads targeting the same file were verified to issue exactly one network request.
- The active download queue and multiple simultaneous task cards were visually inspected.
- The redesigned instance library, three-step download wizard, and final review screen were visually inspected at the default window size.
- Minecraft 1.2.1 legacy asset-index loading completed in approximately 0.5 seconds.
- NeoForge 26.3.0.99 installer completion after prefetching all 30 installer dependencies.
- NeoForge 26.3 instance creation and successful game launch with Java 25.
- Minecraft 26.3 + NeoForge Modrinth search returning 5,000+ compatible mods.
- Sodium installation into a NeoForge 26.3 instance and verification of the resulting JAR.
- Live Modpack Market pagination showing 19,000+ results and switching to page 2.
- Live Mod Library pagination showing 13,000+ mods and switching to page 2.
- All sixteen themes rendering with theme-specific unselected states.
- Embedded Chunkbase map loaded inside the launcher with seed and Java-version selection.
- Mod update detection and replacement verified with a real old Sodium JAR and NeoForge 26.3.
- Seed extraction verified with synthetic NBT worlds; world backup archives verified.
- Local mod import duplicate-skip and collision-renaming verified.
- Vanilla and Fabric instance repair verified while preserving the loader configuration.
- Instance export/import round-trip with mods and saves restored and logs excluded.
- Backup creation, listing, safe replacement, and file restoration verified.
- Crash diagnosis verified against out-of-memory, JVM/JNI, and generic exit logs.
- Syntax validation for the main process, preload, and renderer.

## Build From Source

```powershell
npm install
npm run check
npm run dist
```

The build compiles the Windows DPAPI helper and creates the Windows installer:

```text
release/Aether-Launcher-2.7.0/Aether-Launcher-2.7.0-setup.exe
```

## Third-Party Brand Assets

The Terracotta logo is sourced from the official [burningtnt/Terracotta](https://github.com/burningtnt/Terracotta) repository (`build/linux/icon.png`) and is used to identify the integrated Terracotta service. Terracotta is licensed under AGPL-3.0.

## License

MIT
