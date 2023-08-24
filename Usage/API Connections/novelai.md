# NovelAI

### API Key

To get your NovelAI API key, follow these steps:

1. Select the gear icon at the top of the left sidebar.
 ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/55552572/e0c70838-9775-4dc4-bf07-3daf895de67c)

2. Select "Account" under "User Settings".
![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/55552572/71af84bf-3800-4e22-bfe9-9f84f302451a)

3. Select "Get Persistent API Token".
![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/55552572/5ca0ff03-a75c-4bea-ba7f-2db951aab194)

4. Select the copy icon to copy your NovelAI API token to the clipboard. 
![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/55552572/2765446e-42b2-4027-8ee5-0bbb48aef9c0)

### Models

You should use Kayra. 

Clio is not a bad model, but not as powerful as Kayra. Clio's speed advantage is insignificant. On NovelAI's tablet and scroll tiers, Clio does have a larger context size than Kayra, but trading that off against better coherence/prose quality from Kayra is unlikely to be worth it.

Krake and Euterpe aren't recommended - NovelAI even refers to them as legacy models. 

### Settings

The files with the settings are here (SillyTavern\public\NovelAI Settings).
You can also manually add your own settings files.

#### Response Length

How much text you want to generate per message. Note that NovelAI has a limit of 150 tokens per response. 

#### Context Size

How many tokens of the chat are kept in the context at any given time. How large the maximum context size you can use depends on the model and your subscription tier:
- Kayra (tablet) - 3072 tokens
- Kayra (scroll) - 6144 tokens
- Kayra (opus) and Clio (all tiers) - 8192 tokens

#### Temperature

Value from 0.1 to 2.0

Lower value - the answers are more logical, but less creative.

Higher value - the answers are more creative, but less logical.

#### Repetition penalty

Repetition penalty is responsible for the penalty of repeated words.
If the character is fixated on something or repeats the same phrase, then increasing this parameter will fix it.
It is not recommended to increase this parameter too much for the chat format, as it may break this format.

**The standard value for chat is approximately 1.0 - 1.05**

#### Repetition penalty range

The range of influence of Repetition penalty in tokens.

#### Preamble

Text that is inserted right above the chat to modify the writing style. The recommended format is a list of short tags, like "[ Style: chat, detailed, sensory ]". 

#### Top P

Limits the token pool to however many tokens it takes for their probabilities to add up to p. A lower number is more consistent, but less creative. 

#### Top K

Limits the token pool to the k most likely tokens. A lower number is more consistent, but less creative. 
