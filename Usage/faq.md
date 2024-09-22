---
order: 100
icon: question
---
# FAQ

## Explain what SillyTavern is about

Modern AI language models such as ChatGPT have gotten so powerful that some of them are now convincingly able to simulate a character you create, and who you can chat with, write fiction with, etc. For example, you can tell the AI to pretend to be a Go instructor named Jubei from medieval Japan, and it will act and respond accordingly. You can have a long chat with Jubei, go to the pub together, decide to get in a fight with samurais, whatever you can imagine, and the AI will play along and write/react around this content, acting as your foil and dungeon master. Your imagination is the limit. You can tell the AI to pretend it's Wonder Woman. You can also specify a scenario ("Wonder Woman and I are robbing a bank"), a writing style ("Wonder Woman speaks in ebonics"), or anything else you can think of.

SillyTavern is an app to facilitate these uses:

* It's a user interface that handles the communication with AI language models
* It lets you create new characters (a character is a description of someone that you give to an AI for them to roleplay), and switch between your characters easily
* It lets you import characters created by other people. See below.
* It will keep your chat history with a character, allowing you to resume at any time, start a new chat, review old chats, etc
* In the background, it does the necessary things to prepare the AI prompt for you. Specifically, it will send a system prompt (instructions for the AI) that primes the AI to follow certain rules to improve response accuracy.

## Tell me more about AI models and how they might differ

(detailed description of differeing model capabilities and the reasons thereof coming soon)

## Give me an overview of my AI model options

SillyTavern can interact with two types of AI:

1. Web services (Cloud-based, usually paid, proprietary, closed)
2. Self-hosted (local, free, open-source)

### Paid web service AIs

Paid web models are black boxes. You pay a company to use their AI service. You put your account info in SillyTavern and it will connect to your provider to use the AI on your behalf.

Pros:

* Really easy to get started
* Highest quality AI writing

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

* They are not as capable as the best paid options (i.e. write worse dialog, less creative, etc).
* Running local models requires a GPU with at least 6GB VRAM.
* Harder to install

