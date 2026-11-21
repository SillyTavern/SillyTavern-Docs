---
order: 95
route: /usage/api-connections/crowllm/
---

# CrowLLM

!!!info
CrowLLM is used through SillyTavern's built-in **Custom (OpenAI-compatible)** Chat Completion source. There is no separate CrowLLM entry in the source list.
!!!

[CrowLLM](https://crowllm.com) is an OpenAI-compatible gateway: one endpoint and one API key for many models from different vendors, with a zero-rated shelf of ids that end in `:free`.

- Create an account and an API key: [crowllm.com](https://crowllm.com/)
- Served model list (no auth needed): [crowllm.com/v1/models](https://crowllm.com/v1/models)
- Pricing: [crowllm.com/pricing](https://crowllm.com/pricing)

## Setup

1. Open the **API Connections** panel and choose **Chat Completion** as the API.
2. Set **Chat Completion Source** to **Custom (OpenAI-compatible)**.
3. In **Custom Endpoint (Base URL)**, enter `https://crowllm.com/v1`.
4. Paste your CrowLLM API key into **API Key**. Free ids still need a key; they are rate-limited rather than unauthenticated.
5. Pick a model id from the list, or paste one, for example `deepseek-v4-flash` or `qwen-3.8-27b:free`.
6. Click **Connect**, then use **Test Message** to verify the connection.

!!!tip
Ids ending in `:free` bill at zero and need no card. They are rate-limited, so they suit casual chats rather than long roleplay sessions.
!!!
