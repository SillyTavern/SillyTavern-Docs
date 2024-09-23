---
icon: repo-forked
---

# API Connections

SillyTavern can connect to a wide range of LLM APIs.
Below is a description of their respective strengths, weaknesses, and use cases.

## ELI5: Chat Completions vs Text Completions

When you first navigate to the "API Connections" page in ST, you will notice a drop-down option to select between options using nomenclature such as "Chat Completion" and "Text Completion". It's helpful to understand what this is.

What it's not: It's easy to think of "Text Completion" as local models and "Chat Completion" as cloud-based LLMs but that's not the case. Neither is e.g. "Novel AI" or "Kobold" actually a separate type of model altogether, even though they are separate options in the API dropdown in ST. You can force models into different API structures with the appropriate backend, but that's not the point of this section.

When you send a message using ST, your chat, character description, and other prompts such as lorebooks or author notes are constructed into a single "prompt" to be sent to the model. The API "type" for the model you are using decides how exactly this prompt will be constructed (something that ST takes care of you automatically in the background - you can open your ST terminal and see exactly what the prompt being sent to the AI looks like). 

### Chat Completions

A Chat Completion model, as its name suggests will structure your prompt into a series of messages between the User (you) and the Assistant (the AI) or System (neutral). Models that are trained for Chat Completion help create the feeling of a "Chat", with the AI "responding" to the last message. When you're using the ChatGPT website, you're dealing with a Chat Completions API in the background.

### Text Completions (a.k.a just "Completions")

A Text Completion on the other hand, and again as its name suggests, will convert your prompt into one long string and the model will simply try to continue this (like, literally imagine all your text, your hundreds of messages, all your formatting, newlines, etc. squashed into one very long sentence).

If your messages in ST happen to be formatted as a series of messages between YourPersona: and Character:, the Text Completion model will try to continue this pattern and ST will render it as a new chat message for you, but really the model is just trying to continue the Text. If you offered an input of "The Sun rises in the", a text completion model is likely to finish that message for you with "East". 

Most Text Completion models have a recommended "Instruct Template" (usually mentioned in the model's documentation or download page) that help them "respond" to messages and instructions, just like a Chat Completion model. ST usually has most (if not all) Instruct Templates available for you to choose from in the "Advanced Formatting" page.

## Local APIs

- These LLM APIs can be run on your PC.
- They are free to use and have no content filter.
- Installation process can be complex (**SillyTavern dev team does not provide support for this**).
- Requires separate download of LLM models from [HuggingFace](https://huggingface.co/models?other=LLM) which can be 5-50GB each.
- Most models are not as powerful as cloud LLM APIs.

### KoboldAI

- Runs on your PC, 100% private, wide range of models available
- Gives the most direct control of the AI's generation settings
- Requires large amounts of VRAM in your GPU (6-24GB, depending on the LLM model)
- Models limited to 2k context
- No streaming
- Popular KoboldAI versions:
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

### KoboldCpp

- Easy-to-use API with CPU offloading (helpful for low VRAM users) and streaming
- Runs from a single .exe file on Windows (must be compiled from source on MacOS and Linux)
- Supports GGUF/GGML models
- Slower than GPU-only loaders such as AutoGPTQ and Exllama/v2
- [GitHub](https://github.com/LostRuins/koboldcpp)

### Oobabooga TextGeneration WebUI

- All-in-one Gradio UI with streaming
- Broadest support for quantized (AWQ, Exl2, GGML, GGUF, GPTQ) and FP16 models
- One-click installers available
- Regular updates, which can sometimes break compatibility with SillyTavern
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**Correct Way to Connect SillyTavern to Ooba's new OpenAI API**

1. Make sure you're on the latest update of Oobabooga's TextGen (as of Nov 14th, 2023).
2. Edit the CMD_FLAGS.txt file, and add the `--api` flag there. Then restart Ooba's server.
3. Connect ST to <http://localhost:5000/> (by default) without checking the 'Legacy API' box. You may remove the `/v1` postfix from the URL Ooba's console provides you.

*You can change the API hosting port with the `--api-port 5001` flag, where 5001 is your custom port.*

### TabbyAPI

- Lightweight [Exllamav2](https://github.com/turboderp/exllamav2)-based API with streaming
- Supports Exl2, GPTQ, and FP16 models
- [Official extension](https://github.com/theroyallab/ST-tabbyAPI-loader) allows loading/unloading models directly from SillyTavern
- Not recommended for users with low VRAM (no CPU offloading)
- [GitHub](https://github.com/theroyallab/tabbyAPI)

## Cloud LLM APIs

- These LLM APIs are run as cloud services and require no resources on your PC
- They are stronger/smarter than most local LLMs
- However they all have content filtering of varying degrees, and most require payment

### OpenAI (ChatGPT)

- Easy to set up and acquire an API key
- Requires prepayment for credits and charges per prompt
- Very logical. Creative style can be repetitive and predictable
- Most of the newer models (gpt-4-turbo, gpt-4o) support multimodality
- [Website](https://platform.openai.com/), [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/openai/)

### Claude (by Anthropic)

- Recommended for users who want their AI chats to have a creative, unique writing style
- Requires prepayment for credits and charges per prompt
- The newest models (Claude 3) support multimodality
- Requires a specific prompting style and utilization of [prefills](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response) for reply steering
- [Website](https://console.anthropic.com/)
- 

### Mistral (by Mistral AI)

- Efficient models from various sizes and use cases. You can create an account and API key on [their platform](https://console.mistral.ai/api-keys/).
- From 32k to 128k context sizes for general use, and 32k to 256k context sizes for coding.
- Free Tier with rate limits.
- Reasonable moderation, with Mistrals main principles being to be neutral and empower users, more information [here](https://mistral.ai/terms/). 

### OpenRouter

- WindowAI browser extension allows you to connect to the abovementioned cloud LLMs with your own API key
- Use OpenRouter to pay to use their API keys instead
- Useful if you don't want to create individual accounts on each service
- [WindowAI website](https://windowai.io) and [OpenRouter website](https://openrouter.ai)

### DreamGen

- Uncensored models tuned for steerable creative writing
- Free monthly credits, as well as paid subscription
- Models ranging from 7B to 70B
- [Setup Instructions](/usage/api-connections/DreamGen/)

### AI Horde

- SillyTavern can access this API out of the box with no additional settings required
- Uses the GPU of individual volunteers (Horde Workers) to process responses for your chat inputs
- At the mercy of the Worker in terms of generation wait times, AI settings, and available models
- [Website](https://aihorde.net/)

### Mancer AI

- Service that hosts unconstrained models of various families
- Uses 'credits' to pay for tokens on various models
- Does not log prompts by default, but you can enable it to get credit discounts on tokens.
- Uses an API similar to `Oobabooga TextGeneration WebUI`, see [Mancer docs](https://mancer.tech/docs/clients/#sampling-parameters) for details.
- [Website](https://mancer.tech/), [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/mancer/)

### NovelAI

- No content filter
- Paid subscription required
- [Website](https://novelai.net/), [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/novelai/)
