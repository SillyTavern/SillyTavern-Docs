---
templating: false
---

# Image Captioning

Image Captioning allows SillyTavern to automatically generate text descriptions for images used in chats. 

Use Image Captioning when you want your AI character to "see" and respond to visual content in your conversations.

- Create captions for images you upload or paste into messages
- Add context to existing images in the chat history
- Use various sources for generation, including local models, cloud APIs, and crowdsourced networks

There are options that require no setup, no money, and no GPU. There are also options that require some or all of those things. Choose the one that fits your needs and resources.

The image captioning extension is built-in to SillyTavern and does not need to be installed separately.

## Quick start

1. Set up:
    - Open the **Image Captioning** panel in the **<i class="fa-solid fa-cubes"></i> Extensions** panel
    - Choose a captioning source (most likely "Local" or "Multimodal")
    - For "Multimodal" ensure you've set up the connection in the **<i class="fa-solid fa-plug"></i> API Connections** tab
2. Generate a caption:
    - Choose "**Generate Caption**" from the **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** popup menu
    - Select an image file when prompted
    - Wait for the caption to be generated
3. Review and send:
    - The captioned image will be inserted into your message
    - See the caption using the image tooltip
    - Click **<i class="fa-solid fa-paper-plane"></i> Send** to see what your character thinks of the image!

## Panel controls

### Source Selection

Choose the source for image captioning. Supported options:

