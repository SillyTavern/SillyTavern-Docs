# TabbyAPI
A FastAPI based application that allows for generating text using an LLM using the Exllamav2 backend, with support for Exl2, GPTQ, and FP16 models.

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### Quickstart
1. Follow the [installation instructions](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started) on the official TabbyAPI GitHub.
2. [Create your config.yml](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options) to set your model path, default model, sequence length, etc. You can ignore most (if not all) of these settings if you want.
3. Launch TabbyAPI. If it worked, you should see something like this:

    ![TabbyAPI terminal](/static/tabby-terminal.png)

4. Under the Text Completion API in SillyTavern, select TabbyAPI.
5. Copy your API key from the TabbyAPI terminal into `Tabby API key` and make sure your `API URL` is correct (it should be `http://127.0.0.1:5000` by default).

If you did everything correctly, you should see something like this in SillyTavern:

![TabbyAPI SillyTavern](/static/tabby-config.png)

You can now chat using TabbyAPI!

### TabbyAPI Loader
The developers of TabbyAPI created an official extension to load/unload models directly from SillyTavern. Installation is simple:
1. In SillyTavern, click on the Extensions tab and navigate to Download Extensions & Assets.
2. Copy `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` into Assets URL and click the plug button to the right.
3. You should see something like this. Click the download button next to Tabby Loader.

    ![Tabby Loader](/static/tabby-assets.png)

4. If the installation was successful, you should see a green pop-up message at the top of your screen. Under the extensions tab, navigate to TabbyAPI Loader and copy your admin key from the TabbyAPI terminal into Admin Key.
5. Click the refresh button next to Model Select. When you click on the textbox just below it, you should see all of the models in your model directory.

![Tabby Loader Extension](/static/tabby-loader.png)

You can now load and unload your models directly from SillyTavern!

### Support
Still need help? Visit the [TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI) for a link to the developer's official Discord server and [read the wiki](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started).
