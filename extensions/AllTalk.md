---
order: TTS-AllTalk
---

# AllTalk TTS

AllTalk is voice cloning system based on the Coqui XTTS models, requiring only a good quality wav audio file to attempt to reproduce the original speaker. It also offers finetuning options to improve the reproduction quality of the voice where needed. 

To look over the full feature set, please visit this [link](https://github.com/erew123/alltalk_tts?#alltalk-tts) and [Screenshot here](https://github.com/erew123/screenshots/raw/main/sillytavern.jpg) 

Along with the standard SillyTavern text splitting options to choose what is or isnt spoken, AllTalk offers and additional option to use the AllTalk narrator, so that non-character text can be read out in an alternative voice [example](https://vocaroo.com/18nrv7FR6wuA)

Documentation and custom settings are built into AllTalk and available on a link within the SillyTaven TTS interface.

## 🟩 Setup

You have 2x current options for installation. If you are using Text-generation-webui as your LLM model loader, you can add AllTalk as an extension there. If you wish to use AllTalk as a standalone, you can do that too. For all the latest documentation, support and help please visit [here](https://github.com/erew123/alltalk_tts?#alltalk-tts). Below are two quick installation methods (with videos) however please look at the AllTalk website if you need manual installation methods.

Click to expand one of the below:

<details>
	<summary>QUICK SETUP - Text-Generation-webui</summary><br>

   **NOTE** You will need to **uncheck** "Enable TTS" within the **Text-generation-webui interface**, otherwise when you are using SillyTavern, AllTalk will dual generate TTS due to how Text-generation-webui sends messages. It is possible to set this as start-up setting within the AllTalk settings page.

 If you wish to see this as a video, please go [here](https://www.youtube.com/watch?v=icn2XS5rUH8)
1) To download the AllTalk setup you can either:
   - A) On the AllTalk [page](https://github.com/erew123/alltalk_tts) select **CODE** > **Download ZIP** then extract it to the text-generation-webui extensions folder<br>e.g. `\text-generation-webui\extensions\alltalk_tts\`<br><br>
   - B) Go to a terminal/console, move into the `\text-generation-webui\extensions\` folder<br>and `git clone https://github.com/erew123/alltalk_tts`<br><br>
2) In a terminal/command prompt, in the text-generation-webui folder you will start its Python environment with either `cmd_windows.bat` or `./cmd_linux.sh`
3) Move into the AllTalk folder e.g. `cd extensions` then `cd alltalk_tts`
4) Start the AllTalk setup script `atsetup.bat` or `./atsetup.sh`
5) Follow the on-screen prompts and install the correct requirements files that you need. It's recommended to test AllTalk works before installing DeepSpeed.
6) When the AllTalk server has started its default settings and documentation page will be on http://127.0.0.1:7851/

   Any time you need to make changes to AllTalk, or use Finetuning etc, always start the Text-generation-webui Python environment first.

   Please read the `🟩 Other installation notes` (also additional voices are available there).

   Finetuning & DeepSpeed have other installation requirements (depending on your OS) so please read any instructions in the setup utility and refer to the AllTalk Github page for detailed instructions.<br><br>
</details>

<details>
	<summary>QUICK SETUP - Standalone Installation</summary><br>

 If you wish to see this as a video, please go [here](https://www.youtube.com/watch?v=AQYCccDRbaY)
1) To download the AllTalk setup you can either:
   - A) On the AllTalk [page](https://github.com/erew123/alltalk_tts) select **CODE** > **Download ZIP** then extract it to the folder of your choice<br>e.g. `C:\myfiles\alltalk_tts\`<br><br>
   - B) Go to a terminal/console, move into the folder of your choice e.g `C:\myfiles\` folder<br>and `git clone https://github.com/erew123/alltalk_tts`<br><br>
