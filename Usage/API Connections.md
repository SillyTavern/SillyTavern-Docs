# API Connections

SillyTavern can connect to a wide range of LLM APIs.
Below is a description of their respective strengths, weaknesses, and use cases.

## Local APIs

- These LLM APIs can be run on your PC.
- They are free to use and have no content filter.
- installation process can be complex (**SillyTavern dev team does not provide support for this**)
- require downloading LLM models which can be 10-50GB each.
- Most models are not as powerful as cloud LLM APIs.

### KoboldAI

- Runs on your PC, 100% private, wide range of models available
- gives the most direct control of the AI's generation settings
- requires large amounts of VRAM in your GPU (6-24GB, depending on the LLM model)
- models limited to 2k context
- No streaming
[GitHub](https://github.com/KoboldAI/KoboldAI-Client)

### KoboldCPP

- same functonality as KoboldAI, but uses your CPU and RAM instead of GPU
- very simple to setup on Windows (must be compiled from source on MacOS and Linux)
- slower than GPU APIs
[GitHub](https://github.com/LostRuins/koboldcpp)

### Kobold Horde

- SillyTavern can access this API out of the box with no additional settings required.
- it uses the GPU of individual volunteers (Horde Workers) to process responses for your chat inputs
- at the mercy of the Worker in terms of generation wait times, AI settings, and available models
[website](https://horde.koboldai.net)

### Oobabooga TextGeneration WeuUI

- similar functionality as KoboldAI, but also has streaming and a Gradio interface
[GitHub](https://horde.koboldai.net)

## Cloud LLM APIs

- These LLM APIs are run as cloud services, and require no resources on your PC
- They are stronger/smarter than most local LLMs
- However they all have content filtering of varying degrees, and most require payment

### NovelAI

- No content filter
- Paid subscription required
[Setup Instructions](https://docs.sillytavern.app/usage/api-connections/novelai/)

### ChatGPT (by OpenAI)

- easy to setup and acquire an API key, 4k 8k, 32k context models available.
- free trial requires a valid phone number, and then required to pre-pay for use credits
- writing style for roleplay can be repetitive and predictable
[Setup Instructions](https://docs.sillytavern.app/usage/api-connections/openai/)

### Claude (by Anthropic)

- Reccomended for users who want creative unique writing style
- 4k, 8k, 100k context models available
- strongst content filter of all APIs (as of June 2023)
- limited access to most models
- currently not accepting new account creation; on a wait list (June 2023)
[website](https://www.anthropic.com/index/introducing-claude)

### Poe.com (by Quora)

- Eeccommended API for new users who lack tech savviness.
- free use of OpenAI's ChatGPT 3.5 turbo (4k) and Anthropic's Claude-instant (5.5k)
- paid account can access more models such as ChatGPT 4 and Claude 100k
- customizable bots allow for behavior and character profile to be set on the website
- somewhat tedious initial setup process
[SillyTavern + Poe Setup Guide](https://docs.sillytavern.app/usage/api-connections/poe/)

### WindowAI/OpenRouter

- this browser extension allows you to connect to the abovementioned cloud LLMs
- use your own ChatGPT/Claude API key with WindowAI
- use OpenRouter to pay to use their API keys instead
- useful if you don't want to create individual accounts on each service
[website](https://windowai.io) and [website](https://openrouter.ai)
