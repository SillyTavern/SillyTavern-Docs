---
title: Guides
---

# World Info Guides

**What is Worldinfo / a Lorebook?** In a very simple sense, it is a way to send additional text to the AI apart from your chat and character card description.

World info can be independent of the card you are using, whether it's a solo-chat or group-chat, or you could tie it to a particular chat, or even tie it to a particular character card.

It's all just additional text boxes with ways to "trigger" when they are sent. [Author's note](/Usage/Characters/Author's-Note.md) does the same thing, World Info does it with way more features.

Yeah you could even use WorldInfo for the character description itself! But we'll discuss that in [Getting the most out of World Info](#getting-the-most-out-of-world-info). 

ST documentation should really explain everything you need to know so do try reading [World Info](World_Info.md) first!

## Getting started with World Info

In the initial phase, we are just going to deal with a "global lorebook", i.e. something you just activate and then use with any card/chat.

Let's start from absolute zero - just play it by the ol' ear and set something up from scratch.

![Initial chat](/static/wi-howto/getting-started-1.png)

In this particular example, I have a basic character card with a simple single line description and first message.

> Underscore is a British-Indian woman working on her aunt's banana plantation.

> Hey there, nice to see ya mate!

[![](/static/wi-howto/character-card.png){ width=500}](/static/wi-howto/getting-started-2-1.png)

Click on the <i class="fa-solid fa-book-atlas"></i> **World Info** icon and the <i class="fa-solid fa-globe"></i> **New World** symbol to create your first new world!

![Founding the Banana Farm](/static/wi-howto/banana-farm.png){ width=500}

Now, I'm sure you managed to create a world, (I'm calling my world the "Banana Farm"), but before we get into that, it is useful for you to understand a little of the basics about what your Prompt really is.

Here's my chat, and the response I got from the AI. 

> \*As I walk into the farm, I notice Underscore walking around with a basket of bananas in her arms.\* Hey there! What are you doing?

[![](/static/wi-howto/chat-and-response.png)](/static/wi-howto/getting-started-2-1.png)


Now, if you open your ST console (the black CMD window that you see), you'll see a lot of stuff that pops up from ST doing things as you execute stuff. If you scroll up a little, you'll find something similar to this, i.e. the actual wall of text that was sent to the AI.

[![Chat and response in Terminal](/static/wi-howto/chat-and-response-terminal.png)](/static/wi-howto/getting-started-2-2.png)

You see, every time you send a message to the AI, ST is sending a Full Wall of text to the AI.

* Information from your character card.
* Your chat history
* More stuff (that we shall get into further in this thread!)

You can see that ST sent my character's description, followed by all the messages in my chat history. That entire text was sent to OpenAI, which then created a response based on all that text, and sent it back to me.

Depending on your model, the formatting of the prompt in the cmd window may look a little different! Because every AI model is a little different and has different requirements. But we won't get into that today.

### Creating your first World Info Entry

So now that we created our Banana Farm world after clicking on the globe icon, let's create our first world info entry.
Click on new entry, then click on the dropdown button.

![New entry](/static/wi-howto/new-entry-collapsed.png)

*clicks on the dropdown button <i class="fa-solid fa-circle-chevron-down"></i> to show the full entry*

Damn, that's a lot of buttons.

![New entry expanded](/static/wi-howto/new-entry-expanded.png){ width=500}

But forget that! To create our basic first entry, all we need is these few fields.

[![Basic entry fields, blank](/static/wi-howto/new-entry-fields-blank.png){ width=500}](/static/wi-howto/new-entry-fields-blank.png)

So now I've created an entry that triggers on the keyword `Banana`.

> Bananas: Underscore's Aunt's farm grows rare Purple coloured bananas.

[![Basic entry fields, completed](/static/wi-howto/new-entry-fields-filled.png){ width=500}](/static/wi-howto/new-entry-fields-filled.png)

The keyword itself is only used to activate the entry. This means that I need to specifically write about Bananas in the content again, I can't just write "They have purple ones too". Sure in this example the AI might make sense of it, But only the text in the "Content" field is going to go to the AI.

The Strategy I have selected is 🟢 , i.e. "normal". Your other options are Constant 🔵  and Vectorized 🔗 . 

* **Normal** means that this entry will only get activated if it finds the word "Banana" in the chat.
* **Constant** would mean that the entry would get inserted into the chat regardless of the use of the word Banana anywhere!

Don't forget to activate the lorebook.

![Activated lorebook](/static/wi-howto/lorebook-activated.png){ width=500}

---

> What kind of Banana do you grow here?

Success! She mentioned the purple bananas!

[![Chat screenshot](/static/wi-howto/chat-purple-bananas.png)](/static/wi-howto/getting-started-5-1.png)

But more importantly, let's take a look at the cmd window again.

[![Terminal window with inserted World Info entry](/static/wi-howto/chat-purple-bananas-cmd.png)](/static/wi-howto/getting-started-5-3.png)