| Source                           | Description                                                                                                                                                                                                  |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Multimodal](#multimodal-source) | **Cloud**: OpenAI, Anthropic, Google, MistralAI, and others. <br>**Local**: Ollama, llama.cpp, KoboldCpp, Text Generation WebUI, and vLLM. <br>Supports custom prompts so you can ask your images questions. |
| [Local](#local-source)           | Uses [transformers.js](https://huggingface.co/docs/transformers.js/en/index) running locally inside your SillyTavern server. Zero setup!                                                                     |
| Horde                            | Uses the [AI Horde](https://aihorde.net/) network, a crowdsourced distributed network of image generation models. Nothing to download, configure, or pay for. Variable response times.                       |
| Extras                           | The Extras project was discontinued in April 2024 and is not maintained or supported.                                                                                                                        |

### Caption Configuration
- **Caption Prompt**: Enter a custom prompt for captioning. The default prompt is "What's in this image?"
- **Ask every time**: Toggle to request a custom prompt for each image caption

### Message Template
- **Message Template**: Customize the caption message template. Use `{{caption}}` macro to insert the generated caption. The default template is `[{{user}} sends {{char}} a picture that contains: {{caption}}]`

### Auto-captioning
- **Automatically caption images**: Toggle to enable automatic captioning of images pasted or attached to messages
- **Edit captions before saving**: Toggle to allow editing captions before they are saved

## Captioning images

All the ways to caption images in SillyTavern:

* Choose "**Generate Caption**" from the **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** popup menu and select an image file when prompted
* Click the <i class="fa-solid fa-envelope-open-text"></i> **Caption** icon at the top of an image already in a message
* Paste an image directly into the chat input with [auto-captioning](#auto-captioning) enabled
* Attach an image file to a message using the <i class="fa-solid fa-paperclip"></i> **Embed File or Image** button in the actions of a message.
* Send a message with an embedded image
* Use the `/caption` [slash command](#slash-command-caption)

## Auto-Captioning
The auto-captioning feature allows you to automatically generate captions for images as they are added to the chat, without manually triggering the captioning process each time.

To enable, select the "Automatically caption images" checkbox in the Image Captioning panel. You can also choose to edit captions before they are saved by checking the "Edit captions before saving" box.

Once enabled, auto-captioning will trigger in the following scenarios:

- When an image is pasted directly into the chat input.
- When an image file is attached to a message.
- When a message with an embedded image is sent.

The system will use your selected captioning source (Local, Extras, Horde, or Multimodal) and the configured settings to generate a caption for the image.

### Editing captions before saving (Refine Mode)

If you've enabled the "Edit captions before saving" option:
1. After an image is added, a popup will appear with the generated caption.
2. You can review and edit the caption as needed.
3. Click "OK" to apply the caption, or "Cancel" to discard the caption without saving.

### Caption sending
The generated (and optionally edited) caption will be automatically inserted into the prompt using the Message Template you've configured. By default, it will be sent in this format:

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```


## Slash Command: /caption
The extension provides a `/caption` slash command to use in the chatbox or in scripts. 

### Usage

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt` (optional): A custom prompt for the captioning model. Only supported by multimodal sources.
- `quiet=true|false`: If set to true, suppresses sending a captioned message to the chat. Default is false.
- `mesId=number`: Specifies a message ID to caption an image from an existing message instead of uploading a new one.

If no `mesId` is provided, the command will prompt you to upload an image. When `quiet` is false (default), a new message with the captioned image will be sent to the chat. The generated caption can be used as input for other commands.

### Examples
Caption a new image with the default settings:

```
/caption
```

Caption a new image with a custom prompt:

```
/caption Describe the main colours and shapes in this image
```

Caption an image from message #5 without sending a new message:

```
/caption mesId=5 quiet=true
```

Caption an image from message #10 with a custom prompt then [generate a new image](/extensions/Stable-Diffusion.md) based on the caption:

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine 
```

## Local source

You can change the model in [config.yaml](/Administration/config-yaml.md#extensions-configuration). The key is called `extensions.models.captioning` because reasons. Enter the Hugging Face model ID you want to use. The default is `Xenova/vit-gpt2-image-captioning`. 

You can use any model that supports image captioning (`VisionEncoderDecoderModel` or "image-to-text" pipeline). The model needs be to compatible with the transformers.js library. That is, it needs ONNX weights. Look for models with the `ONNX` and `image-to-text` tags, or that have a folder called `onnx` full of `.onnx` files. 

## Multimodal source

### General configuration

- **Model**: Choose the model for image captioning. Options vary based on the selected API.
- **Allow reverse proxy**: Toggle to allow using a reverse proxy if defined and valid (OpenAI, Anthropic, Google, Mistral, xAI)

API keys and endpoint URLs for captioning sources are managed in the [API Connections](/Usage/API_Connections/index.md) panel. Set the connection up in API Connections first, then select it as your captions source in Captioning.

!!! warning Set it up in the API Connections panel first
One last time: configure the API key/address/port in **<i class="fa-solid fa-plug"></i> API Connections** and use the connection in Captioning.

You can still use Claude for chats and Google AI Studio for image captioning, or whatever. Just set them *both* up in the 'API Connections' tab first. Then flip your Chat Completion source to Claude and your Captioning source to Google AI Studio.
!!!

For most local backends, you will need to set some options in the model backend rather than in SillyTavern. If your backend can only run one model at a time and doesn't support automatic switching, you have several options to use different models for chat and captioning:

1. **Secondary endpoints:** Use the secondary endpoint feature (see [Secondary endpoints](#secondary-endpoints) section below) to connect to a different API server for captioning
2. **Multiple connection types:** Connect to your backend using both Text Completion and Chat Completion modes in API Connections - this gives you two separate connections to the same backend type

### Sources

To use one of these caption sources, select Multimodal in the Source dropdown.

* "I want the best captioning possible, and I don't mind paying for it": Anthropic
* "I don't want to pay anything or run anything": Google AI Studio free tier
* "I want to caption images locally and have it just work": Ollama
* "I want to keep the dream of local AI alive": [KoboldCpp](#koboldcpp)
* "I want to complain when it doesn't work": ~~Extras~~

| API Provider                      | Description                                                                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 01.AI (Yi)                        | Cloud, paid, yi-vision                                                                                                                                                        |
| Anthropic                         | Cloud, paid, Claude models with vision capabilities: claude-3-5-sonnet/haiku, claude-3-opus/sonnet                                                                            |
| Cohere                            | Cloud, paid, Aya Vision 8B / 32B                                                                                                                                              |
| Custom (OpenAI-compatible)        | For custom OpenAI-compatible APIs, uses currently configured model in API Connections tab                                                                                     |
| Google AI Studio                  | Cloud, free tier then paid, Gemini Flash/Pro                                                                                                                                  |
| Google Vertex AI (Express mode)   | Cloud, free tier, Gemini Flash/Pro                                                                                                                                            |
| Groq                              | Cloud, llama-3.2-vision in 11B/90B, LLaVA                                                                                                                                     |
| KoboldCpp                         | Local, must configure model in KoboldCpp                                                                                                                                      |
| llama.cpp                         | Local, must configure model in llama.cpp                                                                                                                                      |
| MistralAI                         | Cloud, paid, pixtral-large, pixtral-12B                                                                                                                                       |
| Ollama                            | Local, can switch between available models and download [additional vision models](https://ollama.com/search?c=vision) within Captioning after configuring in API Connections |
| OpenAI                            | Cloud, paid, GPT-4 Vision, 4-turbo, 4o, 4o-mini                                                                                                                               |
| OpenRouter                        | Cloud, paid (maybe free options), many models, pick from what's available within Captioning after configuring in API connections                                              |
| Pollinations                      | Cloud, free                                                                                                                                                                   |
| Text Generation WebUI (oobabooga) | Local, must configure model in ooba                                                                                                                                           |
| vLLM                              | Local                                                                                                                                                                         |
| xAI (Grok)                        | Cloud, paid, grok-vision                                                                                                                                                      |

### Secondary endpoints

By default, the Multimodal source uses the primary endpoint configured in the API Connections tab.
You can also set up a secondary endpoint specifically for multimodal captioning.

- Open the **Image Captioning** panel in the **<i class="fa-solid fa-cubes"></i> Extensions** panel.
- Select "Multimodal" as the captioning source and a preferred API provider.
- Enter a valid URL for the secondary endpoint in the "Secondary captioning endpoint URL" field.
- Check the "Use secondary URL" box to enable the secondary endpoint.

> Do not append `/v1` or `/chat/completions` to the end of the URL. The extension will handle that automatically.

This is only supported by the following APIs:

- KoboldCpp
- llama.cpp
- Ollama
- Text Generation WebUI (oobabooga)
- vLLM

### Source-specific guides

#### KoboldCpp

For general information on installing and using [KoboldCpp](https://github.com/LostRuins/koboldcpp), see the [KoboldCpp documentation](https://github.com/LostRuins/koboldcpp/wiki).

To use KoboldCpp for multimodal captioning:

* get a multimodal-capable model, trained to process text and image prompts at the same time.
* also get the multimodal projections for the model. These weights allow the model to understand how the text and image parts of the input relate to each other.
* load the model and projections in the KoboldCpp launch GUI or command line interface.

The original and classic local multimodal model is LLaVA. GGUF-format files for the model and projections are available from [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile). To load them from the command line, set the model and projections with the `--model` and `--mmproj` flags. For example:

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... other flags ...
```

Some LLaVA finetunes you can try: [xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf), [xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf).

You can use multimodal projections for the base model that your particular finetune was built from. Projections for some common base models are available from [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main).
