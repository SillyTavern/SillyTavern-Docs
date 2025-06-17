# How to update Node.js

It's important to keep your Node.js runtime up to date for security and performance reasons. Below are the steps to update Node.js depending on your operating system.

We recommend using the latest Long Term Support (LTS) version, which you can find on the [Node.js official website](https://nodejs.org/en/about/previous-releases).

## How to check your current Node.js version

1. Open your terminal or command prompt.
2. Type the following command and press Enter:

```bash
node -v
```

## nvm (Node Version Manager) - Cross-Platform

If you are using `nvm`:

1. Open your terminal.
2. Type in the following command:

[**Unix/Linux/macOS:**](https://github.com/nvm-sh/nvm)

```bash
nvm install --lts
nvm use --lts
```

[**Windows:**](https://github.com/coreybutler/nvm-windows)

```bash
nvm install lts
nvm use lts
```

## Windows - Regular Installation

1. Go to the Node.js [download page](https://nodejs.org/en/download/).
2. Download the Windows Installer for the LTS version.
3. Run the installer and follow the prompts to complete the installation.

## Windows - SillyTavern Launcher

If you're have installed using the SillyTavern Launcher:

1. Open the SillyTavern Launcher.
2. Navigate to `Toolbox / App Installer / Core Utilities / Install Node.js`.

**OR:**

Do it manually using winget in PowerShell:

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. Open the Termux app.
2. Type the following commands:

```bash
pkg update
pkg upgrade nodejs-lts
```

Don't forget to accept any prompts that may appear during the update process by pressing `Y` on the virtual keyboard.

## macOS - Regular Installation

1. Go to the Node.js [download page](https://nodejs.org/en/download/).
2. Download the macOS Installer for the LTS version.
3. Run the `.pkg` file and follow the prompts to complete the installation.

## macOS - Homebrew

If you have Homebrew installed, you can update Node.js with the following commands:

```bash
brew update
brew upgrade node
```

## Linux - Package Manager

The method to update Node.js on Linux depends on your distribution.

But as the version of Node.js in the official repositories may not be the latest, we recommend using the [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm) or the [NodeSource repository](https://github.com/nodesource/distributions).

## Docker

No action required. The prebuilt Docker image we provide is compiled with the up-to-date version of Node.js.
