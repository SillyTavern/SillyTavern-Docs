---
icon: gear
order: user-settings
---

# User Settings


:::callout
**[UI Customization](uicustomization.md)**

Change the theme, look and feel of the chat interface to suit your preferences.
:::



:::callout
**[Visual Novel mode](Visual-Novel.md)**

Chat to characters with sprites, like in visual novels such as Doki Doki Literature Club and other famous VN games.
:::


## General Settings
These are the core settings that affect your overall SillyTavern experience.

### UI Language
SillyTavern's user interface is available in multiple languages. The language selector provides these options:
* **Default**: Uses your system language if available
* **English**: Forces English UI regardless of system settings
* Other languages available through the dropdown
  Note: This setting only affects the user interface text. For AI conversation translation, please use the Chat Translation extension.

### Software Version
Your current version of SillyTavern is displayed in the top-right corner. This information is essential for:
* Troubleshooting problems
* Ensuring compatibility with extensions
* Determining if updates are available
  To update SillyTavern to the latest version, please refer to the [Updating](/Installation/Updating) documentation.

### Account Management
Control your SillyTavern user account, back up your settings and user data, and manage user roles and permissions in [multi-user mode](/Administration/multi-user.md).

#### <i class="fa-fw fa-solid fa-user-shield"></i> Account

In the Account dialog, you can view and edit your profile information, change your password, and manage account settings.

**Profile Information**

* Display name (editable via pencil icon)
* User avatar (can also be changed using [Personas](/Usage/Core_Concepts/personas.md))
* Account handle
* User role
* Account creation date
* Password status (locked/unlocked icon indicates protection)

**Account Actions**

* **Settings Snapshots**: Create, manage, and restore backups of your user settings
* **Download Backup**: Export a complete backup of all your user data
* **Change Password**: Update your account security credentials

**Danger Zone**

Critical account operations that should be used with caution:
* **Reset Settings**: Restore all settings to factory defaults
* **Reset Everything**: Complete account wipe and factory reset

#### <i class="fa-fw fa-solid fa-user-tie"></i> Admin Panel

!!! Applies to: [multi-user mode](/Administration/multi-user.md)

Multi-account features require `enableUserAccounts` to be set to true in config.yaml. 
!!!

Select **Manage Users** to view and manage existing user accounts.

##### User Profile
- Custom avatar management (upload/remove)
- Display name and handle
- Role and status information
- Account creation date
- Password protection status

##### Account Controls
- <i class="fa-fw fa-solid fa-pencil"></i> Edit display name
- <i class="fa-fw fa-solid fa-check"></i> Enable account
- <i class="fa-fw fa-solid fa-ban"></i> Disable account
- <i class="fa-fw fa-solid fa-arrow-up"></i> Promote to admin
- <i class="fa-fw fa-solid fa-arrow-down"></i> Demote to regular user

##### Management Actions
- <i class="fa-fw fa-solid fa-download"></i> Download user data backup
- <i class="fa-fw fa-solid fa-key"></i> Change user password
- <i class="fa-fw fa-solid fa-trash"></i> Delete account

##### New User

Select **New User** to create a new user account.

* Display Name* (e.g., "John Snow")
* User Handle* (lowercase letters, numbers, and dashes only)
* Password (optional)
* Password Confirmation

Creating a new user automatically generates a subfolder in the /data/ directory using the user's handle as the folder name.

#### <i class="fa-fw fa-solid fa-right-from-bracket"></i> Logout

!!! Applies to: [multi-user mode](/Administration/multi-user.md)
!!!

Sign out of your current session.

### Settings Search
A convenient search bar that helps you quickly find specific settings:
* Type any keyword to filter and highlight settings anywhere in User Settings
* Searches through setting names and descriptions
* Helps navigate complex settings more efficiently

## UI Theme

Change the appearance of the chat interface to suit your preferences.

