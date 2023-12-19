---
icon: repo-forked
---

# API Connections

SillyTavern can connect to a wide range of LLM APIs.
Below is a description of their respective strengths, weaknesses, and use cases.

## Local APIs

- These LLM APIs can be run on your PC.
- They are free to use and have no content filter.
- installation process can be complex (**SillyTavern dev team does not provide support for this**)
- requires separate download of LLM models from [HuggingFace](https://huggingface.co/models?other=LLM) which can be 10-50GB each.
- Most models are not as powerful as cloud LLM APIs.

### KoboldAI

- Runs on your PC, 100% private, wide range of models available
- gives the most direct control of the AI's generation settings
- requires large amounts of VRAM in your GPU (6-24GB, depending on the LLM model)
- models limited to 2k context
- No streaming
- popular KoboldAI versions:
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

### KoboldCPP

- same functonality as KoboldAI, but uses your CPU and RAM instead of GPU
- very simple to setup on Windows (must be compiled from source on MacOS and Linux)
- slower than GPU APIs
- [GitHub](https://github.com/LostRuins/koboldcpp)

### Kobold Horde

- SillyTavern can access this API out of the box with no additional settings required.
- it uses the GPU of individual volunteers (Horde Workers) to process responses for your chat inputs
- at the mercy of the Worker in terms of generation wait times, AI settings, and available models
- [website](https://horde.koboldai.net)

### Oobabooga TextGeneration WebUI

- similar functionality as KoboldAI, but also has streaming and a Gradio interface
- supports a wider range of model types than KoboldAI (4-bit & 8-bit quantized models)
- one-click installers available
- regular updates, which can sometimes break compatibility with SillyTavern
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**Correct Way to Connect SillyTavern to Ooba's new OpenAI API**

1. Make sure you're on the latest update of Oobabooga's TextGen (as of Nov 14th, 2023).
2. Edit the CMD_FLAGS.txt file, and add the `--api` flag there. Then restart Ooba's server.
3. Connect ST to <http://localhost:5000/> (by default) without checking the 'Legacy API' box. You may remove the `/v1` postfix from the URL Ooba's console provides you.

*You can change the API hosting port with the `--api-port 5001` flag, where 5001 is your custom port.*

## Cloud LLM APIs

- These LLM APIs are run as cloud services, and require no resources on your PC
- They are stronger/smarter than most local LLMs
- However they all have content filtering of varying degrees, and most require payment

### NovelAI

- No content filter
- Paid subscription required
- [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/novelai/)

### ChatGPT (by OpenAI)

- easy to setup and acquire an API key, 4k 8k, 32k context models available.
- free trial requires a valid phone number
- after freetrial, all useage is charged monthly.
- writing style for roleplay can be repetitive and predictable
- [Setup Instructions](https://docs.sillytavern.app/usage/api-connections/openai/)

### Claude (by Anthropic)

- Recommended for users who want their AI chats to have a creative, unique writing style
- 4k, 8k, 100k context models available
- strongst content filter of all APIs (as of June 2023)
- limited access to most models
- currently not accepting new account creation; on a wait list (June 2023)
- [website](https://www.anthropic.com/index/introducing-claude)

### WindowAI/OpenRouter

- this browser extension allows you to connect to the abovementioned cloud LLMs
- use your own ChatGPT/Claude API key with WindowAI
- use OpenRouter to pay to use their API keys instead
- useful if you don't want to create individual accounts on each service
- [website](https://windowai.io) and [website](https://openrouter.ai)

### Mancer AI

- Service that hosts unconstrained models, no need to jailbreak.
- Uses 'credits' to pay for tokens on various models. Free credits refill daily.
- Does not log prompts by default, but you can enable it to get credit discounts on tokens.
- Uses an API similar to `Oobabooga TextGeneration WebUI`, see [Mancer docs](https://mancer.tech/docs/clients/#sampling-parameters) for details.
- [website](https://mancer.tech/), [SillyTavern Setup Instructions](https://mancer.tech/docs/clients/sillytavern/)
