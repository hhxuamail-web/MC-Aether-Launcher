[SHA256.txt](https://github.com/user-attachments/files/34192581/SHA256.txt)[README.md](https://github.com/user-attachments/files/34192573/README.md)
# Aether Launcher 2.1.0

Aether Launcher is a Windows Minecraft launcher focused on reliable game downloads, modern instance management, Fabric/Forge/NeoForge support, Modrinth content, and flexible Java controls.

Version **2.1.0** is the latest Aether 2.x feature release and introduces a redesigned interface, sixteen persistent themes, NeoForge support, and paginated Modrinth browsing.


## New in 2.1.0: Instance Safety Tools

- **Instance export/import**: package and move an instance as an `.aetherpack` archive, including mods, saves, configuration, resource packs, and shader packs while excluding logs and Natives.
- **Backup and restore**: create timestamped `.aetherbackup` snapshots, browse them from the launcher, and restore safely with temporary-directory swapping.
- **Crash diagnosis**: automatically analyze non-zero game exits and open an actionable report for memory, Java/JNI, JVM arguments, mod conflicts, graphics, permissions, and missing dependencies.

Archives use path traversal checks and a 16 GB extraction limit. Instance backups are stored under the game root in `backups`.

## What's New in 2.1.0

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

1. Run `Aether-Launcher-2.1.0-portable.exe`.
2. Optionally sign in with Microsoft.
3. Open **Download Center**, pick a Minecraft version and loader, then select loader version, Java, and memory.
4. Select **Download and Create**.
5. Open **Instances** and select **Launch Game**.

The executable is unsigned. Windows SmartScreen may require **More info -> Run anyway** if you trust the build.

## Verification

The following v2.1.0 flows were exercised on Windows:

- NeoForge 26.3.0.99 installer completion after prefetching all 30 installer dependencies.
- NeoForge 26.3 instance creation and successful game launch with Java 25.
- Minecraft 26.3 + NeoForge Modrinth search returning 5,000+ compatible mods.
- Sodium installation into a NeoForge 26.3 instance and verification of the resulting JAR.
- Live Modpack Market pagination showing 19,000+ results and switching to page 2.
- Live Mod Library pagination showing 13,000+ mods and switching to page 2.
- All sixteen themes rendering with theme-specific unselected states.
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

The build compiles the Windows DPAPI helper and outputs:

```text
dist/Aether-Launcher-2.1.0-portable.exe
```

## License

MIT

[README.zh-CN.md](https://github.com/user-attachments/files/34192577/README.zh-CN.md)
# Aether Launcher 2.1.0

Aether Launcher 是一款面向 Windows 的 Minecraft 启动器，重点提供可靠下载、现代化实例管理、Fabric/Forge/NeoForge 支持、Modrinth 内容浏览和灵活 Java 控制。

**2.1.0** 是 Aether 2.x 的最新功能版本，带来全新界面、16 套主题、NeoForge 支持和模组/整合包分页。


## 2.1.0 新增：三项实例安全工具

- **实例导入/导出**：使用 `.aetherpack` 打包和迁移实例，包含模组、存档、配置、资源包和光影包，排除日志与 Natives。
- **实例备份/恢复**：创建带时间戳的 `.aetherbackup` 快照，在启动器内浏览并恢复，恢复过程使用临时目录安全替换。
- **游戏崩溃诊断**：游戏异常退出后自动分析日志，针对内存、Java/JNI、JVM 参数、模组冲突、图形驱动、权限和依赖缺失给出报告。

实例包包含路径穿越校验和 16 GB 解压上限。备份默认保存在游戏根目录的 `backups` 文件夹。

## 2.1.0 新增内容

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

1. 运行 `Aether-Launcher-2.1.0-portable.exe`。
2. 可选：使用 Microsoft 登录。
3. 打开 **下载中心**，选择 Minecraft 版本、加载器、加载器版本、Java 与内存。
4. 点击 **下载并创建**。
5. 打开 **实例库**，点击 **启动游戏**。

便携版 EXE 未进行代码签名。若你信任此构建，Windows SmartScreen 可能需要点击 **更多信息 -> 仍要运行**。

## 验证情况

以下 v2.1.0 流程已在 Windows 上实际验证：

- 预取全部 30 个依赖后，NeoForge 26.3.0.99 官方安装器成功完成。
- NeoForge 26.3 实例成功创建，并使用 Java 25 进入游戏。
- Minecraft 26.3 + NeoForge 的 Modrinth 模组搜索返回 5,000+ 个兼容结果。
- Sodium 成功安装到 NeoForge 26.3 实例，并确认生成对应 JAR。
- 整合包市场在线分页显示 19,000+ 个结果，并可切换到第 2 页。
- 模组库在线分页显示 13,000+ 个模组，并可切换到第 2 页。
- 16 套主题均正确渲染，未选中主题卡片保留自身主题色。
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

构建过程会编译 Windows DPAPI 辅助程序，并输出：

```text
dist/Aether-Launcher-2.1.0-portable.exe
```

## 许可证

MIT

[UploadinSHA256: 0F9D7D6EC268DC9F63AE9AC5BF1F9535E38611CA15E9141B47ABE2354A4CE43D
g SHA256.txt…]()

