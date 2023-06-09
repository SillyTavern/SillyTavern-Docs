---
icon: gear
label: Local Installation
---

# Extras Installation

This page contains instructions for installing SillyTavern Extras on your local device.

---
Local installation of Extras can be difficult or impossible on your OS (especially Termux).

#### Use the [Official Extras Colab](https://colab.research.google.com/github/Cohee1207/SillyTavern/blob/main/colab/GPU.ipynb)

* Simple to setup
* Free to use
* No Colab GPU credits required (use the `use_cpu` options)
* See the [Colab Guide Page](https://docs.sillytavern.app/extras/running-extras-in-colab/) for details.

---

## Installation Methods

### MiniConda (recommended)

This method is recommended because Conda makes a 'virtual environment' for the Extras requirement packages to live inside, so they do not affect your system-wide Python setup.

1. Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

_(Important!) Read [how to use Conda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. Install [git](https://git-scm.com/downloads)

_(Chads who installed SillyTavern with git to begin with can skip this step!)_

After you have both of them installed...

Type/paste the commands below `ONE BY ONE` IN THE `CONDA COMMAND PROMPT WINDOW` and hit `Enter` after each one.

3. Create a new Conda environment (let's call it `extras`):

`conda create -n extras`

4. Activate the new environment

`conda activate extras` (you should see `(extras)` pop up on the left side of your command prompt)

5. Install the required system packages (this will take time, torch is a ~2GB download)

`conda install pytorch=2.0.0 torchvision=0.15.0 torchaudio=2.0.0 pytorch-cuda=11.7 git -c pytorch -c nvidia`

6. Clone the Extras GitHub repo

`git clone https://github.com/Cohee1207/SillyTavern-extras`

7. Navigate to your cloned Extras repo

`cd SillyTavern-extras`

8. Install Extras' requirements by using **one** of the following commands (will take time, again):

* `pip install -r requirements.txt` - for minimal features (character expressions, system OS TTS)
* `pip install -r requirements-complete.txt` - for additinoal features like local/remote Stable Diffusion, ChromaDB, Silero TSS

See the [Common Problems](https://docs.sillytavern.app/extras/installation/common-problems/) page if you get errors at this step!

9. See below 'Running Extras After Install'

---

### System-Wide Installation

This is easier, but will affect your system-wide Python installation.

This can cause conflicts if you work with many Python programs that have different requirements.

If this is your first time touching anything Python-related, that should not be a problem.

1. Install Python 3.10: <https://www.python.org/downloads/release/python-31010/>
2. Install git: <https://git-scm.com/downloads>
3. Open a command prompt window and go to a folder in which you have complete access permissions.
4. Clone the repo: `git clone https://github.com/Cohee1207/SillyTavern-extras`, hit Enter.
5. After the clone has finished, type `cd SillyTavern-extras`, hit Enter.
6. Type `python -m pip install -r requirements.txt`
7. See below 'Running Extras After Install'

---

## Running Extras After Install

### Confirm extensions are enabled

1. Open the file called `config.conf`in a text editor. The file is located in ST's base install folder.
2. Look for the line that reads `const enableExtensions`.
3. Make sure that line has `= true`, and not `= false`.

### Decide which module to use

(This only needs to be done once)

* Extras is always started with a Python command line.
* `python server.py` is the bare minimum, but it does not enable any useful modules.
* to enable modules you must use the `--enable-modules=` modifier, with a comma-separated list of module names

Example: `python server.py --enable-modules=caption,summarize,classify`

This would enable Image Captioning, Chat Summary, and live updating Character Expressions.

Below is a table that describes each module.

| Name        | Description                       | Included in default requirements.txt       |
| ----------- | --------------------------------- | ------ |
| `caption`   | Image captioning                  | ✔️ Yes        |
| `summarize` | Text summarization                | ✔️ Yes    |
| `classify`  | Text sentiment classification     | ✔️ Yes      |
| `sd`        | Stable Diffusion image generation | :x: No (✔️ remote)      |
| `silero-tts`       | [Silero TTS server](https://github.com/ouoertheo/silero-api-server) | :x: No |
| `edge-tts` | [Microsoft Edge TTS client](https://github.com/rany2/edge-tts) | ✔️ Yes |
| `chromadb`  | Infinity context server           | :x: No |

* Decide which modules you want to add to your Python command line.
* They will be used in the next step.

**NOTE: There must be `no spaces at all in your Python command's module list!`**

### Start Extras Server

While still in your command prompt window inside the Extras installation folder...

1. Make sure your conda environment is active (if you used the Conda install method)
2. Type `activate extras` if the environment was not active.
3. Type `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`
4. Extras server will load.
5. After a while it will show you a URL at the end. For local installs, this defaults to `http://localhost:5100`.
6. Copy the API URL.

### Connect ST to the Extras server

1. Start your SillyTavern server, and view the SillyTavern interface in your browser.
2. Open the Extensions panel (via the 'Stacked Blocks' icon at the top of the page)
3. Paste the API URL into the input box.
4. Click `Connect`.

To run Extras again, simply activate the environment and run these commands in a command prompt.

`conda activate extras`, Hit Enter.
`python server.py`, Hit Enter.

Be sure to the additional options for server.py (see below) that your setup requires.

## Make a .bat File for Easy Startup

This is Optional and only applies to Windows, but something similar should be possible on MacOS.

1. View your Windows Desktop
2. Right click, select `New`, and then click `Text Document`
3. A new file will appear on your Desktop, asking for a name.
4. Name the file `STExtras.txt`
4. Open the newly created file in a text editor.
5. Paste the following code into it:

```
cd C:\_your_\_full_\_Extras_\_folder_\_path_\
call activate extras
python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
deactivate
pause
```

6. Replace the placeholder folder path with your actual Extras install folder path.
7. Replace the python command line with your actual command line
8. Save the file with a new name `STExtras.bat` (Use `File` >> `Save As` in most text editors)

You can now simply double click on this .bat file to easily start Extras.

If you ever want to change the module list (or any other command line modifiers for the extras server), simply edit the python command inside the .bat file.
