# What is SillyTavern?

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern (or ST for short) is a locally installed user interface that allows you to interact with text generation LLMs, image generation engines, and TTS voice models. Our goal is to empower users with as much utility and control over their LLM prompts as possible, embracing the steep learning curve as part of the fun.

SillyTavern is a passion project brought to you by a dedicated community of LLM enthusiasts and will always be free and open-sourced. Beginning in February 2023 as a fork of TavernAI 1.2.8, SillyTavern now has over 100 contributors and 2 years of independent development under its belt, and continues to serve as a leading software for savvy AI hobbyists.

## Screenshots

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## Installation Requirements

The hardware requirements are minimal: it will run on anything that can run NodeJS 18 or higher. If you intend to do LLM inference on your local machine, we recommend a 3000-series NVIDIA graphics card with at least 6GB of VRAM.

Follow the installation guide for your platform:

* [Windows](/Installation/Windows.md)
* [Linux and Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Branches

SillyTavern is being developed using a two-branch system to ensure a smooth experience for all users.

* `release` -🌟 **Recommended for most users.** This is the most stable and recommended branch, updated only when major releases are pushed. It's suitable for the majority of users. Typically updated once a month.
* `staging` - ⚠️ **Not recommended for casual use.** This branch has the latest features, but be cautious as it may break at any time. Only for power users and enthusiasts. Updates several times daily.

## What do I need other than SillyTavern?

Since SillyTavern is only an interface, you will need access to an LLM backend to provide inference. You can use AI Horde for instant out-of-the-box chatting. Aside from that, we support many other local and cloud-based LLM backends: OpenAI-compatible API, KoboldAI, Tabby, and many more. You can read more about our supported APIs in the [API Connections](/Usage/API_Connections/index.md) section.

## Character Cards

SillyTavern is built around the concept of "character cards". A character card is a collection of prompts that set the behavior of the LLM and is required to have persistent conversations in SillyTavern. They function similarly to ChatGPT's GPTs or Poe's bots. The content of a character card can be anything: an abstract scenario, an assistant tailored for a specific task, a famous personality or a fictional character.

The name field is the only required character card input. To start a neutral conversation with the language model, create a new card simply called "Assistant" and leave the rest of the boxes blank. For a more themed chat, you can provide the language model with various background details, behavior and writing patterns, and a scenario to jump start the chat.

To have a quick conversation without selecting a character card or to just test the LLM connection, simply type your prompt input into the input bar on the Welcome Screen after opening SillyTavern. Please note that such chats are temporary and will not be saved.

To get a general idea on how to define character cards, see the default character (Seraphina) or download selected community-made cards from the "Download Extensions & Assets" menu.

You can also create your own character cards from scratch. Refer to the [Character Design](/Usage/Core_Concepts/characterdesign.md) guide for more information.

## Key Features

* Advanced [text generation settings](/Usage/Core_Concepts/advancedformatting.md) with many community-made presets
* [World Info support](Usage/Core_Concepts/worldinfo.md): create rich lore or save tokens on your character card
* [Group chats](/Usage/Core_Concepts/groupchats.md): multi-bot rooms for characters to talk to you and/or each other
* [Rich UI customization options](/Usage/Core_Concepts/uicustomization.md): theme colors, background images, custom CSS, and more
* [User personas](/Usage/Core_Concepts/personas.md): let the AI know a bit about you for greater immersion
* [Built-in RAG support](/Usage/Core_Concepts/data-bank.md): add documents to your chats for the AI to reference
* Extensive [chat commands](/Usage/Core_Concepts/slashcommands.md) subsystem and own [scripting engine](/For_Contributors/st-script.md)

## Extensions

SillyTavern has extensibility support.

* [Character emotional expressions (sprites)](/extensions/Expression-Images.md)
* [Auto-Summary of the chat history](/extensions/Summarize.md)
* Automatic UI and chat translation
* [Stable Diffusion/FLUX/DALL-E image generation](/extensions/Stable-Diffusion.md)
* [Text-to-speech for AI response messages (via ElevenLabs, Silero, or the OS's System TTS)](/extensions/TTS.md)
* [Web Search capabilities for adding additional real world context to your prompts](/extensions/WebSearch.md)
* Many more are available to download from the "Download Extensions & Assets" menu.

## How can I get in touch with the developers directly?

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [Post a GitHub issue](https://github.com/SillyTavern/SillyTavern/issues)

## I like your project! How do I contribute?

* We welcome pull requests! Follow the [Contribution Guidelines](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md) to get started.
* We also welcome helpful and informed bug reports that use the templates provided in our GitHub.
* We do not accept monetary donations for the project itself.

## Personal Donations

Your support for individual contributors is appreciated, but it will not influence the overall development direction of SillyTavern.

* RossAscends has a personal [Patreon](https://www.patreon.com/RossAscends) & [Kofi](https://ko-fi.com/rossascends)

## License

SillyTavern is a free and open-source project released under the [AGPL-3.0 License](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE).
