---
label: Android (Termux)
route: /installation/android-(termux)/
---

# Android (Termux) Installation

SillyTavern can be run natively on Android devices using Termux.

## Installing Termux

!!!tip
Avoid installing Termux from the Google Play Store, that version is no longer maintained. 
Instead, use F-Droid (recommended) or GitHub releases to get the latest version.
!!!

1. Download Termux from [F-Droid](https://f-droid.org/en/packages/com.termux/) or [GitHub releases](https://github.com/termux/termux-app/releases).
2. Install the downloaded APK file.
3. Open Termux and run your first command:

   ```bash
   termux-change-repo
   ```

4. Select "Mirror group" and choose your closest servers. You can touch the screen or use swipe gestures with [Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en).
5. Update Termux:

   ```bash
   pkg update && pkg upgrade
   ```

## Installing Dependencies

Install the required packages:

```bash
pkg install git nodejs-lts nano
```

!!!warning
If you're running 32-bit Android, see the [Common Errors](#common-errors) section below for additional steps.
!!!

## Installing SillyTavern

Clone the SillyTavern repository ([How to Choose a Branch](/Installation/index.md#branches)):

- **Release Branch:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **Staging Branch:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## Running SillyTavern

To run SillyTavern, navigate to the cloned directory and run the start script:

```bash
cd ~/SillyTavern
bash start.sh
```

To update SillyTavern, navigate to the SillyTavern directory and run:

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

See the [Aliases](#optional-create-aliases) section below for creating shortcuts to simplify this process.

## Common Errors

### Unsupported platform: android arm LEtime-web

32-bit Android requires an external dependency that can't be installed with npm.

Use the following command to install it:

```bash
pkg install esbuild
```

Then proceed with the installation steps above.

## Optional: Create Aliases

You can create shortcuts for common commands to make your workflow easier.

1. Open an editor to modify your `.bashrc` file:

   ```bash
   nano ~/.bashrc
   ```

2. Add the following lines to create aliases:

   ```bash
   # Update Termux packages
   alias pkgup="pkg update && pkg upgrade"
   #Start SillyTavern
   alias st='cd ~/SillyTavern && bash start.sh'
   # Update SillyTavern
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. Save the file and exit the editor (in nano, press `CTRL + X`, then `Y`, then `Enter`).

4. To apply the changes, run:

   ```bash
   source ~/.bashrc
   ```

Now you can use the following commands:

- `st` to start SillyTavern
- `stup` to update SillyTavern
- `pkgup` to update Termux packages

## Further Reading

!!!info
The guides linked below are not maintained by the SillyTavern team.
!!!

- SillyTavern in Termux guide by ArroganceComplex#2659: <https://rentry.org/STAI-Termux>
- Accessing Termux files with Material Files: <https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- Prevent Termux process deep sleep: <https://wiki.termux.com/wiki/Termux-wake-lock>
