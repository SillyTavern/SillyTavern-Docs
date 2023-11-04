---
order: 20
icon: sliders
---

# Common Settings

These settings control the sampling process when generating text using a language model. The meaning of these settings is universal for all the supported backends.

## Context Settings

### Context (tokens)

Controls how much will the language model remember. Every model has its context size limit.

Context comprises character information, system prompts, chat history, etc.
To see a composition of the context after generating the message, click on the "Prompt Itemization" message option (expand the "..." menu and click on the lined square icon).

A dotted line (orange color in the default UI theme) that appears in the chat denotes the last chat message being sent to the API.
The things outside of the context range don't exist for the model and are not considered during the generation.

### Response (tokens)

The maximum amount of tokens that the API will generate to respond. The larger the parameter value, the longer the generation time takes. 

If supported by the API, you can also enable Streaming mode to display the response bit by bit as it is generated.
When Streaming is off, responses will be displayed all at once when they are complete.

## Sampler Parameters

### Temperature

Controls the randomness of the generated text. 

* Lower value - the answers are more logical but less creative.
* Higher value - the answers are more creative but less logical.

### Repetition Penalty

Higher values make the output less repetitive.
If the character is fixated on something or repeats the same phrase, then increasing this parameter can help fix it.
It is not recommended to increase this parameter too much as it may break the outputs.

### Repetition Penalty Range

How many tokens from the last generated token will be considered repeated if they appear in the output.

#### Repetition Penalty Slope

If both this and the Repetition Penalty Range are above 0, then the repetition penalty will have more effect closer to the end of the prompt. The higher the value, the stronger the effect.

Set the value to 1 for linear interpolation or 0 to disable interpolation.

### Top P

Limits the token pool to however many tokens it takes for their probabilities to add up to P. A lower number is more consistent but less creative.

Set the value to 1 to disable its effect.

### Top K

Limits the token pool to the K most likely tokens. A lower number is more consistent but less creative.

Set the value to 0 to disable its effect.

### Top A

This setting allows for a more flexible version of sampling, where the number of words chosen from the most likely options is automatically determined based on the likelihood distribution of the options, but instead of choosing the top P or K words, it chooses all words with probabilities above a certain threshold.

Set the value to 0 to disable its effect.

### Typical Sampling

This setting selects words randomly from the list of possible words, with each word having an equal chance of being selected. This method can produce text that is more diverse but may also be less coherent.

Set the value to 1 to disable its effect.

### Tail Free Sampling

This setting removes the least probable words from consideration during text generation, which can improve the quality and coherence of the generated text.

Set the value to 1 to disable its effect.
