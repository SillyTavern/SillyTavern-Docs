---
order: 112
---

# 1.12.0 Migration Guide

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

The default data root path is `./data`, which means the `data` directory in SillyTavern's repository.

!!! info Note
The data root path should be either a **full absolute** or a **full relative** path. You _can't_ use path shortcuts like `~` or `%APP_DATA%`, as these are resolved by a shell, not the operating system.
!!!

### Migration

#### **IMPORTANT!** Before we begin

1. **Only if you want to move dataRoot from the default location. Otherwise, skip this part.** Set the data root _before_ first running the server after pulling an update. Run `npm install` for the `config.yaml` to populate with a new value, or pass a console argument.
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

!!! info Note
Soft links between the `/public` directory and the `config` volume are no longer needed and are not built into the Docker container!
!!!

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

Please refer to the [Users](/Administration/multi-user.md) documentation for more information.
