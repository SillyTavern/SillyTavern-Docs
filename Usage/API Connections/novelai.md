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

### Models

Select whichever model you like, but you will only be able to get responses from models available to your subscription tier.
