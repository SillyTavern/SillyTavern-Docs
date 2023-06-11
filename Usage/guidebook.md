---
order: 50
icon: book
---
# SillyTavern Guidebook

This page contains descriptions for many of SillyTavern's features and core concepts.

## Instruct Mode

Instruct Mode allows you to adjust the prompting for instruction-following models, such as Alpaca, Metharme, WizardLM, etc.

**This is not supported for OpenAI API.**

### Instruct Mode Settings

#### System Prompt

Added to the beginning of each prompt. Should define the instructions for the model to follow.

For example:

```
Write one reply in internet RP style for \{\{char\}\}. Be verbose and creative.
```

#### Presets

Provides ready-made presets with prompts and sequences for some well-known instruct models.

*Changing a preset resets your system prompt to default!*

#### Input Sequence

Text added before the user's input.

#### Output Sequence

Text added before the character's reply.

#### System Sequence

Text added before the system prompt.

#### Separator Sequence

Text added after the character reply to separate the chat history logs.

#### Stop Sequence

Text that denotes the end of the reply. Will be trimmed from the output text.

#### Include Names

If enabled, prepend character and user names to chat history logs after inserting the sequences.

*Always enabled for group chats!*

#### Wrap Sequences with Newline

Each sequence text will be wrapped with newline characters when inserted to the prompt. Required for Alpaca and its derivatives.

## Chat import

**Import chats into SillyTavern**

