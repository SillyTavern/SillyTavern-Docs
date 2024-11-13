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
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Token Probabilities](#token-probabilities-panel)**: View token generation stats

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

## Token Probabilities Panel

The Token Probabilities panel lets you peek into the AI's sampling and generation process during text generation. It shows you not just what the AI wrote, but what other words and phrases it considered at each point in the text.

When you click any token (word, punctuation, or formatting character) in the generated text, the panel displays alternative tokens the AI considered at that position, along with their probability scores. This gives you insight into the AI's "thought process" and shows other directions the response could have taken. Looking at these alternatives can help you understand whether there were several likely options or a single clear choice.

If you see a token that you think the AI should have chosen differently, choose an alternative and the message will regenerate from that point forward, potentially giving you a different response.

### Rerolling

If you change a specific token and regenerate the response, the part of the new response before the changed token will be the same as the original response. This part is shown in gray. Since it was not generated, there is no probability information for this part.

You may like to see other responses that could have been generated based on your alternative token.

You can click the gray portion to "reroll" the generation, giving you a new variation of the text. Clicking any part of the gray portion will keep the entire gray portion and regenerate the entire white/tinted portion.

Holding Ctrl while clicking a token in the gray portion will retain the gray portion up to the clicked token and regenerate the rest of the text. Your choice of alternative token can not be kept in this case.

### Controls

**Token Display**:

* Generated text is split into individual tokens
* Each token is interactive, click a token to see alternatives considered by the AI
* Tokens are tinted as a visual aid but this does not indicate probability
* Special characters (spaces, newlines) are visibly marked

**Token Selection**:

* Click a token to view alternatives
* Click an alternative to replace the token and regenerate the response
* Hover over a token to see its raw log-probability score

**Window Controls**:

* <i class="fa-solid fa-grip"></i> Drag handle for panel repositioning (MovingUI only)
* <i class="fa-solid fa-window-maximize"></i> Maximize/restore panel size
* <i class="fa-solid fa-circle-chevron-up"></i> Expand/collapse panel content
* <i class="fa-solid fa-circle-xmark"></i> Close panel

### Availability

You must select **Request token probabilities** in [User Settings](/Usage/User_Settings/User_Settings.md#chatmessage-handling) to enable this feature.

Token probabilities are only available for the most recent message, and are not saved to the chat. If token probability information is no longer available for a message, the panel will display a message indicating this.

Token probabilities are not available when using Smooth Streaming.

Token probabilities are not available from all APIs. If you are using an API that does not support token probabilities, the panel will open but will not display any information.

#### Text Completion
* **LlamaCPP**: Available
* **Text Generation WebUI** (oobabooga): Available
* **TabbyAPI**: Available
* **NovelAI**: Available
* **KoboldCPP**: Appears to be unavailable
* **Ollama**: Appears to be unavailable
* **OpenRouter Text**: Appears to be unavailable

#### Chat Completion
* **OpenAI**: Available, but rerolling is not supported
* **Anthropic**: Appears to be unavailable
* **Google AI Studio**: Appears to be unavailable
* **OpenRouter Chat**: Appears to be unavailable
* **Ollama**: Appears to be unavailable
