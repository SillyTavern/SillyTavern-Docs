# KoboldAI & Horde

## Kobold Horde

Horde is a distributed GPU cluster run entirely by volunteers. Your inputs are always anonymous, and prompts are not visible to the workers by default.

However, malicious agents could modify the open-source bridging software to log your activity or produce bad responses. So, when using Horde, avoid sending any personal information such as names, email addresses, etc.

If you encounter any abnormal activity, switch on the "Trusted Workers Only" checkbox and report it to the [KoboldAI Discord](https://koboldai.org/discord).

## KoboldAI

### Basic Settings

Standard KoboldAI settings files are used here. To add your own settings, simply add the file .settings in `SillyTavern\public\KoboldAI Settings`

#### Temperature

Value from 0.1 to 2.0. Lower value - the answers are more logical, but less creative. Higher value - the answers are more creative, but less logical.

#### Repetition penalty

Repetition penalty is responsible for the penalty of repeated words. If the character is fixated on something or repeats the same phrase, then increasing this parameter will fix it. It is not recommended to increase this parameter too much for the chat format, as it may break this format. The standard value for chat is approximately 1.0 - 1.05.

#### Repetition penalty range

The range of influence of Repetition penalty in tokens.

#### Amount generation

The maximum amount of tokens that the AI will generate to respond. One word is approximately 3-4 tokens. The larger the parameter value, the longer the generation time takes.

#### Context size

How much will the AI remember. Context size also affects the speed of generation.

*Important*: The setting of Context Size in SillyTavern GUI overrides the setting for KoboldAI GUI

### Advanced Settings

The settings provided in this section offer a more detailed level of control over the text generation process. It is important to be careful when making changes to these settings without proper consideration, as doing so may result in degraded quality of responses.

#### Single-line mode

In single-line mode the AI generates only one line per request. This allows for quicker generation of shorter prompts, but it does not produce responses that consist of more than one line.

#### Top P Sampling

This setting controls how much of the text generated is based on the most likely options. Only words with the highest probabilities, together summing up to P, are considered. A word is then chosen at random, with a higher chance of selecting words with higher probabilities.

Set value to 1 to disable its effect.

#### Top K Sampling

This setting limits the number of words to choose from to the top K most likely options. Can be used together with Top P sampling.

Set value to 0 to disable its effect.

#### Top A Sampling

This setting allows for a more flexible version of sampling, where the number of words chosen from the most likely options is automatically determined based on the likelihood distribution of the options, but instead of choosing the top P or K words, it chooses all words with probabilities above a certain threshold.

Set value to 0 to disable its effect.

#### Typical Sampling

This setting selects words randomly from the list of possible words, with each word having an equal chance of being selected. This method can produce text that is more diverse but may also be less coherent.

Set value to 1 to disable its effect.

#### Tail Free Sampling

This setting removes the least probable words from consideration during text generation, which can improve the quality and coherence of the generated text.

Set value to 1 to disable its effect.

#### Repetition Penalty Slope

If both this and Repetition Penalty Range are above 0, then repetition penalty will have more effect closer to the end of the prompt. The higher the value, the stronger the effect.

Set value to 1 for linear interpolation or 0 to disable interpolation.

### Soft Prompts

**Soft Prompts allow you to customize the style and behavior of your AI.**

They are created by training the AI with a special type of prompt using a collection of input data. Experimenting with different soft prompts can lead to exciting and unique results. The most successful soft prompts are those that align the AI's output with a literary genre, fictional universe, or the style of a particular author.

#### Common Misconceptions

* Soft prompts do not provide new information to the model, but can effectively influence the model's tone, word choice, and formatting.
* Soft prompts are not a means of compressing a full prompt into a limited token space. Instead, they provide a way to guide the language model's output through data in the context.
