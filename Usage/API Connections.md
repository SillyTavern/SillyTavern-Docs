---
icon: repo-forked
---

# API Connections

SillyTavern can connect to a wide range of LLM APIs.
Below is a description of their respective strengths, weaknesses, and use cases.

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

### Claude (by Anthropic)

- Recommended for users who want their AI chats to have a creative, unique writing style
- 4k, 8k, 100k context models available
- Strongest content filter of all APIs (as of June 2023)
- Limited access to most models
- Currently not accepting new account creation; on a waitlist (June 2023)
- [Website](https://www.anthropic.com/index/introducing-claude)

### DreamGen

- Uncensored models without filters tuned for steerable AI role-play and story-writing
- Free monthly credits, as well as paid subscription
- Models ranging from 7B to 70B
- [Setup Instructions](/usage/api-connections/DreamGen/)

### [Featherless](https://featherless.ai)

- Private model execution service
- Mission: easiest and fastest way to run *any* model from Hugging Face (uncensored, abliterated, fimbulvetr, etc.).
- 15+ models from 8-15B (as June 2024) and growing
- Monthly cost of $10 USD for unlimited chat.
- [Website](https://featherless.ai)
- [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/featherless/)

### Kobold Horde

- SillyTavern can access this API out of the box with no additional settings required
- Uses the GPU of individual volunteers (Horde Workers) to process responses for your chat inputs
- At the mercy of the Worker in terms of generation wait times, AI settings, and available models
- [Website](https://horde.koboldai.net)

### Mancer AI

- Service that hosts unconstrained models, no need to jailbreak.
- Uses 'credits' to pay for tokens on various models. Free credits refill daily.
- Does not log prompts by default, but you can enable it to get credit discounts on tokens.
- Uses an API similar to `Oobabooga TextGeneration WebUI`, see [Mancer docs](https://mancer.tech/docs/clients/#sampling-parameters) for details.
- [Website](https://mancer.tech/), [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/mancer/)

### NovelAI

- No content filter
- Paid subscription required
- [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/novelai/)

### OpenAI (ChatGPT)

- Easy to set up and acquire an API key, 4k-128k context models available
- Free trial requires a valid phone number
- After the trial, all usage is charged monthly
- Writing style for roleplay can be repetitive and predictable
- [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/openai/)

### OpenRouter

- WindowAI browser extension allows you to connect to the abovementioned cloud LLMs with your own API key
- Use OpenRouter to pay to use their API keys instead
- Useful if you don't want to create individual accounts on each service
- [WindowAI website](https://windowai.io) and [OpenRouter website](https://openrouter.ai)
