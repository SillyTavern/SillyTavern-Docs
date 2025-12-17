---
order: tts-alltalk
route: /extensions/alltalk/
---
# AllTalk TTS V2

AllTalk is a voice cloning system based on Coqui XTTS, F5-TTS, VITS, Piper and other TTS model engines, designed to produce high-quality voice reproduction (either zero shot voice cloning or built-in voices). In AllTalk V2, significant updates enhance functionality and ease of use, including multiple TTS engine support, expanded customization, and performance optimizations. For a comprehensive list of features, refer to the [AllTalk Wiki here](https://github.com/erew123/alltalk_tts/wiki).

---

## 🟩 Key Features in AllTalk V2
- **Multi-engine Support**: Easily switch between Coqui XTTS, VITS, Piper, Parler, F5, and custom engines.
- **Voice Conversion (RVC)**: Enhanced retrieval-based voice cloning pipeline.
- **Customizable Settings**: Adjust per-engine settings and save startup configurations.
- **Narrator Functionality**: Specify separate voices for narration and characters.
- **Standalone and Integrated Use**: Seamless integration with SillyTavern.
- **DeepSpeed and Low VRAM Modes**: Performance optimization for resource-limited environments.
- **Screenshots**: See AllTalk V2’s interface [here](https://github.com/erew123/alltalk_tts/discussions/237).

---

## 🟨 Setup and Installation Options

AllTalk offers both standalone and integrated installation methods. The fastest setup involves using one of the quick installation options provided, with scripts automating most of the process.

- **Standalone Installation**: Recommended for most users ([Standalone Guide](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **Text-generation-webui Integration**: For integration into Text-generation-webui ([TGWUI Installation Guide](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 Automated Installation
**This method is for Windows users only.**
For new users who want a quick setup, the automated installation uses the SillyTavern-Launcher. 
Note: This assumes you have already installed the SillyTavern-Launcher. If you haven’t, visit https://github.com/SillyTavern/SillyTavern-Launcher and follow the instructions in the readme.md file to install it.
Once the SillyTavern-Launcher is installed:
1. Run Launcher.bat
2. Go to: `Home > Toolbox > App Installer > Voice Generation`
3. Select the option labeled: **Install AllTalk V2**

#### 🟩 Manual Installation
For advanced users requiring detailed control, follow the [Manual Installation Guide](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide) for a step-by-step setup on Windows, Linux, or Mac (untested).

#### 🟩 Google Colab Installation
Run AllTalk in a cloud environment with the [Google Colab Installation](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB) for users who prefer not to install locally.

---

## 🟨 Using AllTalk within SillyTavern

Once AllTalk is loaded, select it within SillyTavern on the TTS page, ensuring to select the correct AllTalk server version in the settings.

- **Settings Management**: AllTalk may enable or disable specific settings based on your selected configuration.
- **Loading Sequence**: If SillyTavern is loaded before AllTalk, reload the TTS extensions page.
- **Performance Optimization**: Enable DeepSpeed and Low VRAM modes selectively to improve performance based on system resources.
- **Narrator function**: Details of the Narrator function can be found on the [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function).

Full details of the SillyTavern AllTalk Extension will be updated on the [AllTalk Wiki page for SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)

TGWUI Users who user the AllTalk extension for TGWUI need to disable `Enable TGWUI TTS` in the TGWUI chat interface, otherwise you will have duplicate TTS audio generated.

---

## 🟨 Troubleshooting

If you experience issues that you believe are specific to AllTalk within SillyTavern, please refer to the [AllTalk Wiki page for SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension) for the lastest information.

---

### 🟪 Support, Assistance, and Feature Requests

For further assistance:
- Refer to the [Wiki](https://github.com/erew123/alltalk_tts/wiki) and built-in documentation.
- Join discussions on the [Discussion Board](https://github.com/erew123/alltalk_tts/discussions/245).
- Submit bugs or feature requests through the [Issue Tracker](https://github.com/erew123/alltalk_tts/issues).

---
