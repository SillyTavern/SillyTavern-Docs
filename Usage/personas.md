---
order: 110
icon: smiley
route: /usage/core-concepts/personas
templating: false
---

# Personas

## What is a Persona?

A persona in SillyTavern is the identity you use to participate in chats — essentially a combination of your display name, avatar, and optional descriptive text. Personas allow you to easily switch roles or "characters" you speak as, without having to manually update your username/avatar each time.

> **Note:** Legacy user avatars/names that weren't tied to a persona have been removed. Existing data will be migrated to personas. If no name was specified, the persona will be named "[Unnamed Persona]".

## How to Create a Persona?

1. Open the **Persona Management** panel (<i class="fa-solid fa-face-smile"></i> button in the top menu).
2. Create a blank persona with the **Create** button and give it a name.
3. In the persona list, select the newly created persona.
4. On the right side, you can fill in your description and set an avatar via the "Change Persona Image" button. Both are optional.
5. Now your persona is ready to use in chats.

### Convert Character to Persona

Personas can also be created by converting any existing character. Simply open the character, select "More..." and click "Convert to Persona". A persona with the same name and description will be created. Other character card fields like Scenario or Personality will not be used. The character will not be deleted.

!!! Note
Since `{{user}}` and `{{char}}` macros have opposite meanings when used in Persona and Character descriptions, you'll be prompted to swap them if the converted description contains either of them.
!!!

## Persona Description

Each persona can store a custom text description — mental and physical traits, age, occupation, or any personal details. These can also include template macros such as `{{char}}` or `{{user}}` (see [Macros](/Usage/Characters/macros.md)).

Where your persona description is injected into the AI prompt depends on the **Position** setting in the Persona Management panel:

- **None (disabled)**
- **In Story String / Prompt Manager** (the default)
- **Top of Author's Note** / **Bottom of Author's Note** (will only be added when an Author's Note exists)
- **In Chat @ Depth** (this will open up configuration options to set depth and the role)

The position is saved **per persona**.

## Persona Title

The title is an optional text field that can be used to store additional information about the persona and is not used in the prompt, but displayed in the Persona Management panel.

To set a title, click the **<i class="fa-solid fa-pencil"></i> Rename Persona** button in the Persona Management panel and enter the title in the "Persona Title" field, or specify it during persona creation. Setting an empty value when the title already exists will remove it.

## Persona Connections / Locking

Persona connections ensure that a given persona is automatically selected in certain situations. If no persona is connected, the currently chosen persona will stay selected.

There are three types of locking:

1. **<i class="fa-solid fa-unlock"></i> Chat lock** – The persona is locked to the current chat.
2. **<i class="fa-solid fa-unlock"></i> Character lock** – The persona is locked to a specific character.
3. **<i class="fa-solid fa-crown"></i> Default persona** – One persona that is used whenever no other locks apply.

### 1. Lock to a Chat

If a persona is locked to a chat, opening that chat in the future will automatically switch your active persona to the locked one.

- **To lock**: Select the desired persona, then click the **<i class="fa-solid fa-unlock"></i> Chat** button under the "Connections" section (or use `/persona-lock type=chat on`).
- **To unlock**: Click the button again (or use `/persona-lock type=chat off`).

### 2. Lock to a Character

You can also link a persona to a specific character. Opening any chat with that character automatically selects your locked persona.

- **To lock**: Select the desired persona, then click the **<i class="fa-solid fa-unlock"></i> Character** button under the "Connections" section (or use `/persona-lock type=character on`).
- **To unlock**: Click the button again (or use `/persona-lock type=character off`).

The Persona Management panel also shows which characters are linked to that persona (displayed as small avatars). Clicking them navigates directly to that character's chat.

#### Locking Multiple Personas to the Same Character

If another persona was already linked with that character, it will be automatically unlinked by default.

To have multiple personas linked at once, the global setting **Allow multiple persona connections per character** can be used.  
If multiple personas are linked to the same character, you'll see a popup asking which persona to use each time you open or start a new chat with that character (unless a persona is bound to the chat).

### 3. Default Persona

Your **default persona** is used whenever there's no other relevant lock. The default persona is recognizable by a yellow border around its avatar.

- **To set/unset default**: Select the desired persona, then click the **<i class="fa-solid fa-crown"></i> Default** button under the "Connections" section (or use `/persona-lock type=default`).

Only one persona can be chosen as the default persona.

### Temporary Persona

If any of the three connection options connects a persona to the current character/chat, you can still choose to use a different persona. This persona will be marked in the persona panel as "Temporary Persona". Any reload of the browser window or switch to a different chat and back will reset it to the linked persona again.

You can manually *convert* a Temporary Persona to be persistently connected by linking it to the chat.

## Global Persona Settings

All settings under the **Current Persona** are saved per-persona. A few global settings exist too, those can be found under **Global Persona Settings** in the Persona Management panel.

1. **Show notifications on switching personas**
   - Enables persona-related toast messages (e.g., "Persona Auto Selected", "Temporary Persona").

2. **Allow multiple persona connections per character**
   - When **enabled**, you can link multiple personas to a single character. Opening that character's chat will prompt you which persona to use. If disabled, only one persona can be connected to a character at a time.

3. **Auto-lock a chosen persona to the chat**
   - When **enabled**, any time you select a persona (manually or by auto-selection) or create a new chat, it locks that persona to the chat.  
   This combined with "Allow multiple" provides the option to have a persona selection per character, but keep it bound once chosen for a chat.

## Slash Commands for Personas

### `/persona-lock type=<type?>`

- `chat` locks the current persona to your active chat.
- `character` locks the current persona to the character in use.
- `none` (or no argument) unlocks/clears the persona lock for the current context.
- If used without arguments, it returns the current lock state (or an error if none is set).
- The lock state can be chosen via `on`, `off` or `toggle`. Default is toggle.

### `/persona <name>`

- Quickly switch your active persona by name without opening the Persona Management panel.
- Example: `/persona Blaze`.
- Using `mode=temp` allows to temporarily set your name of the **current** persona, even though a persona with the same name might already exist (preserving your current avatar and description).

### `/persona-sync`

- Re-attributes all user messages in the active chat to the **current** persona and its name.

> **Note:** The older `/lock` and `/unlock` commands remain for backward compatibility but may be removed in the future. Use `/persona-lock` instead.

## Pro Tips

1. **Switching personas mid-chat** doesn't re-attribute your past user messages to the new persona; those remain attributed to whichever persona you were using at the time.
2. **Batch re-attribution**: If you ever need all prior messages to match a new persona, hit the **sync** button or use `/persona-sync`.
3. **Replace persona images** without losing description or locks by choosing your persona and clicking the **<i class="fa-solid fa-images"></i> Change Persona Image** button.
4. **Character link popups**: If multiple personas are linked to the same character, you'll get a popup to pick which persona each time you open the chat. This is a handy way to have a small selection of personas to choose from for specific characters.
5. **Backups**: You can back up your entire persona list (names, character connections, descriptions) with the **Backup** button in Persona Management, and restore it later if needed.

!!!tip Backup Remarks

- Images and chat connections are not saved together with personas and will not be backed up via this.
- These backups are not designed to be shared, as they contain internal links.

!!!
