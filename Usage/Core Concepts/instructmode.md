# Instruct Mode

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
