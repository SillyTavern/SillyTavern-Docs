# Realtime Voice Cloning (RVC)

This guide will walk you through using RVC, a technique that allows transferring voice features from one audio clip to another, enabling voices to speak in different tones and styles.

Ever enjoyed those famous "Presidents Play X" videos? They were created using RVC. With the RVC Extra module, you can make your SillyTavern characters speak in any voice you desire, be it anime, movie, or even your own unique voice.

RVC is NOT TTS: it's more like speech-to-speech. It takes an audio clip as its input. In the background, what RVC does is work in tandem with SillyTavern-extras's TTS module: it waits for TTS to generate an audio file (which TTS would've done regardless of whether you use RVC or not), then RVC will perform a second pass that takes the TTS audio file and transforms it into the cloned voice from your RVC configuration.

## RVC Setup

### Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Install the "RVC" extension from the "Download Extensions & Assets" menu in the Extensions panel (stacked blocks icon).
- Make sure you have `ffmpeg` binary in your PATH environment variable. This tool is used to convert incoming audio.
   - Use the Toolbox in Launcher.bat script to install ffmpeg automatically: https://github.com/SillyTavern/SillyTavern-Launcher/blob/main/Launcher.bat
   - Or download the build here: https://www.gyan.dev/ffmpeg/builds/ 
   - How to modify PATH variable: https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/
   - To test whether you did things correctly, open a command prompt and run ```ffmpeg```. It should print the ffmpeg version and info.

### Step-by-Step Setup

1. **Prepare RVC Model Files**:
   - In a file browser, Navigate to: `\SillyTavern-extras\data\models\rvc` 
   - Create a subfolder like 'Betty' and place the `.pth` and `.index` files into it. (Hint: you can download voice files from https://voice-models.com, make sure the voice name says it's RVPME.)

2. **Install Requirements**:
   - Install the necessary requirements using the command: `pip install -r requirements-rvc.txt`.

3. **Make sure TTS is enabled and works**
   - RVC depends on TTS, you need to enable a TTS module. Refer to our TTS guide for how. Your TTS has to be already working properly and narrating your chats before you try to add RVC to the mix!

4. **Run SillyTavern-extras with RVC enabled**:
   - Launch SillyTavern-extras with the RVC module enabled. This example invocation assumes you used Silero TTS which comes pre-installed with SillyTavern-extras:
     ```shell
     python server.py --enable-modules=rvc,silero-tts
     ```
     Optionally, you may wish to run RVC on your GPU if you have a capable one, by adding ```--cuda``` to the startup command. Based on a quick test, VRAM usage was 3.4GB for narrating 50 tokens (~36 words), and 7.6GB for 200 tokens (~150 words).

5. **Configure RVC in SillyTavern**:
   - In SillyTavern, navigate to **Extensions** > **RVC** and enable it.

6. **Set Up Voice Map**:
   - Create a Voice map for RVC. Set your Character to your desired SillyTavern character name, and set Voice to the RVC folder you created at step 1, then click Apply. If you did things correctly, the Voice Map will show something like 'Betty:MyVoice(rvpme)'.

7. **Select Pitch Extraction**:
   - Choose "rmvpe" as the pitch extraction method.
   - If you have trouble with "rmvpe" try other methods.

8. **(Optional) Configure RVC to save your generations to file**:
   - If for testing or troubleshooting purposes you wish to save the generated RVC audio, add ```--rvc-save-file``` to your startup command. This will save the last generation under SillyTavern-extrasdata/tmp/rvc_output.wav:
   ```shell
   python server.py --enable-modules=rvc,silero-tts --rvc-save-file
   ```

### Expression-Based Dynamic Voice

1. **Configure RVC Models**:
   - In your RVC model folder, have separate `.pth` and `.index` files for each classified expression (e.g., anger, fear, joy, love, sadness, surprise).

2. **Enable Modules**:
   - Enable both RVC and classify modules:
     ```shell
     python server.py --enable-modules=rvc,classify
     ```

3. **Use RVC Module**:
   - The remaining setup is similar to using the RVC module alone (as explained above).

## Train Your Own RVC Model

### Using RVC Easy Menu by Deffcolony (Windows Only)

Automatically install and launch Mangio-RVC: https://github.com/deffcolony/rvc-easy-menu

1. **Clone Repository**:
   - Clone the repository to your desired location:
     ```shell
     git clone https://github.com/deffcolony/rvc-easy-menu.git
     ```

2. **Launch RVC-Launcher.bat**:
   - Open the `RVC-Launcher.bat` file.
   - Choose option 1 to install RVC.

3. **Complete Installation**:
   - When prompted, install required packages and dependencies.

4. **Open WebUI for Voice Training**:
   - After installation, choose option 2 to open the WebUI for voice training.

### Mangio-RVC: Training a Voice Model

**Dataset Preparation**:

1. **Prepare Audio**:
   - Place the audio you want to train in the `datasets` folder.
   - Ensure the audio is free of background noise – only raw voice is needed.
   - Longer audio makes a better output quality.

**WebUI Training**:

1. **Access Training Tab**:
   - Click on the training tab in the WebUI.

2. **Configure Experiment**:
   - Enter an experiment name (e.g., `my-epic-voice-model`).
   - Set version to v2.

3. **Process Data and Extract Features**:
   - Click "Process data" and "Feature extraction".
   - Set "Save frequency" to 50.

4. **Training Parameters**:
   - Set "Total training epochs" to 300.
   - Click "Train feature index" and "Train model".
