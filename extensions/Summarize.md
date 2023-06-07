# Summarize

### What is it?

After the AI sends back a message, the Summarize extension looks through the chat history and then uses an AI summarization model that runs on the Extras host machine to create a dynamic summary of events. This summary is then sent along with every user input afterwards.

### How is that useful?

Summarize helps the AI remain aware of the general developments of a long chat.

***

### Setup Instructions

*Summarize **requires** the Extras server to run. It has no offline mode.*

1. Install or Update [Extras](https://github.com/SillyTavern/SillyTavern-extras) to the latest version.
2. Run Extras with the `summarize` module enabled: `python server.py --enable-modules=summarize`

***

### Configuration

Once Summarize is enabled, it will show up in ST's Extensions panel list.
![STExtensionMenuIcon](https://github.com/SillyTavern/SillyTavern/assets/124905043/4545037e-dff8-4373-9513-cddb69780be1)

![Summarize Config Panel](https://files.catbox.moe/s774f8.png)

- **Summary contents box** - this displays the current summary. Summary is updated and embedded into the chat file's metadata for every message recieved from the AI.
- **Restore previous state** - Replaces the current summaryt with the summary from the previous message. This is useful if the summarizer does a poor job at any given point.
- **Stop summarization updates checkbox** - check this to prevent the summary from being automatically updated. This is useful if you want to provide a custom summary of  your own, or to effectively disable the summary by clearing the box and stopping udpates.
- **Buffer length** - This is the number of tokens the model should consider as 'recent events'. Each time the Summarize model begins to create a new summary, this buffer is filled first and given priority.
- **Summary length** - The total length of the finished summary (what you see in the box).
- **Temperature** - determines how creative the summary model is in writing the summary. higher values will produce more variation in the summary, at potential risk of inaccuracy.
- **Repetition penalty** - high numbers here will help reduce the amount of repetitious phrases in the summary.
- **Length penalty** - higher numbers here will prevent the summary from growing too long. helps encourage the summary model to use shorter phrases.

### Changing Summary Model

By default, Summarize uses the [Qiliang/bart-large-cnn-samsum-ChatGPT_v3](https://huggingface.co/Qiliang/bart-large-cnn-samsum-ChatGPT_v3) model for summarization purposes.

This can be changed by using the command line argument `--summarization-model=(###Hugging-Face-Model-URL-Here###)`

A known alternate Summarize model is `Qiliang/bart-large-cnn-samsum-ElectrifAi_v10`.

***

### Useage Notes

1. Sumamry does not begin working until the chat has continued long enough that all chat messages are no longer able to fit into the context window.
2. Summaries are saved individually as metadata for each message in the chat file.
3. Deleting a message from the chat will effectively remove its summary as well.
