# SillyTavern 1.12.0 Migration Guide

> This content describes a pre-release version and is subject to change.

SillyTavern 1.12.0 (codename the "Neo Server" update) includes several critical changes that may affect the way you use SillyTavern.

This guide will prepare you for the update and provide some further guidance.

## Data storage update

1.12.0 changes the way SillyTavern handles the user data.

Previously, all of the persistent data was stored together with the frontend part in the `/public` directory, which created confusion and potential points of failure, as well as making containerization and system-wide app installation quite challenging.

### What's changed?

All persistent information from `/public` such as settings and chats (full list below) was moved into a separate directory with the configurable path, making it portable and independent from the web server itself. When needed for compatibility purposes, for example, for hosting extensions, full-size character cards, user image uploads, etc. a smart redirect has been set up to automatically host user files from the data directory.

### Setting a data root

You can provide either an absolute or a relative (to the ST repository directory) path to the data root either by `config.yaml` or by starting the server with the `--dataRoot` console argument.

> YAML example

```yaml
# -- DATA CONFIGURATION --
# Root directory for user data storage
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> Console example

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# OR
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

The default data root path is `./data`, which means the `data` directory in the SillyTavern's repository.

### Migration

#### **IMPORTANT!** Before we begin

1. Set the data root *before* first running the server after pulling an update. Run `npm install` for the `config.yaml` to populate with a new value, or pass a console argument.
2. All data will be migrated into a `default-user` account. See more on [Users](#users) below.

#### Containerless (bare metal) installs

You don't have to do anything! An automatic migration should handle everything for you when you start the ST server and it detects the old storage format (by checking the existence of the `/public/characters` directory).

Upon moving any files, an automatic backup will be created in the `/backups/_migration/YYYY-MM-DD` (resolved to the current date) directory, but it is always a good practice to make a full manual backup before running the migration.

#### Containerized (Docker) installs

Migrating the data in Docker volumes is a bit trickier but pretty straightforward. While `docker-compose.yml` provided with the repo was updated to reflect the changes, you may need to adjust your custom workflows/deployments.

**Step 1.** Create a new volume, and mount it to the "/home/node/app/data" path within the container. Don't remove the `config` volume.

```yaml
volumes:
  - "./config:/home/node/app/config"
  - "./data:/home/node/app/data"
 ```

**Step 2.** Move everything but the `config.yaml` file from the `config` volume into the `default-user` subdirectory of the `data` volume.

**Step 3.** Rebuild the container and start it up.

> Soft links between the `/public` directory and the `config` volume are no longer needed and are not built into the Docker container!

#### What to migrate?

The following files and directories are subject to the data migration. Assuming the default configuration, the before and after paths are provided in the table below.

| Before                                 | After                                |
| -------------------------------------- | ------------------------------------ |
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## Users

1.12.0 adds a (completely optional) ability to create a multi-user setup on the same server, allowing multiple users to use their own fully isolated SillyTavern instances even at the same time. User accounts can also be password-protected for an additional layer of privacy.

> While we tried to applied most of the well-known security practices, this still doesn't make SillyTavern server secure enough to expose it to the public internet.

**NEVER HOST ANY INSTANCES TO THE OPEN INTERNET WITHOUT ENSURING PROPER SECURITY MEASURES FIRST.**

**WE ARE NOT RESPONSIBLE FOR ANY DAMAGE OR LOSSES IN CASES OF UNAUTHORIZED ACCESS DUE TO IMPROPER OR INADEQUATE SECURITY IMPLEMENTATION.**

### Configuration

To enable and use the multi-user mode, edit the `config.yaml` file:

```yaml
# Enable multi-user mode
enableUserAccounts: true
# Enable discreet login mode: hides user list on the login screen
enableDiscreetLogin: true
```

1. When the user account setting is disabled, a `default-user` fallback admin account is utilized for storing the user data.
2. When the discreet login setting is disabled, a list of active users is displayed on the login screen. If enabled, a user must enter their handle manually.

> You can't *delete* the `default-user` account from the users list because it is used for serving the user data in case if `enableUserAccounts` is set to `false`. But you can *disable* it to hide it from the list and disallow logins.

### User handles

A handle is the unique identifier of a user. It can consist only of lowercase letters, numbers, and dashes.

A path to the user data directory assumes using the following pattern: `%DATA_ROOT%/%USER_HANDLE%`.

Examples of valid user handles:

* default-user
* juan555
* flux-the-cat
* cool-guy1337

### User roles

* **Admin** - can manage (create, delete, modify) other users.
* **User** - can't manage other users.

Except for having admin panel access, both user roles are functionally identical and can use a full range of SillyTavern features without any restrictions. An implementation of user permissions is TBD.

All user accounts are created as regular users first, and then could be promoted to admins if needed.

### Login screen

There you can select a user account to use. Has two styles, depending on the `enableDiscreetLogin` config value.

The login screen is bypassed and not displayed when you have only one active user and it is not password protected.

### User profile

You can access an account self-management menu using an "Account" button under the "User settings" panel in the top menu bar.

1. Display name - used in the login screen, can be changed. Does not correlate with personas - you can still use as many personas as you want.
2. Profile picture - used in the login screen. The picture is taken from the default persona picture if set, or the last used persona otherwise.
3. Password - a lock icon reflects the account protection status (open lock = no password). A password can be set, changed, or removed using the "Change Password" button.
4. Settings Snapshots - access and review the backups of your `settings.json` file, with the ability to create or restore snapshots.
5. Download Backup - download an archive of your user data folder.
6. Reset Settings - reset factory default settings, while leaving other data (character, chats) intact.

### Password recovery

1. A password can be recovered from a login screen. You need access to the server console to get a one-time recovery code (consisting of 4 digits).
2. Alternatively, you can use a utility script in the SillyTavern server to reset a password by providing the user handle.

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

### Security checklist

This is just a recommendation. Please consult a web application security specialist before making your ST instance live.

1. Keep your operating system and runtime software like Node.js updated. This will ensure that your system is up-to-date with the latest security patches and fixes which can help prevent potential vulnerabilities.
2. Use a whitelist and a network firewall. Only allow trusted IP ranges to access the server.
3. Enable basic authentication. It acts as a "master password" before you can proceed to the front-end app.
4. Never leave admin accounts passwordless. A server will warn you upon the startup if you have any unprotected admin accounts.
5. Use the discreet login setting outside of the local network. This will hide the user list from any potential outsiders.
6. Check the access logs often. They are written to the server console and the `access.log` file and provide information on incoming connections, such as IP address and user agent.
7. Configure HTTPS. For a localhost server, you can generate and use a self-signed certificate. Otherwise, you may need to deploy a proxying web server like [Caddy](https://caddyserver.com/docs/getting-started).
