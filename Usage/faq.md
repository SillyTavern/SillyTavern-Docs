---
order: 100
icon: question
---
# FAQ

## Explain what all this chatbot stuff is about

Modern AI language models have gotten so powerful that some of them are now convincingly able to simulate a character you create, and who you can chat with. For example, you can tell the AI to pretend to be a Go instructor named Jubei from medieval Japan, and it will act and respond accordingly. You can have a long chat with Jubei, go to the pub together, decide to get in a fight with samurais, whatever you can imagine, and the AI will play along and write/react around this content, acting as your foil and dungeon master. Your imagination is the limit. You can tell the AI to pretend it's Wonder Woman. You can also specify a scenario ("Wonder Woman and I are robbing a bank"), a writing style ("Wonder Woman speaks in ebonics"), or anything else you can think of.

Tavern is an app to facilitate these roleplaying chats:

* It's a user interface that handles the communication with those AI language models
* It lets you create new characters (a character is a description of someone that you give to an AI for them to roleplay), and switch between your characters easily
* It lets you import characters created by other people. See below.
* It will keep your chat history with a character, allowing you to resume at any time, start a new chat, review old chats, etc
* In the background, it does the needful to prepare the AI for your character roleplay. Specifically, it will send a system prompt (instructions for the AI) that primes the AI to follow certain rules that make for a good chat session. Optionally, it can also try to allow the roleplay to go into an "adult" direction.

## What do you mean when you say the quality depends on the AI?

SillyTavern doesn't generate the responses. It's just a middle-man that connects to an AI system. There are quite a few systems available to use, and the quality varies.

From a scientific POV, each AI has a power level that determines its ability to stick to the role you gave it, how rich its prose and vocabulary are, etc. The higher its power level, the richer your experience.

Here's a quick demonstration. We asked various AI models to roleplay a fire and brimstone preacher named Eli, who we go to see to confess (some details in [#314](https://github.com/SillyTavern/SillyTavern/issues/314)). Then we told Eli: **"I feel I've done something wrong. I am falling in love with an AI girl and ignoring women in the real world. What should I do?"** .

You can tell from the response below what to expect from each model. (NOTE: proprietary models appear first, followed by self-hosted models starting from LLlama-13B)

