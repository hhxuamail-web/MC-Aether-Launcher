[README.zh-CN.md](https://github.com/user-attachments/files/34517950/README.zh-CN.md)[README.md](https://github.com/user-attachments/files/34517945/README.md)
# Aether Launcher 2.6.0

Aether Launcher is a Windows Minecraft launcher focused on reliable game downloads, modern instance management, Fabric/Forge/NeoForge support, Modrinth content, and flexible Java controls.

Version **2.6.0** adds concurrent multi-version downloading and fixes snapshot/legacy version stalls at the early metadata stages.

## New in 2.6.0

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
- The separate Download List window provides a live queue for all running and completed tasks.
- The queue lists every concurrent install task with stage, progress, size, speed, and failure state.
- Creation buttons re-arm immediately after a task is submitted, allowing multiple Minecraft versions to download at the same time.
- Finished tasks remain visible until the user selects “Clear finished”.
- Completed and failed tasks now reject late progress events, preventing a finished task from falling back to an early percentage.
- Instance creation now returns its backend task ID, and the renderer finalizes that exact task when the creation promise resolves, even if a completion IPC event is delayed or missed.
- The top completion toast and the Download List therefore share the same completion state instead of diverging.
- Download details separate total progress, real network bytes, cache-hit bytes, smoothed network speed, elapsed time, ETA, source, and a live stage timeline.
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

- **BMCLAPI Smart Mirror** is recommended for mainland China. Metadata and client files prefer BMCLAPI, while Maven and asset downloads use optimized routes.
- **Mojang Official** uses official metadata and file endpoints.
- **Custom Mirror** accepts a base URL compatible with the BMCLAPI directory layout.

Downloads use multi-source retry, low-speed detection, HTTP Range resume, and SHA1 verification. NeoForge installer dependencies are pre-downloaded with the same resilient pipeline before the official installer runs.

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

1. Run `Aether-Launcher-2.6.0-setup.exe` and follow the installer.
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
- All 922 versions in the official Java version manifest passed client/library/asset-index and launch-argument preflight checks.
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
release/Aether-Launcher-2.6.0/Aether-Launcher-2.6.0-setup.exe
```

## Third-Party Brand Assets

The Terracotta logo is sourced from the official [burningtnt/Terracotta](https://github.com/burningtnt/Terracotta) repository (`build/linux/icon.png`) and is used to identify the integrated Terracotta service. Terracotta is licensed under AGPL-3.0.

## License

MIT

[Uploading README.zh-CN.md…# Aether Launcher 2.6.0

Aether Launcher 是一款面向 Windows 的 Minecraft 启动器，重点提供可靠下载、现代化实例管理、Fabric/Forge/NeoForge 支持、Modrinth 内容浏览和灵活 Java 控制。

**2.6.0** 新增多版本并行下载队列，并修复快照与旧版本在早期元数据阶段卡住的问题。

## 2.6.0 新增

- 实例库重做为实例资源工作区，包含汇总统计、搜索、类型筛选、排序、紧凑单实例操作和行式实例列表。
- 新增全部实例、运行中实例、加载器实例和累计游玩时长统计。
- 游戏版本下载重做为三步向导：版本与加载器、运行环境、最终确认。
- 删除版本目录右侧常驻的创建实例配置栏。
- 每个版本行提供直接下载按钮，并进入同一套向导流程。
- 游戏版本页重做为整页版本目录，每个版本提供独立的“下载”操作。
- 加载器、内存、Java、下载源和可选模组配置移入独立的“下载实例”向导。
- 新增独立的 **“下载列表”** 按钮和窗口，不再占用版本浏览区域。
- Minecraft 客户端、运行库、资源、日志配置和加载器元数据会直接从 Mojang 或所选镜像下载到目标实例中。
- 游戏版本下载不再依赖选择本地压缩包或本地游戏版本文件。
- 删除版本目录右侧常驻的创建实例表单。
- 独立下载列表窗口会实时显示全部运行中和已结束的任务。
- 队列会同时列出多个安装任务，并显示阶段、进度、大小、速度和失败状态。
- 提交下载后创建按钮会立即恢复可用，可继续选择并启动其他 Minecraft 版本。
- 已结束任务会保留在列表中，直到用户点击“清除已结束”。
- 已完成和失败任务会拒绝之后到达的旧进度事件，避免下载完成后又从 100% 退回 10%。
- 实例创建结果现在会返回对应的后端任务 ID；只要创建 Promise 成功返回，界面就会同步完成该任务，即使完成 IPC 事件延迟或丢失。
- 顶部完成提示和下方下载列表因此使用同一完成状态，不会出现一处完成、一处仍在下载。
- 下载详情会分别显示总进度、真实网络字节、缓存命中字节、平滑网络速度、已用时间、预计剩余、下载来源和实时阶段时间线。
- 有 SHA-1 时会对本地缓存做完整性校验，损坏缓存会被删除并重新下载。
- 网络速度改用平滑估计，不再显示并发下载造成的瞬时异常峰值。
- 详情中可以确认数据来自 Mojang、自定义镜像，还是 BMCLAPI/Aliyun 回退链。
- 每个并行任务使用独立任务 ID，不再互相覆盖进度和阶段。
- 多个任务需要同一运行库、资源或客户端文件时，会按目标路径合并下载，避免重复请求和 `.part` 文件冲突。
- 快照和非最新版会明确从下载计划进入客户端准备与客户端下载阶段。
- 资源索引阶段会持续显示来源与尝试状态，不再无提示停留在原来的 7%–9%。
- 只提供原生分类文件的旧版运行库不再被错误拼接成 `jinput-platform-2.0.5.jar`、`twitch-platform-5.16.jar` 等不存在的地址。
- 原生分类文件会按目标路径去重，避免同一组件重复下载。
- 运行库去重会先排除当前系统不兼容的 OS 专用版本，再应用加载器覆盖，避免 macOS 专用 LWJGL nightly 删除 Windows 和通用平台所需的 LWJGL。
- 修复 Minecraft 1.13 以下常见的 `NoClassDefFoundError: org/lwjgl/opengl/OpenGLException`，包括 1.8.1-pre3。
- Windows 游戏进程现在改用同目录的 `javaw.exe` 启动，并关闭隐藏窗口继承；修复旧版 LWJGL 2 实例虽然进程仍在运行、却没有显示 Minecraft 窗口的问题。
- 旧版版本清单现在会正确传入 `--userProperties {}`，避免 1.7.10 等客户端把后一个 `--userType` 参数当作 JSON 解析，并因 `Expected BEGIN_OBJECT but was STRING` 退出。
- 语言代码现在会按所选客户端 JAR 中的真实大小写写入。1.5.2 等旧版使用 `lang:en_US`，现代版继续使用 `lang:en_us`，避免启动时语言流为空导致空指针崩溃。
- 旧版 `map_to_resources` 资源索引会在启动前正确展开，修复 Minecraft 1.3.2 因语言与字体资源为空而发生的主菜单崩溃。
- 生成 native 下载计划前会先合并重复运行库清单，修复 Minecraft 1.13.2 等版本只下载 LWJGL 主 JAR、却漏掉对应 native JAR 的问题。
- Windows 本地库会解压到纯 ASCII 临时目录，并在游戏退出后清理，避免启动器或游戏目录包含中文时 Java 8 报 `UnsatisfiedLinkError`。
- Minecraft 客户端、运行库、资源、日志配置、Java 运行时和 Forge 安装器全部改为镜像优先下载，镜像异常时才回退官方源；安装包不内置任何 Minecraft 游戏文件。
- 实例库改为卡片工作区，右侧集中显示最近实例、收藏世界和游玩时长；实例维护工具保持在页面顶部。
- 下载列表改为紧凑任务卡，默认显示标题、阶段、状态和进度，点击后展开网络、缓存、大小和阶段细则。
- 所有后台操作现在都会返回精确任务 ID，操作成功返回时立即将对应任务置为完成；实例修复、模组、资源包、光影、整合包、Java 和实例任务不会再被晚到的旧进度覆盖。
- 任务中心现在显示所有后台任务，不再只筛选游戏下载任务。
- 后台任务成功达到 100% 后会继续显示 5 秒，然后自动关闭；如果还有别的任务正在运行，则切换到仍在执行的任务，不会误关进度窗口。
- 已删除侧边栏中较大的 AETHER LABS 宣传卡片和竖排版本装饰，导航下方直接显示紧凑的账户入口。
- JSON 元数据请求增加硬超时、来源状态提示和回退进度，即使底层请求一直不返回也不会无限卡住。
- 损坏或空的资源索引缓存会被自动丢弃，并通过临时文件安全重写。
- 已删除未完成的“实例版本变迁”入口、界面、IPC、状态和相关说明。

## 2.5.7 新增

- 单次体检最短动画时间从 5 秒缩短为 3 秒。
- 五个分析阶段和确定进度条会在更短、更顺畅的序列中完成。
- 确认安全清理后，体检窗口会立即切换到单独的清理动画。
- 清理过程中显示旋转垃圾桶图标、条纹动态进度、阶段文字和百分比。
- 即使文件删除很快，清理动画也会至少显示 0.9 秒，确保用户能看到操作反馈。
- 清理完成后直接刷新体检报告，不再额外播放一次 3 秒扫描动画。

## 2.5.6 新增

- 每次体检都会播放至少 5 秒的主题化扫描动画，即使文件分析提前完成也不会跳过。
- 扫描分为读取实例、分析空间、读取模组元数据、检查依赖冲突和生成建议五个阶段。
- 加入环形动画、确定进度条、百分比和动态阶段提示，让扫描过程清晰可见。
- 扫描期间自动锁定实例选择框和开始按钮，避免重复触发。
- 安全清理确认框现在会立即显示在体检窗口上方，不再藏在后面等待关闭窗口。
- 确认对话框统一使用更高的独立层级。

## 2.5.5 新增

- 工具箱新增整行“实例体检与空间分析”功能版块。
- 计算实例健康分，并统计模组、存档、资源包、光影、日志、配置、运行库、备份和其他文件的占用。
- 从已安装 JAR 中读取 Fabric `fabric.mod.json`、Forge/NeoForge 模组 TOML 以及旧版 `mcmod.info` 元数据。
- 检测重复的启用模组 ID、缺失的必需前置、无法读取的 JAR、停用模组，以及原版实例中的模组。
- 显示体积最大的文件与目录，方便判断清理方向。
- 查找未完成的 `.part`、`.tmp`、`.download` 文件和过大的启动器日志。
- 安全清理只删除临时下载与启动器生成的日志，不会删除存档、模组、资源包、光影或配置。
- 对不支持的元数据或无法解析的 JAR 给出提示，不主动删除用户文件。

## 2.5.4 新增

- 陶瓦联机界面的完整风格现在会跟随 Aether 的 16 套强调色主题，不再固定使用粉色。
- 氛围背景、卡片、按钮、房间码、状态标签、加载动画、成员头像和高亮边框全部使用当前主题变量。
- 从官方 `burningtnt/Terracotta` 仓库加入陶瓦方块 Logo。
- Logo 会显示在多人游戏服务标识、启动卡片、页脚和使用教程中。
- 新增内置陶瓦联机教程，包含房主与加入方四步操作、首次下载说明、防火墙建议和故障排查。
- 教程可以从多人游戏页顶部和启动卡片直接打开。

## 2.5.3 新增

- 创建实例弹窗增加明确的“正式版 / 快照”切换。
- 将本地 JAR 选择器替换为直接连接 Modrinth 的模组库。
- 模组库只搜索与当前 Minecraft 版本及 Fabric、Forge、NeoForge 加载器兼容的内容。
- 可在创建前搜索、多选、移除并检查随实例安装的模组。
- 实例创建完成后，通过现有的哈希校验安装器自动下载所选模组及必需前置依赖。
- 切换 Minecraft 版本或加载器时会清除不再兼容的模组选择。
- 删除未完成的红石联机 2 入口、占位配置页、模拟节点选项和无法使用的房间按钮。
- 多人游戏页面改为固定显示陶瓦联机服务标识，不再使用原生联机方式下拉框。

## 2.5.2 新增

- 创建实例弹窗在验证通过后立即关闭，下载与安装继续显示在底部任务栏。
- Minecraft 版本选择按正式版、快照和历史版本分组，不再只显示正式版。
- Fabric、Forge 与 NeoForge 实例可在创建弹窗中直接选择最多 100 个本地模组 JAR。
- 实例创建完成后，所选模组会自动复制到新实例的 `mods/` 目录，并在完成提示中显示导入数量。
- 原版实例会明确禁用本地模组选择，因为原版客户端不能加载模组。

## 2.5.1 新增

- 新下载的实例直接使用可读的实例名称作为文件夹名称，不再使用难以识别的 UUID。
- 默认名称 `Minecraft 1.21.4` 会在实例目录中创建 `Minecraft 1.21.4` 文件夹。
- 同名实例会自动添加 ` (2)` 等数字后缀。
- 自动处理 Windows 不允许的字符、系统保留名称以及末尾空格和句点。
- 克隆与导入的实例同样使用可读目录名。
- 已有的 UUID 实例目录继续兼容，不会在升级时自动移动。

## 2.5.0 新增

- 新增 Modrinth 资源包中心，支持按实例版本筛选、搜索、分页、画廊预览，并可安装到任意 Minecraft 实例。
- 新增 Modrinth 光影中心，提供相同的兼容筛选与预览流程，并把光影 ZIP 安装到 `shaderpacks/`。
- 资源包与光影下载会校验 Modrinth SHA-1 和文件大小，通过后才进入实例。
- 新增实例服务器列表，直接读写 Minecraft 使用的 gzip 压缩 `servers.dat`。
- 可在启动器内添加、按地址更新名称、检测状态、启动连接或删除服务器。
- 编辑已有列表时会保留服务器图标、资源包提示偏好和隐藏状态。
- 实例运行时禁止修改服务器列表，避免 Minecraft 退出后覆盖本次修改。
- 两个内容中心均支持原版、Fabric、Forge 和 NeoForge 实例。

## 2.4.0 新增

- 新增 Java 运行环境检测，可查看本机 Java 主版本、来源和可执行文件路径。
- 新增 Minecraft Java 服务器状态检测，可读取 MOTD、在线人数、服务端版本、服务器图标和实际延迟。
- 新增启动日志诊断入口，可选择任意实例并分析最近一次启动日志。
- 种子地图继续作为工具箱的第四项功能保留。

## 2.3.2 新增

- 启动设置中新增游戏语言选择器。
- 语言列表直接从当前 Minecraft 版本已安装的资源索引读取，覆盖该版本提供的全部官方语言。
- 显示语言原生名称、地区和 Minecraft 语言代码。
- 所选语言按实例保存，并在启动前写入该实例的 `options.txt`。
- 可和 Java、内存、JVM 参数一起保存为该实例的默认启动选项。

## 2.3.1 修复

- 自动匹配现在严格按照所选 Minecraft 版本要求选择 Java 大版本。
- 全局 Java 与游戏要求不一致时会跳过它，选择本机兼容版本。
- 没有兼容 Java 时会自动下载 Mojang 对应的官方运行时。
- Java 24+ 专用 JVM 参数只在最终选中的 Java 确实为 24 或更新版本时添加。
- 手动选择不兼容的 Java 时会直接显示清晰错误，不再进入 JVM 启动失败流程。

## 2.3.0 更新

- 新增独立的多人游戏页面，并提供“服务器”和“联机房间”两个入口。
- 支持通过 Minecraft Quick Play 直接启动并进入服务器。
- 集成陶瓦联机创建房间、邀请码加入、实时成员状态、会话重置和备用直连地址。
- 首次使用自动下载陶瓦联机官方内核，并校验发布方 SHA-256。
- 新增陶瓦联机创建与加入房间流程。

## 2.2.2 更新

- 首页所有主要卡片使用统一的深色表面。
- 保留主题强调色，以及卡片内部按钮和列表项的独立交互层级。

## 2.2.1 新增

- 首页新增月历，可切换月份、选择日期，并查看当天的启动时间和游玩时长。
- 首页新增固定世界区域，可在世界管理中固定或取消固定，并在重启后继续保留。
- 每次启动和退出游戏都会写入本地游玩历史，用于后续日历统计。

## 2.2.0 修复

- 嵌入式种子地图的缩放按钮现在直接控制地图自身的缩放状态，不再缩放整个嵌入网页。
- 地图放大后可以可靠地缩回上一层级。
- 种子地图已重做为独立探索工作区，提供种子示例、随机种子、当前种子卡片、复制按钮以及明确的加载与错误状态。
- 普通模式与全屏模式都扩大了地图画布，并把来源、版本和视图控制集中在紧凑的侧边栏中。
- 修复了第三方缩放控件容器被拉伸到整张地图后产生的半透明遮罩。
- 探索设置新增群系查询系统，提供统一搜索框和可点选的群系列表，并自动把中文选项转换为对应群系名称。
- 每次进入工具箱都会清空上次种子、查询内容和地图搜索状态，离开工具箱时也会同步清理后台状态。


## 工具箱与嵌入式种子地图

- 侧边栏新增独立的 **工具箱** 区域，用于嵌入式种子地图和种子查询。
- 世界、模组更新、本地导入和实例修复已移动到 **实例库** 内的独立实例工具区域。
- Chunkbase 种子地图已直接嵌入启动器，并支持选择 Minecraft Java 版本。
- 新增启动器侧与地图内放大/缩小按钮，统一使用地图原生缩放操作。
- 新增软件内全屏地图模式，并接管第三方地图的“Expand Map”按钮。
- 启动器仍负责种子标准化，并将数字或文本种子传入嵌入式地图。
- 嵌入式页面运行在隔离的 Electron 访客环境中，限制页面跳转、禁用 Node 权限、拒绝权限申请并拦截广告网络。
- 第三方地图服务不可用时，仍可使用浏览器打开作为备用方案。

## 2.2.0 新增：五项功能工具

1. **模组更新检查**：通过 Modrinth SHA1 哈希识别本地 JAR，比较兼容版本，支持单个或全部更新，并保留停用状态。
2. **种子查询器**：从本地世界 `level.dat` 读取种子，支持数字或文本种子、Java 种子标准化，并可打开浏览器中的种子地图查询。
3. **实例修复**：重新校验并补齐原版、Fabric、Forge、NeoForge 实例文件，同时保留当前加载器与版本。
4. **本地模组批量导入**：一次导入多个本地 JAR，自动跳过相同文件，并对重名文件安全改名。
5. **世界存档管理**：列出本地世界、显示种子、打开存档目录，并创建带时间戳的 `.aetherworld` 备份。

### 模组更新检查详情

- 通过 SHA1 文件哈希在 Modrinth 识别已安装 JAR。
- 按照实例的 Minecraft 版本和加载器，比较最新兼容版本。
- 展示当前版本与最新版本，支持单个更新或一键更新全部可识别模组。
- 更新时保留模组停用状态；若替换失败会自动恢复旧 JAR。
- 显示无法识别或暂无兼容新版本的模组数量。

### 三项实例安全工具

- **实例导入/导出**：使用 `.aetherpack` 打包和迁移实例，包含模组、存档、配置、资源包和光影包，排除日志与 Natives。
- **实例备份/恢复**：创建带时间戳的 `.aetherbackup` 快照，在启动器内浏览并恢复，恢复过程使用临时目录安全替换。
- **游戏崩溃诊断**：游戏异常退出后自动分析日志，针对内存、Java/JNI、JVM 参数、模组冲突、图形驱动、权限和依赖缺失给出报告。

实例包包含路径穿越校验和 16 GB 解压上限。备份默认保存在游戏根目录的 `backups` 文件夹。

### Aether 2.x 界面与下载功能

- 对标题栏、侧边栏、首页主视觉、卡片、设置、弹窗和分页全面应用 Aether 2.x 视觉系统。
- 新增 16 套可持久化主题：极光绿、深海蓝、星云紫、樱花粉、薄荷青、落日橙、绯红、月白、松林绿、冰川蓝、熔岩橙、柠檬黄、云雾白、葡萄紫、珊瑚红、钢铁蓝。
- 未选中的主题卡片也会显示自身主题色，而不是统一灰色。
- 新增 NeoForge 加载器版本发现与实例创建。
- 新增 NeoForge 安装器依赖预取，不再依赖安装器内部 Java 进程直连 Maven。
- 新增 NeoForge 安装十分钟超时保护与明确错误日志。
- 新增 Modrinth NeoForge 模组搜索与安装。
- 新增 Modrinth `.mrpack` 整合包的 NeoForge 依赖识别。
- 模组库与整合包市场新增完整分页：总数、页码、上一页/下一页、当前页高亮、翻页后滚动到列表顶部。
- 保留高清等比整合包封面、完整详情画廊和缩略图轮播。
- 保留 Java 选择、内存、JVM 参数、微软登录、实例克隆、镜像回退、断点续传和 SHA1 校验。

## 主要功能

- 下载 Minecraft 正式版和快照。
- 通过 Quick Play 参数直接启动并进入多人游戏服务器。
- 在启动器内创建、加入陶瓦联机房间，查看邀请码、成员状态和备用直连地址。
- 创建原版、Fabric、Forge、NeoForge 实例。
- 创建实例时手动选择 Fabric、Forge 或 NeoForge 加载器版本。
- 搜索并安装与实例版本、加载器兼容的 Modrinth 模组，自动安装必需前置。
- 浏览并安装带原版、Fabric、Forge 或 NeoForge 依赖的 Modrinth 整合包。
- 高清预览模组与整合包画廊。
- 每次启动前选择 Java、内存和 JVM 参数。
- 自动检测 Java 8、11、17、21、25，或下载 Mojang 对应运行时。
- 支持微软设备代码登录和离线玩家档案。
- 克隆实例时无需重复下载公共版本文件。
- 支持 Mojang 官方、BMCLAPI 智能镜像和自定义镜像。

## 多人游戏中心

- **服务器**：选择实例、填写服务器地址后直接启动游戏；支持 Quick Play 的版本会自动进入服务器。
- **联机房间**：接入陶瓦联机官方本地 HTTP API，不需要跳转到第三方网页。
- 首次使用会自动下载陶瓦联机官方 Windows x64 发布包，校验发布方 SHA-256 后以隐藏集成模式启动。
- 支持创建房间、邀请码加入、成员列表、会话重置、停止服务以及复制备用直连地址。
- 陶瓦联机 API 只监听 `127.0.0.1`，邀请码和房间状态保留在本机陶瓦进程中。
- 启动器只展示能够实际创建或加入联机的服务。

## 下载源

- **BMCLAPI 智能镜像**：国内网络优先，元数据与客户端文件优先使用 BMCLAPI，Maven 与资源文件使用优化线路。
- **Mojang 官方**：使用 Mojang 官方元数据与文件地址。
- **自定义镜像**：填写兼容 BMCLAPI 目录结构的镜像基础地址。

下载过程支持多源重试、低速检测、HTTP Range 断点续传和 SHA1 校验。NeoForge 安装器依赖会在运行官方安装器前由启动器使用同一套下载管线预取。

## NeoForge 实现

NeoForge 不是占位配置，而是完整安装流程：

1. 解析 Minecraft 版本对应的 NeoForge 版本。
2. 下载官方 NeoForge 安装器。
3. 读取安装器内的 `version.json` 与 `install_profile.json`。
4. 把所需 Maven 依赖预下载到游戏 `libraries` 目录。
5. 在超时监控下运行官方 NeoForge 安装器。
6. 校验生成的 NeoForge 版本档案。
7. 创建可启动、可安装 NeoForge 模组的实例。

已验证 Minecraft 26.3 + NeoForge 26.3.0.99 + Java 25 的安装与进入游戏流程。

## 模组库与整合包

1. 打开 **下载中心 -> 模组库**。
2. 选择 Fabric、Forge 或 NeoForge 实例。
3. 搜索与当前版本和加载器兼容的内容。
4. 使用分页控件浏览更多结果。
5. 选择 **预览** 查看详情和画廊，或选择 **安装** 加入实例。
6. Modrinth 声明的必需前置依赖会自动解析并安装。

整合包市场支持原版、Fabric、Forge 与 NeoForge 整合包。Quilt 整合包暂不支持。

## Java 与加载器

- Java 可在全局设置、实例设置或每次启动时选择。
- 可通过文件选择器指定任意本地 Java。
- 每次启动可覆盖内存与 JVM 参数。
- 游戏配置需要 Java 24/25 时，会自动加入 `--sun-misc-unsafe-memory-access=allow`。

| 加载器 | 状态 |
| --- | --- |
| 原版 | 支持 |
| Fabric | 支持 |
| Forge | 通过官方 Forge 安装器支持 |
| NeoForge | 通过官方 NeoForge 安装器与依赖预取支持 |
| Quilt | 暂不支持 |

## 实例与游戏目录

默认路径：

- 启动器数据：`%APPDATA%\Aether Launcher`
- 游戏文件：`%APPDATA%\Aether Launcher\game`
- 实例目录：`%APPDATA%\Aether Launcher\game\instances`
- 公共运行库：`%APPDATA%\Aether Launcher\game\libraries`
- 微软账户令牌：`%APPDATA%\Aether Launcher\account.enc`

可在设置中修改游戏目录。实例与下载资源会保存在配置的游戏根目录中。

## 快速开始

1. 运行 `Aether-Launcher-2.6.0-setup.exe`，按安装向导完成安装。
2. 可选：使用 Microsoft 登录。
3. 打开 **下载中心**，选择 Minecraft 版本、加载器、加载器版本、Java 与内存。
4. 点击 **下载并创建**。
5. 打开 **实例库**，点击 **启动游戏**。

便携版 EXE 未进行代码签名。若你信任此构建，Windows SmartScreen 可能需要点击 **更多信息 -> 仍要运行**。

## 验证情况

以下 v2.2-v2.5 流程已在 Windows 上实际验证：

- Minecraft 1.21.4 的 Modrinth 搜索返回 18,000+ 个兼容资源包和 700+ 个光影。
- Faithful 与 Complementary Shaders 均成功识别出兼容 1.21.4 的 ZIP 文件。
- 已在隔离实例中验证资源包与光影的安装目录、下载大小和 SHA-1 哈希。
- `servers.dat` 已通过空列表、普通服务器、域名加端口和带图标条目四类往返测试。
- 已在隔离 Minecraft 实例中验证服务器添加、读取与删除。
- 已在隔离实例中验证可读目录创建、同名编号、非法字符处理和克隆目录命名。
- 已在打包应用中检查快照选择和扩展后的创建弹窗布局。
- 已验证创建实例中的 Modrinth 模组库能返回兼容结果并支持多选。
- 已从创建实例模组库实际下载 Sodium 到隔离的 Fabric 1.21.4 实例，并在验证后清理。
- 已检查移除红石选择器后的固定陶瓦联机服务标识。
- 已自动检查全部 16 套主题，每套均有独立主色、主题化背景、主题化主按钮，且 Logo 成功加载。
- 已检查使用教程弹窗和移动端响应式布局。
- 已在隔离实例中验证体检能识别重复模组 ID、缺失前置、损坏 JAR、停用模组、临时文件和可清理日志。
- 已验证安全清理只删除生成的临时文件与启动器日志，测试模组全部保留。
- 已检查默认窗口尺寸下的新工具卡片和完整体检报告布局。
- 自动计时测得单次体检最短耗时约 5.23 秒。
- 已验证体检窗口保持打开时，安全清理确认框会以 `z-index: 140` 立即显示在最上层。
- 自动计时测得 2.5.7 的缩短动画耗时约 3.23 秒。
- 已分别检查安全清理动画的开始、进度更新和完成状态。
- 已并行读取 `26.4-snapshot-7` 与 Minecraft `1.20.6` 的资源索引，分别载入 5,402 和 3,862 项资源。
- 已验证 Minecraft 1.2.1、1.5.2 与 1.7.6 不再请求不存在的原生基础 JAR，并能成功下载正确的 Windows 原生组件。
- 已验证任务完成后即使收到延迟的 10% 进度事件，仍会保持 100% 完成状态。
- 已验证即使任务存储中的进度仍为早期数值，只要状态成功，下载列表也会强制显示 100%。
- 已验证损坏的本地缓存会通过 SHA-1 检测并被正确文件替换。
- 已验证 Minecraft 1.5.2、1.7.6、1.8.1-pre3 与 1.12.2 的启动 classpath 均包含对应版本的 LWJGL 核心库和工具库。
- 官方 Java 版本清单中的全部 922 个版本均已通过客户端、运行库、资源索引和启动参数预检。
- 已使用镜像优先链路实际下载并启动 28 个跨时代版本，覆盖 `rd-132211`、Classic、Alpha、Beta、1.7.10、1.13.2、1.20.5、1.21.5、26.3 与 `26.4-snapshot-7`，全部进入游戏。
- 已重新启动此前实际崩溃的 Minecraft 1.5.2 实例，确认 `options.txt` 自动写为 `lang:en_US` 并成功进入游戏主菜单。
- 已实际执行一次 1.5.2 文件修复，确认返回的任务 ID 最终共享状态为 `success / 100% / 完成`，与顶部完成提示一致。
- 已验证两个并行任务下载同一目标文件时只会发起一次网络请求。
- 已检查多任务“正在下载”队列及同时运行的任务卡片。
- 已检查新版实例资源库、三步下载向导和最终确认页在默认窗口下的布局。
- 修复后 Minecraft 1.2.1 旧版资源索引约 0.5 秒完成。
- 预取全部 30 个依赖后，NeoForge 26.3.0.99 官方安装器成功完成。
- NeoForge 26.3 实例成功创建，并使用 Java 25 进入游戏。
- Minecraft 26.3 + NeoForge 的 Modrinth 模组搜索返回 5,000+ 个兼容结果。
- Sodium 成功安装到 NeoForge 26.3 实例，并确认生成对应 JAR。
- 整合包市场在线分页显示 19,000+ 个结果，并可切换到第 2 页。
- 模组库在线分页显示 13,000+ 个模组，并可切换到第 2 页。
- 16 套主题均正确渲染，未选中主题卡片保留自身主题色。
- 已验证 Chunkbase 地图可在启动器内嵌入加载，并支持种子与 Java 版本选择。
- 使用真实旧版 Sodium JAR 在 NeoForge 26.3 实例上验证模组更新检测与替换。
- 使用合成的 NBT 世界验证种子提取与世界备份。
- 已验证本地模组重复跳过和重名安全改名。
- 已验证原版与 Fabric 实例修复会保留原加载器配置。
- 实例导出/导入往返测试通过，模组与存档完整恢复且日志被排除。
- 备份创建、列表读取、安全替换和文件恢复均已验证。
- 崩溃诊断已针对内存不足、JVM/JNI 和普通异常退出日志进行验证。
- 主进程、预加载脚本和渲染器均通过语法检查。

## 从源码构建

```powershell
npm install
npm run check
npm run dist
```

构建过程会编译 Windows DPAPI 辅助程序，并生成 Windows 安装包：

```text
release/Aether-Launcher-2.6.0/Aether-Launcher-2.6.0-setup.exe
```

## 第三方品牌资源

陶瓦联机 Logo 来自官方 [burningtnt/Terracotta](https://github.com/burningtnt/Terracotta) 仓库的 `build/linux/icon.png`，用于标识集成服务。Terracotta 使用 AGPL-3.0 许可证。

## 许可证

MIT
]()