If you are interested in using these, refer to the dedicated guide here: [How To Use A Self-Hosted Model](https://docs.sillytavern.app/usage/local-llm-guide/how-to-use-a-self-hosted-model/)

## I'm clueless. Just spoonfeed me the easiest and fastest way I can start using SillyTavern

These base instructions are only for OpenAI, which is a paid service. Guides for other services may come in the future.

### Install SillyTavern

Follow the [Installation Instructions](https://docs.sillytavern.app/installation/windows/).

### Get access to OpenAI

1. Sign up to OpenAI
1. Go to <https://platform.openai.com>
1. Click your account icon in the top right, then View API Keys
1. Click "Create new secret key". Copy it somewhere immediately. **DO NOT SHARE THIS KEY. WHOEVER HAS IT CAN USE YOUR ACCOUNT TO USE GPT AT YOUR EXPENSE.**

### Configure SillyTavern to use your API

1. In SillyTavern's top bar, click API Connections
1. Under API, select Chat Completion (OpenAI)
1. Under Chat Completion Source, select OpenAI
1. Paste the API key you saved in the previous step
1. Click the Connect button. Confirm it says Valid.
1. By default, SillyTavern will use GPT 3.5 Turbo. If you have access to GPT-4, you really want to be using that for the highest quality, but educate yourself on the pricing.

### Test your setup

1. In SillyTavern's top bar, click Character Management at the far right
1. Select an existing character such as Coding Sensei
1. In the text box at the bottom, write something to Coding Sensei, then press Enter or click the Send button

If you did everything right, after a few seconds, Coding Sensei should respond.

## Can I use SillyTavern on my phone or tablet?

iPhones and iPads are not capable of running the whole SillyTavern app, but since it's just a web interface, you can run it on another computer on your home wifi, and then access it in your mobile browser. Refer to <https://docs.sillytavern.app/usage/remoteconnections/>

For Android users, in addition to the above, you can run the whole SillyTavern directly on your phone, without needing a PC, using the Termux app. Refer to <https://rentry.org/STAI-Termux> . (NOTE: Termux installations are not officially supported, and we cnanot guarantee it will work.)

## I tried to import a PNG character card but got an error that it's invalid. Why?

Two possibilities:

1. The card did not have the definitions embedded inside it and was just a normal image file. Some programs or file managers will strip the embedded definitions from the card when you save them. Make sure you're using the raw PNG file as it was posted by the person who shared it.
2. The PNG file was actually a WEBP file with a `.png` filename. You can try renaming the card to `.webp` before importing, or look for a proper PNG version of the image.

## How can I make my own AI character?

It depends on the model/API you're using. KoboldAI seems to use a custom syntax, you can refer to their site for that.

I will speak for the services I know: GPT, Claude, and the Llama or Mistral-based LLMs. With these services, you can just use the natural English language to describe the character. Let's create a very basic new character as an example.

1. Click the Character Management button
1. Click Create New Character
1. Under Character Name, give a name, like Amanda
1. Optionally, click the Select Avatar button to pick an image portrait for this character
1. Under Description, describe the character, and include any information you want that you feel is relevant to the chat. For example: ```Amanda is a student traveling during her gap year. She's 6 feet tall, and a volleyball player. She has an athletic figure. She has long brown hair. She loves the Victorian England period, and watching TV and reading novels relating to that period.```
For example, if you want Amanda to be friendly, then you would add: ```Amanda is extremely cheerful and outgoing.```
1. Under First Message, write the greeting the character when you begin a new chat. For example: ```*Amanda waves at you* Hey! Are you a backpacker too?```
1. Click the Create Character button

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

## Where can I find the old backgrounds?

We're moving to a 100% original content only policy, so old background images have been removed from this repository.

You can find them archived here:

<https://files.catbox.moe/1xevnc.zip>

## Where are my API keys stored? Why can't I see them?

SillyTavern saves your API keys to a `secrets.json` file in the server directory.

By default, they will not be exposed to a frontend after you enter them and reload the page.

To enable viewing your keys by clicking a button in the API block:

1. Set the value of `allowKeysExposure` to `true` in `config.yaml` file.
2. Restart the SillyTavern server.

## Why is the UI so slow/jittery?

* Try enabling the No Blur Effect (Fast UI) mode on the User settings panel.
* make sure your browser is using Hardware Acceleration.

## How to make the AI write more?

Sometimes the AI will only respond with a single sentence when you'd like it to be more verbose.
This is usually a problem with locally run models like Pygmalion.

If you simply want the bot to continue writing from where it left off at the end of its most recent reply, you can send an empty user message by typing nothing into the Input Bar and clicking Send. This will force the bot to continue the story.

Strategies for fixing this:

* Increase the `Response Length` slider
* Design a good `First Message` for the Character, which shows them speaking in a long-winded manner. AI models can improve a lot when given guidance about the writing style you expect.
* Add a phrase in the character's Description Box such as "likes to talk a lot" or "very verbose speaker"
* Do the same thing for your `Author's Note`, or `Post-History Instruction Prompt`
* As a last resort, you can try turning on `Auto-Continue` (in the User Settings panel), but will make responses come out slower because it's making the AI produce small replies back to back, and then combining them all together into one big reply. It may also be incompatible with some API options.

## How to make the AI write less?

This is mostly only a problem for models like ChatGPT or Claude. The same strategies can be applied but in reverse.

* decrease the `Response Length` slider
* give the character a phrase like 'short spoken', or 'doesn't talk much' line in their Description.
* give the character a brief First Message to set the tone and expectation for the chat.
* make sure `Auto-Continue` is turned off.

## How to make the AI stop writing the actions of my character, and driving the plot all on its own?

This should be handled in the `Author's Note` with a combination of phrases like:

* \{\{char\}\}'s responses shall only be passive and reactive to \{\{user\}\}'s actions.
* Your next response shall be solely from the POV of \{\{char\}\}.
* You are never allowed to dictate actions or speech for \{\{user\}\}
