---
order: 10
label: Windows
---
# Windows Installation

> **Warning**

> DO NOT INSTALL INTO ANY WINDOWS CONTROLLED FOLDER (Program Files, System32, etc).

> DO NOT RUN START.BAT WITH ADMIN PERMISSIONS

## Installing via Git (recommended for easy updating)

  1. Install [NodeJS](https://nodejs.org/en) (latest LTS version is recommended)
  2. Install [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
  3. Open Windows Explorer (`Win+E`)
  4. Browse to or Create a folder that is not controlled or monitored by Windows. (ex: C:\MySpecialFolder\)
  5. Open a Command Prompt inside that folder by clicking in the 'Address Bar' at the top, typing `cmd`, and pressing Enter.
  6. Once the black box (Command Prompt) pops up, type ONE of the following into it and press Enter:
  - for Main Branch: `git clone https://github.com/SillyTavern/SillyTavern -b main`
  - for Dev Branch: `git clone https://github.com/SillyTavern/SillyTavern -b dev`
  7. Once everything is cloned, double click `Start.bat` to make NodeJS install its requirements.
  8. The server will then start, and SillyTavern will popup in your browser.

## Installing via ZIP download (discouraged)

  1. Install [NodeJS](https://nodejs.org/en) (latest LTS version is recommended)
  2. Download the zip from this GitHub repo. (Get the `Source code (zip)` from [Releases](https://github.com/SillyTavern/SillyTavern/releases/latest))
  3. Unzip it into a folder of your choice
  4. Run `Start.bat` via double-clicking or in a command line.
  5. Once the server has prepared everything for you, it will open a tab in your browser.
