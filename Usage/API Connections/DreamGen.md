# DreamGen

DreamGen is an app and an API for AI-powered role-playing and story-writing. They have a free tier, as well as a paid subscription that allows unlimited monthly access to their high-quality in-house text generation models made specifically for the purpose of steerable AI role-playing and story-writing. Create an account to get started: <https://dreamgen.com/>.

The (free) credits reset at the start of each calendar month. See [pricing](https://dreamgen.com/pricing) to see the credit cost for each model and [usage](https://dreamgen.com/account/usage) to see your remaining credits.

## Connecting to DreamGen

### Get API Key

Go to the [DreamGen API keys](https://dreamgen.com/account/api-keys) page and click the "New API Key" button. Make sure the API Key is copied into your clipboard.

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### Connect

1. Go to the SillyTavern connection settings.
2. Select API: Text Completion
3. Select API Type: DreamGen
4. Enter the API key
5. (optional) Pick a model

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.jpg)

## Models

DreamGen offers `opus-v1-sm`, `opus-v1-lg`, and `opus-v1-xl`. The larger the model, the better it will be at following instructions and writing good role-play and stories.

## Formatting Settings

The DreamGen models expect a specific input format, which is [documented here](https://dreamgen.com/docs/models/opus/v1).

SillyTavern comes with built-in presets made for DreamGen. Make sure to use these settings as your baseline.
These settings try to stick to the DreamGen format as closely as possible but due to the irregular formatting of character cards, it is not always perfect.

1. Go to the "Advanced Formatting" page.
2. Under "Context Template" pick `DreamGen Role-Play V1`.
3. **Enable "Instruct Mode".**
4. Under "Instruct Mode Presets" pick `DreamGen Role-Play V1`.

![DreamGen context settings](/static/dreamgen/dreamgen_st_context_settings.jpg)
![DreamGen instruct settings](/static/dreamgen/dreamgen_st_instruct_settings.jpg)

## Completion Settings

DreamGen supports:

-   "Temperature", "Top P", "Top K" and "Min P"
-   "Presence Penalty", "Frequency Penalty" and "Repetition Penalty" (without range)
-   "Min Length" -- lets you force the model to generate at least `min(min_length, max_tokens)` tokens

Good starting values might be:

-   Min P: 0.05
-   Temperature: 0.8
-   Repetition Penalty: 1.1

## Tips for Formatting

The DreamGen models differ from the regular instruction-following models like OpenAI's ChatGPT.

The models were fine-tuned for the task of writing a role-play or a story based on the provided description which typically consists of plot description, style description, characters, locations, lore, etc. The models can also be *steered* in the middle of the role-play, making you the director, telling the characters what they should do or how the plot should unfold.

A well-formatted **system prompt** message would look like this:

```
You are an intelligent, skilled, versatile writer.

Your task is to write a story based on the information below.


## Plot description:

The librarian sets up a blind date between Lucifer and Mia. Lucifer immediately falls in love with Mia, but Mia needs more space and time to make up her mind.


## Style description:

The narrative is vivid and intensely sensual, with a strong emphasis on raw emotion conveyed from a first-person point of view. The language is explicit, evoking intense imagery and indulging in the erotic exploration of the characters' passionate encounters.


## Characters

### Lucifer

Lucifer, the red-skinned, horned demon, is the embodiment of fallen grace. Wrestling with his notorious heritage and a newfound desire for love, his complex nature ferments with vulnerability. His character oscillates between hedonism and self-reflection, hungering for acceptance by Mia and the librarian. Embracing his mortal love, he yearns for transformation, embodying the notion that even the damned may seek solace in love's redemption.

### Mia

Mia is a kind woman...
```

Note that the **prompt should be a description of the story**, rather than instructions or directives on how the story should be written. **Avoid using phrases like:**

-   "Write the story as if..."
-   "Make sure to..."
-   etc.

See more [examples](https://dreamgen.com/docs/models/opus/v1#task-story-writing) of what the plot, style and character descriptions should look like.

The default "DreamGen Role-Play V1" template substitutes the different sections as follows:

-   `## Plot description:` will consist of {%{`{{scenario}}`}%} and {%{`{{wiBefore}}`}%}.
-   `## Style description:` is not provided, you should either add it to the system prompt under Advanced Settings, or to the character cards, at the end of {%{`{{scenario}}`}%}. This section is useful to influence the narrative style (first, second, third person), the tense (past, present), the level of detail and verbosity, etc.
-   `## Characters:` will have a {%{`{{char}}`}%} character with description consisting of {%{`{{description}}`}%} and {%{`{{personality}}`}%} and a {%{`{{user}}`}%} character with description consisting of {%{`{{persona}}`}%}.

### Message Examples and Initial Message

The DreamGen models are very responsive to the context -- they will largely stick to the writing style (and facts) presented in the previous conversation turns.
This makes the message examples and the initial message very important.

#### Formatting Message Examples

The {%{`{{mesExamples}}`}%} are appended at the end of the system prompt, allowing us to leverage the ChatML+Text format. Here's how the examples should be formatted:

{%{

```
<|im_end|>
<|im_start|>text names= {{user}}
(user's turn)<|im_end|>
<|im_start|>text names= {{char}}
(char's turn)<|im_end|>
<|im_start|>text names= {{user}}
(user's turn)<|im_end|>
<|im_start|>text names= {{char}}
(char's turn)
```

}%}

Note that we have `<|im_end|>` at the start, to close the system prompt, and that we do not have `<|im_end|>` at the end, as that will be added by SillyTavern.

You can also use `user` turns to provide context for the example conversations:

{%{

```
<|im_end|>
<|im_start|>user
Before the main story starts, {{char}} describes their body to {{user}}.<|im_end|>
<|im_start|>text names= {{user}}
(user's turn)<|im_end|>
<|im_start|>text names= {{char}}
(char's turn)<|im_end|>
<|im_start|>user
{{user}} and {{char}} meet up for coffee.
```

}%}

### Examples

Here are a couple of example cards, adapted for DreamGen, that take into account the unique prompting. These cards also leverage the {%{`{{mesExamples}}`}%} as described above.

#### Seraphina

This is an edit of the popular Seraphina card that's built into SillyTavern by default.

<div style="width:200px;">

[![Seraphina](/static/dreamgen/cards/Seraphina.png)](/static/dreamgen/cards/Seraphina.png)

</div>

#### Lara Lightland

This is an edit of the Lara Lightland card by Deffcolony.

<div style="width:200px;">

[![Lara Lightland](/static/dreamgen/cards/LaraLightland.png)](/static/dreamgen/cards/LaraLightland.png)

</div>

## FAQ

### How can I make the responses longer or shorter? 

You have several options:

-   Change or add the `## Style description:` in the system prompt or model card. You can try adding something like "Sentences are generally long, and the narrative describes the setting in painstaking detail."
-   Change the `Min Length` in the Completion Settings.
-   Add `Last Output Sequence` similar to the following in the Advanced Formatting settings under Instruct Mode:

Here's an example of the `Last Output Sequence` that might help make the model respond in a more verbose way:

{%{

```
<|im_end|>
<|im_start|>user
Length: 400 words
Plot: {{char}} replies to {{user}} in detailed and elaborate way.<|im_end|>
<|im_start|>text names= {{char}}
```

}%}

You can change the text within to something more suitable for your scenario or context.

### How can I stop the model from repeating itself? 

You can try increasing "Repetition Penalty" in the Completion Settings or rephrasing the part of the context that's getting repeated.

### How can I steer the role-play?

If you want to direct the characters to do something, or to steer the plot in certain direction, you can use the `user` role (that is the `<|im_start|>user` preamble).

At this point, this functionality is not neatly integrated into SillyTavern natively, but you can use the `Last Output Sequence` as described above to insert the `user` (instruction) turn. See [examples](https://dreamgen.com/docs/models/opus/v1#prompt-instructions) of what the instructions should look here.
