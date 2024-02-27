---
order: 20
icon: sliders
---

# Common Settings
These settings control the sampling process when generating text using a language model. The meaning of these settings is universal for all the supported backends.

## Context Settings

### Response (tokens)
The maximum number of tokens that the API will generate to respond.
- The higher the response length, the longer it will take to generate the response.
- If supported by the API, you can enable `Streaming` to display the response bit by bit as it is being generated.
- When `Streaming` is off, responses will be displayed all at once when they are complete.

### Context (tokens)
The maximum number of tokens that SillyTavern will send to the API as the prompt, minus the response length.
- Context comprises character information, system prompts, chat history, etc.
- A dotted line between messages denotes the context range for the chat. Messages above that line are not sent to the AI.
- To see a composition of the context after generating the message, click on the `Prompt Itemization` message option (expand the `...` menu and click on the lined square icon).

## Sampler Parameters

### Temperature
Controls the randomness of the generated text. 
- Lower value: responses are more logical but less creative.
- Higher value: responses are more creative but less logical.

### Repetition Penalty
Attempts to curb repetition by penalizing tokens based on how often they occur in the context.
- Sometimes, if the character is fixated on something or repeats the same phrase, increasing this parameter can help.
- This parameter will break responses if set too high. It's best not to go above `1.15` unless you know what you're doing.

Set the value to 1 to disable its effect.

#### Repetition Penalty Range
How many tokens from the last generated token will be considered for the repetition penalty. This can break responses if set too high, as common words like "the, a, and," etc. will be penalized the most.

Set the value to 0 to disable its effect.

#### Repetition Penalty Slope
If both this and `Repetition Penalty Range` are above 0, the repetition penalty will have a greater effect at the end of the prompt. The higher the value, the stronger the effect.

Set the value to 0 to disable its effect.

### Top K
Limits the token pool to the K most likely tokens. A lower number is more consistent but less creative.

Set the value to 0 to disable its effect.

### Top P
Limits the token pool to however many tokens it takes for their probabilities to add up to P. A lower number is more consistent but less creative.

Set the value to 1 to disable its effect.

### Typical P
Selects tokens randomly from the list of possible tokens, with each token having an equal chance of being selected. Produces responses that are more diverse but may also be less coherent.

Set the value to 1 to disable its effect.

### Min P
Limits the token pool by cutting off low-probability tokens relative to the top token. Produces more coherent responses but can also worsen repetition if set too high.
- Works best at low values such as `0.1-0.01`, but can be set higher with a high `Temperature`. For example: `Temperature: 5, Min P: 0.5`

Set the value to 0 to disable its effect.

### Top A
The number of tokens chosen from the most likely options is automatically determined based on the likelihood distribution of the options, but instead of choosing the `Top P` or `Top K` tokens, it chooses all tokens with probabilities above a certain threshold.

Set the value to 0 to disable its effect.

### Tail Free Sampling
This setting removes the least probable tokens from consideration during text generation, which can improve the quality and coherence of the generated text.

Set the value to 1 to disable its effect.

### Smoothing Factor
Increases the likelihood of high-probability tokens while decreasing the likelihood of low-probability tokens using a quadratic transformation. Aims to produce more creative responses regardless of `Temperature`.
- Works best without truncation samplers such as `Top K`, `Top P`, `Min P`, etc.

Set the value to 0 to disable its effect.

### Dynamic Temperature
Scales temperature dynamically based on the likelihood of the top token. Aims to produce more creative outputs without sacrificing coherency.
- Accepts a temperature range from minimum to maximum. For example: `Minimum Temp: 0.75` and `Minimum Temp: 1.25`
- `Exponent` applies an exponential curve based on the top token.

Untick to disable its effect.