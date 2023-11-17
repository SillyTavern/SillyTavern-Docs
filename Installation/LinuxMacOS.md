---
label: MacOS & Linux
order: 5
---

# Linux/MacOS Install

## SillyTavern Launcher

### For Linux users
1. Open your favorite terminal and install git
2. Download Sillytavern Launcher with: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. Navigate to the SillyTavern-Launcher with: `cd SillyTavern-Launcher`
4. Start the install launcher with: `chmod +x install.sh && ./install.sh` and choose what you wanna install
5. After installation start the launcher with: `chmod +x launcher.sh && ./launcher.sh`

### For Mac users
1. Open a terminal and install brew with: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Then install git with: `brew install git`
3. Download Sillytavern Launcher with: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. Navigate to the SillyTavern-Launcher with: `cd SillyTavern-Launcher`
5. Start the install launcher with: `chmod +x install.sh && ./install.sh` and choose what you wanna install
6. After installation start the launcher with: `chmod +x launcher.sh && ./launcher.sh`

## Manual Git install

For MacOS all of these will be done in a Terminal.

1. Install git and nodeJS (the method for doing this will vary depending on your OS)
2. Clone the repo

- for Release Branch: `git clone https://github.com/SillyTavern/SillyTavern -b release`
- for Staging Branch: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. `cd SillyTavern` to navigate into the install folder.
4. Run the `start.sh` script with one of these commands:

- `./start.sh`
- `bash start.sh`
