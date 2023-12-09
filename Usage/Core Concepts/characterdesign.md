# Character Design

### Character Description

Used to add the character description and the rest that the AI should know. This will always be present in the prompt, so all the important facts should be included here.

For example, you can add information about the world in which the action takes place and describe the characteristics of the character you are playing for.

It could be of any length (be it 200 or 2000 tokens) and formatted in any style (free text, W++, conversation style, etc).

### Methods and format

Methods of character formatting is a complicated topic beyond the scope of this documentation page.

Recommended guides that were tested with or rely on SillyTavern's features:

* Trappu's PLists + Ali:Chat guide: https://wikia.schneedc.com/bot-creation/trappu/creation
* AliCat's Ali:Chat guide: https://rentry.co/alichat
* kingbri's minimalistic guide: https://rentry.co/kingbri-chara-guide
* Kuma's W++ guide: https://rentry.co/WPP_For_Dummies

### Character tokens

**TL;DR: If you're working with an AI model with a 2048 context token limit, your 1000 token character definition is cutting the AI's 'memory' in half.**

To put this in perspective, a decent response from a good AI can easily be around 200-300 tokens. In this case, the AI would only be able to 'remember' about 3 exchanges worth of chat history.

***

### Why did my character's token counter turn red?

When we see your character has over half of the model-defined context length of tokens in its definitions, we highlight it for you because this can lower the AI's capabilities to provide an enjoyable conversation.

### What happens if my Character has too many tokens?

Don't worry - it won't break anything. At worst, if the Character's permanent tokens are too large, it simply means there will be less room left in the context for other things (see below).

The only negative side effect this can have is the AI will have less 'memory', as it will have less chat history available to process.

This is because every AI model has a limit to the amount of context it can process at one time.

### 'Context'?

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

* Older models below 6B parameters - 1024
* Pygmalion 6B, LLaMA 1 models (stock) - 2048
* LLaMA 2 and its finetunes - 4096
* OpenAI ChatGPT (3.5 Turbo) - 4096 or 16k
* OpenAI GPT-4 - 8192 or 32k
* Anthropic's Claude - 8000 (older versions) or 100k (Claude 2)
* NovelAI - 8192 (Kayra, Opus tier; Clio, all tiers), 6144 (Kayra, Scroll tier), or 3072 (Kayra, Tablet tier)

### Personality summary

A brief description of the personality.

Examples:

* `Cheerful, cunning, provocative`
* `Aqua likes to do nothing and also likes to get drunk`

### First message

The First Message is an important thing that sets exactly how and in what style the character will communicate.

The character's first message should be long so that later it would be less likely that the character would respond with very short messages.

You can also use asterisks ** to describe the character's actions.

For example:

`*I noticed you came inside, I walked up and stood right in front of you* Welcome. I'm glad to see you here. *I said with a toothy smug sunny smile looking you straight in the eye* What brings you...`

### Examples of dialogue

Describes how the character speaks. Before each example, you need to add the \<START\> tag. The blocks of examples dialogue are only inserted if there's a free space in the context for them and pushed out of context block by block. \<START\> will not be present in the prompt as it is just a marker - it will be instead replaced with "Example Separator" from Advanced Formatting for Text Completion APIs and contents of the "New Example Chat" utility prompt for Chat Completion APIs.

* Use \{\{char\}\} instead of the character name.
* Use \{\{user\}\} instead of the user name.

Example:

\<START\>

\{\{user\}\}: Hi Aqua, I heard you like to spend time in the pub.

\{\{char\}\}: \*excitedly\* Oh my goodness, yes! I just love spending time at the pub! It's so much fun to talk to all the adventurers and hear about their exciting adventures! And you are?

\{\{user\}\}: I'm new here and I wanted to ask for your advice.

\{\{char\}\}: \*giggles\* Oh, advice! I love giving advice! And in gratitude for that, treat me to a drink! *gives signals to the bartender*

\<START\>

\{\{user\}\}: Hello

