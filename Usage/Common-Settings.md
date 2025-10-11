---
order: 160
icon: sliders
route: /usage/common-settings/
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

Temperature controls the randomness in token selection:

- Low temperature (<1.0) leads to more predictable text, favoring higher probability tokens
- High temperature (>1.0) increases creativity and diversity in the output by giving lower probability tokens a better chance.

Set to 1 for the original probabilities.

### Repetition Penalty

Attempts to curb repetition by penalizing tokens based on how often they occur in the context.

Set the value to 1 to disable its effect.

#### Repetition Penalty Range

How many tokens from the last generated token will be considered for the repetition penalty. This can break responses if set too high, as common words like "the, a, and," etc. will be penalized the most.

Set the value to 0 to disable its effect.

#### Repetition Penalty Slope

If both this and `Repetition Penalty Range` are above 0, the repetition penalty will have a greater effect at the end of the prompt. The higher the value, the stronger the effect.

Set the value to 0 to disable its effect.

### Top K

Top K sets a maximum amount of top tokens that can be chosen from. For example, if Top K is 20, this means only the 20 highest ranking tokens will be kept (regardless of their probabilities being diverse or limited).

Set to 0 (or -1, depending on your backend) to disable.

### Top P

Top P (a.k.a. nucleus sampling) adds up all the top tokens required to add up to the target percentage. If the Top 2 tokens are both 25%, and Top P is 0.50, only the Top 2 tokens are considered.

Set the value to 1 to disable its effect.

### Typical P

Typical P Sampling prioritizes tokens based on their deviation from the average entropy of the set. It maintains tokens whose cumulative probability is close to a predefined threshold (e.g., 0.5), emphasizing those with average information content.

Set the value to 1 to disable its effect.

### Min P

Limits the token pool by cutting off low-probability tokens relative to the top token. Produces more coherent responses but can also worsen repetition if set too high.

- Works best at low values such as `0.1-0.01`, but can be set higher with a high `Temperature`. For example: `Temperature: 5, Min P: 0.5`

Set the value to 0 to disable its effect.

### Top A

Top A sets a threshold for token selection based on the square of the highest token probability. For example, if the Top-A value is 0.2 and the top token's probability is 50%, tokens with probabilities below 5% (0.2 * 0.5^2) are excluded.

Set the value to 0 to disable its effect.

### Tail Free Sampling

Tail-Free Sampling (TFS) searches for a tail of low-probability tokens in the distribution, by analyzing the rate of change in token probabilities using derivatives. It retains tokens up to a threshold (e.g., 0.3) based on the normalized second derivative. The closer to 0, the more discarded tokens.

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

### Epsilon Cutoff

Epsilon cutoff sets a probability floor below which tokens are excluded from being sampled. In units of 1e-4; a reasonable value is 3.

Set to 0 to disable.

### Eta Cutoff

Eta cutoff is the main parameter of the special Eta Sampling technique. In units of 1e-4; a reasonable value is 3. See the paper [Truncation Sampling as Language Model Desmoothing by Hewitt et al. (2022)](https://arxiv.org/abs/2210.15191) for details.

Set to 0 to disable.

### DRY Repetition Penalty

DRY penalizes tokens that would extend the end of the input into a sequence that has previously occurred in the input. If you want to allow repeating certain sequences verbatim (e.g. names), you can add them to the sequence breakers list. See the Pull Request [here](https://github.com/oobabooga/text-generation-webui/pull/5677).

Set multiplier to 0 to disable.

### Exclude Top Choices (XTC)

XTC sampling algorithm removes the most likely tokens from consideration instead of pruning the least likely tokens It removes all except the least likely token meeting a given threshold, with a given probability. This ensures that at least one "viable" choice remains, retaining coherence. See the Pull Request [here](https://github.com/oobabooga/text-generation-webui/pull/6335).

Set probability to 0 to disable.

### Mirostat

Mirostat matches the output perplexity to that of the input, thus avoiding the repetition trap (where, as the autoregressive inference produces text, the perplexity of the output tends toward zero) and the confusion trap (where the perplexity diverges). For details, see the paper [Mirostat: A Neural Text Decoding Algorithm that Directly Controls Perplexity by Basu et al. (2020)](https://arxiv.org/abs/2007.14966).

Mode chooses the Mirostat version.

- 0 = disable,
- 1 = Mirostat 1.0 (llama.cpp only),
- 2 = Mirostat 2.0.

### Beam Search

A greedy, brute-force algorithm used in LLM sampling to find the most likely sequence of words or tokens. It expands multiple candidate sequences at once, maintaining a fixed number (beam width) of top sequences at each step.

### Top nsigma

A sampling method that filters logits based on their statistical properties. It keeps tokens within n standard deviations of the maximum logit value, providing a simpler alternative to top-p/top-k sampling while maintaining sampling stability across different temperatures.
