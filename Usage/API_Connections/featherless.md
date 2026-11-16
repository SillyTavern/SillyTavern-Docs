---
order: 11
route: /usage/api-connections/featherless/
---

# Featherless

Featherless is a serverless AI inference platform that makes tens of thousands of open-source AI models instantly available through a single API. That includes many of the roleplay and creative-writing finetunes popular with SillyTavern users.

There are two ways to pay:

- **Chat**: a flat-rate monthly subscription with unlimited requests, made for personal use in apps like SillyTavern.
- **Pay-As-You-Go**: credits deducted per request, for heavier or automated use.

See [Plans](https://featherless.ai/docs/plans) for current prices and limits. Featherless does not log prompts or completions.

!!!info
Chat plans have a concurrency limit, and larger models use more of it. Extensions that make their own requests while you chat count toward it too. See [Concurrency Limits](https://featherless.ai/docs/concurrency-limits) for details.
!!!

- Create a Featherless account: [featherless.ai](https://featherless.ai/)
- [Featherless Models List](https://featherless.ai/models)

## Text Completion

Featherless is built into SillyTavern as a Text Completion source.

1. Select the 'Text Completion' API.
2. Select Featherless as the API type.
3. Create an API key in your [Featherless account settings](https://featherless.ai/account/api-keys) and paste it into the box.
4. Click "Connect".
5. Pick a model in the Featherless Model Selection browser. You can search, sort by name or date, filter by Top / New, and filter by model class.
6. (Optional) Use the "Test Message" button to verify your connection.

## Chat Completion

Featherless also works with the Chat Completion API through the generic OpenAI-compatible source.

1. Select the 'Chat Completion' API.
2. Select 'Custom (OpenAI-compatible)' as the Chat Completion source.
3. Enter `https://api.featherless.ai/v1` as the custom endpoint.
4. Paste your Featherless API key.
5. Select a model from the fetched list, or enter a model ID from the [Featherless Models List](https://featherless.ai/models) manually.
6. Click "Connect", then use "Test Message" to verify your connection.

See [Custom OpenAI-compatible endpoint](/usage/api-connections/openai/#custom-openai-compatible-endpoint) for prompt post-processing options and troubleshooting.
