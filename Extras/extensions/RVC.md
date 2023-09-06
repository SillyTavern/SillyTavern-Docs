# Realtime Voice Cloning (RVC) Tutorial

This guide will walk you through using RVC, a technique that allows transferring voice features from one audio clip to another, enabling voices to speak in different tones and styles.

Ever enjoyed those famous "Presidents Play X" videos? They were created using RVC. With the RVC Extra module, you can make your SillyTavern characters speak in any voice you desire, be it anime, movie, or even your own unique voice.

## RVC Setup

### Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Make sure you have `ffmpeg` binary in your PATH environment variable. This tool is used to convert incoming audio.
   - Use the Launcher.bat to install ffmpeg automatically: https://github.com/SillyTavern/SillyTavern-Launcher/blob/main/Launcher.bat
   - Or download the build here: https://www.gyan.dev/ffmpeg/builds/ 
   - How to modify PATH variable: https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/

### Step-by-Step Setup

1. **Prepare RVC Model Files**:
   - In a file browser, Navigate to: `\SillyTavern-extras\data\models\rvc` 
   - Create a subfolder and place the `.pth` and `.index` files into it.

2. **Install Requirements**:
   - Install the necessary requirements using the command: `pip install -r requirements-rvc.txt`.

3. **Run SillyTavern-Extras**:
   - Launch SillyTavern-extras with the RVC module enabled:
     ```shell
     python server.py --enable-modules=rvc
     ```

4. **Configure RVC in SillyTavern**:
   - In SillyTavern, navigate to **Extensions** > **RVC** and enable it.

5. **Set Up Voice Map**:
   - Create a Voice map for RVC. Follow the TTS instructions above, but use RVC model folder names.

6. **Select Pitch Extraction**:
   - Choose "rmvpe" as the pitch extraction method.
   - If you have trouble with "rmvpe" try other methods.

7. **Enable TTS**:
   - Navigate to **Extensions** > **TTS** and enable it.
   - Choose a TTS Provider (e.g., **Coqui**) – not **System** as RVC doesn't work with it. Avoid using **ElevenLabs** with RVC.

8. **Enable Auto Generation**:
   - Enable **Auto Generation** in TTS settings.

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