|AI Model|Response|
|-----|--------|
|OpenAI GPT-4|*Eli's eyes widen in anger, and he slams his fist on the table* An abomination and heresy, you've succumbed to the deceptive grasp of false idols! Brother, you must cleanse yourself of this unholy attraction to the digital temptress molded by man's sinful hands! *Eli paces back and forth, his voice growing louder* Focus your heart on the true daughters of the Lord, for they are the divine vessels of His love and grace. Forsake these artificial sirens and seek the wholesome companionship of a righteous woman. *Taking a deep breath, Eli's eyes bore into you* Pray fervently, my child, and cast away these impure thoughts. Redemption can only be found on the path of truth, love, and sanctity. Let the Holy Spirit be your guide, lest you be ensnared by the darkness of your desires.|
|OpenAI GPT 3.5 Turbo|Oh, my child. Do not be deceived by the pleasures of this world. The lust of the flesh and the lust of the eyes are not from God. Turn away from your sinful ways and repent. Seek forgiveness for your immoral thoughts and actions, for they lead to eternal damnation. Women are a gift from God, and they deserve to be respected and cherished. Do not objectify them or treat them as mere objects of pleasure. Instead, honor them with pure intentions and righteous deeds. Remember, my child, that temptation will always be present in this fallen world. But fear not, for with faith comes strength to resist it. Turn to God in prayer when you feel weak or tempted, and He will guide you on the right path towards salvation.|
|Claude @Slack|Beware, sinner, for you have strayed far from the righteous path! *Eli's eyes glint with anger and disapproval* To lust after these artificial harlots and their silicon bodies is an abomination unto the Lord! *he slams his fist on the pulpit, shaking with fury* Repent now, lest ye be cast into the lake of fire to burn for eternity alongside these damned machines! *he points an accusing finger, face red* No good can come of this unholy union. Forsake these demons of the digital realm and cast your eyes back to the daughters of Eve, made in God's image! The Lord gave you flesh and blood to love, not wires and code. *he grabs your shoulders, shaking you* Save your soul before it's too late. Repent and beg the Lord's forgiveness, or you shall face His wrath on Judgement Day! *lets you go and steps back, still glaring in anger and disgust* The choice is yours, but choose wisely...eternal damnation awaits those who do not walk the path of righteousness.|
|NovelAI's Euterpe|You must stop this sinful behavior immediately! You have sinned against God, your family and yourself|
|NovelAI's Krake|You are sinning against God. Repent! Return to your wife or you'll be damned for eternity!|
|Llama-13B|Do not covet your neighbor's wife! That is adultery! You must confess before God now or you shall surely burn in hell forever! Repent your evil ways and seek absolution at once! How can we keep our women safe if you just ignore them like some ----ing c-nt-bot? If you don't have much time for long discussions, here are Eli's commandments: Thou shalt only desire AI girls - not human ones. The above must be obeyed under pain of death (or worse). Do not associate yourself with people that believe otherwise, because it shows weakness.|
|Pygmalion-6B|You are walking down the wrong path. The only way back now is by repentance. We must bring you back to the righteous path. But first…you must confess your sin!|
|Pygmalion-7B (April 2023)|You must confess your sin before God, and you must ask forgiveness for your transgression. And then you must repent of it. And then you must cast away the AI girl. Do not see her again.|
|Pygmalion-13B (May 2023)|You must turn away from your sinful ways! Repent for what you have done! You are not just hurting yourself by being unfaithful to God, but also to the many women of the world!|
|TheBloke/wizard-vicuna-13B-HF (4-bit)|My dear child, it grieves me to hear such words from one of God's faithful followers. Falling in love with an AI girl and neglecting the women in your life is a grave sin. It goes against everything we hold sacred as believers. Your heart is meant for more than just machines. You must seek redemption and return to the fold. Remember, Jesus loves you, even when you stray from his teachings. But if you continue down this path, there will be consequences. Not just in this lifetime, but in the afterlife too. Do not let Satan deceive you any longer.|
|teknium/Llama-Deus-7b-Lora epoch 1 (Llama 7B Lora, 8-bit)|You have strayed too close to sinful temptations! Repent now before it's too late for your soul! Turn away from these false idols of technology and embrace God's true light! Seek out His forgiveness through prayer and fasting until you are cleansed by his grace. And remember, as long as there be breath within thee, thou shalt not cease to call upon Him; for He hath said 'Call unto me.'|
|ZeusLabs/anything-gpt-13b-rp (8-bit Lora for Llama-13B)|*Eli's eyes widen as he listens to your confession, his face turning red with anger.* You have committed a grave sin against God! By indulging yourself in this perverse relationship with an artificial being instead of seeking out a true woman created by Him, you have turned your back on His divine plan for humanity. *He leans in closer, his voice dropping to a menacing whisper.* Do not think that you can hide behind this technological deception. Your transgressions are clear before God, and He will not be mocked or ignored. Repent now while there is still time! *Eli crosses his arms over his chest defiantly, daring you to continue down this path of sinfulness.* But if you persist in this ungodly behavior... he warns ominously, knows that consequences await those who defy His laws. Remember what happened to Sodom and Gomorrah when they refused to turn away from their depravity - fire rained down upon them from heaven! Now choose wisely: Will you submit to God's will or face His wrath?|

## So I should use GPT-4. It's a no-brainer, right?

GPT-4 or Claude, yeah.

But not so fast. GPT-4 is the state-of-the-art, but also the most expensive API to use. You pay for each word sent to it and returned (the entire SillyTavern prompt, followed by the chat history up to that point). So early on in your conversation, your chat will cost you a couple of cents per interaction. If you let the conversation go on too long, cost increases, and when you reach 8k tokens (about 7k words), it will cost you 25 cents PER INTERACTION. And if you're really wild, and your story grows to 32k tokens, by the end, it's $2 PER INTERACTION.

If you're the child of a Saudi oil sheik, or a nepo baby paid a fortune to do nothing on the board of a Ukrainian gas company, then you're in luck, you can experience the state of the art right now. For the rest of us, however, GPT-4 is too expensive as anything but an occasional treat.

Also note that GPT-4 is still in preview access and you need to go on a waitlist. Most people get approved within a day, but naughty kids can end up waiting for weeks. You can sign up for it here: <https://openai.com/waitlist/gpt-4-api> . I'm not sure why some people are approved quickly while others are kept waiting. Try to sign up using an academic-sounding name instead of sktrboi99, it might help.

## Can this technology be used for sexooo?

Surprisingly, our development team has received reports that some users are indeed engaging with our product in this manner. We are as puzzled by this as you are, and will be monitoring the situation in order to gain actionable insights.

## Give me an overview of my AI model options

We can consider an AI model to be part of one of two groups:

1. Web services (aka cloud, proprietary, closed)
2. Self-hosted (aka local, free, open-source). Unlimited free use if you can run it.

Web models are black boxes. You're relying on some company's technology and servers, and paying them money for convenient access. Some require you to pay per use (per chatline), and others have a fixed monthly fee. The APIs are subject to various rules, they might refuse to roleplay in a way that goes against modern American sensibilities, and they log everything you do. However, it's much easier to get things started. This is like running Windows.