So that's where that entry went. It's gone right at the top of the prompt. ST Sent it as a part of the wall of text. GPT managed to use that information to talk about the purple bananas.

### Position

Now, supposed you did not want that entry to be inserted at the very top of the prompt, but at the bottom? Or somewhere in the middle?

That's where this option comes into the picture:

[![Position dropdown](/static/wi-howto/position-dropdown.png){ width=500}](/static/wi-howto/getting-started-6-1.png)

**Position** lets you define where in the overall wall of text (the "prompt"), you want your entry to go. If you hover over it, ST's tooltip will pop up.

* **CN** -> Character Definitions. This is what we had picked, the default - "Before Character Definition", and that's why our entry went straight to the top of the prompt, above the Character Definition of Underscore.
* **AN** -> Author's Note. If you had defined an an Author's Note, you could set your entry to be relative to that instead.
* **EM** -> Example Messages. We'll get to this later.

These are all positions to help you define a Relative position for your entry, i.e. it will get inserted before or after some other part of ST's basic default parts of a prompt. i.e. the character definitions, author's note, or example messages.

#### At Depth

The "At Depth" option allows us to define an Objective position. E.g. forget all the other stuff, I want my entry to always be inserted before the second last message in the Chat. That's what we call Depth.

The bottom of the chat is what call Depth 0. This means it would be inserted at the very bottom, even after your last message to the AI. Depth 1 would mean, insert right above the last message. Depth 2 would mean insert above the last 2 messages, and so on.

When inserting a message "At Depth", you can choose whether the message is sent to the AI as the "User" (you), the "Assistant" (the AI/your character) or "System" (basically trying to be a neutral message. We don't want the AI to think that information was part of a previous part of the chat, we want it sort of distinct, plus many AIs freak out if there's no role assigned)

[![Inject at depth 2 as "System" role](/static/wi-howto/position-at-depth.png){ width=500}](/static/wi-howto/getting-started-6-2.png)

This is what the CMD window would look like now, with the entry now at Depth = 2, role, System.

[![Entry injected at depth 2 as "System" role](/static/wi-howto/getting-started-6-3.png)](/static/wi-howto/getting-started-6-3.png)

### Order

Now let's talk about Order!

I created a second entry now, specifying that this Farm is in the British countryside, not India. GPT seemed to assume that my aunt's farm would be back in India, makes sense!

> The Farm is located in an idyllic location on the British countryside, they import Bananas from all over the world.

[![Entries for Bananas and Farm](/static/wi-howto/entries-initial.png)](/static/wi-howto/getting-started-7-1.png)

However, when this prompt is getting sent to the AI, it is getting sent like this. 

[![Terminal window with two inserted World Info entries](/static/wi-howto/entries-initial-terminal.png)](/static/wi-howto/getting-started-7-2.png)

What If I want Purple Bananas to be first, and "The Farm" to be second, when ST structures the entries to send to the AI?

That's where we'll use "Order". Now the numbering for Order goes the other way than Depth.
An order of 1 means topmost, whereas an order of 9999 etc would mean bottom most.

If I set order for Purple Bananas to 1, and The Farm to 2, then I should get what I want. 

[![Reordered entries](/static/wi-howto/entries-reordered.png)](/static/wi-howto/getting-started-7-3.png)

Aaaand Success! 

[![Terminal window for reordered entries](/static/wi-howto/entries-reordered-terminal.png)](/static/wi-howto/getting-started-7-4.png)

So you can combine the Order field with the Position field. If you have a group of entries at a certain position, e.g. Before CN, or After AN, or @Depth 4 or something like that, Order helps you arrange the entries amongst themselves at that position, like a sort of tie-breaker!

### Activation Settings

Now let's take a quick look at all the Activation Settings! 

[![Activation settings](/static/wi-howto/activation-settings.png){ width=500}](/static/wi-howto/getting-started-8-1.png)

These are the "global settings", i.e. the default treatment for every entry. Presently, ST lets you override these settings for every individual Entry too!

Again, the tooltips and the docs already cover this.

#### Scan Depth

