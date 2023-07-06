# Character Design

### Character Description

Used to add the character description and the rest that the AI should know.

For example, you can add information about the world in which the action takes place and describe the characteristics of the character you are playing for.

It could be any of any length (be it 200 or 2000 tokens) and formatted in any style (free text, W++, conversation style, etc) and could span any length.

### Methods and format

Methods of character formatting is a complicated topic beyond the scope of this documentation page.

Recommended guides that were tested with or rely on SillyTavern's features:

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
* Pygmalion 6B, LLaMA models (stock) - 2048
* Poe.com (Claude-instant or ChatGPT) - 2048 by default, but could be unlocked with the use of chunked prompting (the actual limit varies per bot)
* OpenAI ChatGPT (3.5 Turbo) - 4096 or 16k
* OpenAI GPT-4 - 8192 or 32k
* Anthropic's Claude - 7500 or 100k

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

Describes how the character speaks. Before each example, you need to add the \<START\> tag.

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

*A list of tags that are replaced when sending to generate:*

1. \{\{user\}\} and \<USER\> are replaced by the User's Name
2. \{\{char\}\} and \<BOT\> are replaced by the Character's Name
3. \{\{time\}\} is replaced with the current system time.
4. \{\{date\}\} is replaced with the current system date.
5. \{\{idle_duration\}\} inserts a humanized string of the time range since the last user message was sent (examples: 4 hours, 1 day).
6. \{\{random:(args)\}\} returns a random item from the list. (e.g. \{\{random:1,2,3,4\}\} will return 1 of the 4 numbers at random. Works with text lists too.

### Favorite Character

Mark the character as a favorite to quickly filter on the side menu bar by pressing the "star" button.
