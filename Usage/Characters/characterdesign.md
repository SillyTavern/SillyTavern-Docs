---
order: 100
route: /usage/core-concepts/characterdesign/
templating: false
---

# Character Design

!!!tip
Character Name is the only required field. You can leave the rest empty and still use the character in chats.
!!!

## Character Description

Used to add the character description and other relevant information for the AI. This information is always included in the prompt, so all important facts should be included here.

For example, you can add information about the world in which the action takes place, describe the character's appearance, personality, and background.

It could be of any length (be it 200 or 2000 tokens) and formatted in any style (free text, pseudo-code conversation style, etc.).

### Methods and format

Methods of character formatting are a complicated topic beyond the scope of this documentation page.

Recommended guides that were tested with or rely on SillyTavern's features:

* Trappu's PLists + Ali:Chat guide: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* AliCat's Ali:Chat guide: <https://rentry.co/alichat>
* kingbri's minimalistic guide: <https://rentry.co/kingbri-chara-guide>

## Character tokens

**TL;DR: If you're working with an AI model with a 2048 context token limit, a 1000-token character definition cuts the AI's 'memory' in half.**

To put this in perspective, a decent response from a good AI can easily be around 200-300 tokens. In this case, the AI would only be able to 'remember' about 3 exchanges worth of chat history.

### Why did my character's token counter turn red?

When we see your character has over half of the model-defined context length of tokens in its definitions, we highlight it for you because this can lower the AI's capabilities to provide an enjoyable conversation.

### What happens if my Character has too many tokens?

Don't worry - it won't break anything. At worst, if the Character's permanent tokens are too large, it simply means there will be less room left in the context for other things (see below).

The only negative side effect this can have is that the AI will have less 'memory', as it will have less chat history available to process.

This is because every AI model has a limit to the amount of context it can process at one time.

## 'Context'?

This is the information that gets sent to the AI each time you ask it to generate a response. SillyTavern automatically calculates the best way to allocate the available context tokens before sending the information to the AI model.

Read more about how the context is built in the [Prompts](/Usage/Prompts/index.md) section.

### What are a Character's 'Permanent Tokens'?

These will always be sent to the AI with every generation request:

* Character Name
* Character Description Box
* Character Personality Box
* Scenario Box

### What parts of a Character's Definitions are NOT permanent?

* The first message box - only sent once at the start of the chat.
* Example messages box - only kept until chat history fills up the context (optionally these can be forced to be kept in context)

### Popular AI Model Context Token Limits

* LLaMA 3 and its finetunes - 8192
* OpenAI GPT-4 - up to 128k
* Google Gemini - up to 2M
* Anthropic's Claude - 200k (Claude 3)
* NovelAI - 8192 (Erato and Kayra, Opus tier; Clio, all tiers), 6144 (Kayra, Scroll tier), or 3072 (Kayra, Tablet tier)

## First message

The First Message is an important element that defines how and in what style the character will communicate. The model is more likely to pick up the style and length constraints from the first message than anything else, so it's important to write it in a way that you want the responses to be (short and concise, long and detailed, etc.).

Supports Markdown and HTML formatting.

For example:

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well-being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## Alternate Greetings

Messages added here are displayed as additional 'swipes' for the character's first message when starting a new chat. If the character is part of a group chat, the system randomly selects one of these greetings to initiate the conversation.

## Favorite Character

Click the **<i class="fa-solid fa-star"></i> Add to Favorites** button to mark the character as a favorite to quickly filter them on the side menu bar by selecting the "Favorites" sort option. Favorite characters have a golden highlight in the list. This will also make the character portrait appear in the hotswaps area (if enabled in User Settings).

## Advanced Definitions

!!!info
The following fields are hidden by default. To access and edit them, you need to click on the **<i class="fa-solid fa-book"></i> Advanced Definitions** button on the menu bar of the character definition page.
!!!

### Prompt Overrides

* **Main Prompt**: If the "Prefer Char. Prompt" user setting is enabled, any text you put here will override the [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt) for the character.
* **Post-History Instructions**: If the "Prefer Char. Instructions" user setting is enabled, any text you put here will be used as the [post-history instructions](/Usage/Prompts/index.md#post-history-instructions) for the character.

!!!tip
Insert `{{original}}` into either box to include the respective default prompt from system settings in a designated place.
!!!

### Creator's Metadata

!!!info
Not used for prompt building, but provides additional metadata about the character.
!!!

* **Created by**: The name of the character's creator. Can be displayed in the character list if the "Char List Subheader" user setting is set accordingly.
* **Character Version**: The version of the character. Can be displayed in the character list if the "Char List Subheader" user setting is set accordingly.
* **Creator's Notes**: Any additional notes about the character that the creator wants to share. The first few lines are displayed in the character list, and the full text is displayed in the "Creator's Notes" section on the character's page. Supports Markdown/HTML formatting.
* **Tags to Embed**: A comma-separated list of tags that will be embedded in the character's description. These tags are not imported by default when importing the character, but you can merge them with your existing tags by selecting "Import Tags" from the "More..." menu on the character's page.

### Personality summary

A brief summary of the character's personality.

### Scenario

The circumstances and context of the dialogue.

### Character's Note

A text to be used as an in-chat prompt injection for the character at a specific message depth. It is usually used to reinforce certain character traits, as it always stays at a static depth in the chat history, regardless of its progression.

* **@ Depth**: The number of messages in the chat history after which this note will be injected (in order from newest to oldest). If set to 0, it will be injected after the last message.
* **Role**: The role of the message. Can be "User", "System", or "Assistant".

### Talkativeness

Determines the probability of the character's response being triggered in group chats when using a [Natural](/Usage/Characters/groupchats.md#natural-order) activation order. Ranges from 0% to 100%, with 50% being the default value.

### Examples of dialogue

Describes how the character speaks. Before each example, you need to add the `<START>` tag. The blocks of example dialogue are only inserted if there is free space in the context for them and are pushed out of context block by block. `<START>` will not be present in the prompt as it is just a marker; it will be replaced with the "Example Separator" from Advanced Formatting for Text Completion APIs and the contents of the "New Example Chat" utility prompt for Chat Completion APIs.

* Use the `{{char}}:` prefix to denote a character message.
* Use the `{{user}}:` prefix to denote a user message.

Example:

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
