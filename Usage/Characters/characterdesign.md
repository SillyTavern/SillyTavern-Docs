---
order: character-10
route: /usage/core-concepts/characterdesign
templating: false
---

# Character Design

## Character Description

Used to add the character description and the rest that the AI should know. This will always be present in the prompt, so all the important facts should be included here.

For example, you can add information about the world in which the action takes place and describe the characteristics of the character you are playing for.

It could be of any length (be it 200 or 2000 tokens) and formatted in any style (free text, W++, conversation style, etc).

### Methods and format

Methods of character formatting is a complicated topic beyond the scope of this documentation page.

Recommended guides that were tested with or rely on SillyTavern's features:

* Trappu's PLists + Ali:Chat guide: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* AliCat's Ali:Chat guide: <https://rentry.co/alichat>
* kingbri's minimalistic guide: <https://rentry.co/kingbri-chara-guide>

## Character tokens

**TL;DR: If you're working with an AI model with a 2048 context token limit, your 1000 token character definition is cutting the AI's 'memory' in half.**

To put this in perspective, a decent response from a good AI can easily be around 200-300 tokens. In this case, the AI would only be able to 'remember' about 3 exchanges worth of chat history.

### Why did my character's token counter turn red?

When we see your character has over half of the model-defined context length of tokens in its definitions, we highlight it for you because this can lower the AI's capabilities to provide an enjoyable conversation.

### What happens if my Character has too many tokens?

Don't worry - it won't break anything. At worst, if the Character's permanent tokens are too large, it simply means there will be less room left in the context for other things (see below).

The only negative side effect this can have is the AI will have less 'memory', as it will have less chat history available to process.

This is because every AI model has a limit to the amount of context it can process at one time.

## 'Context'?

This is the information that gets sent to the AI each time you ask it to generate a response:

* Character definitions
* Chat history
* Author's Notes
* Special Format strings
* [bracket commands]

SillyTavern automatically calculates the best way to allocate the available context tokens before sending the information to the AI model.

### What are a Character's 'Permanent Tokens'?

These will always be sent to the AI with every generation request:

* Character Name (keep the name short! Sent at the start of EVERY Character message)
* Character Description Box
* Character Personality Box
* Scenario Box

### What parts of a Character's Definitions are NOT permanent?

* The first message box - only sent once at the start of the chat.
* Example messages box - only kept until chat history fills up the context (optionally these can be forced to be kept in context)

### Popular AI Model Context Token Limits

* LLaMA 3 and its finetunes - 8192
* OpenAI GPT-4 - up to 128k
* Anthropic's Claude - 200k (Claude 3) or 100k (Claude 2)
* NovelAI - 8192 (Erato and Kayra, Opus tier; Clio, all tiers), 6144 (Kayra, Scroll tier), or 3072 (Kayra, Tablet tier)

## Personality summary

A brief description of the personality.

Examples:

* `Cheerful, cunning, provocative`
* `Aqua likes to do nothing and also likes to get drunk`

## First message

The First Message is an important thing that sets exactly how and in what style the character will communicate.

The character's first message should be long so that later it would be less likely that the character would respond with very short messages.

You can also use asterisks ** to describe the character's actions.

For example:

```txt
*I noticed you came inside, I walked up and stood right in front of you* Welcome. I'm glad to see you here. *I said with a toothy smug sunny smile looking you straight in the eye* What brings you...
```

## Examples of dialogue

Describes how the character speaks. Before each example, you need to add the \<START\> tag. The blocks of examples dialogue are only inserted if there's a free space in the context for them and pushed out of context block by block. \<START\> will not be present in the prompt as it is just a marker - it will be instead replaced with "Example Separator" from Advanced Formatting for Text Completion APIs and contents of the "New Example Chat" utility prompt for Chat Completion APIs.

* Use \{\{char\}\} instead of the character name.
* Use \{\{user\}\} instead of the user name.

Example:

