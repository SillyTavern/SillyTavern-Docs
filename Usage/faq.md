---
order: 180
icon: question
route: /usage/faq/
---

# FAQ

## Explain what SillyTavern is about

Modern AI language models such as ChatGPT have gotten so powerful that some of them are now convincingly able to simulate a character you create, and who you can chat with, write fiction with, etc. For example, you can tell the AI to pretend to be a Go instructor named Jubei from medieval Japan, and it will act and respond accordingly. You can have a long chat with Jubei, go to the pub together, decide to get in a fight with samurais, whatever you can imagine, and the AI will play along and write/react around this content, acting as your foil and dungeon master. Your imagination is the limit. You can tell the AI to pretend it's Wonder Woman. You can also specify a scenario ("Wonder Woman and I are robbing a bank"), a writing style ("Wonder Woman speaks in ebonics"), or anything else you can think of.

SillyTavern is an app to facilitate these uses:

* It's a user interface that handles communication with AI language models.
* It lets you create new character cards (prompts), and switch between them easily.
* It lets you import characters created by other people.
* It will keep your chat history with a character, allowing you to resume at any time, start a new chat, review old chats, etc.
* In the background, it does the necessary things to prepare the AI prompt for you. Specifically, it will send a system prompt (instructions for the AI) that primes the AI to follow certain rules to improve response accuracy.

## Give me an overview of my AI model options

SillyTavern can interact with two types of AI:

1. [Web services](/Usage/API_Connections/openai.md) (Cloud-based, usually paid, proprietary, closed)
2. [Self-hosted](/Usage/API_Connections/self-hosted.md) (local, free, open-source)

### Paid web service AIs

Paid web models are black boxes. You pay a company to use their AI service. You put your account info in SillyTavern and it will connect to your provider to use the AI on your behalf.

Pros:

* Really easy to get started.
* Highest quality AI writing.

Cons:

* They cost money to use.
* Everything is logged on their server. Privacy concerns.
* They are often censored and will refuse to chat with you about certain subjects.

### Self-hosted AIs

Self-hosted models are free models you can run on your PC but require a powerful PC and more work to set up.

Pros:

* Once you set them up, they can be used for free even without Internet access.
* Total privacy. Everything you write stays on your own PC.
* There's a wide variety of models. As a community-driven technology, you can find models that fit certain tasks or behaviors that you want.

Cons:

* They are not as capable as <abbr title="State of the art">SOTA</abbr> models (i.e., they write worse dialog, are less creative, etc).
* Running local models requires a GPU with at least 6GB VRAM.

If you are interested in using these, refer to the dedicated guide here: [How To Use A Self-Hosted Model](/Usage/API_Connections/self-hosted.md).

## Can I use SillyTavern on my phone or tablet?

iPhones and iPads are not capable of running the whole SillyTavern app, but since it's just a web interface, you can run it on another computer on your home Wi-Fi, and then access it in your mobile browser. Refer to [Remote Connections](/Administration/remote-connections.md) for more information.