We just spoke about [depth](#at-depth) a little earlier, so "Scan Depth" means that ST will "scan" only those many messages in your chat history (from most recent, going upwards) when looking for the keyword. 

Right now I have scan depth set to 4, meaning, if I Send that next message, which will get inserted as message 7, ST will only look at Messages #7, #6, #5, and #4 for the keyword "Banana".

[![Messages scanned at scan depth 4](/static/wi-howto/scan-depth4.png)](/static/wi-howto/getting-started-8-2.png)

My "Farm" entry is set to 🔵 **Constant**, so keywords don't matter.

**Context %**: How much of your total Context do you want to allow for world info. Your total context size is what you define in the <i class="fa-solid fa-sliders"></i> **[Response Configuration](/Usage/Common-Settings.md)** menu, this basically says "If all the world info entries getting triggered exceed x% of the total context I have set, don't insert any more entries."

**Budget Cap**: Same principle, This is more a hard cap, "If all the entries activated so far exceed Y number of tokens, don't insert any more entries"

**Case Sensitive**: lets your keywords be case sensitive.

**Match whole words**: With match whole words on, "Banana" will trigger an entry, but "Bananas" won't.

**Alert on Overflow** = Get a warning popup if WI budget is exceeded.

These are the basic options that you need to care about. We'll cover the rest later.

**Min Activations**: Fun option! It tells ST to ignore "scan depth". Keep looking through the chat history until you find atleast Z number of entries to activate.

**Max Depth**: A cap for the Min Activations option, don't scan only up to this depth.

## Getting the most out of World Info

Worldinfo / Lorebook is a misnomer. It is basically the most powerful prompt builder ST offers you.

Here, I will make an argument for you to use only worldinfo for ALL your content. Let's quickly explore what worldinfo offers you currently: 

* Always on / Disabled / Keyword activation 
* Custom insertion position 
* Trigger % to define the likelihood of the entry being inserted if activated. 
* Automation ID (linking a lore to executing an STScript)
* Entry level Custom Scan Depth 
* Case sensitivity for keywords 
* Match whole words 
* Recursion, disalbing recursion for entry, preventing entry from recursive activation 
* Filter to characters / exclude characters 
* Inclusion Groups 
* Sticky/Cooldown (keep the entry activated / prevent reactivation for X messages)
* Delay (prevent activation until chat has X messages)

[![World Info Example](/static/wi-howto/dropdowns.png)](/static/wi-howto/dropdowns.png)

### Post-History Instructions

Use [Post-History Instructions](/Usage/Prompts/prompts.md#post-history-instructions) for JBs? Put it into world info instead. Select @D insertion depth, and choose the role you want it assigned to.

Now you can have multiple JBs in the same chat, you can leave it always on, enabled by keywords, etc. Creating a dynamic experience, rather than just having one single JB pounding away into the chat.

At a super BASIC level, you could have an SFW JB and an NSFW JB. Many options here!

[![Post-History Instructions](/static/wi-howto/phi.png)](/static/wi-howto/phi.png)

### Character Descriptions

Character description? Put it in WorldInfo. Use the "Filter to Char" field and filter it only to that character. Now it is functionally exactly the same as having a character description for a card.

Why? If you are using a large context model, trust me, when your chat grows large enough; your character description is going to fade away into near obscurity, and your chat history will be determining what your character is like.

Use worldinfo! Insert your character descriptions at lower depths. Maybe at A/N level, maybe at a custom depth. Up to you!
Authors Notes

Authors notes have the ease of access going for them - but just to round off this post you can set an entry to a position of A/N and you're good to go.

### Automation ID

The automation ID allows you to execute an STscript whenever a particular entry is triggerred.

First you go to Quick Reply, and click on the three dots next to the script you want to execute:

[![Editing a Quick Reply](/static/wi-howto/qr-edit-btn.png){ width=500}](/static/wi-howto/qr-edit-btn.png)

Then you define some text as the automationID for that script:

[![Quick Reply](/static/wi-howto/qr-autoexec-automation-id.png){ width=500}](/static/wi-howto/qr-edit-detail.png)

Then you set that same automation ID against any worldinfo entry. Whenever that entry is activated, that script will run too!

### Inclusion Groups

!!! 
See [Inclusion Groups](World_Info.md#inclusion-group) for more details.
!!!

When left blank, this does nothing. When you enter something into the Inclusion Group field, if there are multiple entries with the same Inclusion Group that get activated at the same time, only ONE out of those entries will be inserted into the context.

(the probability of an entry being selected is the weighted average of the probability defined in trigger%)
You can now define the probability of the entry within the group separately.

Taking my JB example above, you could use this to ensure a random JB is triggered each time, without accidentally triggerring two of them.

#### Group scoring
[Use Group Scoring](World_Info.md#use-group-scoring) is an additional setting in "Activation Settings" and individual entries for worldinfo lorebooks, that impacts Inclusion groups.

It will check all of the entries and see which one has had the most keys trigerred  in the scanned chat.

So with some careful definition of multiple keywords, you could ensure that the "appropriate" version of your character's lore/background/description is chosen out of a set of entries for for that character. 

### Order ID and Depth

Order ID is basically just a tie-breaker between entries to decide which goes towards the top of the prompt and which goes further below.

If you have written multiple entries and you want them to be sent to the LLM like this:

> ABC: ABC is a great empire in the potato dimension

> XYZ: XYZ is a kingdom in the ABC Empire

Let us assume that both these entries have a position of "before CN", and we want to ensure if they are both mentioned, they are inserted int that order.

Then you want to assign ABC an order id of 1, and XYZ and order of 2.

* Lower order ID number = Towards the top of text of entries at a similar position.
* Higher order ID number = towards the bottom of text of entries at a similar position.

Order ID helps define a RELATIVE position. Depth on the other hand helps define an ABSOLUTE position.

Depth 0 = absolute bottom of the prompt. When the prompt is sent to the AI, this text will be at absolute bottom (this is typically where some people put their JBs)

Depth counts messages in your chat history to try to define its placement. 

