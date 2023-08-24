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

* Lower value - the answers are more logical, but less creative.
* Higher value - the answers are more creative, but less logical.

#### Repetition penalty

Higher values make the output less repetitive.
If the character is fixated on something or repeats the same phrase, then increasing this parameter will (likely) fix it.
It is not recommended to increase this parameter too much as it may break the outputs.

#### Repetition penalty range

How many tokens from the last generated token will be considered repeated if they appear in the output.

### Models

Use Kayra.

Clio is not a bad model, but not as powerful as Kayra and Clio's speed advantage over Kayra is insignificant. On tablet and scroll tier, Clio has a larger context size than Kayra, but trading that off against better coherence/prose quality from Kayra is rarely worth it. 

Euterpe and Krake are available, but NovelAI considers them legacy models and doesn't recommend their use anymore. 
