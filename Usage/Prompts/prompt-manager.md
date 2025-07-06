---
order: prompts-100

---

# Prompt Manager

The Prompt Manager is a system that allows for more control over the [prompt-building](prompts.md) strategy for Chat Completion APIs.

!!! Applies to: Chat Completion APIs
For equivalent settings in Text Completion APIs, use [Advanced Formatting](advancedformatting.md).
!!!

!!! Naming Prompt Manager Presets
Please bear in mind that if a preset shares a name with one of your character cards, it will get automatically selected when starting a chat with that particular character. Name presets something unique to avoid this behaviour. 
!!!

Access Prompt Manager by clicking on the "AI Response Configuration" button in the navigation bar. Prompt manager is below the [common settings](/Usage/Common-Settings.md) panel.



## Quick Prompts Edit

Provides space to quickly edit some of the common prompt sections, such as **Main Prompt**, **Auxiliary Prompt**, and the **Post-History Instructions**. More information on these prompts can be found on the [prompt-building](prompts.md) page.



## Utility Prompts

These prompts are sent to the Chat Completion model to help it understand the information sent to it, or asks it to act specific ways when certain factors are met.

### Format Templates

These are the tags used to insert information from World Info and Characters.The default SillyTavern values are provided.

### Group Nudge prompt template

Used only in Group Chats. It instructs the model to send a message as a specified Character.

### New Chat, New Group Chat, New Example Chat

These are sent at the beginning of chats and Example Dialogue to inform the model that the content below these lines is a new chat or a Character's Example Dialogue.

### Continue Nudge

Instructs the model on what to do when Continue is triggered, such as when the Continue button is pressed or when triggered by STScript.

!!! Chat Completion 'Continues'
Please bear in mind that Chat Completion models handle Continues differently than **Text Completion** models, and may not always deliver seamless results regardless of your Continue Nudge.
!!!

### Replace Empty Message

Sends the box's contents instead of a blank message when the text box is empty and **Send a message** is hit.



## Character Names Behavior

Provides different strategies for instructing the model on how to associate messages with Characters. If a Chat Completion model is having trouble parsing which messages belong to which Character, it may need a different strategy selected.



## Continue Postfix

When Continue is triggered, the 'continued' message returned by the model will have the selected Continue Postfix appended to the front. For example, it can add a space before the continued text.



## Additional Settings

### Wrap in Quotes

Wraps entire user message in hidden quotation marks before sending. This is for sessions where the Characters do not use quotes to indicate their speech. If your session uses quotation marks to indicate speech, leave this unchecked.

### Continue Prefill

Continue sends the Continue Nudge as the Assistant role instead of a System message.

### Squash system messages

Combines consecutive System messages as one combined message (excluding Example Dialogue). This may improve coherence for some models, but it may be detrimental to others.

### Enable web search

If the Chat Completion model has the capability to search the web, this enables or disables its ability to do so.

### Enable function calling

Allows extensions to use function calling. Some extensions require it for their features to work properly.

### Send inline images, Send inline videos

If the Chat Completion model has the capability to process submitted images and videos, this enables or disables its ability to do so.

### Request inline images

Allows the model to return image attachments.

### Use system prompt

Merges all system messages up until the first message with a non-system role (User/Assistant) and sends them as a system_instruction field. Whether a Chat Completion model requires this varies.



## Reasoning Settings

If the Chat Completion model uses reasoning, these settings affect its visibility and functionality.

### Request model reasoning

Toggles the visibility of the Chat Completion model's reasoning process.

!!! Reasoning Models Still Reason!
The 'Request model reasoning' setting only affects the **visibility** of the reasoning. Settings on the actual content of the model's reasoning are available in the **Reasoning Effort** dropdown!
!!!

### Reasoning Effort

This setting may alter how many tokens the reasoning portion of the model's output is allocated. This can affect the length of time spent waiting for the output, as well as affecting the quality of the reasoning itself. The functionality of this setting depends on the specific model. When set to 'Auto', the model uses its defaults.

!!! Effort =/= Quality!
Some Chat Completion models may give responses of higher quality when they reason *less*. Some models are not specifically geared towards roleplay or storytelling, and allowing these models to reason for longer may be a detriment to the creativity of the response. This is subjective, so you may want to experiment with this setting to see how it affects the model's output quality.
!!!



## The Prompt Manager

The Prompt Manager is the bones of the prompt sent to the Chat Completion model. It affects what is sent as well as what *order* it is sent.

### The 'Prompts' dropdown

Contains a dropdown list of all of the (non-default) prompts that the current Chat Completion Preset has included. In order for one of these prompts to be added to the outgoing message, it needs to be selected from the dropdown list and then added to the Prompt Manager by pressing the **Insert prompt** button. (To create a new prompt to add to this dropdown list, press the **New prompt** button. Once the new prompt is written and saved, it is added to the dropdown and can then be inserted.

### Currently Selected Prompts

This is a drag-and-drop interface which lists the prompts selected to potentially be sent to the Chat Completion model. Prompts placed closer to the **top** of the interface get sent earlier. The **bottom** of the list is the **very last thing** sent to the model (typically, this would be for your **Post-History Instructions**.)

!!! 'Pinned' prompts = Default prompts
The default prompts cannot be removed from the list of selected prompts. This includes Main Prompt, World Info (before/after), Persona Description, Character Description, Character Personality, Scenario, Enhance Definitions, Auxiliary Prompt, Chat Examples, Chat History, and Post-History Instructions. If these are not desired, they can be **toggled 'OFF'**, but not outright removed or deleted.
!!!



## Editing a Prompt

Clicking the **pencil button** on a prompt will bring you to the **Edit interface**. Here, you can edit the prompt directly.

!!! Make sure to save your changes!
In order to permanently save changes to these prompts in your Chat Completion preset, you must hit the **Save floppy disk button** in the bottom right of the **Edit interface**, as well as the preset itself by using the **Save floppy disk button** located at the top of the **AI Response Configuration section**! Otherwise, changes made will be lost when the Chat Completion preset is switched to a different one.
!!!

### Name

The name of the prompt. This is not sent to the Chat Completion model, it is for your own reference within Prompt Manager only.

### Role

Which role sends the prompt. Can choose between System, AI Assistant, or User.

### Position

When Position is set to **Relative**, this prompt is sent where it's located in the drag-and-drop interface with every other prompt. When it is set to **In-Chat** and given a **Depth**, it is instead sent **within the Chat History** as the selected Role, and **ignores** the order of the drag-and-drop interface.



## Building Your Prompt: Tips and Tricks

Visit the [prompt-building](prompts.md) section of the SillyTavern Documentation for more info on how to write effective prompts. The information can largely be applied to Chat Completion presets.
