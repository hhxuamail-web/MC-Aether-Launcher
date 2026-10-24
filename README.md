[README.zh-CN.md](https://github.com/user-attachments/files/33903406/README.zh-CN.md)[README.md](https://github.com/user-attachments/files/33903404/README.md)
# MC-Aether-Launcher# Aether Launcher 1.0.0

A lightweight Windows Minecraft launcher focused on reliable game installation, mirror-aware downloads, and practical instance management.

## Highlights

- Download and install vanilla Minecraft releases and snapshots.
- Create Fabric and Forge instances through their official metadata and installers.
- Browse and install vanilla or Fabric modpacks from Modrinth.
- Automatic Java runtime matching with optional per-instance Java overrides.
- Official Mojang, BMCLAPI, and custom mirror download modes.
- Multi-source fallback, low-speed node recovery, HTTP Range resume, and SHA1 verification.
- Instance detail panel, launcher log viewer, quick folder access, and download-source latency testing.
- Static homepage artwork with a lightweight desktop UI.

## System Requirements

- Windows 10 or Windows 11, 64-bit.
- An internet connection for downloading game files or modpacks.
- No Node.js installation is required for the portable executable.
- Java is detected automatically. If no matching runtime is installed, the launcher can download the required Mojang runtime.

## Quick Start

1. Download `Aether-Launcher-1.0.0-portable.exe`.
2. Run the executable. It is currently unsigned, so Windows SmartScreen may display a warning. Select **More info** -> **Run anyway** only if you trust the build.
3. Open **Download Center**, select a Minecraft version, and choose Vanilla, Fabric, or Forge.
4. Set the instance name, memory allocation, and Java runtime.
5. Select **Download and Create**.
6. Open **Instances**, then select **Launch Game**.

## Download Sources

- **BMCLAPI Smart Mirror** (recommended for mainland China): version metadata and the client prefer BMCLAPI. Maven libraries use the official high-speed repository, and asset objects use the Mojang asset CDN to avoid stalled mirror nodes.
- **Mojang Official**: all metadata and files use official endpoints.
- **Custom Mirror**: provide a base URL compatible with the BMCLAPI directory layout.

Failed downloads can automatically switch to another endpoint. Interrupted downloads resume from the existing `.part` file and completed files are verified with SHA1.

## Java Support

- The launcher selects the Java major version from the Minecraft version metadata.
- Java 8, 11, 17, 21, and 25 installations are detected automatically.
- Missing runtimes can be downloaded from Mojang.
- Each instance can use automatic matching or a manually selected Java executable.
- Java 24 and 25 receive the required `--sun-misc-unsafe-memory-access=allow` flag automatically.

## Supported Loaders

| Loader | Status |
| --- | --- |
| Vanilla | Supported |
| Fabric | Supported |
| Forge | Supported through the official Forge Installer |
| NeoForge | Not yet supported |
| Quilt | Not yet supported |

Fabric and Forge installation is verified against official metadata. Forge 1.21.4 and Forge 26.3 have been tested end to end.

## Data Directory

The default game directory is:

```text
%APPDATA%\Aether Launcher\game
```

Instances are stored in `game/instances/<instance-id>/`. Shared libraries, assets, Java runtimes, and version metadata are cached under the same `game` root.

## Known Limitations

- Authentication currently uses an offline profile; Microsoft account login is not implemented.
- Fabric Loader 0.20.1 currently has an upstream Java 25 compatibility issue with Minecraft 26.3. For modded 26.3, use Forge; for Fabric, use a Java 21-compatible release such as 1.21.4.
- Forge and Fabric instances use the official installation flow and may require network access to their respective Maven repositories.
- The portable executable is unsigned and may trigger SmartScreen.

## Build From Source

```powershell
npm install
npm run check
npm run dist
```

Output:

```text
dist/Aether-Launcher-1.0.0-portable.exe
```

## Verification

The following flows have been exercised on Windows:

- Minecraft 1.20.6 / 1.21.4 / 26.3 game file downloads.
- Vanilla, Fabric, and Forge instance launches.
- Automatic Java 21 and Java 25 selection.
- Per-instance Java override.
- Forge 1.21.4 and Forge 26.3 installation.
- Modrinth modpack search and index parsing.
- Interrupted-download resume, low-speed node failover, and SHA1 validation.
- Instance detail, log viewer, and download-source latency test.

## License

MIT

[Uploading# Aether Launcher 1.0.0

一个面向 Windows 的轻量 Minecraft 启动器，重点解决游戏实例下载、镜像线路不稳定和实例管理问题。

## 主要功能

- 下载并安装 Minecraft 正式版与快照。
- 通过官方元数据和安装器创建 Fabric、Forge 实例。
- 搜索并安装 Modrinth 原版 / Fabric 整合包。
- 按游戏版本自动匹配 Java，也可为每个实例单独指定 Java。
- 支持 Mojang 官方、BMCLAPI 智能镜像和自定义镜像。
- 支持多线路回退、低速节点重连、HTTP Range 断点续传和 SHA1 校验。
- 提供实例详情、启动日志、模组 / 存档 / 日志目录快捷入口和下载源测速。
- 首页使用静态主视觉，界面轻量、启动开销低。

## 系统要求

- Windows 10 或 Windows 11，64 位。
- 下载游戏或整合包时需要联网。
- 便携版 EXE 不需要安装 Node.js。
- 启动器会自动检测 Java；缺少匹配运行时时可自动下载 Mojang 官方运行时。

## 快速开始

1. 下载 `Aether-Launcher-1.0.0-portable.exe`。
2. 双击运行。当前版本未签名，Windows SmartScreen 可能提示风险；确认信任后选择“更多信息 → 仍要运行”。
3. 打开“下载中心”，选择 Minecraft 版本，并选择原版、Fabric 或 Forge。
4. 设置实例名称、内存和 Java 运行时。
5. 点击“下载并创建”。
6. 打开“实例库”，点击“启动游戏”。

## 下载源说明

- **BMCLAPI 智能镜像**（推荐）：版本元数据和客户端优先使用 BMCLAPI；Maven 运行库使用官方高速仓库；资源对象使用 Mojang 资源 CDN，避免 BMCLAPI 慢节点拖死整个下载。
- **Mojang 官方**：所有元数据和文件均使用官方地址。
- **自定义镜像**：填写兼容 BMCLAPI 目录结构的镜像基础地址。

下载失败后会自动尝试其他线路。中断的下载会从已有 `.part` 文件继续，并在完成后执行 SHA1 校验。

## Java 支持

- 根据 Minecraft 版本元数据自动选择 Java 主版本。
- 自动检测 Java 8、11、17、21、25。
- 缺少运行时可从 Mojang 下载。
- 每个实例可选择“自动匹配”或手动指定 Java。
- Java 24/25 会自动附加 `--sun-misc-unsafe-memory-access=allow` 参数。

## 加载器支持

| 加载器 | 状态 |
| --- | --- |
| 原版 | 支持 |
| Fabric | 支持 |
| Forge | 支持，使用官方 Forge Installer |
| NeoForge | 暂未支持 |
| Quilt | 暂未支持 |

Fabric 和 Forge 安装均通过官方元数据完成。Forge 1.21.4 与 Forge 26.3 已完成端到端测试。

## 数据目录

默认游戏目录：

```text
%APPDATA%\Aether Launcher\game
```

实例位于 `game/instances/<实例 ID>/`。公共运行库、资源、Java 运行时和版本元数据会缓存到同一个 `game` 根目录。

## 已知限制

- 当前使用离线玩家档案，尚未接入 Microsoft 账户登录。
- Fabric Loader 0.20.1 在 Minecraft 26.3 的 Java 25 环境下存在上游兼容问题。26.3 建议使用 Forge；需要 Fabric 时请使用 Java 21 可兼容的版本，例如 1.21.4。
- Forge 与 Fabric 安装依赖对应 Maven 仓库的联网访问。
- 便携版未进行数字签名，可能出现 SmartScreen 提示。

## 从源码构建

```powershell
npm install
npm run check
npm run dist
```

输出文件：

```text
dist/Aether-Launcher-1.0.0-portable.exe
```

## 验证情况

以下流程已在 Windows 上完成验证：

- Minecraft 1.20.6 / 1.21.4 / 26.3 游戏文件下载。
- 原版、Fabric、Forge 实例启动。
- Java 21、Java 25 自动选择和实例级 Java 覆盖。
- Forge 1.21.4、Forge 26.3 安装与启动。
- Modrinth 整合包搜索与索引解析。
- 断点续传、低速节点切换和 SHA1 校验。
- 实例详情、日志查看和下载源延迟测试。

## 许可证

MIT
 README.zh-CN.md…]()
