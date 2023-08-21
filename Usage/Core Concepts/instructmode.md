# Instruct Mode

Instruct Mode allows you to adjust the prompting for instruction-following models, such as Alpaca, Metharme, WizardLM, etc.

**This is not supported for Chat Completions API.**

### Instruct Mode Settings

#### System Prompt

Added to the beginning of each prompt. Should define the instructions for the model to follow. Supports substitutions via any of the supported \{\{macro\}\} parameters.

For example:

> Write one reply in internet RP style for \{\{char\}\}. Be verbose and creative.

#### Presets

Provides ready-made presets with prompts and sequences for some well-known instruct models.

*Changing a preset resets your system prompt to default! Don't forget to save your preset if you made any changes you don't want to lose.*

#### Activation Regex

If defined as a valid regular expression, when connected to a model and its name matches this regex, will automatically select this preset.

Instruct mode needs to be enabled prior. Only the first regex match across presets will be selected (evaluated in alphabetical order). 

#### Default preset (heart icon)

If toggled, connecting to an API will automatically select this preset if no other presets were triggered by the regex match.

Instruct mode needs to be enabled prior. Only one preset can be marked as default.

#### Input Sequence

Text added before the user's input.

#### Output Sequence

Text added before the character's reply.

#### Last Sequence

Text added to the last line of the prompt. If not defined, the Output Sequence will be used in its place.

#### System Sequence

Text added before the system prompt.

#### Separator Sequence

Text added after the character reply to separate the chat history logs.

#### Stop Sequence

Text that denotes the end of the reply. Will be trimmed from the output text.

#### Include Names

If enabled, prepend characters and user names to chat history logs after inserting the sequences.

*Automatically enabled for group chats and messages sent using personas, unless **Force for Groups and Personas** setting is unchecked!*

#### Replace Macro in Sequences

If enabled, known \{\{macro\}\} substitutions will be replaced if defined in Input or Output sequences.

#### Wrap Sequences with Newline

Each sequence text will be wrapped with newline characters when inserted into the prompt. Required for Alpaca and its derivatives.
