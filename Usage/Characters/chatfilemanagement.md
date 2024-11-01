---
order: character-15
route: /usage/core-concepts/chatfilemanagement
---

# Chat File Management

This page describes the ways you can manage your AI chat files.

!!! info Note
Some of these options are available in the "Manage chat files" dialog that opens from the bottom left options menu.
!!!

## Solo Chats vs Group Chats

The simplest way to use a character card is a Solo chat; just click on their card and start chatting.

Once you have a few character cards, you can also use the "Create New Chat Group" button to create a [group chat](/Usage/Characters/groupchats.md) including multiple characters which will then interact with each other and you.

## Chat import

**Import chats from Character.AI into SillyTavern.**

To import Character.AI chats and bots, use the CAI Tools browser extension: [https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools).

Other programs and tools that you can import chats from include:

* TavernAI (original): [https://github.com/TavernAI/TavernAI](https://github.com/TavernAI/TavernAI)
* Text Generation WebUI (oobabooga): [https://github.com/oobabooga/text-generation-webui](https://github.com/oobabooga/text-generation-webui)
* Agnai: [https://github.com/agnaistic/agnai](https://github.com/agnaistic/agnai)

## Export as .jsonl

When clicking on "Manage chat files", each entry on the the chat file list will have a button to export it in a format that can then be re-imported as is. Use this to share or migrate chats including all their metadata (but excluding images and file attachments).

If you're mindful of privacy, be sure to inspect the exported JSONL file and scrub anything you don't want to share.

## Export as .txt

You can also export a simplified text-only version with the "Download chat as plain text document" button. It can't be re-imported again as it loses important metadata!

## Checkpoints

"Checkpoints" are clones of the current chat, in that they copy all messages from the given chat up to a certain point, and they store a link to the source (by chat file name).

From the three dots button at the right of each chat message, you have two ways to create checkpoints:

* "Create Branch" will clone the current chat up to that message and switch to it
* "Create Checkpoint" will clone current chat up to that message, ask for a name and create it but NOT switch to it

You can think of them as roughly as "open link in new tab" and "open link in new tab in the background" in a browser.

You can go back to the parent from a checkpoint by entering the burger menu button on the left of the message text box, then clicking "Back to parent chat".

## Rename Chat

By default chat files are given a named with the date and time they were started.

You can change this by clicking the pencil icon and typing in a new name.

Note that this will break links to that chat from checkpoints (since they are linked by chat file name).
