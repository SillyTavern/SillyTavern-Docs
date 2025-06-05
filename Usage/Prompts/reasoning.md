---
tags: ['>=1.12.12']
---

# Reasoning

In language models, reasoning (also known as model thinking) refers to a chain-of-thought (CoT) technique that mirrors human problem-solving through step-by-step analysis. SillyTavern provides several features that make the use of reasoning models more efficient and consistent across supported backends.

## Common issues

1. When using reasoning models, the model's internal reasoning process consumes part of your response token allowance, even if this reasoning isn't shown in the final output (e.g. o3-mini or Gemini Thinking). If you notice your responses are coming back incomplete or empty, you should try adjusting the Max Response Length setting found in the **<i class="fa-solid fa-sliders"></i> AI Response Configuration** panel. For reasoning models, it's typical to use significantly higher token limits - anywhere from 1024 to 4096 tokens - compared to standard conversational models.

## Configuration

!!!
Most reasoning-related settings can be configured in the "Reasoning" section of **<i class="fa-solid fa-font"></i> Advanced Formatting** panel.
!!!

Reasoning blocks appear in the chat as collapsible message sections. They can be added manually, automatically by the backend, or through response parsing (see below).

By default, reasoning blocks are collapsed to save space. Click a block to expand and view its contents. You can set blocks to expand automatically by enabling **Auto-Expand** in the reasoning settings.

When a reasoning block is expanded, you can copy or edit its contents using the **<i class="fa-solid fa-copy"></i> Copy** and **<i class="fa-solid fa-pencil"></i> Edit** buttons.

Some models models support reasoning, but will not send their thoughts back. It is possible to still show the reasoning block with reasoning time for those by toggling the **Show Hidden** setting.

## Adding Reasoning

### Manually

Add a reasoning block to any message through the **<i class="fa-solid fa-pencil"></i> Message Edit** menu. Click **<i class="fa-solid fa-lightbulb"></i>** while editing to add a reasoning section. Third-party extensions can also add reasoning by writing to the `extra.reasoning` field of the message object before adding it to the chat.

### With a Command

Use the `/reasoning-set` STscript command to add reasoning to a message. The command takes `at` (message ID, defaults to the last message) and reasoning text as arguments.

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### By Backend

If your chosen LLM backend and model support reasoning output, enabling "Request model reasoning" in the **<i class="fa-solid fa-sliders"></i> AI Response Configuration** panel will add a reasoning block containing the model's thinking process.

Supported sources:

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)

"Request model reasoning" does not determine whether a model does reasoning. Claude and Google (2.5 Flash) allow thinking mode to be toggled; see [Reasoning Effort](#reasoning-effort).

### By Parsing

Enable "Auto-Parse" in the **<i class="fa-solid fa-font"></i> Advanced Formatting** panel to automatically parse reasoning from the model's output.

The response must contain a reasoning section wrapped in configured Prefix and Suffix sequences. The sequences provided by default correspond to the DeepSeek R1 reasoning format.

Example with prefix `<think>` and suffix `</think>`:

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## Prompting with Reasoning

By default, recognized reasoning block contents are not sent back to the model. To include reasoning in prompts, enable "Add to Prompts" in the **<i class="fa-solid fa-font"></i> Advanced Formatting** panel. Reasoning content will be wrapped in configured Prefix and Suffix sequences and separated by a Separator from the main context. The Max Additions numeric setting controls how many reasoning blocks can be included, counting from the end of the prompt.

!!!
Most model providers do not recommend sending CoT back to the model in multi-turn conversations.
!!!

### Continuing from Reasoning

A special case when the reasoning can be sent back to the model without having the "Add to Prompts" toggle enabled is when the generation is continued (e.g. by pressing "Continue" from the **<i class="fa-solid fa-bars"></i> Options** menu), but the message being continued contains only the reasoning without an actual content. This gives the model an opportunity to finish an incomplete reasoning and start generating the main content. The prompt will be sent as follows:

```txt
<think>
Incomplete reasoning...
```

## Regex Scripts

Regular expression scripts from the [Regex extension](/extensions/Regex.md) can be applied to the contents of reasoning blocks. Check "Reasoning" in the "Affects" section of the script editor to target reasoning blocks specifically.

Different ephemerality options affect reasoning blocks in the following ways:

1. No ephemerality: reasoning content is permanently changed.
2. Run on edit: regex script will be re-evaluated when the reasoning block is edited.
3. Alter chat display: regex is applied to the reasoning block's display text, not the underlying content.
4. Alter outgoing prompts: regex is only applied to reasoning blocks before they are sent to the model.

## Reasoning Effort

Reasoning Effort is a Chat Completion setting in the **<i class="fa-solid fa-sliders"></i> AI Response Configuration** panel that influences how many tokens may potentially be used on reasoning. The effect of each option depends on the source connected to. Currently, Auto simply means the relevant parameter is not included in the request.

| Option  | Claude (≤ 21333 if no streaming) | AI Studio / Vertex AI (≤ 32768) | AI Studio / Vertex AI (≤ 24576)   | OpenAI (keyword)     | OpenRouter (keyword)             | xAI (Grok) (keyword) |
| ------- | -------------------------------- | ------------------------------- | --------------------------------- | -------------------- | -------------------------------- | -------------------- |
| Models  | Opus 4, Sonnet 4/3.7             | Gemini 2.5 Pro                  | Gemini 2.5 Flash                  | o4-mini, o3\*, o1\*  | applicable models                | grok-3-mini          |
| Auto    | not specified, **no thinking**   | not specified                   | not specified                     | not specified        | not specified, effect depends    | not specified        |
| Minimum | budgets 1024 tokens              | budgets 128 tokens              | budgets 0 tokens, **no thinking** | "low"                | "low", or 20% of max response    | "low"                |
| Low     | 15% of max response, min 1024    | 15% of max response             | 15% of max response               | "low"                | "low", or 20% of max response    | "low"                |
| Medium  | 25% of max response, min 1024    | 25% of max response             | 25% of max response               | "medium"             | "medium", or 50% of max response | "low"                |
| High    | 50% of max response, min 1024    | 50% of max response             | 50% of max response               | "high"               | "high", or 80% of max response   | "high"               |
| Maximum | 95% of max response, min 1024    | lower of max response or 32768  | lower of max response or 24576    | "high"               | "high", or 80% of max response   | "high"               |

- For Claude, budget is capped to 21333 if streaming is disabled. If the calculated budget would be less than 1024, then max response is changed to 2048.
- For Google AI Studio and Vertex AI, budget is capped to 24576 tokens, regardless of the streaming setting.
- For OpenRouter, only an OpenAI-style keyword is sent.
