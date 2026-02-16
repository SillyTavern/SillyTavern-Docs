---
route: /usage/api-connections/dreamgen/
---

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
- Lucid Base (in API called `lucid-v1-base` or `lucid-v1-medium`) -- corresponds to the weight-available [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface).

Lucid Base uses much fewer credits and is faster, while Lucid Max is more creative and is able to handle more complex instructions and narratives.

## Settings

The Lucid V1 DreamGen models use an extension of the Llama 3 chat template optimized for role-play and writing. They work best with a specific system prompt.

We strongly recommend to start with one of these master presets:

- Make sure that you have instruct mode enabled and select all checkboxes when importing.
- [DreamGen Lucid V1 Role-Play preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [DreamGen Lucid V1 Story preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

These presets come with built-in support for `/sys` to send instructions to the model. You can use those to steer the plot or control character's actions.

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

Other resources:

- [**Detailed DreamGen + SillyTavern guide**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [Detailed Lucid V1 prompt format documentation](https://dreamgen.com/docs/models/lucid-v1).
- [DreamGen + SillyTavern role-play demo](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [DreamGen + SillyTavern story-writing demo](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [Tips for making your own scenarios](https://v2.dreamgen.com/docs/scenario-editor)

## FAQ

### How can I make the responses longer or shorter?

You can set the `Last Assistant Prefix` in your formatting presets.

For long messages:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

For short messages:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Make sure to preserve all newlines, including the two at the end.

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

You can also include writing style description in your card or system prompt, e.g.:

```txt
## Style

<your description>
```

See the ["Style" documentation](https://v2.dreamgen.com/docs/scenario-editor#style) to learn more and see some examples.

### How can I steer the role-play / story?

Use the `/sys` option to send instructions to the model. Some examples:

> The innkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[See it in action.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
