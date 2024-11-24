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

You can also switch between styles using the `/imagine-style` command (or `/sd-style` or `/img-style`).

## Chat Message Visibility

Generated images inserted into the chat are hidden in the main API prompts by default, but this can be overriden individually per generation initiator ("Magic wand" icon, slash command, interactive mode). This can be used for making the experience more immersive by letting the characters "acknowledge" the images. Multimodal models in Chat Completions API may also 'see' the images if "Send inline images" is enabled.

A text message can be customized by changing the "Chat Message Template" under Image Prompt Templates. All regular macros can be used in this template, plus a special \{\{prompt\}\} macro to specify where the image prompt will be added.

## ComfyUI Configuration

ComfyUI is a fast and very flexible option for image generation. 

If you're familiar with ComfyUI, the tl;dr is: make your workflow in ComfyUI, download it **in API format**, and paste it into the SillyTavern ComfyUI Workflow Editor. ST will submit your workflow to ComfyUI's API and you will get an image in your chat. But with great power comes great responsibility, and the main responsibility is inserting placeholders in your workflow JSON so you can change settings from SillyTavern.

If you're not familiar with ComfyUI, you can still use it to generate images in SillyTavern using the default workflow. Later, when you want great power, you can learn how to use ComfyUI...

### Controls

This panel allows you to configure and manage your ComfyUI integration with SillyTavern.

Enter the URL of your ComfyUI server in the **ComfyUI URL** input field. The default value is `http://127.0.0.1:8188`. If you are using SwarmUI, the default port for the managed ComfyUI server is `7821`, 20 ports higher than the default port for SwarmUI.

After entering the URL, choose <i class="fa-solid fa-check"></i> **Connect** to validate and establish a connection. The ComfyUI server must be accessible from the SillyTavern host machine.
 
### Workflow Management

Select a ComfyUI workflow from the dropdown menu. Two default workflows are provided:

- Default_Comfy_Workflow.json: A basic text-to-image workflow supporting the most common image generation settings.
- Char_Avatar_Comfy_Workflow.json: A sample image-to-image workflow that uses the character avatar, plus the prompt, to generate an image.

Use the following buttons to manage your workflows:

- <i class="fa-solid fa-pen-to-square"></i> **Open workflow editor** to view and modify the selected workflow.
- <i class="fa-solid fa-plus"></i> **Create new workflow** to create a new workflow with a custom name.
- <i class="fa-solid fa-trash-can"></i> **Delete workflow** to remove the selected workflow.

### Workflow Editor

The ComfyUI Workflow Editor allows you to view and modify ComfyUI workflows for use with SillyTavern.

The main component of the editor is a large text area where you can insert or edit your ComfyUI workflow in JSON format.

To add a ComfyUI workflow to the editor, follow these steps:

1. Enable 'Dev Mode' in ComfyUI settings.
2. Use the 'Save (API Format)' option in ComfyUI to download the JSON data.
3. Create a new workflow in SillyTavern and open the editor.
4. Paste the downloaded JSON data into the text area.
5. Replace specific values with placeholders as needed for your use case.

!!! info Tips
You can add the API-format JSON file directly to the `data/default-user/user/workflows` directory in your SillyTavern installation. This will save you from steps 3 and 4.

Retain the original JSON file. If you need to open the workflow again in ComfyUI to make changes, it is much more convenient to edit the original file than the one with all the placeholders.
!!!

#### Pre-defined placeholders

The editor provides a list of predefined placeholders that can be used in your workflow JSON. These placeholders are replaced with dynamic values when the workflow is executed in SillyTavern.

Placeholders marked with ✅ are present in your workflow JSON. Placeholders marked with ❌ are not present in your workflow JSON. You can add these placeholders to your workflow JSON as needed. You do not need to add all the placeholders, only the ones that your workflow uses and you want to replace dynamically.

The `%prompt%` and `%negative_prompt%` placeholders are used to insert the image generation prompts into the workflow. These contain the final prompts generated by SillyTavern, including the generated prompt for your chosen `/sd` mode, the common prompt prefix, negative prompt, and character-specific prompt prefix.

The `%model` placeholder will insert the value of the selected model in the image generation settings. 

!!! info Non-standard model types
Stable Diffusion checkpoints, SD UNets, and GGUF-quantized UNets all appear in the Model dropdown. You must ensure that your chosen workflow has the correct loader node for your chosen model type. If not, the error message from ComfyUI will indicate a problem with the loader node, but will not be particularly helpful apart from that.
!!!

Most other placeholders use the values of the corresponding controls in image generation settings, or the values that you specify with the `/sd` command:

- `%vae%`, but most SD models include a VAE so the default workflows do not use this placeholder. Use it with custom workflows to load a VAE alongside a UNet, override the default VAE, etc.
- `%sampler%`
- `%scheduler%`
- `%steps%`
- `%scale%`
- `%width%`
- `%height%`
- `%denoise%`: for the sample image-to-image workflow, vary the denoise amount between about 0.5 (barely-noticeable changes to the source image) and 1.0 (a completely different image as if no source image was used). Not used by the default text-to-image workflow because there's no point using a value other than 1.0 for text-to-image.
- `%clip_skip%`: not used by the default workflows but available for custom workflows.

The `%seed%` placeholder will insert the seed value from the control if you have specified one. If you set the seed to `-1`, SillyTavern will generate a new random seed for each image in `%seed%`. 

Use the `%user_avatar%` and `%char_avatar%` placeholders to include the user and character avatars in the workflow. These placeholders are replaced with the PNG data of the avatars when the workflow is executed. The image data is encoded in base64 format, so you must decode it in your workflow. A popular choice for this task is the [Load image (Base64)](https://github.com/Acly/comfyui-tooling-nodes) node.

#### Custom placeholders

You can add custom placeholders to your workflow:

1. Look for the "Custom" section below the predefined placeholders.
2. Click the "+" button to add a new custom placeholder.
3. Enter a name for the placeholder in the `find` field.
4. Enter the value that you want to replace the placeholder with in the `replace` field.

Custom placeholders will appear in a separate list below the predefined ones.

### Comfy tricks

Read all the general information on this page so you're familiar with the image generation options. Options such as switchable styles and common prompt prefixes, when combined wih the total flexibility of ComfyUI workflows, allow you to create a wide variety of image generation setups.

#### Loading LoRAs

Use a LoRA tag loader node (such as [Load LoRA Tag](https://github.com/badjeff/comfyui_lora_tag_loader)) to load any LoRAs specified in the prompt. Now you can add as many LoRAs as you like to your prompt with tags like `<lora:CroissantStyle:0.8>`, and they will be loaded into your workflow.

#### Setting workflow values from styles or slash-commands**

You can use macros in custom placeholder values. As a practical example, let's say you sometimes want to generate images without a background, and you'd like this to be switchable with a slash-command or image style. Here's how you could do it:

1. Make a ComfyUI workflow that removes the image background, or not, depending on the value of an input
2. Use a custom placeholder to set the value of that input, but use `{{getvar::remove_background}}` as the replace value
3. Now you can set the value of `remove_background` with `/setvar key=remove_background true` or `/setvar key=remove_background false` before generating an image
4. The workflow will use the value you set to determine whether to remove the background
5. Make an image style "No background" with common prompt prefix `{{setvar::remove_background::true}}{prompt}`
6. Use the style control or `/imagine-style No background` to set the value of `remove_background` to `true` before generating an image


