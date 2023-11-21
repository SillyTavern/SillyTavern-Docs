# XTTS with voice cloning

Greetings! So, you've been blown away by those Reddit posts showcasing how far the technology went for the AI text-to-speech?

Feeling excited to give your robotic waifu/husbando a new shiny voice modulator?

Fear not, this stunning groundbreaking technology is already available at your local SillyTavern, you just need a simple...

## Prerequisites

1. Latest `staging` branch of SillyTavern.
2. [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) installed.
3. (Windows) [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) installed.
4. WAV files with voice clips to clone from (~10 seconds per file).
5. Create a folder with "speakers" and "output" subfolders. Put WAV files into "speakers".

Example folder structure:
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## Installing 

[daswer123](https://github.com/daswer123) made an API server that runs the XTTSv2 model on your computer and connects to SillyTavern's TTS extension.

It's completely independent of Extras API and would use a separate environment.

**Very important:** Don't install the following requirements to your Extras environment or system Python.
It will break your other packages, do unnecessary downgrades, etc.

The following instruction is provided using Miniconda, but you can also do it with venv (not covered here).
Open the Anaconda command prompt and follow the instructions line by line.

### Getting the server up and running

1. Navigate to the folder you've created at step 4 of prerequisites.
```
cd C:\xtts
```
2. Create a new conda env. From now on, we'll call it `xtts`.
```
conda create -n xtts
```
3. Activate a newly created env.
```
conda activate xtts
```
4. Install Python 3.10 to your env. Confirm with "y" when prompted.
```
conda install python=3.10
```
5. Install the XTTS server with its requirements.
```
pip install xtts-api-server
```
6. Install PyTorch. This can take some time. The following line installs PyTorch with GPU acceleration support (CUDA).
If you want to use just the CPU inference, drop the last part that starts with `--index-url`.
```
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```
7. Start the XTTS server on the default host and port: <http://localhost:8020>
```
python -m xtts_api_server
```
8. During your first startup, the model will be downloaded (about ~2 GB).
Don't forget to read the legal notice from Coqui AI very carefully. Lol, I'm kidding, just hit "y" again.

### Connecting to SillyTavern

1. Open the extensions panel, expand the TTS menu, and pick "XTTSv2" in the provider list.
2. Choose your text-to-speech language in the Language dropdown (I'll be sad if it's not Polish).
3. Verify that the provider endpoint points to <http://localhost:8020> and "Available voices" shows a list of your voice samples.
4. Pick any character and set a mapping between the voice sample and the character.
If the characters list is empty, hit "Reload" a couple of times.
5. Configure the rest of the TTS settings according to your preferences.

### You're all set now!

Click on the bullhorn icon in the context actions menu for any message and hear the beautiful cloned voice emanating
from your speakers. The generation takes some time and it's not real-time even on high-end RTX GPUs.

### How to restart the TTS server?

Just do steps 1, 3 and 7 from the installation instruction.

### Android??

Unlikely, it can't run apps that require PyTorch without some arcane black magic that we don't provide support for. You can try it out at your own risk, but no support will be provided if you face any problems.

Your best solution is to host the TTS API on your PC over the local network, just don't forget to specify the host and port to listen on - see [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md).
