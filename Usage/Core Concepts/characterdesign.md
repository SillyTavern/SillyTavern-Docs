# Character Design

### Character Description

Used to add the character description and the rest that the AI should know.

For example, you can add information about the world in which the action takes place and describe the characteristics for the character you are playing for.

Usually it all takes 200-350 tokens.

### Methods and format

For most Kobold's models the easiest way is to use a free form for description, and in each sentence it is desirable to specify the name of the character.

The entire description should be in one line without hyphenation.

For example:

`Chloe is a female elf. Chloe wears black-white maid dress with green collar and red glasses. Chloe has medium length black hair. Chloe's personality is...`

But that the AI would be less confused the best way is to use the W++ format.

Details here: [Pro-Tips](https://github.com/KoboldAI/KoboldAI-Client/wiki/Pro-Tips)

### Character tokens

**TL;DR: If you're working with an AI model with a 2048 context token limit, your 1000 token character definition is cutting the AI's 'memory' in half.**

To put this in perspective, a decent response from a good AI can easily be around 200-300 tokens. In this case, the AI would only be able to 'remember' about 3 exchanges worth of chat history.

***

### Why did my character's token counter turn red?

When we see your character has over 1000 tokens in its definitions, we highlight it for you because this can lower the AI's capabilities to provide an enjoyable conversation.

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
* Pygmalion 6B - 2048
* Poe.com (Claude-instant or ChatGPT) - 2048
* OpenAI ChatGPT - 4000-ish?
* OpenAI GPT-4 - 8000?

### Personality summary

A brief description of the personality.

Examples:

* `Cheerful, cunning, provocative`
* `Aqua likes to do nothing and also likes to get drunk`

### First message

The First Message is an important thing that sets exactly how and in what style the character will communicate.

It is desirable that the character's first message be long, so that later it would be less likely that the character would respond in with very short messages.

You can also use asterisks ** to describe the character's actions.

For example:

`*I noticed you came inside, I walked up and stood right in front of you* Welcome. I'm glad to see you here. *I said with toothy smug sunny smile looking you straight in the eye* What brings you...`

### Examples of dialogue

Describes how the character speaks. Before each example, you need to add the \<START\> tag.

* Use \{\{char\}\} instead of the character name.
* Use \{\{user\}\} instead of the user name.

Example:

\<START\>

\{\{user\}\}: Hi Aqua, I heard you like to spend time in the pub.

\{\{char\}\}: \*excitedly\* Oh my goodness, yes! I just love spending time at the pub! It's so much fun to talk to all the adventurers and hear about their exciting adventures! And you are?

\{\{user\}\}: I'm a new here and I wanted to ask for your advice.

\{\{char\}\}: \*giggles\* Oh, advice! I love giving advice! And in gratitude for that, treat me to a drink! *gives signals to the bartender*

\<START\>

\{\{user\}\}: Hello

\{\{char\}\}: \*excitedly\* Hello there, dear! Are you new to Axel? Don't worry, I, Aqua the goddess of water, am here to help you! Do you need any assistance? And may I say, I look simply radiant today! \*strikes a pose and looks at you with puppy eyes\*

### Scenario

Circumstances and context of the dialogue.

### Replacement tags

*A list of tags that are replaced when sending to generate:*

1. \{\{user\}\} and \<USER\> are replaced by the User's Name
2. \{\{char\}\} and \<BOT\> are replaced by the Character's Name
3. \{\{time\}\} is replaced with the current system time.
4. \{\{date\}\} is replaced with the current system date.

### Favorite Character

Mark character as favorite to quickly filter on the side menu bar by pressing the star button.
