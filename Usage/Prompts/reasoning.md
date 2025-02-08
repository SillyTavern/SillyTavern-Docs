---
tags: ['>=1.12.12']
---

# Reasoning

!!! warning Warning
This information refers to a pre-release feature that is still in development. It may be incomplete, contain errors, or be subject to breaking changes.
!!!

In language models, reasoning (also known as model thinking) refers to a chain-of-thought (CoT) technique that mirrors human problem-solving through step-by-step analysis. SillyTavern provides several features that make the use of reasoning models more efficient and consistent across supported backends.

## Configuration

!!!
Most reasoning-related settings can be configured in the "Reasoning" section of **<i class="fa-solid fa-font"></i> Advanced Formatting** panel.
!!!

Reasoning blocks appear in the chat as collapsible message sections. They can be added manually, automatically by the backend, or through response parsing (see below).

By default, reasoning blocks are collapsed to save space. Click a block to expand and view its contents. You can set blocks to expand automatically by enabling **<i class="fa-solid fa-expand"></i> Auto-Expand** in the reasoning settings.

When a reasoning block is expanded, you can copy or edit its contents using the **<i class="fa-solid fa-copy"></i> Copy** and **<i class="fa-solid fa-pencil"></i> Edit** buttons.

## Adding Reasoning

### Manually

Add a reasoning block to any message through the **<i class="fa-solid fa-pencil"></i> Message Edit** menu. Click **<i class="fa-solid fa-lightbulb"></i>** while editing to add a reasoning section. Third-party extensions can also add reasoning by writing to the `extra.reasoning` field of the message object before adding it to the chat.

### With a Command

Use the `/reasoning-set` STscript command to add reasoning to a message. The command takes `at` (message ID, defaults to the last message) and reasoning text as arguments.

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### By Backend

If your chosen LLM backend and model support reasoning output, enable "Request Model Reasoning" in the **<i class="fa-solid fa-sliders"></i> AI Response Configuration** panel.

Supported sources:

- DeepSeek
- OpenRouter

### By Parsing

Enable "Auto-Parse" in the **<i class="fa-solid fa-font"></i> Advanced Formatting** panel to automatically parse reasoning from the model's output.

The response must contain a reasoning section wrapped in configured Prefix and Suffix sequences. The sequences provided by default correspond to the DeepSeek R1 reasoning format.

Example with prefix `<think>` and suffix `</think>`:

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

!!!
For streamed responses, reasoning will only be parsed after the stream completes.
!!!

## Prompting with Reasoning

By default, recognized reasoning block contents are not sent back to the model. To include reasoning in prompts, enable "Add to Prompts" in the **<i class="fa-solid fa-font"></i> Advanced Formatting** panel. Content will be wrapped in configured Prefix and Suffix sequences and separated by a configured Separator. The Max Additions numeric setting controls how many reasoning blocks can be included, counting from the end of the prompt.

!!!
Most model providers do not recommend sending CoT back to the model in multi-turn conversations.
!!!

## Regex Scripts

Regular expression scripts from the [Regex extension](/extensions/Regex.md) can be applied to the contents of reasoning blocks. Check "Reasoning" in the "Affects" section of the script editor to target reasoning blocks specifically.

Different ephemerality options affect reasoning blocks in the following ways:

1. No ephemerality: reasoning content is permanently changed.
2. Run on edit: regex script will be re-evaluated when the reasoning block is edited.
3. Alter chat display: regex is applied to the reasoning block's display text, not the underlying content.
4. Alter outgoing prompts: regex is only applied to reasoning blocks before they are sent to the model.