\{\{char\}\}: \*excitedly\* Hello there, dear! Are you new to Axel? Don't worry, I, Aqua the goddess of water, am here to help you! Do you need any assistance? And may I say, I look simply radiant today! \*strikes a pose and looks at you with puppy eyes\*

### Scenario

Circumstances and context of the dialogue.

### Replacement tags (macros)

**This list may be incomplete. Use the `/help macros` slash command in SillyTavern chat to get the list of macros that work in your instance.**

A list of tags that are replaced when sending to generate:

1. \{\{user\}\} and \<USER\> => User's Name.
2. \{\{charPrompt\}\} => Character's Main Prompt override
3. \{\{charJailbreak\}\} => Character's Jailbreak Prompt override
4. \{\{char\}\} and \<BOT\> => Character's Name.
5. \{\{description\}\} => Character's Description.
6. \{\{scenario\}\} => Character's Scenario or chat scenario override (if set).
7. \{\{personality\}\} => Character's Personality.
8. \{\{persona\}\} => User's Persona description.
9. \{\{mesExamples\}\} => Character's Examples of Dialogue (unaltered and unsplit).
10. \{\{lastMessageId\}\} => last chat message ID.
11. \{\{lastMessage\}\} => last chat message text.
12. \{\{currentSwipeId\}\} => 1-based ID of the currently displayed last message swipe.
13. \{\{lastSwipeId\}\} => number of swipes in the last chat message.
11. \{\{original\}\} can be used in Prompt Overrides fields (Main Prompt and Jailbreak) to include the respective default prompt from the system settings. Applied to Chat Completion APIs and Instruct mode only.
12. \{\{time\}\} => current system time.
13. \{\{time_UTC±X\}\} => current time in the specified UTC offset (timezone), e.g. for UTC+02:00 use \{\{time_UTC\+2\}\}.
14. \{\{date\}\} => current system date.
15. \{\{input\}\} => contents of the user input bar.
16. \{\{weekday\}\} => the current weekday
17. \{\{isotime\}\} => the current ISO date (YYYY-MM-DD)
18. \{\{isodate\}\} => the current ISO time (24-hour clock)
19. \{\{idle_duration\}\} inserts a humanized string of the time range since the last user message was sent (examples: 4 hours, 1 day).
20. \{\{random:(args)\}\} returns a random item from the list. (e.g. \{\{random:1,2,3,4\}\} will return 1 of the 4 numbers at random). Works with text lists too.
21. \{\{roll:(formula)\}\} generates a random value and returns it using the provided dice formula using D&D dice syntax: XdY+Z. For example, \{\{roll:d6\}\} will generate a random value in the 1-6 range (standard six-sided dice).
22. \{\{bias "text here"\}\} sets a behavioral bias for the AI until the next user input. Quotes around the text are important.
23. \{\{// (note)\}\} allows to leave a note that will be replaced with blank content. Not visible for the AI.

#### Instruct Mode and Context Template Macros:

(enabled in the Advanced Formatting settings)

1. \{\{exampleSeparator\}\} – context template example dialogues separator
2. \{\{chatStart\}\} – context template chat start line
3. \{\{instructSystem\}\} – instruct system prompt
4. \{\{instructSystemPrefix\}\} – instruct system prompt prefix sequence
5. \{\{instructSystemSuffix\}\} – instruct system prompt suffix sequence
6. \{\{instructInput\}\} – instruct user input sequence
7. \{\{instructOutput\}\} – instruct assistant output sequence
8. \{\{instructFirstOutput\}\} – instruct assistant first output sequence
9. \{\{instructLastOutput\}\} – instruct assistant last output sequence
10. \{\{instructSeparator\}\} – instruct turn separator sequence
11. \{\{instructStop\}\} – instruct stop sequence
12. \{\{maxPrompt\}\} - max size of the prompt in tokens (context length reduced by response length)

#### Chat variables Macros:

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

### Favorite Character

Mark the character as a favorite to quickly filter on the side menu bar by pressing the "star" button.
