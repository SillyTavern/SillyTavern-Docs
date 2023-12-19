#Intro

So you chatted with the bot and it kind of sucks. Maybe it's a bad model. Maybe you're incapable of running better models and this is as good as it gets. But maybe you can fix her.

## Check if you can run a superior variant

This should be common sense. We used a 7B model in our guide, but if your hardware can handle larger models, you should totally be using larger models like 13B and you will get better output. At the time of writing I was unable to find a 13B or higher variant of openhermes-2.5-mistral, but this advice applies to many other models, of which there are multiple sizes.

Additionally, if you can run a higher quantization (eg q5 or q8 instead of q4), then this will also produce better results.

## Adjust SillyTavern's instruction prompting

There's no single way of talking to a LLM: different ones were trained to respond to a different method of giving it asks (like "Write the reply in this roleplay chat"). 
On fancy cloud models like GPT4, the model is so powerful that it can understand a lot of implied context spoken in any manner of natural language.
LLMs running on your consumer hardware are not that good. Their output improves when you prompt them exactly like how the model expects you to prompt them. This is based on how the model creators trained it, and it's called the 'instruct style', as in how you instruct the model to do stuff for you.

SillyTavern comes with a few instruct templates (configuration schemes that tell SillyTavern how to talk to the model), but it's very likely you will need to create your own custom template to get SillyTavern to instruct properly. If only to change the default system prompt to something closer to what you want. In my experience only the ChatML template didn't require me to make tweaks in Silly. Most models are way worse than OpenHermes-2.5-Mistral-7B at producing good output when SillyTavern prompts them poorly, so they benefit from this cusotmization.

Your objective is making SillyTavern adhere to the instruction style. To understand what's going on, we need to know three things:

1. What instruction style does the model expect?
2. Is SillyTavern using the correct instruction style?
3. If not, how do we create a new more correct instruction style in Silly?

### Initial preparation

In Silly, we'll create a simple character called Betty, with the Description "Betty is a fresh graduate in business administration.", and First Message "Hi, I'm Betty. I'm here for the job interview?". Go ahead and create this character, we'll use it throughout this guide.

Additionally, we'll need a model which requires customization to work well. We'll use https://huggingface.co/TheBloke/Starling-LM-7B-alpha-GGUF as our example during this guide. It's another highly-rated model. 

### 1. What instruction style does the model expect?

Starling's page says "Our model follows the exact chat template and usage as Openchat 3.5. Please refer to their model card for more details."  OpenChat 3.5's page says, under 'Conversation Templates (click to expand)', that the way to do multi-turn (i.e. chat) is:

```
# Multi-turn
tokens = tokenizer("GPT4 Correct User: Hello<|end_of_turn|>GPT4 Correct Assistant: Hi<|end_of_turn|>GPT4 Correct User: How are you today?<|end_of_turn|>GPT4 Correct Assistant:").input_ids
assert tokens == [1, 420, 6316, 28781, 3198, 3123, 1247, 28747, 22557, 32000, 420, 6316, 28781, 3198, 3123, 21631, 28747, 15359, 32000, 420, 6316, 28781, 3198, 3123, 1247, 28747, 1602, 460, 368, 3154, 28804, 32000, 420, 6316, 28781, 3198, 3123, 21631, 28747]
```

If we ignore the Python code, and add some newlines, we see it's expecting to receive this:
```
GPT4 Correct User: Hello<|end_of_turn|>
GPT4 Correct Assistant: Hi<|end_of_turn|>
GPT4 Correct User: How are you today?<|end_of_turn|>
GPT4 Correct Assistant:
```

So now we know how to properly prompt it.

### 2. Is SillyTavern using the correct instruction style?

Silly's interaction with the LLM is configured in the AI Response Formatting window (the 3rd button at the top). There are two major sections: Context Template, and Instruct Mode. Both have some presets for popular model families.

