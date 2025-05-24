---
order: 20
---
# Chat Completions

## OpenAI

### API key

**How to get:**

1. Go to [OpenAI](https://platform.openai.com/) and sign in.
2. Use "[View API keys](https://platform.openai.com/account/api-keys)" option to create a new API key.

**Important!**

*Lost API keys can't be restored! Make sure to keep it safe!*

## Claude

If you have access to Anthropic's Claude API:

- Select 'Claude' for 'Chat Completion Source'.
- Input your API key.
- Click connect.

## Mistral AI

Mistral AI is a team developing both open and proprietary models with high scientific standards and a focus on openness. You can run their models locally or through their API service, La Plateforme.

### API

- The first step is to create an account on [La Plateforme](https://console.mistral.ai/).
- Once that's done, you can choose a [plan](https://console.mistral.ai/billing/plans) and set up your payment information or opt for the Free Tier.
- Next, you can create your [API key](https://console.mistral.ai/api-keys/). You may need to wait a couple of minutes before the key becomes valid!

**Important!**  
*Lost API keys can't be restored! You would have to create a new one. Make sure to keep it safe!*

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

## Connecting

To access this feature:

1. Switch to the 'Chat Completion' API type
2. Select 'Custom (OpenAI-compatible)' for 'Chat Completion Source'

Enter the custom endpoint URL and an API key if required. For example, TabbyAPI requires an API key for authentication.

> **Hint:** If you experience connection issues, try adding `/v1` to the end of the endpoint URL. Do NOT add the `/chat/completions` suffix.

## Selecting a Model

If the custom API implements the `/v1/models` endpoint to provide a list of available models, you can choose from a dropdown list. Otherwise, use the text field to manually input a model ID.

Check 'Bypass API status check' to prevent SillyTavern from alerting you about a non-functioning API endpoint. Enable this option if your API endpoint works properly but SillyTavern continues to display warnings.

Click "Test Message" to verify connectivity by sending a simple prompt to the model.

## Prompt Post-Processing

!!!warning
**Note:** Tool Calling is not supported when any Post-Processing option except "None" is selected.
!!!

Some endpoints may impose specific restrictions on the format of incoming prompts, such as requiring only one system message or strictly alternating roles.

SillyTavern provides built-in prompt converters to help meet these requirements (from least to most restrictive):

1. None - no explicit processing applied unless strictly required by the API
2. Merge consecutive messages from the same role
3. Semi-strict - merge roles and allow only one optional system message
4. Strict - merge roles, allow only one optional system message, and require a user message to be first
5. Single user message - merge all messages from all roles into a single user message

Less restrictive options have no effect on more restrictive endpoints implemented in SillyTavern other than "Custom OpenAI-compatible"; Custom may error upon invalid request.

In strict mode, if no user message exists before the first assistant message, then `promptPlaceholder` from `config.yaml` will be inserted, which by default is "\[Start a new chat]".
