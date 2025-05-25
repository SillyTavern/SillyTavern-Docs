---
tags: ['>=1.13.0']
icon: people
---

# Welcome Page Assistants

SillyTavern features a Welcome Screen that can greet you with a designated "Assistant" character. This screen appears when you launch SillyTavern without an active chat or after you close your last chat session.

!!! Note
If you don't see a Welcome Screen on app startup, make sure the "Auto-Load Last Chat" option is disabled in the "Chat/Message Handling" section of the **<i class="fa-solid fa-user-cog"></i> User Settings** panel. If this option is enabled, SillyTavern will automatically load your last chat instead of showing the Welcome Screen.
!!!

## The Welcome Screen

When no chat is active, the Welcome Screen provides several useful elements:

* **SillyTavern Version:** Displays the application's logo and current version.
* **Quick Links:** Easy access to:
  * **Docs:** Opens the official SillyTavern documentation (you're already here!).
  * **GitHub:** Takes you to the SillyTavern GitHub repository (<https://github.com/SillyTavern/SillyTavern>).
  * **Discord:** Provides a link to the official SillyTavern Discord server (<https://discord.gg/sillytavern>).
* **Temporary Chat Button:** Allows you to quickly start a new, temporary chat session with the default neutral assistant, which won't be saved to your chat history unless you explicitly save it.
* **Recent Chats Section:** Lists your recent conversations for quick access. You can:
  * Show or hide this section.
  * Expand the list if more than 3 chats are available (up to 15 recent chats).

## Temporary Chat

!!! Note
Due to a technical limitation, the Temporary Chat feature will not use your customized Welcome Page Assistant. It will always start an empty chat without any additional prompts or character information.
!!!

The Temporary Chat button allows you to quickly start a new chat session without saving it to your chat history. This is useful for testing or casual conversations without cluttering your saved chats. This chat will be deleted as soon as you close it or switch to another chat.

* **Save** button will allow you to export the temporary chat as a JSONL file, which you can then import later.
* **Load** button will allow you to restore a previously saved temporary chat file.

## What is a Welcome Page Assistant?

A Welcome Page Assistant is a character you choose to be featured on the Welcome Screen. This allows for a personalized greeting and a quick way to start a chat with a familiar character right from the start.

### Setting and Unsetting an Assistant

You can choose any of your characters to act as your Welcome Page Assistant.

**To set an assistant:**

1. Navigate to the **Character Management** panel (usually found in the right-hand sidebar via the <i class="fa-solid fa-address-card"></i> icon).
2. Find the character you wish to set as your assistant in the list.
3. Click on "More..." and select **"Set / Unset as Welcome Page Assistant"** from the dropdown menu.
4. A small icon (<i class="fa-solid fa-user-graduate"></i>) will appear next to the character's name, indicating they are now your active Welcome Page Assistant.

**To unset an assistant:**

1. Go to the Character Management panel.
2. Locate your current Welcome Page Assistant (they will have the <i class="fa-solid fa-user-graduate"></i> icon).
3. Click on "More..." and select **"Set / Unset as Welcome Page Assistant"** again.
4. The character will no longer be your assistant, and the <i class="fa-solid fa-user-graduate"></i> icon will disappear.
5. SillyTavern will revert to using the Default Assistant (see below).

### Interacting with the Assistant

Once the Welcome Screen is displayed with your chosen assistant, simply type your message into the chat input bar at the bottom of the screen and press Enter or click the send button. This will start a new chat session with your Welcome Page Assistant.

To open a previous chat with the assistant, use the Recent Chats section or find the chat in the **Manage chat files** dialog (accessible via the **<i class="fa-solid fa-bars"></i> Options** menu).

## Default Assistant

SillyTavern will automatically create a default character named "Assistant" when you interact with the Welcome Screen for the first time. This character serves as a fallback option if you haven't set a specific character as your Welcome Page Assistant.

The default assistant doesn't have any specific prompts attached to it, and you're free to customize it as you like (e.g. renaming, adding a picture, or setting a personality).

* If you haven't explicitly set a character as your Welcome Page Assistant, this default assistant will be used.
* If you unset your chosen assistant, the system will revert to this default assistant.
* If a character you had set as an assistant is deleted, the system will also revert to the default assistant.

**Note:** You cannot "unset" the default system assistant in the same way you unset a character you've chosen. To change from the default assistant, you must set one of your other characters as the assistant.