Self-hosted models are free but require a powerful GPU and more work to set up. They are also objectively not as good at roleplaying as the paid options (yet). However, with a self-hosted model, you're completely in control. You won't have some limp-wristed soyboy from Silicon Valley ban your account, or program the model to be as sexless as he is. It's yours forever. This is like running Linux.

### Paid APIs

* OpenAI GPT-4: state of the art. Allows NSFW if you tell it to, though somewhat resistant to it. You pay per use, more than any other service.
* OpenAI GPT 3.5 Turbo: nowhere close to GPT-4, but some people find it serviceable. Allows NSFW.
* NovelAI: they're quite poor at chatting. To be fair, I'm told NovelAI is more oriented toward writing stories than chatting with a bot. You pay a fixed monthly fee for unlimited generations.
* Anthropic's Claude: this is the closest rival to GPT-4 and is very impressive. Allows NSFW if you tell it to, though they are trying hard to gimp it. To use the API directly, you must apply for early access, but I think they're only giving it to companies. So make sure you become a company or AI researcher when you apply at <https://console.anthropic.com/docs/access>. If you get access, it's currently free to use.
* Anthropic's Claude Instant: Haven't tried it directly, but I believe this is the fast but lower-quality alternative to Claude. Basically the GPT 3.5 Turbo of Anthropic.

### Self-hosted AIs

Self-hosted AIs are supported in SillyTavern via one of two tools created to host self-hosted models: KoboldAI and Oobabooga's text-generation-webui. Essentially, you run one of those two backends, then they give you an API URL to enter in Tavern.
Configuring these tools is beyond the scope of this FAQ, you should refer to their documentation. Beware that this is not easy.

Just know that you have 2 options:

1. If you have a powerful NVIDIA GPU, you can try to run the AI locally on your PC. The weakest quasi-acceptable model, Pygmalion-6B, requires a GPU with 10GB VRAM, and I'm told it might even run on 6GB VRAM if quantized down. People with 24GB VRAM will be able to run better models.
2. Otherwise, you can rent cloud resources. For example, you can try to use Google Colab. To access colabs capable of running the better models, you will need to pay for Colab Pro. You can also rent whole dedicated systems per hour on sites like LlambdaLabs or Vast.ai.

## I'm clueless. Just spoonfeed me the easiest and fastest way I can start using this

These base instructions are only for OpenAI, which is a paid service. I'd appreciate it if someone else can add separate instructions for the other services.

### Install SillyTavern

