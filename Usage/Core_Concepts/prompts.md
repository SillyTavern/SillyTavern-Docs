---
order: prompts
icon: pencil
---

# Prompts

When you send a message to your AI, the text you write is combined with other text to form a single request that's sent to the AI. This combined text is called a "prompt" or sometimes the "request" or "context."

The prompt can include a variety of different types of text, including:

* [Main instructions](#main-prompt) to the AI about how to generate a response
* Definitions of the [roles that the AI should take on](characterdesign.md)
* Definitions of [the role that you are taking on](personas.md)
* [Information about the "world"](worldinfo.md) that the AI is interacting with
* Relevant documents or information from [Data Bank](data-bank.md)
* [Summaries](/extensions/Summarize.md) of the past conversation
* Results of [web searches](/extensions/WebSearch.md) or other [external data sources](/For_Contributors/Function-Calling.md)
* Previous messages in the conversation
* **Your message to the AI**
* Final instructions for the AI about how to generate a response

This can be a lot to manage! To help you understand how to structure and modify the request that's sent to the AI, SillyTavern identifies different elements that you might want to include in your prompt. You can then structure your prompt to include the things that make sense for the way you want to interact with the AI.

Many of these elements are explained in the sections where you will change them. For example, to describe the role that you would like the AI to take on, you could use the [Description](characterdesign.md#personality-summary) field in [Character Design](characterdesign.md).

## Viewing the Prompt

Reading the final prompt that's sent to the AI is very helpful for understanding what the AI was told, and why it generated the response that it did. You can view the prompt in several ways:

* Using the Prompt Itemization icon on the reply message from the AI
* Using the [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector) extension
* Checking the logs in the terminal window that you're running SillyTavern in
* Checking the console in your browser's developer tools

## Changing how the Prompt is Built

Presenting all the parts of your prompt to the AI in the right way is crucial for getting the best responses. You can control how the prompt is built.

+++ Text Completion APIs

Use the [Advanced Formatting](advancedformatting.md) panel to customize prompt construction for Text Completion APIs.

+++ Chat Completion APIs

Use the [Prompt Manager](prompt-manager.md) to customize prompt construction for Chat Completion APIs.

+++


## Common Prompt Elements

Some prompts are set for every message you send to the AI. These are explained here.

### Main Prompt

The Main Prompt (or System Prompt) defines the general instructions for the model to follow. It sets the tone and context for the conversation. For example, it tells the model to act as an AI assistant, a writing partner, or a fictional character. 

Supports substitutions via any of the supported [\{\{macro\}\}](characterdesign.md#macros-replacement-tags) parameters.

For example:

> You are a helpful AI assistant. You should provide useful information and help \{\{user\}\} with their questions.

+++ Text Completion APIs

The [System Prompt](advancedformatting.md#system-prompt) is a part of the [Story String](context-template.md#story-string) and usually the first part of the prompt that the model receives.

+++ Chat Completion APIs

The Main Prompt is one of the default prompts in [Prompt Manager](prompt-manager.md). It is usually the first message in the context that the model receives, attributed to ("sent by") the system role.

+++

### Post-History Instructions

Post-History Instructions are additional instructions that are sent to the AI after the main prompt and the user message. They can be used to provide additional context or instructions to the AI based on the conversation history.

Since the Post-History Instructions are sent after the user message, they are the final instructions that the AI receives before generating a response. The AI usually gives them a higher priority than the main prompt, and they can override the main prompt's instructions.

+++ Text Completion APIs

Post-History Instructions cannot be defined globally, as far as I know... You could achieve the same effect with an [Author's Note](Author's-Note.md).

To use per-character Post-History Instructions, add them to the character's [Post-History Instructions](characterdesign.md) and enable **both** [Prefer Char. Instructions](user-settings.md) and [Allow Post-History Instructions](context-template.md#allow-post-history-instructions).

+++ Chat Completion APIs

Post-History Instructions is one of the default prompts in [Prompt Manager](prompt-manager.md). It is usually the last message in the context that the model receives, attributed to ("sent by") the system role.

+++