```txt
<START>
{{user}}: Hi Aqua, I heard you like to spend time in the pub.
{{char}}: *excitedly* Oh my goodness, yes! I just love spending time at the pub! It's so much fun to talk to all the adventurers and hear about their exciting adventures! And you are?
{{user}}: I'm new here and I wanted to ask for your advice.
{{char}}: *giggles* Oh, advice! I love giving advice! And in gratitude for that, treat me to a drink! *gives signals to the bartender*
<START>
{{user}}: Hello
{{char}}: *excitedly* Hello there, dear! Are you new to Axel? Don't worry, I, Aqua the goddess of water, am here to help you! Do you need any assistance? And may I say, I look simply radiant today! *strikes a pose and looks at you with puppy eyes*
```

## Scenario

Circumstances and context of the dialogue.

## Favorite Character

Mark the character as a favorite to quickly filter on the side menu bar by selecting the "Favorites" sort option. Favorite characters have a golden highlight in the list. This will also make the character portrait appear in the hotswaps area (if enabled in User Settings).

## Macros (replacement tags)

**This list may be incomplete or outdated. Use the `/help macros` slash command in any SillyTavern chat to get the list of macros that work in your instance.**

Note: some extensions may also add special context-specific macros that only work in certain places. These will not be documented here.

A list of macros that are replaced when sending a prompt to generate:

| Macro | Description |
|-------|-------------|
| `{{pipe}}` | Only for slash command batching. Replaced with the returned result of the previous command. |
| `{{newline}}` | Inserts a newline. |
| `{{trim}}` | Trims newlines surrounding this macro. |
| `{{noop}}` | No operation, just an empty string. |
| `{{user}}` or `<USER>` | User's name. |
| `{{charPrompt}}` | Character's Main Prompt override. |
| `{{charJailbreak}}` | Character's Post-History Instructions Prompt override. |
| `{{group}}` or `{{charIfNotGroup}}` | Comma-separated list of group member names or character name in solo chats. |
| `{{groupNotMuted}}` | Same as `{{group}}` but excludes muted members. |
| `{{char}}` or `<BOT>` | Character's name. |
| `{{description}}` | Character's description. |
| `{{scenario}}` | Character's scenario or chat scenario override (if set). |
| `{{personality}}` | Character's personality. |
| `{{persona}}` | User's persona description. |
| `{{mesExamples}}` | Character's examples of dialogue (unaltered and unsplit). |
| `{{char_version}}` | The character's version number. |
| `{{model}}` | Text generation model name for the currently selected API. **Can be inaccurate!** |
| `{{lastMessageId}}` | Last chat message ID. |
| `{{lastMessage}}` | Last chat message text. |
| `{{firstIncludedMessageId}}` | The ID of the first message included in the context. Requires generation to be run at least once in the current session. |
| `{{lastCharMessage}}` | Last chat message sent by character. |
| `{{lastUserMessage}}` | Last chat message sent by user. |
| `{{currentSwipeId}}` | 1-based ID of the currently displayed last message swipe. |
| `{{lastSwipeId}}` | Number of swipes in the last chat message. |
| `{{lastGenerationType}}` | Type of the last queued generation request. Values: "normal", "impersonate", "regenerate", "quiet", "swipe", "continue". |
| `{{original}}` | Can be used in Prompt Overrides fields to include the default prompt from system settings. Applied to Chat Completion APIs and Instruct mode only. |
| `{{time}}` | Current system time. |
| `{{time_UTC±X}}` | Current time in the specified UTC offset (timezone), e.g. for UTC+02:00 use `{{time_UTC+2}}`. |
| `{{timeDiff::(time1)::(time2)}}` | The time difference between time1 and time2. Accepts time and date macros. |
| `{{date}}` | Current system date. |
| `{{input}}` | Contents of the user input bar. |
| `{{weekday}}` | The current weekday. |
| `{{isotime}}` | The current ISO time (24-hour clock). |
| `{{isodate}}` | The current ISO date (YYYY-MM-DD). |
| `{{datetimeformat ...}}` | Current date/time in specified format (e.g. `{{datetimeformat DD.MM.YYYY HH:mm}}`). |
| `{{idle_duration}}` | Inserts a humanized string of the time range since the last user message was sent (examples: 4 hours, 1 day). |
| `{{random:(args)}}` | Returns a random item from the list (e.g. `{{random:1,2,3,4}}` will return 1 of the 4 numbers at random). |
| `{{random::arg1::arg2}}` | Alternate syntax for random that supports commas in its arguments. |
| `{{pick::(args)}}` | Alternative to random, but the selected argument is stable on subsequent evaluations in the current chat if the source string remains unchanged. |
| `{{roll:(formula)}}` | Generates a random value using D&D dice syntax: XdY+Z (e.g. `{{roll:d6}}` generates a value 1-6). |
| `{{bias "text here"}}` | Sets a behavioral bias for the AI until the next user input. Quotes around text are required. |
| `{{// (note)}}` | Allows leaving a note that will be replaced with blank content. Not visible for the AI. |
| `{{banned "text here"}}` | Dynamically adds quoted text to banned word sequences for Text Generation WebUI backend. Does nothing for other backends. Quotes required. |
| `{{reverse:(content)}}` | Reverses the content of the macro. |

