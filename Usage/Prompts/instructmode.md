---
order: 80
route: /usage/core-concepts/instructmode/
---

# Instruct Mode

Instruct Mode allows you to adjust the prompting for instruction-following models trained on various prompt formats, such as Alpaca, ChatML, Llama2, etc.

!!! Applies to: Text Completion APIs
For equivalent settings in Chat Completion APIs, use [Prompt Manager](prompt-manager.md).
!!!

## API support

### Text Completion API

Fully supported. This includes:

* All of the sources under Text Completion
* KoboldAI Classic
* AI Horde

#### Choosing a formatting

A chosen instruct template must match the expectations of an actual model that is running on a backend.

This is usually reflected in a model card on HuggingFace, and some even provide SillyTavern-compatible JSON files.

Example: [NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### Chat Completion API (OpenAI, Claude, etc)

This is not supported **(and not needed)** for Chat Completion APIs. They use an entirely different prompt builder.

### NovelAI

While *technically* supported for NovelAI, none of their models were trained to understand instruct formatting. NovelAI models can use a special instruct module that is activated *automatically* when an instruction wrapped in curly braces is encountered in chat messages, so using Instruct Mode for the entire prompt will lead to **degraded quality** of the outputs.

Here's an example that auto-activates the instruct module for NovelAI:

```txt
User: { Write a happy song about Nintendo Switch. }
```

## Instruct Mode Settings

### System Prompt

!!!warning Recent change
The System Prompt is now a separate entity. See the [Advanced Formatting](advancedformatting.md#system-prompt) page for more details.
!!!

### Templates

Provides ready-made templates with sequences for some well-known instruct models.

*Changing a template resets the unsaved settings to the last saved state! Don't forget to save your template if you made any changes you don't want to lose.*

### Activation Regex

If defined as a valid regular expression, when connected to a model and its name matches this regex, will automatically select this template.

Instruct mode needs to be enabled prior. Only the first regex match across templates will be selected (evaluated in alphabetical order).

### Wrap Sequences with Newline

Each sequence text will be wrapped with newline characters when inserted into the prompt. Required for Alpaca and its derivatives.

Disable if you want to have full control over line terminators.

### Replace Macro in Sequences

If enabled, known \{\{macro\}\} substitutions will be replaced if defined in message wrapping sequences.

Also, a special \{\{name\}\} macro can be used in message prefixes to reference the actual name attached to a message (rather than a currently active \{\{char\}\} or \{\{user\}\}), which can be helpful when using group chats or /sendas command. If the name can't be determined, "System" is used as a fallback placeholder.

### Include Names

If enabled, prepend characters and user names to chat history logs after the prefix sequence.

The following options are available:

* **Never**: Do not add name prefixes before the message contents.
* **Groups and Past Personas**: Only add name prefixes to messages from group characters and past personas.
* **Always**: Always add name prefixes before the message contents.

### Sequences: Story String Wrapping

!!!warning Recent change
System Prompt wrapping has been removed and replaced with Story String wrapping.
!!!

Define how the Story String will be wrapped when the Position is set to "Default (top of context)"

#### Story String Prefix

Inserted before a Story String.

#### Story String Suffix

Inserted after a Story String.

### Sequences: Chat Messages Wrapping

These settings define how messages belonging to different roles will be wrapped upon building a prompt.

All prefix sequences will also be automatically used as stopping strings.

#### User Message Prefix

Inserted before a User message and as a last prompt line when impersonating.

#### User Message Suffix

Inserted after a User message.

#### Assistant Message Prefix

Inserted before an Assistant message and as a last prompt line when generating an AI reply.

#### Assistant Message Suffix

Inserted after an Assistant message

#### System Message Prefix

Inserted before a System (added by slash commands or extensions) message.

#### System Message Suffix

Inserted after a System message.

#### System same as User

If checked true, System messages will be using User role message sequences.

Otherwise, System messages use their own sequences (if not empty) or will not do any wrapping at all (if empty).

### Misc. Sequences

Various advanced configurations for finer tuning of the prompt building

#### First Assistant Prefix

Inserted before the first Assistant's message.

!!!info
Only the first message of the **chat history** counts, not the message that actually goes into the prompt first!
!!!

#### Last Assistant Prefix

Inserted before the last Assistant's message or as a last prompt line when generating an AI reply.

!!!info
Not used when generating text in a background (e.g. Stable Diffusion prompts or Summaries). System Instruction Prefix or Regular Assistant Prefix will be used instead.
!!!

#### System Instruction Prefix

Inserted as a last prompt line when generating neutral/system text in a background (e.g. Stable Diffusion prompts or Summaries).

#### User Filler Message

Will be inserted at the start of the chat history if it doesn't start with a User message.

**Use case:** when an instruct format *strictly requires* prompts to be user-first and have messages with alternating roles only, examples: Llama 2 Chat, Mistral Instruct.

#### Stop Sequence

Text that denotes the end of the reply. Also sent as a stopping string to the backend API.

If a stop sequence is generated, everything past it will be removed from the output (including the sequence itself).
