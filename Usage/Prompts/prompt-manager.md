---
order: prompts-100
templating: false
---

# Prompt Manager

The Prompt Manager is a system that provides more control over the [prompt-building](prompts.md) strategy for Chat Completion APIs.

!!! Applies to: Chat Completion APIs
For equivalent settings in Text Completion APIs, use [Advanced Formatting](advancedformatting.md).
!!!

!!!tip Naming Presets
If a preset shares a name with one of your character cards, it will be automatically selected when starting a chat with that character. Name presets something unique to avoid this behavior.
!!!

Access the Prompt Manager by clicking on the "AI Response Configuration" button in the navigation bar. The Prompt Manager is located below the [common settings](/Usage/Common-Settings.md) panel.

## Quick Prompts Edit

Provides space to quickly edit common prompt sections, such as **Main Prompt**, **Auxiliary Prompt**, and **Post-History Instructions**. More information on these prompts can be found on the [prompt-building](prompts.md) page.

## Utility Prompts

These prompts are sent to the Chat Completion model to help it understand the information being sent to it, or to instruct it to act in specific ways during certain types of interactions.

### Format Templates

!!!tip
If the format template is not set, the information will be sent as-is, without any wrapping.
!!!

These are string templates used to wrap the information pulled from [World Info](/Usage/worldinfo.md) and [Character Cards](/Usage/Characters/characterdesign.md).

A special marker is used to indicate where the information should be inserted:

- `{0}` for the World Info format template.
- `{{scenario}}` for the Scenario format template.
- `{{personality}}` for the Personality format template.

### Group Nudge Prompt Template

Used only in group chats. Placed at the end of the prompt to force a reply from a specific character.

Leave this empty to disable Group Nudge functionality.

### New Chat, New Group Chat, New Example Chat