For more information on the settings in this section of <i class="fa-fw fa-solid fa-user-gear" title="User Settings icon"></i> **User Settings**, see [UI Customization](uicustomization.md#ui-theme).

## Character Handling
* **Char List Subheader**: Choose what additional information to display under character names in the [<i class="fa-fw fa-solid fa-address-card" title="Characters icon"></i> Characters](characterdesign.md) list:
    - Character Version
    - Created by
* **Import Card Tags**: Controls how tags are handled when importing character cards:
    - Ask - Show dialog for each import
    - None - Don't import any tags
    - All - Import all tags
    - Existing - Only import tags that already exist
* **Advanced Character Search**: When enabled, uses fuzzy matching and searches all character data fields, not just names.
* **Prefer Char. Prompt**: If enabled, uses the character card's System Prompt override when available.
* **Prefer Char. Instructions**: If enabled, uses the character card's Post-History Instructions override when available.
* **Never resize avatars**: Prevents cropping/resizing of imported character images. When disabled, images are resized to 512x768.
* **Show avatar filenames**: Displays actual filenames of character avatars in the character list.
* **Spoiler Free Mode**: Hides character definitions behind a spoiler button in the editor panel.

## Miscellaneous
* **Reload Chat**: Reloads and redraws the current chat.
* **[Debug Menu](#debug-menu)**: Access debugging options.
* **Smooth Streaming**: Experimental feature for smoother text generation. Includes speed control slider.
* **[Message Sound](uicustomization.md#message-sound)**: Plays a sound when message generation completes.
    - **Background Sound Only**: Only plays sounds when browser tab is unfocused.
* **Relaxed API URLs**: Reduces formatting requirements for API URLs.
* **Lorebook Import Dialog**: Shows import dialog for World Info/Lorebook when importing characters with embedded lore.
* **Auto-select Input Text**: Automatically selects text in certain input fields when clicked.
* **Markdown Hotkeys**: Enables keyboard shortcuts for markdown formatting.
* **Restore User Input**: Preserves unsaved user input when page is refreshed.
* **MovingUI**: Allows repositioning UI elements by dragging (PC only).
    - <i class="fa-solid fa-recycle" title="Reset icon"></i> **Reset** button to restore default positions
    - Preset system for saving/loading UI layouts

## Chat/Message Handling
### Message Display Settings
Controls how messages are loaded and displayed in the chat interface. These settings affect the overall chat experience and performance.
* **# Messages to Load**: Number of chat history messages to load before pagination (0 = All)
* **Streaming FPS**: Update speed of streamed text (5-100 FPS)
* **Example Messages Behavior**:
    - Gradual push-out
    - Always include examples
    - Never include examples
### Input & Response Controls
Settings that determine how messages are sent and how the AI continues its responses.
* **Enter to Send**: Choose between Disabled, Automatic (PC), or Enabled
* **"Send" to Continue**: Use Send button to continue AI responses
* **Quick "Continue" button**: Show button to extend AI's last message
* **Quick "Impersonate" button**: Show button for single-message character impersonation
* **Swipes**: Show arrow buttons for alternative AI responses (PC and mobile)
* **Gestures**: Enable swipe gestures for generation (Mobile only)

### Auto-Management
Automated features that help manage chat flow and content.
* **Auto-load Last Chat**: Automatically load the most recent chat on startup
* **Auto-scroll Chat**: Automatically scroll to newest messages
* **Auto-save Message Edits**: Save message edits without confirmation
* **Confirm message deletion**: Prompt before deleting messages
* **Auto-fix Markdown**: Automatically correct markdown formatting

#### Auto-swipe
Automatically reject and regenerate AI messages based on configurable criteria.
* **Enable Auto-swipe**: Master toggle for the auto-swipe function
* **Minimum generated message length**: Triggers an auto-swipe if the message is shorter than this value
* **Blacklisted words**: List of words that can trigger auto-swipe, separated by commas
* **Blacklisted word count to swipe**: Minimum number of blacklisted words that must be detected to trigger an auto-swipe

#### Auto-Continue
Automatically continues a response if the model stopped before reaching a certain length.

This lets your AI write a long response in multiple parts, so that you can have a short [response length setting](/Usage/Common-Settings.md#response-tokens) while still getting long replies. 

It will not make the AI write more than it would have otherwise. Asking the AI to continue a message that it considers "finished" does not usually work. See [How to make the AI write more?](/Usage/faq.md#how-to-make-the-ai-write-more) for other ideas.

* **Enable Auto-continue**: Master toggle for automatic continuation
* **Allow for Chat Completion APIs**: Enables auto-continue functionality for Chat Completion API endpoints
* **Target length (tokens)**: The desired message length in tokens - will trigger continue if message is shorter than this value (0-1024)

### Message Formatting & Display
Controls how messages are formatted and what content is displayed.
* **Forbid External Media**: Block embedded media from external domains
* **Show {\{char}}: in responses**: Retain character name prefix in responses if generated
* **Show {\{user}}: in responses**: Retain user name prefix in responses if generated
* **Show tags in responses**: Allow (some) HTML tags in responses to be displayed as HTML 
* **Relax message trim in Groups**: Allow AI to speak for other characters in group chats, rather than stopping the response generation
* **Show group chat queue**: Display response order in the character list for group chats

### Prompt Inspection and Debugging

* **Log prompts to console**: Output prompts to browser console
* **Request token probabilities**: Request token probabilities for AI responses from the API. Where available, these can be viewed in <i class="fa-solid fa-bars" title="Burger Menu icon"></i> Token Probabilities.

### AutoComplete
- Auto-hide details
- Matching style (Starts with/Includes/Fuzzy)
- Visual style (Theme/Dark/Light)
- Keyboard selection options
- Font scaling
- Width controls

## STscript Settings
Configuration options for the [STscript parser](/For_Contributors/st-script.md#parser-flags).

### STRICT_ESCAPING

* Pipes don't need to be escaped in quoted values.
* A backslash in front of a symbol can be escaped to provide the literal backslash followed by the functional symbol.

See [Strict Escaping](/For_Contributors/st-script.md#strict-escaping) for more information.

### REPLACE_GETVAR

Helps to avoid double-substitutions when the variable values contain text that could be interpreted as macros.

See [Replace Variable Macros](/For_Contributors/st-script.md#replace-variable-macros) for more information.

## Debug menu

!!! warning These functions are intended for advanced users only.

Do not use them unless you fully understand their consequences.
!!!

The Debug Menu provides functionality for troubleshooting, maintenance, and development purposes. These functions should be used with caution as they can significantly impact your SillyTavern installation.

Because extensions can add debug functions, the available options will vary depending on the extensions you have installed.

### Translation & Locale Functions
* **Get missing translations**: Analyzes the current locale (or all locales if English is selected) for missing translations and outputs results to browser console
* **Apply locale**: Forces a refresh of the current language settings by reapplying the selected locale
### Cache & Storage Management
* **Clear WebSearch cache**: Removes all stored search results from local cache
* **Purge all vector indices**: Completely removes all stored vectors across all sources
* **Reset token cache**: Clears stored token counts, forcing complete re-tokenization of all chats
* **Delete itemized prompts**: Removes all itemized prompts from local storage
### Data & Statistics
* **Refresh Stat File**: Rebuilds the statistics file using existing chat data
* **Backfill token counters**: Recalculates token counts for all messages in current chat
    - Useful when switching between models with different tokenizers
    - Triggers chat reload after completion
    - Visual changes only, does not modify chat content
### API & Extension Testing
* **Change Mancer base URL**: Modify the base URL for Mancer API server
* **Test WebSearch extension**: Performs a test search using current settings
* **Send a generation request**: Tests text generation using the currently selected API
### System & Debug Tools
* **Force onboarding**: Restarts the onboarding process
* **Toggle event tracing**: Enables/disables event tracking for debugging
* **Copy ST setup**: [Work in Progress] Copies system configuration data to clipboard for bug reports
  Each function can be executed using the "Execute" button beneath its description. Consider backing up your data before using these tools, as some operations cannot be undone.
