# Image Generation

Use local or cloud-based Stable Diffusion, FLUX or DALL-E APIs to generate images.
The free mode is also supported via the `/sd (anything_here)` command in the chat input bar.
Most common Stable Diffusion generation settings are customizable within the SillyTavern UI.

## Supported Sources

* [Block Entropy](https://blockentropy.ai/)
* [ComfyUI](https://github.com/comfyanonymous/ComfyUI). Requires a workflow JSON file.
* [Draw Things](https://drawthings.ai/)
* [HuggingFace Serverless Inference](https://huggingface.co/docs/api-inference/index)
* [NovelAI Diffusion](https://novelai.net/). Requires an active subscription.
* [OpenAI DALL-E 2/3](https://platform.openai.com/)
* [Pollinations](https://pollinations.ai/)
* [SD.Next / vladmandic](https://github.com/vladmandic/automatic)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras) (deprecated, not recommended)
* [Stability AI](https://platform.stability.ai/)
* [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
* [Stable Horde](https://stablehorde.net/)
* [TogetherAI](https://api.together.xyz/models)

## Generation modes

| Wand menu item     | Slash command argument | Description                                    | Remarks                               |
|:-------------------|:-----------------------|:-----------------------------------------------|:--------------------------------------|
| "Yourself"         | `you`                  | A full-body portrait of the current character. | -                                     | 
| "Your Face"        | `face`                 | A close-up portrait of the current character.  | Forces a portrait aspect ratio.       |
| "Me"               | `me`                   | A portrait of the user persona.                | -                                     |
| "The Whole Story"  | `scene`                | A visual recap of the chat events.             | -                                     |
| "The Last Message" | `last`                 | A visual recap of the last chat message.       | -                                     |
| "Raw Last Message" | `raw_last`             | Last message used as a prompt verbatim.        | -                                     |
| "Background"       | `background`           | A chat background based on story context.      | Forces a wide landscape aspect ratio. |

## How to generate an image

1. Use the "Image Generation" item in the extensions context menu (wand).
2. Type a `/sd (argument)` slash command with an argument from the Generation modes table. Anything else would trigger a "free mode" to make SD generate whatever you prompted. Example: `/sd apple tree` would generate a picture of an apple tree.
3. Look for a paintbrush icon in the context actions for chat messages. This will force the "Raw Message" mode for the selected message.

Every generation mode besides raw message and free mode will trigger a prompt generation using your currently selected main generation API to convert chat context into the SD prompt.
You can configure the instruction template for generating prompts for every generation mode using the "SD Prompt Templates" settings drawer in the extensions panel.

### Tips and tricks for the `/sd` command usage

#### View all generated images

To view all saved images for a character (including other chats), open a gallery from a "More..." dropdown menu on a character info panel, or use a `/show-gallery` slash command.

#### Specify a negative prompt

Use a `negative` named argument before the prompt to enforce a specific negative prompt for this generation.

```stscript
/sd negative="ugly, fat" cute girl eating a burger
```

#### Include a character-specific prefix

Use a special \{\{charPrefix\}\} macro in free-prompt mode to include positive and negative prompt prefixes (if defined) for the current character.

```stscript
/sd {{charPrefix}}, riding a bike
```

#### Suppress a chat message

You can avoid posting a generated image into the chat by passing a `quiet=true` named argument. The image will still be added into the character gallery, and the command will produce a relative URL to the image that can be consumed by other commands.

The example below will send the generated image using Markdown as a user persona.

```stscript
/sd quiet=true me | /send Here's a picture of me: ![my portrait]({{pipe}})
```

### Image swipes

Images swipes allow to reroll the image generation while keeping the same prompt. If a fixed seed is set, it will be randomized for the next generation. 

To cycle through images, hover a mouse cursor (tap on mobile) over a generated image to reveal arrow buttons and swipes counter. Tapping right arrow on the latest image will generate a new one.

*'Swipes' here is just a name, don't try the actual swiping gesture, as this will regenerate the message itself, not the attached image.*

## Options

### Edit prompts before generation

Allow to edit the automatically generated prompts manually before sending them to the Stable Diffusion API.

### Use function tool

Uses [function calling](/extensions/Stable-Diffusion.md) to automatically detect the intention to generate an image.

**Requirements:**

1. Must have image generation configured with a supported source.
2. Must use a supported Chat Completion API model and have function tool calling enabled in the AI Response settings.
3. The "Use function tool" option must be enabled in the Image Generation settings.
4. The user should express an intent to generate an image in the chat message, e.g. "Send me a picture of a cat".

!!! warning
The interactive mode will not trigger when the function tool is enabled.
!!!

### Use interactive mode

Allows to trigger an image generation instead of text as a reply to a user message that follows the special pattern:

1. Contains one of the following verbs: send, mail, imagine, generate, make, create, draw, paint, render
2. Followed by one of the following nouns (not further than 10 characters away): pic, picture, image, drawing, painting, photo, photograph
3. Followed by a target subject of image generation, could be optionally preceded by phrases like "of a" or "of this".

Examples of valid requests and captured subjects:

* `Can you please send me a picture of a cat` => `cat`
* `Generate a picture of the Eiffel tower` => `Eiffel tower`
* `Let's draw a painting of Mona Lisa` => `Mona Lisa`

Some special subjects trigger a predefined generation mode:

* 'you, 'yourself' => "Yourself"
* 'your face', 'your portrait', 'your selfie' => "Your Face"
* 'me', 'myself' => "Me"
* 'story', 'scenario', 'whole story' => "The Whole Story"
* 'last message' => "The Last Message"
* 'background', 'scene background', 'scene', 'scenery', 'surroundings', 'environment' => "Background"

### Extend free-mode prompts

When using the interactive mode of the slash command, automatically extend free-mode generation subject descriptions by prompting your main API.

### Snap auto-adjusted resolutions

Snap image generation requests with a forced aspect ratio (portraits, backgrounds) to the nearest known resolution, while trying to preserve the absolute pixel counts. Refer to the "Resolution" dropdown for the list of possible options.

**Recommended for SDXL models**.

## Common prompt prefix

Added before every generated or free-mode prompt. Commonly used for setting the overall style of the picture.

Example: `best quality, anime lineart`.

**Pro tip:** Use `{prompt}` macro to specify where exactly the generated prompt will be inserted.

## Negative prompt

Characteristics of the image you don't want to be present in the output.

Example: `bad quality, watermark`.

## Character-specific prompt prefix

Any characteristics that describe the currently selected character. Will be added after a common prefix.

Example: `female, green eyes, brown hair, pink shirt`.

You can also specify a negative prompt prefix for any unwanted content. It will be combined with the general negative prompt.

Limitations:
1. Works only in 1-to-1 chats. Will not be used in groups.
2. Won't be used for backgrounds and free mode generations.

If you want to share the prefixes with others, tick the "Shareable" checkbox. This will save them with the character data, rather than your local settings. 

**Pro tip:** If supported by the generation source, you can also use LoRAs/embeddings here, for example: `<lora:DonaldDuck:1>`.

## Styles

Use this to quickly save and restore your favorite style/quality presets to use them later or when switching between models. The following is included in the Style preset:

1. Common Prompt Prefix
2. Negative Prompt

## Chat Message Visibility

Generated images inserted into the chat are hidden in the main API prompts by default, but this can be overriden individually per generation initiator ("Magic wand" icon, slash command, interactive mode). This can be used for making the experience more immersive by letting the characters "acknowledge" the images. Multimodal models in Chat Completions API may also 'see' the images if "Send inline images" is enabled.

A text message can be customized by changing the "Chat Message Template" under Image Prompt Templates. All regular macros can be used in this template, plus a special \{\{prompt\}\} macro to specify where the image prompt will be added.