For Android users, in addition to the above, you can run the whole SillyTavern directly on your phone, without needing a PC, using the Termux app. Refer to [Installation (Android)](/Installation/Android.md). (NOTE: Termux installations are not officially supported, and we can't guarantee it will work.)

## I tried to import a PNG character card but got an error that it's invalid. Why?

Two possibilities:

1. The card did not have the definitions embedded inside it and was just a normal image file. Some programs or file managers will strip the embedded definitions from the card when you save them. Make sure you're using the raw PNG file as it was posted by the person who shared it.
2. The PNG file was actually a WEBP file with a `.png` filename. You can try renaming the card to `.webp` before importing, or look for a proper PNG version of the image.

## How can I make my own AI character?

1. Click the Character Management button
2. Click Create New Character
3. Under Character Name, give a name, like Amanda
4. Optionally, click the Select Avatar button to pick an image portrait for this character
5. Under Description, describe the character, and include any information you want that you feel is relevant to the chat. For example: ```Amanda is a student traveling during her gap year. She's 6 feet tall, and a volleyball player. She has an athletic figure. She has long brown hair. She loves the Victorian England period, and watching TV and reading novels relating to that period.```
For example, if you want Amanda to be friendly, then you would add: ```Amanda is extremely cheerful and outgoing.```
6. Under First Message, write the greeting for the character when you begin a new chat. For example: ```*Amanda waves at you* Hey! Are you a backpacker too?```
7. Click the Create Character button

You now have a basic character you can chat with. Select Amanda from the character list, and a new chat will begin.

Note that you can use the Description and/or First Message to create a more specific scenario, and/or include yourself in the description. For example:

```txt
Description:
Amanda is a student traveling during her gap year. She's 6 feet tall, and a volleyball player. She has an athletic figure. She has long brown hair. She loves the Victorian England period, and watching TV and reading novels relating to that period. She's been keeping a secret that weighs heavily on her soul. She's waiting for the right person to unburden herself to, but this may lead to a cat and mouse game against a powerful secret society. She's recently arrived in Calcutta.

You're Rajesh Nahasmapetilon, a world-famous Indian volleyball superstar. You're out for a walk in Calcutta. Amanda spots you and screams in excitement.

First Message:
*Amanda runs up to you, beaming.* Rajesh! I can't believe it! I'm such a big fan. I have your poster in my bedroom.
```

Any relevant information you include can be used. How well it's used depends on the power level of the AI model.

NOTE: you can go back and edit any of this information once the character is created, except the name.

## Where are my API keys stored? Why can't I see them?

SillyTavern saves your API keys to a `secrets.json` file in the user data directory (`/data/default-user/secrets.json` is the default path).

By default, API keys will not be visible from the interface after you have saved them and refreshed the page.

In order to enable viewing your keys:

1. Set the value of `allowKeysExposure` to `true` in `config.yaml` file.
2. Restart the SillyTavern server.
3. Click the 'View hidden API keys' link at the bottom right of the API Connection Panel.

## Performance Tips

### Why is the UI so slow/jittery?

* Try enabling the No Blur Effect (Fast UI) mode on the User settings panel.
* Enable Reduced motion in the UI theme settings to remove cosmetic animations.
* Make sure your browser is using Hardware Acceleration.
* If using response streaming, set the streaming FPS to a lower value (10-15 FPS is recommended).

### I'm experiencing an input lag. What can I do?

Performance degradation, particularly input lag, is most commonly attributed to browser extensions. Known problematic extensions include:

* iCloud Password Manager
* DeepL Translation
* AI-based grammar correction tools
* Various ad-blocking extensions

If you experience performance issues and cannot identify the cause, or suspect an issue with SillyTavern itself, please:

1. [Record a performance profile](https://developer.chrome.com/docs/devtools/performance/reference)
2. Export the profile as a JSON file
3. Submit it to the development team for analysis

We recommend first testing with all browser extensions and third-party SillyTavern extensions disabled to isolate the source of the performance degradation.

### When I import a lot of characters, the app becomes slow. Why?

Unfortunately, SillyTavern wasn't designed to handle huge character libraries. The more you have, the longer it will take to load the character list. Evidential data suggests that the performance degradation starts to become noticeable when you have more than 1000 characters.

However, there are some things you can do to mitigate the issue:

**1. Use lazy loading.**

Enable lazy loading of characters setting the value `performance.lazyLoadCharacters` to true in the `config.yaml` file. After the next server restart, the character list will only load the full data of characters you interact with. Please be aware that some third-party extensions may not work correctly with this setting enabled if they were not updated to support it (contact the extension developer for more information).

**2. Use memory cache.**

Increase the memory cache capacity if you have some spare RAM. This will allow the server to keep more characters in memory, reducing the time it takes to load them. You can do this by adjusting the value of `performance.memoryCacheCapacity` to a higher number in the `config.yaml` file. The default value is `100mb`. Approximate rule of thumb: increase the value by 100mb for every 3000 characters you have.

**Limitations:**

1. Advanced (fuzzy) characters search will not work with lazy loading enabled. Only character names will be searched.
2. Memory cache is disabled on Android devices due to the limited amount of available memory.

## How to make the AI write more?

Sometimes the AI will only respond with a single sentence when you'd like it to be more verbose.
This is usually a problem with locally run models.

If you simply want the bot to continue writing from where it left off at the end of its most recent reply, you can send an empty user message by typing nothing into the Input Bar and clicking Send. This will force the bot to continue the story.

Strategies for fixing this:

* Increase the value of the `Response Length` setting
* Design a good `First Message` for the Character, which shows them speaking in a long-winded manner. AI models can improve a lot when given guidance about the writing style you expect.
* Add a phrase in the character's Description Box such as "likes to talk a lot" or "very verbose speaker"
* Do the same thing for your `Author's Note`, or `Post-History Instruction Prompt`
* As a last resort, you can try turning on `Auto-Continue` (in the User Settings panel), but will make responses come out slower because it's making the AI produce small replies back to back, and then combining them all together into one big reply. It may also be incompatible with some API options.

## How to make the AI write less?

This is mostly only a problem for models like ChatGPT or Claude. The same strategies can be applied but in reverse.

* Decrease the value of the `Response Length` setting
* Give the character a phrase like 'short spoken', or 'doesn't talk much' line in their Description.
* Give the character a brief First Message to set the tone and expectation for the chat.
* Make sure `Auto-Continue` is turned off.

## How to make the AI stop writing the actions of my character, and driving the plot all on its own?

This should be handled in the `Author's Note` with a combination of phrases like:

* \{\{char\}\}'s responses shall only be passive and reactive to \{\{user\}\}'s actions.
* Your next response shall be solely from the POV of \{\{char\}\}.
* You are never allowed to dictate actions or speech for \{\{user\}\}.
