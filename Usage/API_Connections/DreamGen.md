# DreamGen

DreamGen is an app and an API for AI-powered role-playing and story-writing. They have a free tier, as well as a paid subscription that allows unlimited monthly access to their high-quality in-house text generation models made specifically for the purpose of steerable AI role-playing and story-writing. Create an account to get started: <https://dreamgen.com/>.

The (free) credits reset at the start of each calendar month. See [pricing](https://dreamgen.com/pricing) to see the credit cost for each model and [usage](https://dreamgen.com/account/usage) to see your remaining credits.

## Connecting to DreamGen

### Get API Key

Go to the [DreamGen API keys](https://dreamgen.com/account/api-keys) page and click the "New API Key" button. Make sure the API Key is copied into your clipboard.

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### Connect

1. Go to the SillyTavern connection settings.
2. Select API: Text Completion
3. Select API Type: DreamGen
4. Enter the API key
5. (optional) Pick a model

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## Models

DreamGen API offers several models of different sizes.

- Lucid Max (in API called `lucid-v1-max` or `lucid-v1-extra-large`)
- Lucid Base (in API called `lucid-v1-base` or `lucid-v1-medium`) -- corresponds to the weight-available [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1-hf).

Lucid Base uses much less credits and is faster, while Lucid Max is more creative and is able to handle more complex instructions and narratives.

## Settings

The DreamGen models expect a specific input format, which is [documented in detail here](https://dreamgen.com/docs/models/lucid-v1).

SillyTavern comes with built-in presets made for DreamGen called `DreamGen Lucid V1 Role-Play` and `DreamGen Lucid V1 Story` that mimic the format. Make sure to use these settings as your baseline.

If you can't find these presets in your SillyTavern installation, either update to the latest `staging` version, or import the presets from these files:

- [DreamGen Lucid V1 Role-Play preset](https://huggingface.co/dreamgen/lucid-v1-nemo/raw/main/resources/sillytavern/presets/role_play_basic.json)
- [DreamGen Lucid V1 Story preset](https://huggingface.co/dreamgen/lucid-v1-nemo/raw/main/resources/sillytavern/presets/writing_basic.json)

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

As far as sampler settings, start with:

-   Min P: 0.05
-   Temperature: 0.6
-   DRY:
    - Multiplier: 0.6
    - Base: 1.5
    - Allowed Length: 3

The preset comes with built-in support for `/sys` to send instructions to the model. You can use those to steer the plot or control character's actions.

Other resources:

- [**Detailed DreamGen + SillyTavern guide**](https://dreamgen.com/docs/sillytavern)
- [DreamGen + SillyTavern role-play demo](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [DreamGen + SillyTavern story-writing demo](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [Tips for making your own scenarios](https://v2.dreamgen.com/docs/scenario-editor)

## FAQ

### How can I make the responses longer or shorter? 

You can set the `Last Assistant Prefix` in your formatting presets.

For long messages:

```
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

For short messages:

```
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Make sure to preserve all newlines, including the two at the end.

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

You can also include writing style description in your card or system prompt, e.g.:

```
## Style

<your description>
```

See the ["Style" documentation](https://v2.dreamgen.com/docs/scenario-editor#style) to learn more and see some examples. 

### How can I steer the role-play / story?

Use the `/sys` option to send instructions to the model. Some examples:

> The inkeeper offers Daria and the others a pint of ale. 

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[See it in action.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
