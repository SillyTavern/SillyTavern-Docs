---
title: Multi-user mode
icon: people
order: -10
---

Multi-user mode allows several people to use one SillyTavern server. Each user has their own settings, extensions, and data. User accounts can also be password-protected.

!!!warning
User passwords provide basic privacy between users of a multi-user setup. They are not a security feature and should not be considered as such. All user data (including chat history, API keys, and other sensitive information) is stored in plain text on the server. It can be viewed and modified by anyone with access to the server's filesystem. **Do not use SillyTavern on a public server or with untrusted users.**
!!!

## Configuration

To enable and use the multi-user mode, edit the `config.yaml` file:

```yaml
# Enable multi-user mode
enableUserAccounts: true
# Enable discreet login mode: hides user list on the login screen
enableDiscreetLogin: true
```

1. When the user account setting is disabled, a `default-user` fallback admin account is utilized for storing the user data.
2. When the discreet login setting is disabled, a list of active users is displayed on the login screen. If enabled, a user must enter their handle manually.

!!!info
You can't _delete_ the `default-user` account from the users list because it is used for serving the user data in case if `enableUserAccounts` is set to `false`. But you can _disable_ it to hide it from the list and disallow logins.
!!!

## User handles

A handle is the unique identifier of a user. It can consist only of lowercase letters, numbers, and dashes.

A path to the user data directory assumes using the following pattern: `%DATA_ROOT%/%USER_HANDLE%`.

Examples of valid user handles:

- default-user
- juan555
- flux-the-cat
- cool-guy1337

## Roles

- **Admin** - can manage (create, delete, modify) other users. Can install extensions for all users.
- **User** - can't manage other users. Can install extensions only for themselves.

Except for having admin panel access, both user roles are functionally identical and can use a full range of SillyTavern features without any restrictions. An implementation of user permissions is TBD.

All user accounts are created as regular users first, and then could be promoted to admins if needed.

### Login screen

There you can select a user account to use. Has two styles, depending on the `enableDiscreetLogin` config value.

The login screen is bypassed and not displayed when you have only one active user and it is not password protected.

### User profile

You can access an account self-management menu using an "Account" button under the "User settings" panel in the top menu bar.

1. Display name - used in the login screen, can be changed. Does not correlate with personas and is not visible for the AI APIs - you can still use as many personas as you want.
2. Profile picture - used in the login screen. You can either use a custom picture, the default persona picture (if set), or the last used persona otherwise.
3. Password - a lock icon reflects the account protection status (open lock = no password). A password can be set, changed, or removed using the "Change Password" button.
4. Settings Snapshots - access and review the backups of your `settings.json` file, with the ability to create or restore snapshots.
5. Download Backup - download an archive of your user data folder.
6. Reset Settings - reset factory default settings, while leaving other data (character, chats) intact.

## Password recovery

1. A password can be recovered from a login screen. You need access to the server console to get a one-time recovery code (consisting of 4 digits).
2. Alternatively, you can use a utility script in the SillyTavern server to reset a password by providing the user handle.

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

## Content scaffolding

To add custom content for users, you can use the content scaffolding feature. This feature allows you to define a set of files that will be copied to each user's data directory when the server starts up.

You must create an `index.json` file in the `/default/scaffold` directory for this feature to work. The syntax is the same as for default content. All file paths should be relative to the `/default/scaffold` directory, and you can organize files using subdirectories.

Scaffolded files are copied before default files, which means they will override any default files (presets/settings/etc.) that have the same file name.

!!!tip
Every user data directory has a `content.log` file that lists all files copied from the scaffold and default directories. Remove this file to force the server to sync the content again on the next restart.
!!!

### Recognized content types

| Type                          | Value                |
|-------------------------------|----------------------|
| settings.json                 | `'settings'`         |
| Character card                | `'character'`        |
| Character sprites             | `'sprites'`          |
| Background image              | `'background'`       |
| World Info file               | `'world'`            |
| Persona avatar                | `'avatar'`           |
| UI theme                      | `'theme'`            |
| ComfyUI workflow              | `'workflow'`         |
| KoboldAI Classic preset       | `'kobold_preset'`    |
| Chat Completion preset        | `'openai_preset'`    |
| NovelAI preset                | `'novel_preset'`     |
| Text Completion preset        | `'textgen_preset'`   |
| Instruct Mode template        | `'instruct'`         |
| Context Formatting template   | `'context'`          |
| MovingUI preset               | `'moving_ui'`        |
| Quick Replies set             | `'quick_replies'`    |
| System Prompt template        | `'sysprompt'`        |
| Reasoning Formatting template | `'reasoning'`        |

### Example (`/default/scaffold/index.json`)

```json
[
    {
        "filename": "themes/Midnight.json",
        "type": "theme"
    },
    {
        "filename": "backgrounds/city.png",
        "type": "background"
    },
    {
        "filename": "characters/Charlie.png",
        "type": "character"
    }
]
```
