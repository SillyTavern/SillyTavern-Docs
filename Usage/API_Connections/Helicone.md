---
order: 11
route: /usage/api-connections/helicone/
---

# Helicone

!!!info
Helicone AI Gateway is available as a Chat Completion source, providing unified access to 100+ models across multiple providers including OpenAI, Anthropic, Google, Meta, and more.
!!!

Want access to all the latest AI models without managing multiple API keys and billing accounts? Want to track your usage and billing automatically? Use Helicone AI Gateway.

Helicone works by providing a single unified endpoint to access models from different providers like OpenAI, Anthropic, Google, and others, with features like automatic fallbacks, web search integration, and comprehensive analytics.

It supports both BYOK (Bring Your Own Keys) and PTB (Pass-Through Billing) models. You can use your existing API keys from various providers or pay through Helicone's unified billing system (recommended).

- Create a Helicone account: [helicone.ai](https://helicone.ai/)
- Get your API key: [Helicone API Keys](https://us.helicone.ai/settings/api-keys)
- [Helicone Model Registry](https://helicone.ai/models)

![Helicone-ConnectionPanel](/static/helicone-connection.png)

From top to bottom (see image above):

1. Select the 'Chat Completion' API.
2. Select Helicone as the source.
3. Enter your Helicone API key from the [API Keys page](https://us.helicone.ai/settings/api-keys).
4. Click "Connect" and select a model.
5. (Optional) Enable "Web Search" for Claude models to add internet access.
6. (Optional) Add custom properties in JSON format for analytics tracking.
7. (Optional) Use the "Test Message" button to verify your connection.

## Features

### Provider Routing with Fallbacks
Helicone handls provider routing and fallbacks automatically, so you don't have to worry about it.

If you want to set up your own routing and fallbacks manually, you can do so by specifying multiple providers in the model name:
- `gpt-4o/openai,gpt-4o/azure` - Try OpenAI first, fall back to Azure
- `claude-3-sonnet/anthropic,claude-3-sonnet/bedrock` - Try Anthropic first, fall back to AWS Bedrock

### Custom Properties
Helicone allows you to add custom metadata to your requests so you can filter and track your usage in the Helicone observability dashboard for deeper customization and analytics.

Add custom metadata to your requests using JSON format in the Custom Properties field:
```json
{
  "user_id": "12345",
  "session_type": "roleplay",
  "character": "assistant"
}
```

These properties appear in your Helicone dashboard for analytics and filtering. For additional information, see the [Helicone Custom Properties documentation](https://docs.helicone.ai/features/advanced-usage/custom-properties).

### Model Selection
Helicone provides access to 100+ models including:
- **OpenAI**: GPT-4o, GPT-4 Turbo, GPT-3.5 Turbo
- **Anthropic**: Claude 4, Claude 3.5 Sonnet, Claude 3 Haiku
- **Google**: Gemini Pro, Gemini Flash
- **Meta**: Llama models
- And many more from all major providers!

## Pricing
Helicone uses a pay-per-use model with transparent pricing. You only pay for what you use, with no subscription fees. Pricing varies by model and provider - see the [Helicone Models page](https://helicone.ai/models) for current rates.

## Support
- [Helicone Documentation](https://docs.helicone.ai/)
- [Helicone Discord](https://discord.gg/bnGpD4uG)
- [GitHub Issues](https://github.com/Helicone/helicone)