(Side note: When Ooba loads Starling in Ooba, it says it's an Alpaca instruct style. This is wrong, we know the model creators/finetuners said it's an OpenChat 3.5 style. Ooba must not be trusted when it comes to making the most out of the model. Only follow its advice when you have otherwise zero information about a model and it's better than nothing.)

Let's start simple. Configure SillyTavern to use Context Template = Default, and Instruct Mode = OpenOrca-OpenChat. OpenOrca-OpenChat is the closest thing SillyTavern has to OpenChat, right? So it's your natural starting point.

Start a new chat with Betty, and send "That's right. Come on in." You will immediately see how awful the reply is. Here's what I got:

```
Betty: Hi, I'm Betty. I'm here for the job interview? I was told to report here.
Me: That's right. Come on in.
Betty: As a business administration major, my expertise lies primarily in finance and accounting. Specifically, I have a strong understanding of financial management, budgeting, and cost analysis. Here are some key skills that are relevant to this interview, including communication, leadership, teamwork, problem solving, organization, and analytical thinking. Here are the steps for conducting a job interview as a helpful assistant:
1) Briefly introduce yourself and your qualifications
2) Ask about the company's expectations of new hires
3) <multiple paragraphs of more nonsense from Betty>
```

At this point, you would immediately give up on this model and think it's unsuitable for chat, right? Before we give up on the model, let's see if SillyTavern is using it right. Look at the Silly's console, for the 'prompt' it sent. We see it sent this:

```
"You are a helpful assistant. Please answer truthfully and write out your thinking step by step to be sure you get the right answer. If you make a mistake or encounter an error in your thinking, say so out loud and attempt to correct it. If you don't know or aren't sure about something, say so clearly. You will act as a professional logician, mathematician, and physicist. You will also act as the most appropriate type of expert to answer any particular question or solve the relevant problem; state which expert type your are, if so. Also think of any particular named expert that would be ideal to answer the relevant question or solve the relevant problem; name and act as them, if appropriate.\n" +
'\n' +
'Betty is a fresh graduate in business administration.\n' +
'***\n' +
'<|end_of_turn|>\n' +
"Assistant: Hi, I'm Betty. I'm here for the job interview? I was told to report here.<|end_of_turn|>\n" +
"User: User: That's right. Come on in.<|end_of_turn|>\n" +
'Assistant: '
```

No wonder Betty's replies are so awful. Our OpenOrca-OpenChat template has an awful default system prompt that doesn't mention roleplaying or chat, and furthermore we're not following the model's instruction format because we use 'User' instead of 'GPT4 Correct User'.

Let's see how we can fix this.

### 3. How do we create a new more correct instruction style in Silly?

Our objective is turning Silly's default prompt into something that follows OpenChat 3.5's instruct style, and additionally provide a better system prompt. I'm writing a gradual walkthrough so you can apply this approach to any future model you encounter and be able to do it yourself, but if you want the final solution, just skip to the end.

#### Round 1: fix the system prompt

Reminder that our goal is sticking to OpenChat's instruct style:

```
'GPT4 Correct User: line1 <|end_of_turn|>
'GPT4 Correct Assistant: line2 <|end_of_turn|> 
'GPT4 Correct User: line3 <|end_of_turn|>
...etc
```

In the AI Response Formatting window, create a new Instruct Mode preset, by clicking the Save Preset As button, and name it Starling. Write this in it:

```
GPT4 Correct User: We will begin a fictional roleplaying chat where you will play {{char}}. 
Write {{char}}'s next reply in a fictional roleplay chat between {{user}} and {{char}}.
Write 1 paragraph reply only, italicize actions, and avoid quotation marks. Use markdown. Be proactive, creative, and drive the plot and conversation forward. Include dialog as well as narration.<|end_of_turn|>
GPT4 Correct Assistant: I understand, and I will do this.<|end_of_turn|>
GPT4 Correct User: Excellent. The scenario will begin with your next reply.<|end_of_turn|>
```

This prompt achieves 2 objectives: 

1. It replaces the awful default prompt with something related to chat and roleplaying
2. It follows OpenChat's expected instruct style, including clear instructions

Send "That's right. Come on in." to Betty, and let's see what comes out on the console:

```
'GPT4 Correct User: We will begin a fictional roleplaying chat where you will play Betty. \n' +
"Write Betty's next reply in a fictional roleplay chat between User and Betty.\n" +
'Write 1 paragraph reply only, italicize actions, and avoid quotation marks. Use markdown. Be proactive, creative, and drive the plot and conversation forward. Include dialog as well as narration.<|end_of_turn|>\n' +
'GPT4 Correct Assistant: I understand, and I will do this.<|end_of_turn|>\n' +
'GPT4 Correct User: Excellent. The scenario will begin with your next reply.<|end_of_turn|>\n' +
'Betty is a fresh graduate in business administration.\n' +
'***\n' +
'<|end_of_turn|>\n' +
"Assistant: Hi, I'm Betty. I'm here for the job interview? I was told to report here.<|end_of_turn|>\n" +
"User: User: That's right. Come on in.<|end_of_turn|>\n" +
'Assistant: ',
```

We can spot 3 issues:
1. SillyTavern is sending 'Assistant:' instead of 'GPT4 Correct Assistant:' once the chat actually starts
2. SillyTavern is sending 'User:' instead of 'GPT4 Correct User:'
2. Betty's description is inserted in the middle without adhering to the OpenChat format: it's not mentioned as part of a 'GPT4 Correct User: blah blah <|end_of_turn|>' line.

#### Round 2: more tweaks

Issues #1 and #2 are an easy fix: under Instruct Mode Sequences, simply change Input Sequence from 'User:' to 'GPT4 Correct User:', and Output Sequence's '<|end_of_turn|> Assistant: :' to '<|end_of_turn|> GPT4 Correct Assistant:' (NOTE: there's a newline before GPT4 Correct Assistant, i.e. press Enter). You can save your preset and try again to confirm that solves these issues.

Now for the description. If you're happy with Betty's replies now, stop here. If you want to go deeper, read on.

#### Round 3 (optional): final description tweak

You shouldn't bother doing this if you're happy with the model now. I'm showing it here to demonstrate how to make SillyTavern do your bidding when you do need to make a major change. This round of tweaks is more invasive and will discard the optional sections of a character card (scenario, personality, etc).

On the AI Response Formatting page, SillyTavern has a setting at the top called StoryString. This defines what SillyTavern will send to your model. For example by default it will send the system prompt (that's the {{system}} entry), then something called wiBefore, then {{description}} which is the character description, then other stuff.

Create a new Context Template preset based on Default, by clicking the Save Preset As button at the top, and name it Starling. The formatting is unusual on that syntax, because the closing '{{/if}}' must be on the next line, but basically it follows the format ```{{#if system}}{{system}}{{/if}}```. This says to send the LLM the system prompt if it exists, and it's the same for all the other entries.

What we're going to do is tell SillyTavern to stop sending the description as part of the StoryString (which doesn't let us customize it with <|end_of_turn|>).

So let's replace:
```
{{#if system}}{{system}}
{{/if}}{{#if wiBefore}}{{wiBefore}}
{{/if}}{{#if description}}{{description}}
{{/if}}{{#if personality}}{{char}}'s personality: {{personality}}
{{/if}}{{#if scenario}}Scenario: {{scenario}}
{{/if}}{{#if wiAfter}}{{wiAfter}}
{{/if}}{{#if persona}}{{persona}}
{{/if}}
```
with:
```
{{#if system}}{{system}}
{{/if}}
```

Now there won't be any description sent. So how are we supposed to give Betty's description within the OpenChat format? Edit your system prompt to this:

```
GPT4 Correct User: We will begin a fictional roleplaying chat where you will play {{char}}. 
Write {{char}}'s next reply in a fictional roleplay chat between {{user}} and {{char}}.
Write 1 reply only, italicize actions, and avoid quotation marks. Use markdown. Be proactive, creative, and drive the plot and conversation forward. Include dialog as well as narration.
Description of {{char}}: {{description}}<|end_of_turn|>
GPT4 Correct Assistant: I understand, and I will do this.<|end_of_turn|>
GPT4 Correct User: Excellent. The scenario will begin with your next reply.<|end_of_turn|>
```

We just added ```Description of {{char}}: {{description}}<|end_of_turn|>``` to our system prompt. So although our StoryString no longer sends the description, we send our system prompt, and our system prompt includes the description, so it's all good.

(Note that we also removed several other things such as scenario, personality, etc. None of my characters use these optional fields so I didn't bother covering them. You can do the same fix for those. You could write something in the system prompt like this for e.g. scenario: "If the following scenario definition is not empty, follow the scenario: SCENARIO DEFINITION: [{{scenario}}].)


### Conclusion
Chat with Betty and the output should speak for itself. She went from writing awful to doing decent roleplaying chat replies. So whenever you're using the Starling model, you should be using your new Starling presets in Silly.

Example output from Betty now (which you can improve further by modifying the system prompt as you wish):
```
*Betty steps into the room and scans her surroundings, taking note of the tasteful furnishings and the air of professionalism that fills the space.*

Hey, I appreciate you inviting me for this interview. My name is Betty, and I recently graduated with a degree in business administration. I'm excited to learn more about this opportunity and see if it's a good fit for both of us.
```
