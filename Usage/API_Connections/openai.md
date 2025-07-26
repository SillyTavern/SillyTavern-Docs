---
order: 20
---
# Chat Completions

## Source-specific instructions

**Important!**

Most API platforms allow to view the generated API key only once, at the time of its creation. If you lose it, you will need to generate a new one. Make sure to keep it safe!

### OpenAI

Use OpenAI's developer platform to access various OpenAI models, including gpt-4o, gpt-4.1, o3, etc.

**How to get an API key:**

1. Go to [OpenAI](https://platform.openai.com/) and sign in.
2. Use "[View API keys](https://platform.openai.com/account/api-keys)" option to create a new API key.

### Claude

Claude is a family of AI models developed by Anthropic. You can access Claude models through the Anthropic console.

**How to get an API key:**

1. Go to [Anthropic Console](https://console.anthropic.com/) and sign in.
2. Use the "[Get API Key](https://console.anthropic.com/settings/keys)" section to create a new API key.

### Mistral AI

Mistral AI is a team developing both open and proprietary models with high scientific standards and a focus on openness. You can run their models locally or through their API service, La Plateforme.

**How to get an API key:**

1. The first step is to create an account on [La Plateforme](https://console.mistral.ai/).
2. Once that's done, you can choose a [plan](https://console.mistral.ai/billing/plans) and set up your payment information or opt for the Free Tier.
3. Next, you can create your [API key](https://console.mistral.ai/api-keys/). You may need to wait a couple of minutes before the key becomes valid!

### DeepSeek

DeepSeek Platform provides access to the latest DeepSeek models through an API. They offer a range of models, including DeepSeek V3 and DeepSeek R1.

**How to get an API key:**

1. Sign up on the [DeepSeek Platform](https://platform.deepseek.com/).
2. After signing up and topping up your account, you can create an API key in the "[API keys](https://platform.deepseek.com/api_keys)" section.

## Custom OpenAI-compatible endpoint

!!!warning
It is important to note that we do not provide support for possible issues that you may have!
We do not guarantee compatibility with every possible API endpoint!
!!!

!!!
If you intend to use this feature to use a local endpoint, like TabbyAPI, Oobabooga, Aphrodite, or any like those, you might want to check out the [built-in compatibility for those](/Usage/API_Connections/index.md) instead. The custom endpoint feature is mainly intended for use with other services and programs that expose an OpenAI-compatible API Chat Completion endpoint.

Most Text Completion APIs support far greater customization options than OpenAI's standards allow for. These greater customization options, such as the Min-P sampler, may be worthwhile for SillyTavern users to check out, which can greatly improve the quality of generations.
!!!

You can configure an alternative endpoint for the Chat Completions backend. This custom endpoint can connect to any server that supports the generic OpenAI API schema.

Examples of compatible backends include:

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

### Connecting

To access this feature:

1. Switch to the 'Chat Completion' API type
2. Select 'Custom (OpenAI-compatible)' for 'Chat Completion Source'

Enter the custom endpoint URL and an API key if required. For example, TabbyAPI requires an API key for authentication.

!!!tip
**Hint:** If you experience connection issues, try adding `/v1` to the end of the endpoint URL. Do NOT add the `/chat/completions` suffix.
!!!

### Selecting a Model

If the custom API implements the `/v1/models` endpoint to provide a list of available models, you can choose from a dropdown list. Otherwise, use the text field to manually input a model ID.

Check 'Bypass API status check' to prevent SillyTavern from alerting you about a non-functioning API endpoint. Enable this option if your API endpoint works properly but SillyTavern continues to display warnings.

Click "Test Message" to verify connectivity by sending a simple prompt to the model.

## Prompt Post-Processing

!!!warning
**Note:** Tool Calling is not supported when Post-Processing option with "no tools" is used!
!!!

Some endpoints may impose specific restrictions on the format of incoming prompts, such as requiring only one system message or strictly alternating roles.

SillyTavern provides built-in prompt converters to help meet these requirements (from least to most restrictive):

1. None - no explicit processing applied unless strictly required by the API
2. Merge consecutive messages from the same role
3. Semi-strict - merge roles and allow only one optional system message
4. Strict - merge roles, allow only one optional system message, and require a user message to be first
5. Single user message - merge all messages from all roles into a single user message

Merge, semi-strict, and strict additionally remove any tool calls from the prompt, unless the "with tools" variant is selected. This is useful for APIs that do not support tool calling and your existing prompts contain tool calls.

Less restrictive options have no effect on more restrictive endpoints implemented in SillyTavern other than "Custom OpenAI-compatible"; Custom may error upon invalid request.

In strict mode, if no user message exists before the first assistant message, then `promptPlaceholder` from `config.yaml` will be inserted, which by default is "\[Start a new chat]".
