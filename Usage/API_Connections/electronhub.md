---
order: 25
---
# Electron Hub

Electron Hub is a unified OpenAI-compatible platform that resells access to models from multiple vendors through a single API key. SillyTavern's integration lets you browse the catalog directly from the **API Connections → Chat Completions** panel and unlock extra quality-of-life features for choosing the right model.

## Get an API key

1. Create an account at [Electron Hub](https://playground.electronhub.ai/console).
2. Generate an API key from the **Console → API Keys** page.
3. Keep the key safe—Electron Hub will only show it when you create it.

## Connecting in SillyTavern

1. Open **API Connections → Chat Completions** and select **Electron Hub** as the source.
2. Paste your API key into the **Electron Hub API Key** field and click **Connect**.
3. Use the **View Remaining Credits** link to jump straight to the Electron Hub console if you need to top up.
4. Pick a model from the dropdown. The search-enabled list shows each model's context length, pricing, and capability icons:
   - 👁️ Vision support
   - 🧠 Reasoning mode availability
   - 🔧 Tool/function calling support
   - 👑 Subscription-only models

## Sorting and grouping the catalog

Use the **Electron Hub Model Sorting** drawer to tune how the catalog is presented:

- **Alphabetically** (default)
- **Input Price (cheapest)**
- **Output Price (cheapest)**
- **Context Size** (largest first)

Enable **Group by vendors** to split the dropdown into sections (OpenAI, Anthropic, etc.). Sorting applies within each group, so you can e.g. list the cheapest model from every provider at a glance.

## Prompt cost estimator

When Electron Hub is the active source, SillyTavern now shows a **Max prompt cost** value. The estimate combines your configured **Max Response Length** and **Max Prompt Size** to project the highest possible price for a single request with the currently selected model. Adjusting token limits or switching models updates the estimate automatically.

## Advanced settings

Electron Hub now supports the same logit bias tools as other OpenAI-compatible backends. Any bias preset you configure in SillyTavern will apply when the Electron Hub connector is active.

## Text-to-Speech

Electron Hub also exposes voice models through the TTS extension. See the [TTS guide](/extensions/TTS.md#electron-hub) for setup details.
