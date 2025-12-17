---
order: 50
icon: package
expanded: true
route: /installation/
---

# Installation

Follow the installation guide for your platform:

* [Windows](/Installation/Windows.md)
* [Linux and Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Branches

SillyTavern is being developed using a two-branch system to ensure a smooth experience for all users.

* `release` -🌟 **Recommended for most users.** This is the most stable and recommended branch, updated only when major releases are pushed. It's suitable for the majority of users. Typically updated once a month.
* `staging` - ⚠️ **Not recommended for casual use.** This branch has the latest features, but be cautious as it may break at any time. Only for power users and enthusiasts. Updates several times daily.

## Global / Standalone mode

There are two modes of running SillyTavern that differ in how they handle the configuration and data paths.

* **Standalone mode** (default) - uses the `config.yaml` file and `data` directory in the server directory. All data will be constrained to the installation path. This is the recommended mode for most users.
* **Global mode** - uses the system-wide paths for configuration and data. This is useful for installing SillyTavern as a package or when you want to share the same configuration and data across multiple installations.

!!!info
Installations made by using the [official npm package](https://www.npmjs.com/package/sillytavern) (e.g. `npx sillytavern@latest`) will run in global mode by default.
!!!

### Data paths

**Standalone mode** paths are relative to the SillyTavern installation directory:

* **Config path**: `./config.yaml`
* **Data root**: `./data/`

**Global mode** paths are OS-dependent:

* **Linux**: `~/.local/share/SillyTavern/config.yaml` (or `$XDG_DATA_HOME/SillyTavern/config.yaml`) and `~/.local/share/SillyTavern/data/` (or `$XDG_DATA_HOME/SillyTavern/data/`)
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` and `%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` and `~/Library/Application Support/SillyTavern/data/`

### How to run in global mode

!!!warning
`dataRoot` and `configPath` can't be overridden with [CLI arguments](../Administration/config-yaml.md#command-line-arguments) or [config.yaml](../Administration/config-yaml.md) when running in global mode.
!!!

1. Pass the `--global` argument to the server startup command (e.g. `node server.js --global`).
2. Pass the `--global` argument to the shell startup script (e.g. `Start.bat --global` or `./start.sh --global`).
3. Use the `start:global` script in the `package.json` file (e.g. `npm run start:global`).
