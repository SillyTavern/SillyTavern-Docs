# Advanced Formatting

The settings provided in this section allow for more control over the prompt-building strategy.

### Context Template

**Most of the settings here do not apply to Chat Completions APIs as they are governed by the prompt manager system instead.**

Usually, AI models require you to provide the character data to them in some specific way. SillyTavern includes a list of pre-made conversion rules for different models, but you may customize them however you like.

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
10. `mesExamples` - (optional) character's Example Dialogues, instruct-formatted with separator. Set "Example Messages Behavior" to "Never include examples" to avoid duplication.

A special \{\{trim\}\} macro is supported to remove any newlines that surround it. Use it in case you want some part of text NOT be separated with a newline from the previous line (_spaces **are not** trimmed_).

**WARNING**: If some of the above parameters are missing from the story string template, they are not going to be sent in the prompt at all.

#### Example Separator

Used as a block header and a separator between the example dialogue blocks. Any instance of `<START>` tags in the example dialogues will be replaced with the contents of this field.

#### Chat Start

Inserted as a separator after the rendered story string and after the example dialogues blocks, but before the first message in context.

### Use as Stop Strings

Adds "Example Separator" and "Chat Start" to the list of stop strings.

Helpful if the model tends to hallucinate or leak whole blocks of example dialogue preceded by the separator.

### Allow Jailbreak

Includes Jailbreak at the end of the prompt, formatted as the last user message.

The jailbreak prompt should be defined in the character card and "Prefer Char. Jailbreak" setting should be enabled.

Should be used with care, as placing instructions low in the context can lead to degraded quality of the outputs of smaller models.

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

1. NovelAI Clio: NerdStash tokenizer.
2. NovelAI Kayra: NerdStash v2 tokenizer.
3. Text Completion: API tokenizer (if supported) or LLaMA tokenizer.
4. KoboldAI Classic / AI Horde: LLaMA tokenizer.
5. Koboldcpp: model API tokenizer.

If you get inaccurate results or wish to experiment, you can set an *override tokenizer* for SillyTavern to use while forming a request to the AI backend:

1. None. Each token is estimated to be ~3.3 characters, rounded up to the nearest integer. **Try this if your prompts get cut off on high context lengths.** This approach is used by KoboldAI Lite.
2. LLaMA tokenizer. Used by LLaMA 1/2 models family: Vicuna, Hermes, Airoboros, etc. **Pick if you use a LLaMA 1/2 model.**
3. NerdStash tokenizer. Used by NovelAI's Clio model. **Pick if you use the Clio model.**
4. NerdStash v2 tokenizer. Used by NovelAI's Kayra model. **Pick if you use the Kayra model.**
5. Mistral tokenizer. Used by Mistral models family and their finetunes. **Pick if you use a Mistral model.**
6. Yi tokenized. User by Yi models. **Pick if you use a Yi model.**
7. API tokenizer. Queries the generation API to get the token count directly from the model. Known backends to support: Text Generation WebUI (ooba), koboldcpp, TabbyAPI, Aphrodite API. **Pick if you use a supported backend.**

Chat Completion APIs **(non-overridable)**:

1. GPT via OpenAI / OpenRouter / Window: model-dependant tokenizer via [tiktoken](https://github.com/openai/tiktoken).
2. Claude: Model-dependant tokenizer via [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp).
3. OpenRouter: Llama and Mistral tokenizers for their respective models.
4. Scale API: GPT-4 tokenizer.
5. AI21 API: GPT-3.5 turbo (default, fast) or API tokenizer (if toggled on, can cause slowdowns!).
6. Fallback tokenizer (for proxies): GPT-3.5 turbo tokenizer.

### Token Padding

**Important: This section doesn't apply to Chat Completions API. SillyTavern will always use a matching tokenizer for these models.**

SillyTavern cannot use a proper tokenizer provided by the model running on a remote instance of KoboldAI or Oobabooga's TextGen, so all token counts assumed during prompt generation are estimated based on the selected [tokenizer](#tokenizer) type.

Since the results of tokenization can be inaccurate on context sizes close to the model-defined maximum, some parts of the prompt may be trimmed or dropped, which may negatively affect the coherence of character definitions.

To prevent this, SillyTavern allocates a portion of the context size as padding to avoid adding more chat items than the model can accommodate. If you find that some part of the prompt is trimmed even with the most-matching tokenizer selected, adjust the padding so the description is not truncated.

You can input negative values for reverse padding, which allows allocating more than the set maximum amount of tokens.

## Custom Stopping Strings

Accepts a JSON-serialized array of stopping strings. Example: `["\n", "\nUser:", "\nChar:"]`. If you're unsure about the formatting, use an [online JSON validator](https://jsonlint.com/).

Supported APIs:

1. KoboldAI (versions 1.2.2 and higher) or KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI (ooba), Tabby, Aphrodite, Mancer, TogetherAI, Ollama, etc.
4. NovelAI
5. OpenAI, including via OpenRouter (max 4 strings)
6. Claude
7. Google MakerSuite
