---
order: 99
icon: repo-pull
---

# How to Update SillyTavern

Find your OS below and follow the instructions to update ST.

(This guide assumes you have already installed SillyTavern once and know how to run it on your OS.)

This is not an installation guide. If you need installation instructions, go to the Installation page for your OS (can be found in site navigation).

----

## Linux/Termux

You definitely installed via git, so just 'git pull' inside the SillyTavern directory.

- `cd SillyTavern` to enter the correct folder.
- `git pull` to get the update.
- `./start.sh` or `bash start.sh` to start ST.

----

## Windows/MacOS

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

Thankfully we have [instructions](https://docs.sillytavern.app/installation/windows/) on how to do so.

Once you have used git to install a NEW SillyTavern into a DIFFERENT folder, come back to this page and proceed to **Step 4** of the 'Zip Update' instructions below.

### Method 2 - ZIP

If you insist on installing via a zip, here is the tedious process for doing the update:

1. Download the new release zip.
2. Unzip it into a folder OUTSIDE of your current ST installation.
3. Do the usual setup procedure for your OS to install NodeJS requirements.

4. Copy the following files/folders as necessary(*) from your old ST installation:

  (*) 'As necessary' = "If you made any custom content related to those folders".
  None of the folders are mandatory, so only copy what you need.

#### NOTE: DO NOT COPY THE ENTIRE /PUBLIC/ FOLDER

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
  
5. Once those folders/files are copied, Paste them into the /Public/ folder (with secrets.json going into the base folder) of the new install.

6. Start SillyTavern once again with the method appropriate to your OS, and pray you got it right.

7. If everything shows up, you can safely delete the old ST folder.

### Common Update Problems

#### File changes prevent git pull

- If you change SillyTavern system files, `git pull` may not work.
- Sometimes an update may require us to change an important file, which can cause the same problem.
- Usually it is `config.conf` or `package-lock.json`.
- In this case you can try moving the file to a different folder (or deleting the file) and then do `git pull`.
- Another solution is using `git pull --rebase --autostash`