To import Character.AI chats, use this tool: [https://github.com/0x000011b/characterai-dumper](https://github.com/0x000011b/characterai-dumper).

## Tokenizer

**Important: This section doesn't apply to OpenAI API. SillyTavern will always use a matching tokenizer for OpenAI models.**

A tokenizer is a tool that breaks down a piece of text into smaller units called tokens. These tokens can be individual words or even parts of words, such as prefixes, suffixes, or punctuation. A rule of thumb is that one token generally corresponds to 3~4 characters of text.

SillyTavern can use the following tokenizers while forming a request to the AI backend:

1. None. Each token is estimated to be ~3.3 characters, rounded up to the nearest integer. **Try this if your prompts get cut off on high context lengths.** This approach is used by KoboldAI Lite.
2. GPT-3 tokenizer. **Use to get more accurate counts on OpenAI character cards.** Can be previewed here: [OpenAI Tokenizer](https://platform.openai.com/tokenizer).
3. (Legacy) GPT-2/3 tokenizer. Used by original TavernAI. **Pick this if you're unsure.** More info: [gpt-2-3-tokenizer](https://github.com/josephrocca/gpt-2-3-tokenizer).
4. Sentencepiece tokenizer. Used by LLaMA model family: Alpaca, Vicuna, Koala, etc. **Pick if you use a LLaMA model.**
5. NerdStash tokenizer. Used by NovelAI's Krake model. **Pick if you use the Krake model.**
6. NerdStash v2 tokenizer. Used by NovelAI's Clio model. **Pick if you use the Clio model.**

## Token Padding

**Important: This section doesn't apply to OpenAI API. SillyTavern will always use a matching tokenizer for OpenAI models.**

SillyTavern cannot use a proper tokenizer provided by the model running on a remote instance of KoboldAI or Oobabooga's TextGen, so all token counts assumed during prompt generation are estimated based on the selected [tokenizer](#tokenizer) type.

Since the results of tokenization can be inaccurate on context sizes close to the model-defined maximum, some parts of the prompt may be trimmed or dropped, which may negatively affect the coherence of character definitions.

To prevent this, SillyTavern allocates a portion of the context size as padding to avoid adding more chat items than the model can accommodate. If you find that some part of the prompt is trimmed even with the most-matching tokenizer selected, adjust the padding so the description is not truncated.

You can input negative values for reverse padding, which allows allocating more than the set maximum amount of tokens.

## Advanced Formatting

The settings provided in this section allow for more control over the prompt building strategy. Most specifics of the prompt building depend on whether a Pygmalion model is selected or special formatting is force-enabled. The core differences between the formatting schemas are listed below.

### Custom Chat Separator

Overrides the default separators controlled by "Disable example chats formatting" and "Disable chat start formatting" options (see below).

### For *Pygmalion* formatting

#### Disable description formatting

`**NAME's Persona:**`won't be prepended to the content of your character's Description box.

#### Disable scenario formatting

`**Scenario:**`won't be prepended to the content of your character's Scenario box.

#### Disable personality formatting

`**Personality:**`won't be prepended to the content of your character's Personality box.

#### Disable example chats formatting

`<START>` won't be added at the beginning of each example message block.
*(If custom separator is not set)*

#### Disable chat start formatting

`<START>` won't be added between the character card and the chat log.
*(If custom separator is not set)*

#### Always add character's name to prompt

Doesn't do anything (Included in Pygmalion formatting).

### For *non-Pygmalion* formatting

#### Disable description formatting

Has no effect.

#### Disable scenario formatting

`**Circumstances and context of the dialogue:**`won't be prepended to the content of your character's Scenario box.

#### Disable personality formatting

`**NAME's personality:**`won't be prepended to the content of your character's Personality box.

#### Disable example chats formatting

`This is how **Character** should talk` won't be added at the beginning of each example message block.
*(If custom separator is not set)*

#### Disable chat start formatting

`Then the roleplay chat between **User** and **Character** begins` won't be added between the character card and the chat log.
*(If custom separator is not set)*

#### Always add character's name to prompt

Appends character's name to the prompt to force the model to complete the message as the character:

```
** OTHER CONTEXT HERE **
Character:
```

## Group Chats

### Reply order strategies

Decides how characters in group chats are drafted for their replies.

#### Natural order

Tries to simulate the flow of a real human conversation. The algorithm is as follows:

1. Mentions of the group member names are extracted from the last message in chat.

Only whole words are recognized as mentions! If your character's name is "Misaka Mikoto", they will reply only activate on "Misaka" or "Mikoto", but never to "Misa", "Railgun", etc.

Unless "Allow bot responses to self" setting is enabled, characters won't reply to mentions of their name in their own message!

2. Characters are activated by the "Talkativeness" factor.

Talkativeness defines how often the character speaks if they were not mentioned. Adjust this value on "Advanced definitions" screen in character editor. Slider values are on a linear scale from **0% / Shy** (character never talks unless mentioned) to **100% / Chatty** (character always replies). Default value for new characters is 50% chance.

3. Random character is selected.

If no characters were activated at previous steps, one speaker is selected randomly, ignoring all other conditions.

#### List order

Characters are drafted based on the order they are presented in group members list. No other rules apply.

## Multigen

*This feature provides a pseudo-streaming functionality which conflicts with token streaming. When Multigen is enabled and generation API supports streaming, only Multigen streaming will be used.*

SillyTavern tries to create faster and longer responses by chaining the generation using smaller batches.

### Default settings

First batch = 50 tokens

Next batches = 30 tokens

### Algorithm

1. Generate the first batch (if amount of generation setting is more than batch length).
2. Generate next batch of tokens until one of the stopping conditions is reached.
3. Append the generated text to the next cycle's prompt.

### Stopping conditions

1. Generated enough text.
2. Character starts speaking for You.
3. &lt;|endoftext|&gt; token reached.
4. No text generated.
5. Stop sequence generated. (Instruct mode only)

## User Settings

### Message Sound

To play your own custom sound on receiving a new message from bot, replace the following MP3 file in your SillyTavern folder:

`public/sounds/message.mp3`

Plays at 80% volume.

If "Background Sound Only" option is enabled, the sound plays only if SillyTavern window is **unfocused**.

### Formulas Rendering

Enables math formulas rendering using the [showdown-katex](https://obedm503.github.io/showdown-katex/) package.

The following formatting rules are supported:

#### LaTeX syntax

```
$$ formula goes here $$
```

#### Asciimath syntax

```
formula goes here $
```

More information: [KaTeX](https://katex.org/)
