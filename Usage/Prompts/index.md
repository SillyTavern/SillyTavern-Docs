---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# Prompts

When you send a message to your AI, the text you write is combined with other text to form a single request that's sent to the AI. This combined text is called a "prompt" or sometimes the "request" or "context."

The prompt can include a variety of different types of text, including:

* [Main instructions](#main-prompt-system-prompt) to the AI about how to generate a response
* Definitions of the [roles that the AI should take on](/Usage/Characters/characterdesign.md)
* Definitions of [the role that you are taking on](/Usage/personas.md)
* [Information about the "world"](/Usage/worldinfo.md) that the AI is interacting with
* Relevant documents or information from [Data Bank](/Usage/Characters/data-bank.md)
* [Summaries](/extensions/Summarize.md) of the past conversation
* Results of [web searches](/extensions/WebSearch.md) or other [external data sources](/For_Contributors/Function-Calling.md)
* Previous messages in the conversation
* **Your message to the AI**
* [Final instructions](#post-history-instructions) for the AI about how to generate a response

This can be a lot to manage! To help you understand how to structure and modify the request that's sent to the AI, SillyTavern identifies different elements that you might want to include in your prompt. You can then structure your prompt to include the things that make sense for the way you want to interact with the AI.

Many of these elements are explained in the sections where you will change them. For example, to describe the role that you would like the AI to take on, you could use the [Description](/Usage/Characters/characterdesign.md#personality-summary) field in [Character Design](/Usage/Characters/characterdesign.md).

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

## Main Prompt (System Prompt)

The Main Prompt (or System Prompt) defines the general instructions for the model to follow. It sets the tone and context for the conversation. For example, it tells the model to act as an AI assistant, a writing partner, or a fictional character. 

+++ Text Completion APIs

The [System Prompt](advancedformatting.md#system-prompt) is a part of the [Story String](context-template.md#story-string) and usually the first part of the prompt that the model receives.

+++ Chat Completion APIs

The Main Prompt is one of the default prompts in [Prompt Manager](prompt-manager.md). It is usually the first message in the context that the model receives, attributed to ("sent by") the system role.

+++

The default Main Prompt is:

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

The \{\{char\}\} and \{\{user\}\} placeholders are replaced with the names of the character and persona that you've defined in the conversation. 

You can use any of the supported [\{\{macro\}\}](/Usage/Characters/macros.md) tags in the Main Prompt to include information that might vary between conversations or changes as the conversation progresses.

### Adjusting the Main Prompt

The default main prompt helps the model understand what it's expected to do with the character and persona information that follows, how to interpret the past conversation, and what kind of response to generate. It's a flexible general-purpose prompt that works well for many situations, because it establishes that the AI is writing as a character in a conversation with your persona.

However, you can adjust the main prompt to better suit your needs. Here are some common reasons to adjust the main prompt:

* **Provide additional instructions**: for example, you want the AI to explain its reasoning, follow specific rules, or avoid certain topics
* **Clarify the role of the AI**: for example, you want the AI to act as a narrator, a storyteller, or a guide
* **Change the context of the conversation**: for example, you want the AI to respond as if it were an AI assistant, text adventure game, or a writing partner

!!! Try things out and see what works best for you
All the examples in this guide have worked well for other users, but the prompt that works for your needs and the model you're using might be different. Experiment with different instructions and prompting styles to see what works best for you. If you're not sure what to try, you can always ask for help in the [SillyTavern Discord](https://discord.gg/sillytavern).
!!!

Giving the AI additional instructions in the Main Prompt can help it understand what you want from the conversation.

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

The AI will more easily follow instructions about what it should do than what it should not do. For example, if you want the AI to avoid writing in a certain way, it's better to tell it how you want it to write instead. And while *"Do not decide what \{\{user\}\} says or does"* is commonly included in prompts to prevent the AI from controlling your persona, some users find *"Write  \{\{char\}\}'s responses in a way that respects  \{\{user\}\}'s autonomy"* is more effective.

There is often a better place than the Main Prompt to include information about the user or characters, modify a character's writing and speaking style, or give other specific instructions. The Main Prompt is best used for general instructions about the conversation as a whole, or about a type of conversation that you want to have.

### Effect of Message History

When adjusting the main prompt to improve the AI's responses, consder that the AI picks up a lot from the message history. The history is its memory of past events, character interactions and relationships, and its style guide for word choice and writing style.

Use this to your advantage by also providing [example messages](/Usage/Characters/characterdesign.md#examples-of-dialogue) showing how you want the AI to respond. Showing what you want is often easier than trying to explain it!

When your conversation already has history, changing the main prompt has a limited effect on the AI's responses. In terms of events and relationships, the AI assumes that the main prompt occurred in the distant past, and the message history updates it. In terms of writing style and word choice, the AI assumes that all the messages in history were generated according to the rules in the *current* main prompt, and that it should continue to generate messages in the same way. Some suggestions for dealing with this are:

* insert current instructions close to or after the end of message history, for example by using an [Author's Note](/Usage/Characters/Author's-Note.md)
* test your changes to the main prompt by starting a new conversation
* edit the message history to remove or correct examples of unwanted behaviour
* use the [Post-History Instructions](#post-history-instructions) to provide final instructions to the AI

!!! Get it right the first time!
Never let the AI "get away" with something you don't want it to do. If you don't like the AI's response, don't continue the conversation as if it was correct. Instead, modify the prompts, regenerate the message, and continue from there. This will help the AI learn what you want.
!!!

### Removing the "Fictional Chat" Context

There are situations where "fictional chat" might not be the right context for your conversation. 

You can remove the "fictional" context from the Main Prompt:

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

You may not want the AI to think of itself as role-playing at all. Instead of removing the idea of a character, you can remove the idea of an AI:

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### AI as Narrator or Storyteller

What if you want the AI to act as a narrator, describing events from an omniscient perspective, inventing its own characters and settings?

One approach is to create a named character for the AI to use as a narrator. This character could be called "Narrator" or "AI", suggesting that the AI is a general-purpose storyteller, or it could be named after a specific scenario or setting, giving the AI the task of narrating a story in that setting. The details of the setting can then be defined in the [Character](/Usage/Characters/characterdesign.md) or in [World Info](/Usage/worldinfo.md).

You will need to adjust the default main prompt to reflect the AI's role. For a general-purpose narrator, you might use:

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

or for a specific setting:

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

It helps to clarify the role of the user in the conversation. Are your messages part of the story, or are they instructions to the narrator about what your character does or says? An example that includes the user in the story:

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

An example that keeps the user out of the story:

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

Defining the role of the user not only helps the AI understand how to respond to your messages, but also to what extent it is allowed to control your persona. This avoids situations where the AI makes decisions for your persona that you would rather make yourself.

## Post-History Instructions

Post-History Instructions (PHI) are additional instructions sent to the AI after the main prompt and the user message. They can be used to provide additional context or instructions to the AI based on the message history.

Since the Post-History Instructions are sent after the user message, they are the final instructions that the AI receives before generating a response. The AI usually gives them a higher priority than the main prompt, and they can override the main prompt's instructions.

To use per-character Post-History Instructions, add them to the character's [Post-History Instructions](/Usage/Characters/characterdesign.md) and enable [Prefer Char. Instructions](/Usage/User_Settings/index.md). To preserve the globally defined PHI while using character-specific instructions, you can use the `{{original}}` macro in the character's Post-History Instructions field.

+++ Text Completion APIs

Post-History Instructions are defined in the [Advanced Formatting](/Usage/Prompts/advancedformatting.md) panel under the System Prompt category. The Post-History Instructions is added as an invisible user role injection that precedes the last line of the prompt (usually containing a response message "header"). Note that the "Enable System Prompt" toggle must be enabled for the Post-History Instructions to be applied (even if the System Prompt itself is empty).

+++ Chat Completion APIs

Post-History Instructions is one of the default prompts in [Prompt Manager](prompt-manager.md). It is usually the last message in the context that the model receives, attributed to ("sent by") the system role. If your Chat Completion API does not support the system role, it will usually be attributed to the user role instead.

+++

## Adding to the Prompt (World Info)

You can insert additional information anywhere in the prompt using the [World Info](/Usage/worldinfo.md) feature. By setting the conditions for when the information should be inserted, you can guide the AI to include specific details, change how it responds, or add new elements to the conversation.

Some common uses of World Info include:

* a "lorebook" or "encyclopedia" with information about the world or setting
* a way to manage different system prompts for various characters and situations
* a place to store memories that the AI should "recall" in the conversation
* a more modular system for creating, editing, and sharing character details
* a source of random events and surprises for the AI to react to, or to make you react to!