2) In a terminal/command prompt, move into the AllTalk folder e.g. `cd alltalk_tts`
3) Start the AllTalk setup script `atsetup.bat` or `./atsetup.sh`
4) Follow the on-screen prompts and install the correct requirements files that you need. It's recommended to test AllTalk works before installing DeepSpeed.
5) When the AllTalk server has started its default settings and documentation page will be on http://127.0.0.1:7851/

   DeepSpeed on Windows machines will be installed as standard. Linux machines have other requirements which are detailed within the setup utility and on this page.

   Please read the `🟩 Other installation notes` (also additional voices are available there).

   Finetuning has other installation requirements so please read any instructions in the setup utility and refer to the AllTalk Github page for detailed instructions.<br><br>
</details>

#### 🟩 Other installation notes
On first startup, AllTalk will download the Coqui XTTSv2 2.0.2 model to its **models** folder (1.8GB space required). Check the command prompt/terminal window if you want to know what its doing. After it says "Model Loaded" AllTalk is usually available on its IP address a few seconds later, for you to connect to in your browser.

Once the extension is loaded, please find all documentation and settings on http://127.0.0.1:7851/

**Where to find voices** https://aiartes.com/voiceai or https://commons.wikimedia.org/ or interviews on youtube etc. Instructions on how to cut down and prepare a voice sample are within the built in documentation.

Please read the note about ensuring your character cards are set up correctly.

Some extra voices for AllTalk are downloadable [here](https://drive.google.com/file/d/1bYdZdr3L69kmzUN3vSiqZmLRD7-A3M47/view?usp=drive_link)

#### 🟩 A note on Character Cards & Greeting Messages
Messages intended for the Narrator should be enclosed in asterisks `*` and those for the character inside quotation marks `"`. However, AI systems often deviate from these rules, resulting in text that is neither in quotes nor asterisks. Sometimes, text may appear with only a single asterisk, and AI models may vary their formatting mid-conversation. For example, they might use asterisks initially and then switch to unmarked text. A properly formatted line should look like this:

`"`Hey! I'm so excited to finally meet you. I've heard so many great things about you and I'm eager to pick your brain about computers.`"` `*`She walked across the room and picked up her cup of coffee`*`

Most narrator/character systems switch voices upon encountering an asterisk or quotation marks, which is somewhat effective. AllTalk has undergone several revisions in its sentence splitting and identification methods. While some irregularities and AI deviations in message formatting are inevitable, any line beginning or ending with an asterisk should now be recognized as Narrator dialogue. Lines enclosed in double quotes are identified as Character dialogue. For any other text, you can choose how AllTalk handles it: whether it should be interpreted as Character or Narrator dialogue (most AI systems tend to lean more towards one format when generating text not enclosed in quotes or asterisks).

You can monitor what AllTalk identifies as Narrator lines on the terminal/command prompt and adjust its behavior if needed (Text Not Inside - Function).

## 🟨 Using AllTalk within SillyTavern & understanding the settings

When you have AllTalk loaded, within SillyTavern on the TTS page, select AllTalk and at the bottom is a link "AllTalk Config & Docs". Click that link and it will take you to your AllTalk server where you can find explanations of the settings.

Some settings cannot be used at the same time, so AllTalk will enable/disable certain settings as needed.

If SillyTavern is loaded before AllTalk is available, you may need to click the Reload button on SillyTavern's TTS extensions page.

A few quick tip on using AllTalk within SillyTavern:

- Only change DeepSpeed, Low VRAM or Model one at a time. Wait for it to say Ready before changing something else.
- You can permanently change the AllTalk startup settings or DeepSpeed, Low VRAM and Model in the AllTalk settings page.
- Different AI models use quotes and asterisks differently, so when using the AllTalk narrator, you may need to change "Text not inside" depending on model.
- Add new voice samples to the voices folder. You can Finetune a model to make it sound even closer to the original sample.
- DeepSpeed will improve processing time to TTS to be 2-3x faster (Installed as default on Stanalone installations, but needs activating)
- Low VRAM can be very beneficial if you don't have much memory left after loading your LLM.

## 🟨 Support, Help, Assistance & other questions

Please visit [here](https://github.com/erew123/alltalk_tts?#alltalk-tts)