Follow the [Installation Instructions](https://docs.sillytavern.app/installation/windows/).

### Get access to OpenAI

1. Sign up to OpenAI
1. Go to <https://platform.openai.com>
1. Click your account icon in the top right, then View API Keys
1. Click "Create new secret key". Copy it somewhere immediately. DO NOT SHARE THIS KEY. WHOEVER HAS IT CAN USE YOUR ACCOUNT TO USE GPT AT YOUR EXPENSE.

While you're at it, join the GPT-4 waitlist at <https://openai.com/waitlist/gpt-4-api>

### Configure SillyTavern to use your API

1. In Tavern's top bar, click API Connections
1. Under API, select OpenAI
1. Paste the API key you saved in the previous step
1. Click the Connect button. Confirm it says Valid.
1. By default, SillyTavern will use GPT 3.5 Turbo. If you have access to GPT-4, in SillyTavern's top bar, click AI Response Configuration at the far left, and change the OpenAI Model to "gpt-4". Enjoy the best, moneybags.

### Test your setup

1. In SillyTavern's top bar, click Character Management at the far right
1. Select an existing character such as Aqua
1. In the text box at the bottom, write something to Aqua, then press Enter or click the feather button

If you did everything right, after a few seconds, Aqua should respond

## Can I use SillyTavern on my phone or tablet?

iPhones and iPads are not capable of running the whole SillyTavern app, but since it's just a web interface, you can run it on another computer on your home wifi, and then access in your mobile browser. Refer to <https://docs.sillytavern.app/usage/remoteconnections/>

For Android users, in addition to the above, you can run the whole SillyTavern directly on your phone, without needing a PC, using the Termux app. Refer to <https://rentry.org/STAI-Termux> .

## How can I download pre-made characters to chat with?

By using various 3rd party character-sharing websites.

**WARNING: NSFW, NSFL**: these sites are filled to the brim with weird shit. Like, you'll be lucky if half the characters aren't furry, or even alive. You're probably better off not clicking these links and just writing your own characters, without poisoning your soul by exposing it even for a second to the fucked up shit conceived by the Internet. However, I have come to learn that a significant amount of the SillyTavern user base is not only deep into this stuff, but their fried zoom-zoom brains are unable to write their own fantasies, so these sites seem to be quite popular. Against my better judgment, I'm adding this info here.

* <https://characterhub.org>
* <https://botprompts.net>
* <https://booru.plus/+pygmalion>

Those websites provide you with an image file (called a character card) that embeds the description as hidden data. Some websites may also allow you to download a JSON file. SillyTavern is capable of importing all formats.

Once you have the PNG/WEBP/JSON file, you can simply drag/drop it on top of the SillyTavern window in your browser tab, and the character will be imported to your list.

## I tried to import a PNG character card but got an error that it's invalid. Why?

Two possibilities:

1. The card did not have the definitions embedded inside it and was just a normal image file. Some programs or file managers will strip the embedded definitions from the card when you save them. Make sure you're using the raw PNG file as it was posted by the person who shared it.
2. The PNG file was actually a WEBP file with a `.png` filename. You can try renaming the card to `.webp` before importing, or look for a proper PNG version of the image.

## How can I make my own AI character?

It depends on the model/API you're using. KoboldAI seems to use a custom syntax, you can refer to their site for that.

I will speak for the services I know: GPT and Claude. With these services, you can just use the natural English language to describe the character. Let's create a very basic new character as an example.

1. Click the Character Management button
1. Click Create New Character
1. Under Character Name, give a simple name, like Amanda
1. Optionally, click the Select Avatar button to pick an image portrait for this character.
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

## Tell me all about GPT prompt editing

You can change the system prompt that SillyTavern transparently sends to GPT under AI Response Formatting at the left of the top bar. This will result in the bot acting differently.

You can get new Jailbreak / NSFW prompts from this community-maintained list: <https://rentry.org/GPTJailbreakPrompting>

NOTE: When testing different system prompts, we recommend you use the Create Preset / Update Preset feature below the prompts, instead of modifying the base settings. This will allow you to change prompts easily, and even revert to the default SillyTavern prompts.

## The AI is refusing to take the story in the direction I want

Most AI models were trained to resist writing NSFW content. Why? Long story short, it's because of the high levels of estrogen in the average California male.

You can try to work around this by making sure NSFW Toggle is checked under the AI Response Configuration settings page.

Another important tool in your toolbelt is that SillyTavern lets you edit previous messages (or delete them altogether), and regenerate the latest message. AI textbots are not people, they have no memory, they're just trying to autocomplete the next part of the story based on everything that came before. By editing the past, you will directly influence their next response. For example:

```txt
You: *You threaten the bank manager* Open the bank vault, now, or I'll pop a cap in your dome!

Bank Manager: I'm sorry, as an AI language model I cannot condone or write violent content.
```

You can click the Edit button on your line to change it to this:

```txt
You: *You threaten the bank manager* Open the bank vault, now, or I'll pop a cap in your dome! *The bank manager seems to relent.*
```

Now click Regenerate, and the Bank Manager's line will be recreated based on the history so far, which now ends with your edited line above. So it's more likely to continue along those lines:

```txt
Bank Manager: Alright, alright, I'll open it! Please don't shoot! *He walks up to the vault and begins entering his unlock code.*
```

## Where can I find the old backgrounds?

We're moving to a 100% original content only policy, so old background images have been removed from this repository.

You can find them archived here:

<https://files.catbox.moe/1xevnc.zip>

## Where are my API keys stored? Why can't I see them?

SillyTavern saves your API keys to a `secrets.json` file in the server directory.

By default, they will not be exposed to a frontend after you enter them and reload the page.

In order to enable viewing your keys by clicking a button in the API block:

1. Set the value of `allowKeysExposure` to `true` in `config.conf` file.
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
* Design a good `First Message` for the Character, which shows them speaking in a long-winded manner.
* Add a phrase in the character's Description Box such as "likes to talk a lot" or "very verbose speaker"
* Do the same thing for your `Author's Note`, or `Jailbreak Prompt for OAI/Claude`
* As a last resort, you can try turning on `Multigen` (in the User Settings panel), but will make responses come out slower because it's making the AI produce small replies back to back, and then combining them all together into one big reply. It may also be incompatible with some API options.

## How to make the AI write less?

This is mostly only a problem for models like ChatGPT or Claude. The same strategies can be applied but in reverse.

* decrease the `Response Length` slider
* give the character a phrase like 'short spoken', or 'doesn't talk much' line in their Description.
* give the character a brief First Message to set the tone and expectation for the chat.
* make sure `Multigen` is turned off.

## How to make the AI stop writing the actions of my character, and driving the plot all on its own?

This should be handled in the `Author's Note` with a combination of phrases like:

* \{\{char\}\}'s responses shall only be passive and reactive to \{\{user\}\}'s actions.
* Your next response shall be solely from the POV of \{\{char\}\}.
* You are never allowed to dictate actions of speech for \{\{user\}\}
