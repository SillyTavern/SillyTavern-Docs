# RVC

RVC stands for Realtime Voice Cloning. This technique allows transferring voice features from one audio clip to another, essentially making it speak in a different voice.

Have you ever seen popular "Presidents Play X" videos? Yes, that was RVC too. You can make your SillyTavern characters speak any voice (anime/movie/your own) using the RVC Extra module.

## RVC Setup

PREREQUISITES:

- `sillytavern-extras`: Switch to the `neo` branch
- `sillytavern`: Switch to the `staging` branch

1. In a file browser navigate to **\SillyTavern-extras\data\models\rvc**, create a subfolder and put **.pth** and **.index** into the created folder
2. Install requirements with: `pip install -r requirements-rvc.txt`
3. Run SillyTavern-extras with an RVC module enabled (add other models and parameters if needed):
```yaml
python server.py --enable-modules=rvc
```
4. In SillyTavern, go to **Extensions --> RVC** and enable it
5. Setup a Voice map for RVC: read the instructions for TTS above, but instead of TTS voices, use RVC model folder names
6. Select pitch extraction: **rmvpe**
7. Go to **Extensions --> TTS** and enable it
8. Select TTS Provider: **Coqui** or your other preferred provider, but not **System** - RVC doesn't work with it. **ElevenLabs** is not recommended since it has its own voice cloning capabilities
9. Enable **Auto Generation**

## Train your own RVC model

### RVC Easy Menu by Deffcolony (only for Windows).

Automatically install and launch Mangio-RVC: https://github.com/deffcolony/rvc-easy-menu

1. Choose a location where you want to git clone the repo because RVC will install in the same location where you launch the script:
```
git clone https://github.com/deffcolony/rvc-easy-menu.git
```
3. Open RVC-Launcher.bat
4. Choose 1 since you want to install RVC.
5. When 7-zip pops up just, click install because it's a requirement for the 7z package that will get extracted automatically.
6. After installation when the menu returns choose 2 to open WebUI for Voice Training.

### Mangio-RVC - Train a voice model

Dataset preparation:

1. Put the voice you want to train in the `datasets` folder.
2. Make sure there is NO BACKGROUND NOISE in the audio file; only raw voice!
3. The output quality will be better the longer the audio.

In the WebUI:

1. Click on the training tab
2. Enter the experiment name, for example, `my-epic-voice-model`
3. Set version to v2
4. Click on "Process data"
5. Click on "Feature extraction"
6. Set "Save frequency" to 50
7. Set "Total training epochs" to 300
8. Click on "Train feature index"
9. Click on "Train model"
