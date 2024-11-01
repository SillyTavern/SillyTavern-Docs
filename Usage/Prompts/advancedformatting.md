---
order: prompts-10
---

# Advanced Formatting

The settings provided in this section allow for more control over the [prompt-building](prompts.md) strategy, primarily for Text Completion APIs.

Most of the settings in this panel do not apply to Chat Completions APIs as they are governed by the prompt manager system instead.

+++ Text Completion APIs
* [System Prompt](#system-prompt)
* [Context Template](#context-template)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++ Chat Completion APIs
* System Prompt: not applicable, use [Prompt Manager](prompt-manager.md)
* Context Template: not applicable, use [Prompt Manager](prompt-manager.md)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++

## System Prompt

!!! Applies to: Text Completion APIs
For equivalent settings in Chat Completion APIs, use [Prompt Manager](prompt-manager.md). The **Main Prompt** is the equivalent of the System Prompt in Chat Completion APIs.
!!!

The System Prompt defines the general instructions for the model to follow. It sets the tone and context for the conversation. For example, it tells the model to act as an AI assistant, a writing partner, or a fictional character.

The System Prompt is a part of the [Story String](context-template.md#story-string) and usually the first part of the prompt that the model receives.

See the [prompting guide](prompts.md#main-prompt-system-prompt) to learn more about the System Prompt.

## Context Template

!!! Applies to: Text Completion APIs
For equivalent settings in Chat Completion APIs, use [Prompt Manager](prompt-manager.md).
!!!

Usually, AI models require you to provide the character data to them in some specific way. SillyTavern includes a list of pre-made conversion rules for different models, but you may customize them however you like. 

The options for this section are explained in [Context Template](context-template.md).

## Tokenizer

A tokenizer is a tool that breaks down a piece of text into smaller units called tokens. These tokens can be individual words or even parts of words, such as prefixes, suffixes, or punctuation. A rule of thumb is that one token generally corresponds to 3~4 characters of text.

The options for this section are explained in [Tokenizer](tokenizer.md).

## Custom Stopping Strings

Accepts a JSON-serialized array of stopping strings. Example: `["\n", "\nUser:", "\nChar:"]`. If you're unsure about the formatting, use an [online JSON validator](https://jsonlint.com/). If the model output **ends** with any of the stop strings, they will be removed from the output.

Supported APIs:

1. KoboldAI Classic (versions 1.2.2 and higher) or KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI (ooba), Tabby, Aphrodite, Mancer, TogetherAI, Ollama, etc.
4. NovelAI
5. OpenAI (max 4 strings) and compatible APIs
6. OpenRouter (both Text and Chat Completion)
7. Claude
8. Google AI Studio
