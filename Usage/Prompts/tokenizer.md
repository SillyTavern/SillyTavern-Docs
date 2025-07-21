---
order: prompts-40
---

# Tokenizer

A tokenizer is a tool that breaks down a piece of text into smaller units called tokens. These tokens can be individual words or even parts of words, such as prefixes, suffixes, or punctuation. A rule of thumb is that one token generally corresponds to 3~4 characters of text.

SillyTavern provides a "Best match" option that tries to match the tokenizer using the following rules depending on the API provider used.

Text Completion APIs **(overridable)**:

1. NovelAI Clio: NerdStash tokenizer.
2. NovelAI Kayra: NerdStash v2 tokenizer.
3. Text Completion: API tokenizer (if supported) or Llama tokenizer.
4. KoboldAI Classic / AI Horde: Llama tokenizer.
5. KoboldCpp: model API tokenizer.

If you get inaccurate results or wish to experiment, you can set an _override tokenizer_ for SillyTavern to use while forming a request to the AI backend:

1. None. Each token is estimated to be ~3.3 characters, rounded up to the nearest integer. **Try this if your prompts get cut off on high context lengths.** This approach is used by KoboldAI Lite.
2. Llama tokenizer. Used by Llama 1/2 models family: Vicuna, Hermes, Airoboros, etc. **Pick if you use a Llama 1/2 model.**
3. Llama 3 tokenizer. Used by Llama 3/3.1 models. **Pick if you use a Llama 3/3.1 model.**
4. NerdStash tokenizer. Used by NovelAI's Clio model. **Pick if you use the Clio model.**
5. NerdStash v2 tokenizer. Used by NovelAI's Kayra model. **Pick if you use the Kayra model.**
6. Mistral V1 tokenizer. Used by older Mistral models family and their finetunes. **Pick if you use an older Mistral model.**
7. Mistral Nemo tokenizer. Used by Mistral Nemo models family and their finetunes. **Pick if you use a Mistral Nemo/Pixtral model.**
8. Yi tokenizer. Used by Yi models. **Pick if you use a Yi model.**
9. Gemma tokenizer. Used by Gemini/Gemma models. **Pick if you use a Gemma model.**
10. DeepSeek tokenizer. Used by DeepSeek models (such as R1). **Pick if you use a DeepSeek model.**
11. API tokenizer. Queries the generation API to get the token count directly from the model. Known backends to support: Text Generation WebUI (ooba), koboldcpp, TabbyAPI, Aphrodite API. **Pick if you use a supported backend.**

Chat Completion APIs **(non-overridable)**:

1. OpenAI: model-dependant tokenizer via [tiktoken](https://github.com/openai/tiktoken).
2. Claude: model-dependant tokenizer via [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp).
3. OpenRouter: Llama, Mistral, Gemma, Yi tokenizers for their respective models.
4. Google AI Studio: Gemma tokenizer.
5. AI21 API: Jamba tokenizer (requires a one-time download).
6. Cohere API: Command-R or Command-A tokenizer (requires a one-time download).
7. MistralAI API: Mistral V1 or V3 tokenizer (requires a one-time download).
8. DeepSeek API: DeepSeek tokenizer (requires a one-time download).
9. Fallback tokenizer: GPT-3.5 turbo tokenizer.

#### Additional Tokenizers

These tokenizers are not included in the default installation due to their size A one-time download is required when they're used for the first time.

1. Qwen2 tokenizer.
2. Command-R / Command-A tokenizers. Used by Cohere source in Chat Completion.
3. Mistral V3 (Nemo) tokenizer. Used by MistralAI source in Chat Completion (Nemo and Pixtral models).
4. DeepSeek (deepseek-chat) tokenizer. Used by DeepSeek source in Chat Completion.

If you don't want to use internet downloads, the opt-out option exists in config.yaml: `enableDownloadableTokenizers`. Set to `false` to disable downloads.

You can also download tokenizers manually from the [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers) repository. Download the JSON files and put them in the `_cache` subdirectory of your data root, the path is `./data/_cache` by default. Create the `_cache` directory if it doesn't exist. After that, restart the SillyTavern server to re-initialize tokenizers.

If the required tokenizer model is not cached and downloads are disabled, a fallback tokenizer (Llama 3) will be used for counting.

### Token Padding

!!! Applies to: Text Completion APIs
SillyTavern will always use the matching tokenizer for Chat Completion models, so there is no need for token padding.
!!!

Unless SillyTavern uses a tokenizer provided by the remote backend API that runs the model, all token counts assumed during prompt generation are estimated based on the selected [tokenizer](#tokenizer) type.

Since the results of tokenization can be inaccurate on context sizes close to the model-defined maximum, some parts of the prompt may be trimmed or dropped, which may negatively affect the coherence of character definitions.

To prevent this, SillyTavern allocates a portion of the context size as padding to avoid adding more chat items than the model can accommodate. If you find that some part of the prompt is trimmed even with the most-matching tokenizer selected, adjust the padding so the description is not truncated.

You can input negative values for reverse padding, which allows allocating more than the set maximum amount of tokens.
