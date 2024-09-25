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
* Kuma's W++ guide: <https://rentry.co/WPP_For_Dummies>

## Character tokens

**TL;DR: If you're working with an AI model with a 2048 context token limit, your 1000 token character definition is cutting the AI's 'memory' in half.**

To put this in perspective, a decent response from a good AI can easily be around 200-300 tokens. In this case, the AI would only be able to 'remember' about 3 exchanges worth of chat history.

***

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

`*I noticed you came inside, I walked up and stood right in front of you* Welcome. I'm glad to see you here. *I said with a toothy smug sunny smile looking you straight in the eye* What brings you...`

## Examples of dialogue

Describes how the character speaks. Before each example, you need to add the \<START\> tag. The blocks of examples dialogue are only inserted if there's a free space in the context for them and pushed out of context block by block. \<START\> will not be present in the prompt as it is just a marker - it will be instead replaced with "Example Separator" from Advanced Formatting for Text Completion APIs and contents of the "New Example Chat" utility prompt for Chat Completion APIs.

* Use \{\{char\}\} instead of the character name.
* Use \{\{user\}\} instead of the user name.

Example:

---

\<START\>

\{\{user\}\}: Hi Aqua, I heard you like to spend time in the pub.

\{\{char\}\}: \*excitedly\* Oh my goodness, yes! I just love spending time at the pub! It's so much fun to talk to all the adventurers and hear about their exciting adventures! And you are?

\{\{user\}\}: I'm new here and I wanted to ask for your advice.

\{\{char\}\}: \*giggles\* Oh, advice! I love giving advice! And in gratitude for that, treat me to a drink! *gives signals to the bartender*

\<START\>

\{\{user\}\}: Hello

\{\{char\}\}: \*excitedly\* Hello there, dear! Are you new to Axel? Don't worry, I, Aqua the goddess of water, am here to help you! Do you need any assistance? And may I say, I look simply radiant today! \*strikes a pose and looks at you with puppy eyes\*

---

## Scenario

Circumstances and context of the dialogue.

## Favorite Character

Mark the character as a favorite to quickly filter on the side menu bar by selecting the "Favorites" sort option. Favorite characters have a golden highlight in the list. This will also make the character portrait appear in the hotswaps area (if enabled in User Settings).

## Macros (replacement tags)

**This list may be incomplete or outdated. Use the `/help macros` slash command in any SillyTavern chat to get the list of macros that work in your instance.**

Note: some extensions may also add special context-specific macros that only work in certain places. These will not be documented here.

A list of macros that are replaced when sending a prompt to generate:

