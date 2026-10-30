[README.md](https://github.com/user-attachments/files/34193607/README.md)
# Aether Launcher 2.1.1

Aether Launcher is a Windows Minecraft launcher focused on reliable game downloads, modern instance management, Fabric/Forge/NeoForge support, Modrinth content, and flexible Java controls.

Version **2.1.1** is the latest Aether 2.x feature release and introduces a redesigned interface, sixteen persistent themes, NeoForge support, and paginated Modrinth browsing.


## New in 2.1.1: Instance Safety Tools

- **Instance export/import**: package and move an instance as an `.aetherpack` archive, including mods, saves, configuration, resource packs, and shader packs while excluding logs and Natives.
- **Backup and restore**: create timestamped `.aetherbackup` snapshots, browse them from the launcher, and restore safely with temporary-directory swapping.
- **Crash diagnosis**: automatically analyze non-zero game exits and open an actionable report for memory, Java/JNI, JVM arguments, mod conflicts, graphics, permissions, and missing dependencies.

Archives use path traversal checks and a 16 GB extraction limit. Instance backups are stored under the game root in `backups`.

## What's New in 2.1.1

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

1. Run `Aether-Launcher-2.1.1-portable.exe`.
2. Optionally sign in with Microsoft.
3. Open **Download Center**, pick a Minecraft version and loader, then select loader version, Java, and memory.
4. Select **Download and Create**.
5. Open **Instances** and select **Launch Game**.

The executable is unsigned. Windows SmartScreen may require **More info -> Run anyway** if you trust the build.

## Verification

The following v2.1.1 flows were exercised on Windows:

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
dist/Aether-Launcher-2.1.1-portable.exe
```

## License

MIT
[README.zh-CN.md](https://github.com/user-attachments/files/34193613/README.zh-CN.md)
