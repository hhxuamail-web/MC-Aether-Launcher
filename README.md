[README.md](https://github.com/user-attachments/files/35550469/README.md)
# Aether Launcher 1.0.0

**First official public release**

Aether Launcher is a Windows launcher for Minecraft Java Edition focused on reliable downloads, straightforward instance maintenance, unified content discovery, and practical server tools. Version 1.0.0 consolidates all previous test-stage work into the first official release.

> Version note: all earlier builds carrying 1.x or 2.x numbers were internal test builds. The official release line starts here with **1.0.0**.

## Highlights

### Verified downloads

- One inventory covers the client, libraries, natives, asset index, assets, and logging configuration.
- Exact size and SHA-1 verification are required before a file is reused.
- The complete inventory is audited before an instance is committed.
- Concurrent file downloads, pause/resume, cancellation, retries, and source fallback.
- BMCLAPI, Mojang, domestic Maven, and official Maven fallback chains.
- BMCLAPI libraries try both `/maven/` and `/libraries/`.
- Verified clients, libraries, assets, and Java runtimes are reused across instances.
- Automatic direct networking when the Windows system proxy fails.

### Instance management

- Vanilla, Fabric, Forge, and NeoForge.
- Multiple instances and concurrent downloads.
- Per-instance Java, memory, JVM arguments, language, and server launch settings.
- Export, import, clone, backup, restore, repair, health scan, and crash diagnosis.
- Automatic matching and preparation of Minecraft Java runtimes.

### Content centers

- PCL-inspired Mod Center with target-instance compatibility, filters, sorting, details, exact versions, and dependency planning.
- Modpack market with compatible versions, loaders, target runtime, and new-instance installation.
- Resource packs, shaders, and data packs with project details and exact file-version selection.

### Advanced toolbox

- **Safe Mode Launch**: temporarily removes mods and restores them after game exit.
- **World Time Machine**: backs up all worlds and restores individual snapshots with a safety backup.
- **One-Click Server Forge**: downloads the official server, prepares Java, EULA, server properties, and launch files.
- Online/offline server authentication switching.
- Java scanning, server ping, log diagnosis, instance health analysis, and seed maps.

### Interface

- 16 themes with theme-aware cards and controls.
- Microsoft YaHei for UI text and Times New Roman for decorative English labels.
- Redesigned instance library, download center, and task list.
- Fully collapsible task window while background work continues in Download List.
- The title bar displays the actual release version.

## Installation

Requirements:

- Windows 10 or Windows 11 x64
- Internet access
- At least 4 GB RAM; 8 GB or more is recommended for large modpacks

1. Run `Aether-Launcher-1.0.0-setup.exe`.
2. Choose the installation directory.
3. Optionally clear old test-build user data; this is disabled by default.
4. Finish installation and launch Aether.

The installer does not bundle Minecraft clients, libraries, assets, Java, or Modrinth content. Game files are downloaded from external sources when an instance is installed.

## Quick start

1. Optionally sign in with Microsoft; otherwise use an offline profile.
2. Open **Download Center**, choose a Minecraft version and loader.
3. Select Java and memory, then add the installation to the Download List.
4. Launch the game from **Instances** after installation.
5. Use **Safe Mode Launch** when diagnosing mod conflicts.
6. Open **Toolbox → One-Click Server Forge** to create a local Java server.

## Server Forge

Choose the Minecraft version, server name, memory, port, and authentication mode, then accept the Minecraft EULA.

Common console commands:

```text
whitelist add <player>
whitelist reload
op <player>
save-all
stop
```

Always stop the server with `stop`; do not close the console window directly.

## Data locations

```text
%APPDATA%\Aether Launcher
```

- Settings and account data: `%APPDATA%\Aether Launcher`
- Game root: `%APPDATA%\Aether Launcher\game`
- Instances: `%APPDATA%\Aether Launcher\game\instances`
- Shared libraries: `%APPDATA%\Aether Launcher\game\libraries`
- Servers: `%APPDATA%\Aether Launcher\game\servers`
- Backups: `%APPDATA%\Aether Launcher\game\backups`

## Updates and uninstall

- The installer offers optional cleanup of old-version user data.
- Cleanup is disabled by default and requires a second confirmation.
- The standalone full-uninstall tool removes programs, instances, saves, mods, caches, account data, settings, shortcuts, and uninstall registry entries.
- The uninstall tool requests administrator permission and relocates itself to a temporary directory before removing the installation.

## Official release folder

```text
release/Aether-Launcher-1.0.0-official
```

It contains:

- `Aether-Launcher-1.0.0-setup.exe`
- `Aether-Launcher-1.0.0-full-uninstall.exe`
- English and Chinese README files
- English and Chinese 1.0.0 release notes
- `SHA256.txt`
- `UNINSTALLER-SHA256.txt`

## Verification

```powershell
npm run check
npm run test:catalog
npm run test:stable-download
npm run test:proxy-fallback
npm run test:advanced-tools
npm run dist
```

## Known limitations

- Windows x64 is the current target platform.
- The executable is unsigned; SmartScreen may require **More info → Run anyway**.
- Minecraft, BMCLAPI, Fabric, Forge, NeoForge, Modrinth, and Terracotta availability depends on third-party services.
- Public servers should use online authentication or a whitelist.

## Documentation

- [Download Center](DOWNLOAD-CENTER.md)
- [Mod Center](MOD-CENTER.md)
- [Advanced Toolbox](ADVANCED-TOOLS.md)
- [Full Uninstaller](UNINSTALL.md)
- [1.0.0 release notes](RELEASE-NOTES-1.0.0.md)

## License

MIT
