---
order: prompts-50
---

# CFG

Page written by: kingbri

Contributors: kingbri, Guillaume "Vermeille" Sanchez, AliCat

## What is it?

CFG, or classifier-free guidance is a method that's used to help make parts of a prompt less or more prominent.

### Supported Backend APIs

Currently, the supported backends are oobabooga's textgen WebUI, NovelAI, and TabbyAPI. 
NovelAI had its own [documentation for CFG](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html).

WARNING: CFG increases vram usage due to ingesting more than 1 prompt! If your GPU memory runs out while generating a prompt with CFG on, consider reducing your context size, using a lesser parameter model, or turning off CFG entirely.

---

## Configuration

Accessing CFG settings are the same as accessing Author's note:

![CFGhamburgermenupng](/static/cfg-hamburger.png)

And here's what the CFG panel looks like:

![CFGchatpanelpng](/static/cfg-panel.png)

There are four dropdowns in the CFG panel:

- Chat CFG
  
  - Scopes the CFG scale and prompts to only this chat
- Character CFG
  
  - Scopes the CFG scale and prompts to the specified character
- Global CFG
  
  - Globally overrides the CFG scale and prompts (also overrides the model preset!)
- CFG Advanced Settings (formerly called CFG Prompt Cascading)
  
  - A place to combine prompts from the previous 3 dropdowns and set insertion depth.

NOTE: If the guidance scale is set to 1, nothing will be sent since that's when CFG is in an "off" state.

#### Group Chats

In group chats, the CFG scale panel looks like this:

![CFGpanelgcpng](/static/cfg-groups.png)

The main change is that character CFG is removed and a checkbox called `Use Character CFG Scales` is present in the chat CFG dropdown. This allows for the current character's guidance scale to be used instead of whatever the chat CFG scale is set to.

The main utility of this feature is to alter the scale based on each character's individual needs.

In addition, checking the `Character Negatives` box in prompt cascading will append the independent character negative prompts along with the chat ones (if enabled).

---

## Concepts

### Isn't this in Stable Diffusion?

Yes and no. CFG with LLMs works in a different way than what one might be used to in Stable Diffusion. LLM-based CFG works on the principle of "prompt mixing". The CFG formula takes a positive and negative prompt, then mixes the *differences* between them. From there, a combined prompt is sent and a response is generated!

Here's an illustration to help visualize this concept. The red represents the negative prompt, the blue represents the neutral prompt, and the purple represents the mixed result that's interpreted. All the white space is the same across all 3 prompts, so those are not used for CFG mixing.

![stcfgdiagrampng](/static/cfg-diagram.png)

If you want to know more about CFG and LLMs, Vermifuge's original paper is located here. I'd suggest giving it a read/listen:

- Paper - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)
  
- Audio version - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)
  

### Do I need CFG prompts?

No! CFG prompts are completely optional. Just adjusting the guidance scale above `1` will also help produce an effect on responses, which can accentuate chats and character interaction.

### What makes a good CFG prompt?

So, we established that CFG prompting is not the same as Stable Diffusion's negative tags and embeddings. How do we make a prompt?

Warning: This assumes that you have created a character using PLists and Ali:Chat. If you have not, feel free to experiment with various prompting techniques.

Let's say I have a character named "John". John is supposed to feel happy and excited all the time from his example dialogues. However, when chatting with John, he's sometimes sad and depressed.

To remove this, CFG comes to the rescue! Just make the negative prompt `[John's feelings: sad, depressed]` to help remove the sadness portions. You can optionally make the positive prompt `[John's feelings: happy, joyful]` to further bring out John's happy parts.

### Positive Prompts

I went over this in the previous section, but I'd like to touch on this a bit more. Positive prompts are used to further accentuate parts of a character. Let's use John again as our example. By making him happier with a positive prompt of `[John's feelings: happy, joyful]`, John should start outputting dialogue with a more happy feeling than if the positive prompt was not included.

### But...

These are just **loose guidelines** from experience with one specific character format. There are many other ways to create prompts that you should experiment with. Feel free to share your thoughts with other users!

### Guidance Scale

Here's a rule of thumb. A guidance scale of `1` means that CFG is disabled. In fact, SillyTavern won't send anything to your backend if the guidance scale is 1. A guidance scale `>1` will give the results shown in the other sections at varying degrees.

However, a guidance scale of `<1` will give the *opposite* effect since the negative prompt is used as the primary prompt here.

Let's use the example with John again. The negative prompt is `[John's feelings: sad, depressed]` and the positive prompt is `[John's feelings: happy, joyful]` with a guidance scale of `0.8`.

This will in turn accentuate the *negative* prompt more and you'll see John start to act sadder than normal rather than happier.

tldr; Use a guidance scale of `1.5` and work up and down from there based on your outputs.

### Prompt Cascading

Negatives and positives can be cascaded between CFG types (the types being per-chat, per-character, and global overrides). See the Configuration header for more information.

### Insertion Depth

Follow the basic rule: The lower something is located in the prompt, the more influential it is to the response. For chatting, I recommend using the default depth of `1` since it's very flexible with other components of SillyTavern.

However, if you want to experiment, an insertion depth of `0` is open. However, these can dramatically alter how your response will look and it's NOT recommended to use prompt cascading here!