1. \{\{pipe\}\} => only for slash command batching. Replaced with the returned result of the previous command.
2. \{\{newline\}\} => just inserts a newline.
3. \{\{trim\}\} => trims newlines surrounding this macro.
4. \{\{noop\}\} => no operation, just an empty string.
5. \{\{user\}\} and \<USER\> => User's Name.
6. \{\{charPrompt\}\} => Character's Main Prompt override
7. \{\{charJailbreak\}\} => Character's Post-History Instructions Prompt override
8. \{\{char\}\} and \<BOT\> => Character's Name.
9. \{\{description\}\} => Character's Description.
10. \{\{scenario\}\} => Character's Scenario or chat scenario override (if set).
11. \{\{personality\}\} => Character's Personality.
12. \{\{persona\}\} => User's Persona description.
13. \{\{mesExamples\}\} => Character's Examples of Dialogue (unaltered and unsplit).
14. \{\{char_version\}\} => the Character's version number.
15. \{\{model\}\} => a text generation model name for the currently selected API. **Can be inaccurate!**
16. \{\{lastMessageId\}\} => last chat message ID.
17. \{\{lastMessage\}\} => last chat message text.
18. \{\{firstIncludedMessageId\}\} => the ID of the first message included in the context. Requires generation to be run at least once in the current session.
19. \{\{lastCharMessage\}\} => last chat message sent by character.
20. \{\{lastUserMessage\}\} => last chat message sent by user.
21. \{\{currentSwipeId\}\} => 1-based ID of the currently displayed last message swipe.
22. \{\{lastSwipeId\}\} => number of swipes in the last chat message.
23. \{\{original\}\} can be used in Prompt Overrides fields (Main Prompt and Post-History Instructions) to include the respective default prompt from the system settings. Applied to Chat Completion APIs and Instruct mode only.
24. \{\{time\}\} => current system time.
25. \{\{time_UTC±X\}\} => current time in the specified UTC offset (timezone), e.g. for UTC+02:00 use \{\{time_UTC\+2\}\}.
26. \{\{timeDiff::(time1)::(time2)\}\} => the time difference between time1 and time2. Accepts time and date macros.
27. \{\{date\}\} => current system date.
28. \{\{input\}\} => contents of the user input bar.
29. \{\{weekday\}\} => the current weekday
30. \{\{isotime\}\} => the current ISO time (24-hour clock)
31. \{\{isodate\}\} => the current ISO date (YYYY-MM-DD)
32. \{\{idle_duration\}\} inserts a humanized string of the time range since the last user message was sent (examples: 4 hours, 1 day).
33. \{\{random:(args)\}\} returns a random item from the list. (e.g. \{\{random:1,2,3,4\}\} will return 1 of the 4 numbers at random). Works with text lists too.
34. \{\{random::arg1::arg2\}\} => alternate syntax for random that supports commas in its arguments.
35. \{\{pick::(args)\}\} => alternative to random, but the selected argument is stable on subsequent evaluations in the current chat if the source string remains unchanged.
36. \{\{roll:(formula)\}\} generates a random value and returns it using the provided dice formula using D&D dice syntax: XdY+Z. For example, \{\{roll:d6\}\} will generate a random value in the 1-6 range (standard six-sided dice).
37. \{\{bias "text here"\}\} sets a behavioral bias for the AI until the next user input. Quotes around the text are important.
38. \{\{// (note)\}\} allows to leave a note that will be replaced with blank content. Not visible for the AI.
39. \{\{banned "text here"\}\} dynamically add text in the quotes to banned word sequences, if Text Generation WebUI backend is used. Does nothing for other backends. Can be used anywhere (Character description, WI, AN, etc.) Quotes around the text are important.

### Instruct Mode and Context Template Macros

(enabled in the Advanced Formatting settings)

1. \{\{exampleSeparator\}\} – context template example dialogues separator
2. \{\{chatStart\}\} – context template chat start line
3. \{\{instructSystemPrompt\}\} – instruct system prompt
4. \{\{instructSystemPromptPrefix\}\} – system prompt prefix sequence
5. \{\{instructSystemPromptSuffix\}\} – system prompt suffix sequence
6. \{\{instructUserPrefix\}\} – user message prefix sequence
7. \{\{instructAssistantPrefix\}\} – assistant message prefix sequence
8. \{\{instructSystemPrefix\}\} – system message prefix sequence
9. \{\{instructUserSuffix\}\} – user message suffix sequence
10. \{\{instructAssistantSuffix\}\} – assistant message suffix sequence
11. \{\{instructSystemSuffix\}\} – system message suffix sequence
12. \{\{instructFirstAssistantPrefix\}\} – assistant first output sequence
13. \{\{instructLastAssistantPrefix\}\} – assistant last output sequence
14. \{\{instructSystemInstructionPrefix\}\} – system instruction prefix sequence
15. \{\{instructUserFiller\}\} – user filler message text
16. \{\{instructStop\}\} – instruct stop sequence
17. \{\{maxPrompt\}\} - max size of the prompt in tokens (context length reduced by response length)

### Chat variables Macros

- Local variables = unique to the current chat
- Global variables = works in any chat for any character

1. \{\{getvar::name\}\} – replaced with the value of the local variable "name"
2. \{\{setvar::name::value\}\} – replaced with empty string, sets the local variable "name" to "value"
3. \{\{addvar::name::increment\}\} – replaced with empty strings, adds a numeric value of "increment" to the local variable "name"
4. \{\{incvar::name\}\} – replaced with the result of the increment of value of the variable "name" by 1
5. \{\{decvar::name\}\} – replaced with the result of the decrement of value of the variable "name" by 1
6. \{\{getglobalvar::name\}\} – replaced with the value of the global variable "name"
7. \{\{setglobalvar::name::value\}\} – replaced with empty string, sets the global variable "name" to "value"
8. \{\{addglobalvar::name::value\}\} – replaced with empty string, adds a numeric value of "increment" to the global variable "name"
9. \{\{incglobalvar::name\}\} – replaced with the result of the increment of value of the global variable "name" by 1
10. \{\{decglobalvar::name\}\} – replaced with the result of the decrement of value of the global variable "name" by 1
