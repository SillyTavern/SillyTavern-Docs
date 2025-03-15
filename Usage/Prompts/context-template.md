---
order: prompts-20
---

# Context Template

!!! Applies to: Text Completion APIs
For equivalent settings in Chat Completion APIs, use [Prompt Manager](prompt-manager.md).
!!!

Usually, AI models require you to provide the character data to them in some specific way. SillyTavern includes a list of pre-made conversion rules for different models, but you may customize them however you like.

Edit these settings in the "[Advanced Formatting](advancedformatting.md)" panel.

## Story string

This field is a template for pre-chat character data (known internally as a story string).
This is the main way to format your character card for text completion and instruct models.

The template supports Handlebars syntax and any custom text injections or formatting. See the language reference here: <https://handlebarsjs.com/guide/>

We provide the following parameters to the Handlebars evaluator (wrap them into double-curly braces):

1. `description` - character's Description
2. `scenario` - character's Scenario
3. `personality` - character's Personality
4. `system` - [system prompt](advancedformatting.md#system-prompt) OR character's main prompt override (if exists and "Prefer Char. Prompt" is enabled in User Settings)
5. `persona` - selected persona description
6. `char` - character's name
7. `user` - selected persona name
8. `wiBefore` or `loreBefore` - combined activated World Info entries with Position set to "Before Char Defs"
9. `wiAfter` or `loreAfter` - combined activated World Info entries with Position set to "After Char Defs"
10. `mesExamples` - (optional) character's Example Dialogues, instruct-formatted with separator. **Important:** Set "Example Messages Behavior" in the User Settings panel to "Never include examples" to avoid duplication.

A special \{\{trim\}\} macro is supported to remove any newlines that surround it. Use it in case you want some part of text NOT be separated with a newline from the previous line (_spaces **are not** trimmed_).

**WARNING**: If some of the above parameters are missing from the story string template, they are not going to be sent in the prompt at all.

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

## Allow Post-History Instructions

Includes the Post-History Instructions at the end of the prompt, formatted as the last user message.

The Post-History Instructions prompt should be defined in the character card and "Prefer Char. Instructions" setting should be enabled.

Should be used with care, as placing instructions low in the context can lead to degraded quality of the outputs of smaller models.

## Always add character's name to prompt

Appends the character's name to the prompt to force the model to complete the message as the character:

```txt
** OTHER CONTEXT HERE **
Character:
```
