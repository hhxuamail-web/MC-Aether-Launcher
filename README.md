[README.md](https://github.com/user-attachments/files/34492454/README.md)
# Aether Launcher 2.5.3

Aether Launcher is a Windows Minecraft launcher focused on reliable game downloads, modern instance management, Fabric/Forge/NeoForge support, Modrinth content, and flexible Java controls.

Version **2.5.3** connects instance creation directly to the Modrinth mod library and removes the unfinished Redstone multiplayer placeholder.

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

1. Run `Aether-Launcher-2.5.3-setup.exe` and follow the installer.
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
release/Aether-Launcher-2.5.3/Aether-Launcher-2.5.3-setup.exe
```

## License

MIT

[README.zh-CN.md](https://github.com/user-attachments/files/34492458/README.zh-CN.md)
# Aether Launcher 2.5.3

Aether Launcher 是一款面向 Windows 的 Minecraft 启动器，重点提供可靠下载、现代化实例管理、Fabric/Forge/NeoForge 支持、Modrinth 内容浏览和灵活 Java 控制。

**2.5.3** 将创建实例流程直接连接到 Modrinth 模组库，并移除尚未完成的红石联机占位功能。

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

1. 运行 `Aether-Launcher-2.5.3-setup.exe`，按安装向导完成安装。
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
release/Aether-Launcher-2.5.3/Aether-Launcher-2.5.3-setup.exe
```

## 许可证

MIT
