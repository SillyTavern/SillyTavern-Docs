---
icon: report
order: 170
expanded: false
---

# Chatting

When you are [connected to an API](/Usage/API_Connections/index.md), send messages to the AI by typing in the chat bar at the bottom of the screen. Then click <i class="fa-solid fa-paper-plane"></i> **Send** or press Enter. 
![Chat bar](/static/chatbox.png)

The AI will respond with a message that continues the conversation.

![Chat message](/static/chatmessage.png)

You can now:

* **Send another message**
* **Swipe the response**: Click the <i class="fa-solid fa-chevron-right"></i> **Swipe** button on the message to generate a different response.
* **Edit the message**: Click the <i class="fa-solid fa-pencil"></i> **Edit** button on any message to [edit the message content](#edit-message-content).
* **Message actions**: Click the <i class="fa-solid fa-ellipsis"></i> **Message actions** button on a message for more [message options](#message-actions-panel) like translation, image generation, and story branching.
* **Chat options**: Click the <i class="fa-solid fa-bars"></i> **Options** button next to the chat bar for more [chat options](#chat-options-panel) like author's notes and chat file management.

!!! Edit and swipe
If you wish you'd said something different, you can edit your message and then swipe the AI's response to get a new one.
!!!

!!! Keyboard shortcuts
You can also use the **Right** arrow key to swipe, and the **Up** arrow key to edit the last message in the chat. For more hotkeys, use the `/help hotkeys` [slash command](/Usage/Chatting/slashcommands.md) in the chat or check the [HotKeys](/Usage/Chatting/hotkeys.md) page.
!!!

## Message actions panel

Manage individual chat messages via the ellipsis (•••) button on the message.

To display these options for all messages in your chats, enable the [Expand Message Actions](/Usage/User_Settings/uicustomization.md#theme-toggles) setting in your user settings.

### Core Functions

* <i class="fa-solid fa-language"></i> **Translate**: Convert message to different language
* <i class="fa-solid fa-paintbrush"></i> **Generate Image**: [Create an image](/extensions/Stable-Diffusion.md) from message content
* <i class="fa-solid fa-bullhorn"></i> **Narrate**: [Text-to-speech](/extensions/TTS.md) conversion
* <i class="fa-solid fa-square-poll-horizontal"></i> **Prompt**: View the generation prompt and token usage

### Message Visibility

* <i class="fa-solid fa-eye"></i> **Included**: AI sees this message; click to exclude it
* <i class="fa-solid fa-eye-slash"></i> **Excluded**: AI does not see this message; click to include it

### Content Management

* <i class="fa-solid fa-paperclip"></i> **Embed**: [Attach files or images](/Usage/Characters/data-bank.md#about-documents)
* <i class="fa-solid fa-flag-checkered"></i> **Checkpoint**: Create story checkpoint
* <i class="fa-solid fa-flag"></i> **Checkpoint Navigation**: Click to open checkpoint chat, Shift+Click to update
  existing checkpoint
* <i class="fa-solid fa-code-branch"></i> **Branch**: Start alternate story path
* <i class="fa-solid fa-copy"></i> **Copy**: Copy message text
* <i class="fa-solid fa-pencil"></i> **Edit**: Edit message content

## Edit message content

A compact panel of message manipulation tools that appears when you <i class="fa-solid fa-pencil"></i> **Edit** a chat
message. 

### Core Actions

* <i class="fa-solid fa-check"></i> **Confirm**: Save message changes
* <i class="fa-solid fa-xmark"></i> **Cancel**: Discard message changes

### Message Operations

* <i class="fa-solid fa-copy"></i> **Copy**: Duplicate message content
* <i class="fa-solid fa-trash-can"></i> **Delete**: Remove message

### Message Position

* <i class="fa-solid fa-chevron-up"></i> **Move Up**: Shift message higher in chat
* <i class="fa-solid fa-chevron-down"></i> **Move Down**: Shift message lower in chat

Note: Movement controls may be disabled based on message position in chat history.

## Chat options panel

Manage chat settings and operations via the <i class="fa-solid fa-bars"></i> **Options** button at the bottom left of
the chat interface.

### Display Controls

* <i class="fa-lg fa-solid fa-times"></i> **Close chat**: Exit current chat session
* <i class="fa-lg fa-solid fa-cog"></i> **Toggle Panels**: Show/hide [interface panels](/Usage/index.md#control-panels)

### Generation Settings

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[Author's Note](/Usage/Characters/Author's-Note.md)**: Custom context instructions
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[CFG Scale](/Usage/Prompts/CFG.md)**: Adjust response creativity
* <i class="fa-lg fa-solid fa-pie-chart"></i> **Token Probabilities**: View token generation stats

### Chat Navigation

* <i class="fa-lg fa-solid fa-left-long"></i> **Back to parent chat**: Return to main conversation
* <i class="fa-lg fa-solid fa-flag"></i> **Save checkpoint**: Create story checkpoint
* <i class="fa-lg fa-solid fa-people-arrows"></i> **Convert to group**: Transform into [group chat](/Usage/Characters/groupchats.md)

### Chat Management

* <i class="fa-lg fa-solid fa-comments"></i> **Start new chat**: Begin fresh conversation
* <i class="fa-lg fa-solid fa-address-book"></i> **Manage chat files**: [Chat file operations](/Usage/Characters/chatfilemanagement.md) such as import, export, and renaming

### Message Controls

* <i class="fa-lg fa-solid fa-trash-can"></i> **Delete messages**: Select and remove multiple messages
* <i class="fa-lg fa-solid fa-repeat"></i> **Regenerate**: Create new response
* <i class="fa-lg fa-solid fa-user-secret"></i> **Impersonate**: AI writes message as user
* <i class="fa-lg fa-solid fa-arrow-right"></i> **Continue**: Extend last message

Note: Some options may be hidden depending on context and chat state.
