---
order: prompts-20
---

# Context Template

!!! Applies to: Text Completion APIs
For equivalent settings in Chat Completion APIs, use [Prompt Manager](prompt-manager.md).
!!!

Usually, AI models require you to provide the character data to them in some specific way. SillyTavern includes a list of pre-made conversion rules for different models, but you may customize them however you like.

Edit these settings in the "[Advanced Formatting](advancedformatting.md)" panel.

## Story String

This field is a template for pre-chat character data (known internally as a story string).
This is the main way to format your character card for text completion and instruct models.

The template supports Handlebars syntax and any custom text injections or formatting. See the language reference here: <https://handlebarsjs.com/guide/>

We provide the following parameters to the Handlebars evaluator (wrap them into double-curly braces):

1. `anchorBefore` - prompts set to use the "Before Story String" position
2. `anchorAfter` - prompts set to use the "After Story String" position
3. `description` - character's Description
4. `scenario` - character's Scenario
5. `personality` - character's Personality
6. `system` - [system prompt](advancedformatting.md#system-prompt) OR character's main prompt override (if exists and "Prefer Char. Prompt" is enabled in User Settings)
7. `persona` - selected persona description
8. `char` - character's name
9. `user` - selected persona name
10. `wiBefore` or `loreBefore` - combined activated World Info entries with Position set to "Before Char Defs"
11. `wiAfter` or `loreAfter` - combined activated World Info entries with Position set to "After Char Defs"
12. `mesExamples` - (optional) character's Example Dialogues, instruct-formatted with separator.

!!!tip **Important**  
When using `mesExamples` in the Story String, set **"Example Messages Behavior"** in the **<i class="fa-solid fa-user-cog"></i> User Settings** panel to **"Never include examples"** to avoid duplication of example messages in the prompt.
!!!

A special \{\{trim\}\} macro is supported to remove any newlines that surround it. Use it in case you want some part of text NOT be separated with a newline from the previous line (_spaces **are not** trimmed_).

**WARNING**: If some of the above parameters are missing from the story string template, they are not going to be sent in the prompt at all.

### Prompt Anchors

The `anchorBefore` and `anchorAfter` are generic placeholders for prompts added by various extensions and miscellaneous features in a chosen static position, for example:

* [Author's Note](/Usage/Characters/Author's-Note.md)
* [Summaries](/extensions/Summarize.md)
* [Chat Vectorization](/extensions/Chat-vectorization.md) / [Data Bank](/Usage/Characters/data-bank.md)
* [STscript injections](/For_Contributors/st-script.md#prompt-injections)
* [Web Search](/extensions/WebSearch.md)

### Story String position

By default, the rendered story string (with all placeholders replaced) is placed at the very beginning of the prompt, followed by example messages and the visible chat history.

Alternatively, you can move it to a dynamic position by choosing the "In-chat @ Depth" option, which places the story string at a specific depth in the chat context.

!!!warning **Attention**
If the template contains static prompt elements (model-specific prefixes or suffixes) for wrapping the story string, using the "In-Chat @ Depth" position will cause it to be incorrectly double-wrapped with duplicate sequences, which may lead to unexpected results.

In this case, you can fix the issue in one of the following ways:

1. **Built-in templates**: Reset the templates to their defaults using the steps described in [Advanced Formatting](/Usage/Prompts/advancedformatting.md#resetting-templates).
2. **Custom templates**: Move the static elements from the story string template to [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
!!!

### Story String wrapping

!!!
The following section only applies when **Instruct Mode** is ON.
!!!

* **Default** position: The rendered Story String will be wrapped using the sequences defined in [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
* **In-chat @ Depth** position: The rendered Story String will be wrapped using the sequences defined in [Chat Messages Sequences](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping) for a chosen role (default: System).

## Example Separator

Used as a block header and a separator between the example dialogue blocks. Any instance of `<START>` tags in the example dialogues will be replaced with the contents of this field.

## Chat Start

Inserted as a separator after the rendered story string and after the example dialogues blocks, but before the first message in context.

## Separators as Stop Strings

Adds "Example Separator" and "Chat Start" to the list of stop strings.

Helpful if the model tends to hallucinate or leak whole blocks of example dialogue preceded by the separator.

## Names as Stop Strings

Adds Character and User Persona names to the list of stop strings.

Recommended to keep it on to prevent model impersonation.

## Always add character's name to prompt

!!!info  
This setting has no effect when Instruct Mode is ON. The name behavior is instead defined by the selected [Include Names](/Usage/Prompts/instructmode.md#include-names) option.
!!!

Appends the character's name to the prompt to force the model to complete the message as the character:

```txt
** OTHER CONTEXT HERE **
Character:
```
