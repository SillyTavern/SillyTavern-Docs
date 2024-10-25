---
label: Updating
icon: repo-pull
order: -1
expanded: false
---

# How to Update SillyTavern

Find your OS below and follow the instructions to update ST.

!!! For installation instructions, see the [Installation](/Installation/index.md) page.

This guide assumes you have already installed and run SillyTavern at least once.
!!!

----

## Linux/Termux or MacOS

You definitely installed via git, so just 'git pull' inside the SillyTavern directory.

- `cd SillyTavern` to enter the correct folder.
- `git pull` to get the update.
- `./start.sh` or `bash start.sh` to start ST.

----

## Windows

>First try using the `UpdateAndStart.bat` which is located in your SillyTavern installation base folder.

If that fails, come back here and continue reading.

### Method 1 - GIT

We always recommend users install using 'git'. Here's why:

When you have installed via `git clone`, all you have to do to update is type `git pull` [in a command line in the ST folder](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder).
Alternatively, if the command prompt gives you problems (and you have GitHub Desktop installed), you can use the `Repository` menu and select `Pull`.

The updates are applied automatically and safely.

#### "Help I originally installed via Zip and now want to convert to Git install"

You have chosen a wise path.

Since your installation was done via Zip, you will need to make a new install using git.

Thankfully we have [instructions](/Installation/Windows.md) on how to do so.

Once you have used git to install a NEW SillyTavern into a DIFFERENT folder, come back to this page and proceed to **Step 4** of the 'Zip Update' instructions below.

### Method 2 - ZIP

If you insist on installing via a zip, here is the tedious process for doing the update:

1. Download the new release zip.
2. Unzip it into a folder OUTSIDE of your current ST installation.
3. Do the usual setup procedure for your OS to install NodeJS requirements.

4. Copy the following files/folders as necessary(*) from your old ST installation:

(*) 'As necessary' = "If you made any custom content related to those folders".

#### Updating >=1.12.0

Copy the `/data` directory and `config.yaml` file from one installation to another.

#### Updating from <1.12.0 to >1.12.0

1.12.0 includes an automated migration procedure. The steps below are required *only* if the migration was interrupted or errored.

5. Run the updated server install at least once to create the `/data/default-user` directory.
6. Transfer the files from old `/public` to new `/data/default-user` as necessary.

None of the folders are mandatory, so only copy what you need.

**NOTE: DO NOT COPY THE ENTIRE /PUBLIC/ FOLDER**

Doing so could break the new install and prevent new features from being present.

```plaintext
Assets
Backgrounds
Characters
Chats
Context
Groups
Group chats
Instruct
movingUI
KoboldAI Settings
NovelAI Settings
OpenAI Settings
QuickReplies
TextGen Settings (textgen = ooba)
Themes
User Avatars
Worlds
User
settings.json
secrets.json <---- this one is in the base folder, not /public/
```

7. Once those folders/files are copied, paste them into the /data/default-user folder (with secrets.json going into the folder root) of the new install.
8. Start SillyTavern once again with the method appropriate to your OS, and pray you got it right.
9. If everything shows up, you can safely delete the old ST folder.

### Common Update Problems

#### I use Docker and all my data is gone after the update!

You must follow the [Migration guide for Docker containers](/Installation/Updating/ST-1.12.0-Migration-Guide.md#containerized-docker-installs)
 to update volume mappings for the new data model introduced in 1.12.0

#### "There are unresolved conflicts in the working directory."

This means that you've modified default files that have been changed in the remote repository (such as setting presets).

To fix this, run this in the terminal. Use cautiously, as it can be destructive. Make sure to have a backup if needed.

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### File changes prevent git pull

- If you change SillyTavern system files, `git pull` may not work.
- Sometimes an update may require us to change an important file, which can cause the same problem.
- Usually it is default preset files or `package-lock.json`.
- In this case you can try moving the file to a different folder (or deleting the file) and then do `git pull`.
- Another solution is using `git pull --rebase --autostash`

#### Error: Cannot find module "***" when starting the server

- This means that SillyTavern added a new npm package requirement.
- Run `npm install` in the SillyTavern directory to fix this. Provided Start.bat and start.sh scripts will do that automatically.
- Didn't help? Remove the node_modules folder

**Windows**

```bash
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**

```bash
rm -rf node_modules
npm cache clean --force
npm install
```
