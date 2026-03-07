---
order: 5
route: /usage/api-connections/tensorix/
---

# Tensorix

[Tensorix](https://tensorix.ai) is an OpenAI-compatible API gateway that provides access to 50+ open-source models with EU-hosted inference and zero data retention. Use it with SillyTavern to access models like DeepSeek, GLM, MiniMax, and Kimi through a single endpoint.

- No subscriptions — pay only for what you use
- EU-hosted (Frankfurt) with zero data retention
- OpenAI-compatible API — works with SillyTavern's Custom (OpenAI-compatible) source

## Setup

1. Create a Tensorix account and get an API key at [app.tensorix.ai](https://app.tensorix.ai/register)

2. In SillyTavern, select the **Chat Completion** API

3. Select **Custom (OpenAI-compatible)** as the Chat Completion Source

4. Enter the following settings:

    | Setting | Value |
    |---------|-------|
    | **Custom Endpoint (Base URL)** | `https://api.tensorix.ai/v1` |
    | **Custom API Key** | Your Tensorix API key |
    | **Custom Model** | See model list below |

5. Click **Connect** and send a test message

## Available Models

| Model | Context | Best For |
|-------|---------|----------|
| `z-ai/glm-5` | 203K | General chat, coding, reasoning |
| `deepseek/deepseek-chat-v3.1` | 164K | Balanced chat and writing |
| `deepseek/deepseek-r1-0528` | 164K | Complex reasoning and analysis |
| `minimax/minimax-m2.5` | 197K | Creative writing, tool use |
| `moonshotai/kimi-k2.5` | 262K | Long context, vision |
| `minimax/minimax-m2` | 197K | Fast responses |
| `meta-llama/llama-4-maverick` | 1M | Very long context |

Type the full model name (e.g., `z-ai/glm-5`) in the Custom Model field.

## Tips

- Set your **Context Size** in SillyTavern to match the model's context window
- For creative writing and roleplay, `deepseek/deepseek-chat-v3.1` works well with default settings
- For long conversations, use `moonshotai/kimi-k2.5` (262K context) or `meta-llama/llama-4-maverick` (1M context)
- Adjust **Max Response Length** based on your use case (1024–4096 for chat, up to 16384 for long-form)

## Troubleshooting

- **Connection failed**: Verify the endpoint is exactly `https://api.tensorix.ai/v1` and your API key is correct
- **Model not found**: Type the full model name manually (e.g., `z-ai/glm-5`)
- **Empty responses**: Increase Max Response Length in generation settings
- **Credit issues**: Check your balance at [app.tensorix.ai/dashboard](https://app.tensorix.ai/dashboard)

## Links

- [Tensorix Website](https://tensorix.ai)
- [Tensorix Models](https://tensorix.ai/models)
- [Tensorix Dashboard](https://app.tensorix.ai/dashboard)