These are sent before the chat history and before each [Example Dialogue](/Usage/Characters/characterdesign.md#examples-of-dialogue) block to inform the model where background information ends and chat history begins.

- **New Chat:** Used for individual chats.
- **New Group Chat:** Used for group chats.
- **New Example Chat:** Used for example dialogue blocks.

Leave these empty to disable this functionality.

### Continue Nudge

Sent at the end of the prompt to instruct the model on what to do when Continue is triggered, such as when the Continue button is pressed or when triggered by STScript.

!!! Chat Completion 'Continues'
Keep in mind that Chat Completion models handle Continues differently than **Text Completion** models, and may not always deliver seamless results regardless of your Continue Nudge.
!!!

### Replace Empty Message

Sends the contents of this field instead of a blank message when the text box is empty and **Send a message** is pressed.

## Character Names Behavior

Provides different strategies for instructing the model on how to associate messages with characters. If a Chat Completion model is having trouble determining which messages belong to which character, it may need a different strategy selected.

## Continue Postfix

When Continue is triggered, the 'continued' message returned by the model will have the selected Continue Postfix prepended to the beginning. For example, it can add a space before the continued text.

## Additional Settings

### Wrap in Quotes

!!!warning
Deprecated option. Prefer [Regex scripts](/extensions/Regex.md) instead.
!!!

Wraps the entire user message in hidden quotation marks before sending. This is useful for sessions where characters do not use quotes to indicate speech. If your session uses quotation marks to indicate speech, leave this unchecked.

### Continue Prefill

!!!warning
May not work with all Chat Completion sources.
!!!

Sends the Continue Nudge as an Assistant role message instead of a System message. If this is enabled, the Continue Nudge prompt will not be used.

### Squash system messages

!!!warning
Deprecated option. Prefer [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing) instead.
!!!

Combines consecutive System messages into a single combined message (excluding Example Dialogue).

### Enable web search

!!!
Not to be confused with the [Web Search extension](/extensions/WebSearch.md).
!!!

Enables web search capabilities provided by the Chat Completion backend. The prompt is usually enriched with search results by the model provider and may incur additional costs.

### Enable function calling

See [Function Calling](/For_Contributors/Function-Calling.md)

### Send inline images, Send inline videos

!!!
Not to be confused with the [Image Captioning extension](/extensions/captioning.md).
!!!

If the Chat Completion model has multimodal capabilities to process submitted images and videos, this toggles its ability to do so. To append media to the prompt, use the **Attach A File** option in the "Magic Wand" menu.

### Request inline images

!!!
Not to be confused with the [Image Generation extension](/extensions/Stable-Diffusion.md).
!!!

Allows the model to return image attachments.

### Use system prompt

!!!
Only supported by Google Gemini and Anthropic Claude backends.

Despite having very similar settings for these two, they are technically separate options, so they can be configured separately.
!!!

Merges all system messages up until the first message with a non-system role (User/Assistant) and sends them as a separate system instruction field.

## Reasoning Settings

If the Chat Completion model uses reasoning, these settings affect its visibility and functionality.

### Request model reasoning

See [Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend).

### Reasoning Effort

See [Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort).

## "Prompts"

The Prompt Manager forms the backbone of the prompt sent to the Chat Completion model. It controls what is sent as well as the *order* in which it is sent.

### The 'Prompts' Dropdown

Contains a dropdown list of all (non-default) prompts that the current Chat Completion preset includes. For one of these prompts to be added to the outgoing message, it needs to be selected from the dropdown list and then added to the Prompt Manager by pressing the **Insert prompt** button. To create a new prompt to add to this dropdown list, press the **New prompt** button. Once the new prompt is written and saved, it is added to the dropdown and can then be inserted.

### Prompts List

This is a drag-and-drop interface that lists the prompts selected to potentially be sent to the Chat Completion model. Prompts placed closer to the **top** of the interface are sent earlier. The **bottom** of the list is the **last thing** sent to the model (typically, this would be your **Post-History Instructions**).

!!! 'Pinned' prompts = Default prompts
The default prompts cannot be removed from the list of selected prompts. This includes Main Prompt, World Info (before/after), Persona Description, Character Description, Character Personality, Scenario, Enhance Definitions, Auxiliary Prompt, Chat Examples, Chat History, and Post-History Instructions. If these are not desired, they can be **toggled 'OFF'**, but not removed or deleted outright.
!!!

## Editing a Prompt

Clicking the **pencil button** on a prompt will bring you to the **Edit interface**. Here, you can edit the prompt directly.

!!! Make sure to save your changes!
To permanently save changes to these prompts in your Chat Completion preset, you must click the **Save** button in the bottom right of the **Edit interface**, as well as save the preset itself by using the **Save** button located at the top of the **AI Response Configuration** section! Otherwise, changes made will be lost when the Chat Completion preset is switched to a different one.
!!!

### Name

The name of the prompt. This is not sent to the Chat Completion model; it is for your reference within the Prompt Manager only.

### Role

Which role sends the prompt. You can choose between System, AI Assistant, or User.

### Triggers

The generation types for which this prompt is sent. If nothing is selected, the prompt will be sent for all generation types. If one or more are selected, the prompt will only be sent for those specific generation types:

- **Normal:** Regular message generation request.
- **Continue:** When the Continue button is pressed.
- **Impersonate:** When the Impersonate button is pressed.
- **Swipe:** When the generation is triggered by swiping.
- **Regenerate:** When the Regenerate button is pressed in solo chats.
- **Quiet:** Background generation requests, usually triggered by [extensions](/extensions/index.md) or [STscript](/For_Contributors/st-script.md) commands.

!!!
The "Regenerate" trigger is not available in group chats as it uses different regeneration logic: all messages from the last reply are deleted, and messages are queued using the "Normal" generation type according to the chosen [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies).
!!!

### Position

When Position is set to **Relative**, this prompt is sent where it's located in the drag-and-drop interface with all other prompts. When it is set to **In-Chat** and given a **Depth**, it is instead sent **within the Chat History** as the selected Role, and **ignores** the order of the drag-and-drop interface.

### Depth

When Position is set to **In-Chat**, this defines how deep the prompt is sent within the chat history. The higher the number, the deeper it is sent. For example, a Depth of 0 will be sent after the last chat message, a Depth of 1 will be sent before the last chat message, and a Depth of 2 will be sent before the second-to-last chat message, and so on.

### Order

!!!
Prompts that have the same Role and Depth will be grouped together and ordered by their Order value.
The order is as follows (from top to bottom): User, AI Assistant, System.
!!!

When Position is set to **In-Chat**, this defines the order in which the prompt is sent within the chat history. The lower the number, the earlier it is sent.

## Building Your Prompt: Tips and Tricks

Visit the [prompt-building](prompts.md) section of the SillyTavern documentation for more information on how to write effective prompts. The information can largely be applied to Chat Completion presets.
