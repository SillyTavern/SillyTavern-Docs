# NovelAI

### API Key

To get a NovelAI API key, follow these instructions:

1. Go to the NovelAI website and Login.
2. Create a new story, or open an existing story.
3. Open the Network Tools on your web browser. (For Chrome or Firefox, you do this by pressing Ctrl+Shift+I, then switching to the Network tab.)
4. Generate something. You should see two requests to [api.novelai.net/ai/generate-stream](http://api.novelai.net/ai/generate-stream), which might look something like this:

![](/static/nai-stream.png)

5. Select the second request, then in the Headers tab of the inspection panel, scroll down to the very bottom. Look for a header called Authorization:

![](/static/nai-key.png)

The long string (after "Bearer", not including it) is your API key.

* Proxies and Cloudflare-type services may interfere with connection.

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

If your subscription tier is Paper, Tablet or Scroll use only Euterpe model otherwise you can not get an answer from NovelAI API.
