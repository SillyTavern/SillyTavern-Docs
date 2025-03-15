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

If your chosen LLM backend and model support reasoning output, enable "Request Model Reasoning" in the **<i class="fa-solid fa-sliders"></i> AI Response Configuration** panel.

Supported sources:

- DeepSeek
- OpenRouter

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
