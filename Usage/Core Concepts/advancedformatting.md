# Advanced Formatting

The settings provided in this section allow for more control over the prompt-building strategy.


### Context Template

**Most of the settings here do not apply to Chat Completions APIs as they are governed by the prompt manager system instead.**

Usually, AI models require to provide the character data to them in some specific way. SillyTavern includes a list of pre-made conversion rules for different models, but you may customize them however you like.

#### Story string

This field is a template for pre-chat character data (known internally as a story string).
This is the main way to format your character card for text completion and instruct models.

The template supports Handlebars syntax and any custom text injections or formatting. See the language reference here: https://handlebarsjs.com/guide/

We provide the following parameters to the Handlebars evaluator (wrap them into double-curly braces):

1. `description` - character's Description
2. `scenario` - character's Scenario
3. `personality` - character's Personality
4. `system` - [instruct mode] system prompt OR character's main prompt override (if exists and "Prefer Char. Prompt" is enabled in User Settings)
5. `persona` - selected persona description
6. `char` - character's name
7. `user` - selected persona name
8. `wiBefore` or `loreBefore` - combined activated World Info entries with Position set to "Before Char Defs"
9. `wiAfter` or `loreAfter` - combined activated World Info entries with Position set to "After Char Defs"

**WARNING**: If some of the above parameters are missing from the story string template, they are not going to be sent in the prompt at all.

#### Example Separator

Used as a block header and a separator between the example dialogue blocks. Any instance of `<START>` tags in the example dialogues will be replaced with the contents of this field.

#### Chat Start

Inserted as a separator after the rendered story string and after the example dialogues blocks, but before the first message in context.

#### Always add character's name to prompt

Appends the character's name to the prompt to force the model to complete the message as the character:

```
** OTHER CONTEXT HERE **
Character:
```

### Tokenizer

A tokenizer is a tool that breaks down a piece of text into smaller units called tokens. These tokens can be individual words or even parts of words, such as prefixes, suffixes, or punctuation. A rule of thumb is that one token generally corresponds to 3~4 characters of text.

SillyTavern provides a "Best match" option that tries to match the tokenizer using the following rules depending on the API provider used.

Text Completion APIs **(overridable)**:
2. NovelAI Clio: NerdStash tokenizer.
3. NovelAI Kayra: NerdStash v2 tokenizer.
4. TextGen / KoboldAI / AI Horde: LLaMA tokenizer.
5. Koboldcpp: model API tokenizer.

If you get inaccurate results or wish to experiment, you can set an *override tokenizer* for SillyTavern to use while forming a request to the AI backend:

1. None. Each token is estimated to be ~3.3 characters, rounded up to the nearest integer. **Try this if your prompts get cut off on high context lengths.** This approach is used by KoboldAI Lite.
2. LLaMA tokenizer. Used by LLaMA 1/2 models family: Vicuna, Hermes, Airoboros, etc. **Pick if you use a LLaMA 1/2 model.**
3. NerdStash tokenizer. Used by NovelAI's Clio model. **Pick if you use the Clio model.**
4. NerdStash v2 tokenizer. Used by NovelAI's Kayra model. **Pick if you use the Kayra model.**
5. API tokenizer. Queries the generation API to get the token count directly from the model. Only supported by Oobabooga's TextGen. **Pick if you use the latest version of TextGen API.**

Chat Completion APIs **(non-overridable)**:
1. OpenAI / Claude / OpenRouter / Window: model-dependant tokenizer via [tiktoken](https://github.com/openai/tiktoken).
2. Scale API: GPT-4 tokenizer.
3. Fallback tokenizer (for proxies): GPT-3.5 turbo tokenizer.

### Token Padding

**Important: This section doesn't apply to Chat Completions API. SillyTavern will always use a matching tokenizer for these models.**

SillyTavern cannot use a proper tokenizer provided by the model running on a remote instance of KoboldAI or Oobabooga's TextGen, so all token counts assumed during prompt generation are estimated based on the selected [tokenizer](#tokenizer) type.

Since the results of tokenization can be inaccurate on context sizes close to the model-defined maximum, some parts of the prompt may be trimmed or dropped, which may negatively affect the coherence of character definitions.

To prevent this, SillyTavern allocates a portion of the context size as padding to avoid adding more chat items than the model can accommodate. If you find that some part of the prompt is trimmed even with the most-matching tokenizer selected, adjust the padding so the description is not truncated.

You can input negative values for reverse padding, which allows allocating more than the set maximum amount of tokens.

## Custom Stopping Strings

Accepts a JSON-serialized array of stopping strings. Example: `["\n", "\nUser:", "\nChar:"]`. If you're unsure about the formatting, use an [online JSON validator](https://jsonlint.com/).

Supported APIs:

1. KoboldAI (versions 1.2.2 and higher)
2. oobabooga's Text Generation WebUI
3. NovelAI
4. OpenAI, including via OpenRouter (max 4 strings)