### Instruct Mode and Context Template Macros

(enabled in the Advanced Formatting settings)

| Macro | Description |
|-------|-------------|
| `{{exampleSeparator}}` | Context template example dialogues separator. |
| `{{chatStart}}` | Context template chat start line. |
| `{{instructSystemPrompt}}` | Instruct system prompt. |
| `{{instructSystemPromptPrefix}}` | System prompt prefix sequence. |
| `{{instructSystemPromptSuffix}}` | System prompt suffix sequence. |
| `{{instructUserPrefix}}` | User message prefix sequence. |
| `{{instructAssistantPrefix}}` | Assistant message prefix sequence. |
| `{{instructSystemPrefix}}` | System message prefix sequence. |
| `{{instructUserSuffix}}` | User message suffix sequence. |
| `{{instructAssistantSuffix}}` | Assistant message suffix sequence. |
| `{{instructSystemSuffix}}` | System message suffix sequence. |
| `{{instructFirstAssistantPrefix}}` | Assistant first output sequence. |
| `{{instructLastAssistantPrefix}}` | Assistant last output sequence. |
| `{{instructFirstUserPrefix}}` | Instruct user first input sequence. |
| `{{instructLastUserPrefix}}` | Instruct user last input sequence. |
| `{{instructSystemInstructionPrefix}}` | System instruction prefix sequence. |
| `{{instructUserFiller}}` | User filler message text. |
| `{{instructStop}}` | Instruct stop sequence. |
| `{{maxPrompt}}` | Max size of the prompt in tokens (context length reduced by response length). |
| `{{systemPrompt}}` | System prompt content, including character prompt override if allowed and available. |
| `{{defaultSystemPrompt}}` | System prompt content (excluding character prompt override). |
### Chat variables Macros

- Local variables = unique to the current chat
- Global variables = works in any chat for any character

| Macro | Description |
|-------|-------------|
| `{{getvar::name}}` | Replaced with the value of the local variable "name". |
| `{{setvar::name::value}}` | Replaced with empty string, sets the local variable "name" to "value". |
| `{{addvar::name::increment}}` | Replaced with empty string, adds a numeric value of "increment" to the local variable "name". |
| `{{incvar::name}}` | Replaced with the result of incrementing the value of variable "name" by 1. |
| `{{decvar::name}}` | Replaced with the result of decrementing the value of variable "name" by 1. |
| `{{getglobalvar::name}}` | Replaced with the value of the global variable "name". |
| `{{setglobalvar::name::value}}` | Replaced with empty string, sets the global variable "name" to "value". |
| `{{addglobalvar::name::value}}` | Replaced with empty string, adds a numeric value of "increment" to the global variable "name". |
| `{{incglobalvar::name}}` | Replaced with the result of incrementing the value of global variable "name" by 1. |
| `{{decglobalvar::name}}` | Replaced with the result of decrementing the value of global variable "name" by 1. |
| `{{var::name}}` | Replaced with the value of the scoped variable "name" (STscript only). |
| `{{var::name::index}}` | Replaced with the value at index of the scoped variable "name" (for arrays/objects in STscript). |

### Extension-specific Macros

Added by extensions and only work in specific contexts.

| Macro | Description |
|-------|-------------|
| `{{summary}}` | Replaced with the summary of the current chat session (if available). |
