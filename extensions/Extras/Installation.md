---
icon: gear
label: Local Installation
route: /extensions/extras/installation/
---

# Extras Installation

This page contains instructions for installing SillyTavern Extras on your local device.

!!! Discontinued
The Extras project was discontinued in April 2024 and won't receive any new updates or modules. The vast majority of modules are available natively in the main SillyTavern application. You may still install and use it but don't expect to get immediate support if you face any issues.
!!!

Local installation of Extras can be difficult or impossible on your OS (especially Termux).

## Use the [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* Simple to set up
* Free to use
* No Colab GPU credits required (use the `use_cpu` options)
* See the [Colab Guide Page](/extensions/Extras/Installation.md#running-extras-in-colab) for details.

### Running Extras in Colab

* Open the [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* Select the desired "Extra" options
* select `use_cpu` to run Extras without requiring GPU credit
  * this will make Stable Diffusion slower, but everything else will run normally
* Not required, but recommended: select the `secure` option to generate the API key to protect your shared instance.
* Click the Start button on the left (looks like a triangle 'play' button)
* Wait for it to finish loading everything
* Look for the `trycloudflare.com` link at the bottom of the output. Ignore the localhost link, it won't work (we tried!).
* It will start with the text `Running on`
* Copy the API URL link that is listed under that line. (**DO NOT copy the 'localhost' URL, use the other one**)
* Start SillyTavern with extensions support: (set `enableExtensions` to `true` in your `config.yaml` if necessary)
* Navigate to SillyTavern's Extensions menu (click the 'stacked blocks' icon at the top of the page).
* Paste the API URL into the box at the top. (**NOT the API Key box**)
* If you have NOT enabled the `secure` option, make sure the API Key box is completely empty when using the official colab.
* If you have enabled the `secure` option, paste the generated API key into the API Key box.
* API key will appear in the colab's console output, for example: `Your API key is fee2f3f559`
* Click "Connect"

---

## Local Installation Methods

### MiniConda (recommended)

This method is recommended because Conda makes a 'virtual environment' for the Extras requirement packages to live inside, so they do not affect your system-wide Python setup.

1. Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    _(Important!) Read [how to use Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. Install [git](https://git-scm.com/downloads)

    _(Chads who installed SillyTavern with git to begin with can skip this step!)_
    
    After you have both of them installed...
    
    Type/paste the commands below `ONE BY ONE` IN THE `CONDA COMMAND PROMPT WINDOW` and hit `Enter` after each one.

3. Create a new Conda environment (let's call it `extras`):

    `conda create -n extras`

4. Activate the new environment

    `conda activate extras` (you should see `(extras)` pop up on the left side of your command prompt)

5. Install the required system packages (this will take some time)

    `conda install python=3.11 git`

6. Clone the Extras GitHub repo

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. Navigate to your cloned Extras repo

    `cd SillyTavern-extras`

8. Install Extras' requirements by using **one** of the following commands (will take time, again):

   * `pip install -r requirements.txt` - for basic features
   * `pip install -r requirements-rvc.txt` - for real-time voice cloning
   * `pip install -r requirements-coqui.txt` - for Coqui TTS (not recommended)

    See the [Common Problems](/extensions/Extras/Installation.md#extras-install-common-problems) page if you get errors at this step!

9. See below 'Running Extras After Install'

---

### System-Wide Installation

This is easier, but will affect your system-wide Python installation.

This can cause conflicts if you work with many Python programs that have different requirements.

If this is your first time touching anything Python-related, that should not be a problem.

1. Install Python 3.11: <https://www.python.org/downloads/release/python-3115/>
2. Install git: <https://git-scm.com/downloads>
3. Open a command prompt window and go to a folder in which you have complete access permissions.
4. Clone the repo: `git clone https://github.com/SillyTavern/SillyTavern-extras`, hit Enter.
5. After the clone has finished, type `cd SillyTavern-extras`, hit Enter.
6. Type `python -m pip install -r requirements.txt`
7. See below 'Running Extras After Install'

---

## Running Extras After Install

### Confirm extensions are enabled

1. Open the file called `config.yaml` in a text editor. The file is located in ST's base install folder.
2. Look for the line that reads `enableExtensions`.
3. Make sure that line has `true`, and not `false`.

### Decide which module to use

(This only needs to be done once)

* Extras is always started with a Python command line.
* `python server.py` is the bare minimum, but it does not enable any useful modules.
* to enable modules you must use the `--enable-modules=` modifier, with a comma-separated list of module names

Example: `python server.py --enable-modules=caption,summarize,classify`

This would enable Image Captioning, Chat Summary, and live updating Character Expressions.

Below is a table that describes each module.

| Name         | Description                                                         |
|--------------|---------------------------------------------------------------------|
| `caption`    | Image captioning                                                    |
| `summarize`  | Text summarization                                                  |
| `classify`   | Text sentiment classification                                       |
| `sd`         | Stable Diffusion image generation                                   |
| `silero-tts` | [Silero TTS server](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [Microsoft Edge TTS client](https://github.com/rany2/edge-tts)      |
| `chromadb`   | Vector storage server                                               |
| `coqui-tts`  | Coqui TTS                                                           |
| `rvc`        | Real-time voice cloning                                             |

* Decide which modules you want to add to your Python command line.
* They will be used in the next step.

**NOTE: There must be `no spaces at all in your Python command's module list!`**

### Start Extras Server

While still in your command prompt window inside the Extras installation folder...

1. Make sure your conda environment is active (if you used the Conda install method)
2. Type `activate extras` if the environment is not active.
3. Type `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`
4. The extras server will load.
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

Be sure to use the additional options for server.py (see below) that your setup requires.

## Make a .bat File for Easy Startup

This is Optional and only applies to Windows, but something similar should be possible on MacOS.

1. View your Windows Desktop
2. Right-click, select `New`, and then click `Text Document`
3. A new file will appear on your Desktop, asking for a name.
4. Name the file `STExtras.txt`
5. Open the newly created file in a text editor.
6. Paste the following code into it:

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
    call conda deactivate
    pause
    ```

7. Replace the placeholder folder path with your actual Extras install folder path.
8. Replace the python command line with your actual command line
9. Save the file with a new name `STExtras.bat` (Use `File` >> `Save As` in most text editors)

You can now simply double-click on this .bat file to easily start Extras.

If you ever want to change the module list (or any other command line modifiers for the extras server), simply edit the python command inside the .bat file.

## Extras Install Common Problems

This section lists common questions and problems encountered while installing SillyTavern Extras.

### Error: Could not import the 'talkinghead' module on Linux

It requires the installation of an additional package because it's not installed automatically due to incompatibility with Colab. Run this after you install other requirements:

`pip install wxpython`

### Extras server can't connect to AUTOMATIC1111's Stable Diffusion Web UI

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**Make sure webui-user.bat that you start Stable Diffusion with contains --api command line option in the COMMANDLINE_ARGS variable.**

Find and replace that line in your "webui-user.bat": `set COMMANDLINE_ARGS=--api`

![How it should look](/static/extensions/sd-user.png)

If the API mode is disabled for SD Web UI, the Extras server won't be able to make a connection and you won't be able to generate images!

#### Still doesn't work?

Ensure that you start everything in the proper order, waiting for every program to finish loading before proceeding to the next step:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

The extras server can't reconnect to the Stable Diffusion API if it was loaded after.

### hnswlib wheel building error when installing ChromaDB

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

Before installing the ChromaDB module you must first do `one of the following`:

* Install Visual C++ build tools: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* Install the `hnswlib` package with conda: `conda install -c conda-forge hnswlib`

---

### Error when installing Python requirements on Mac

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac does not support CUDA, so torch packages should be installed without CUDA support.

Install the requirements using the `requirements-silicon.txt` file instead.

---

### Missing modules?

* You must specify a list of module names in your Python command line, with the `--enable-modules` modifier.
* See [Modules](/extensions/Extras/Installation.md#decide-which-module-to-use) section.

---

### What is the API Key box for?

* The API Key box in SillyTavern's Extensions panel is only used when you have:
  * created a text file named `api_key.txt` in your Extras install folder, which contains your chosen Extras 'password'.
  * started extras with the `--secure` commandline argument.
* This makes the Extras API 'password locked', so only users who have that key in their API Key box can access it.
* This is mainly useful for people who want to make their own public deployment of Extras (colab, etc).
* Users running Extras on their own PC for personal use should not type anything into the API Key box.

### What about mobile/Android/Termux? 🤔

* There are some folks in the community having success running Extras on their phones via Ubuntu on Termux.
* However, Extras were not made with mobile support in mind.
* No support will be provided for people running Extras on their Android devices.
* Direct all your questions to the creator of the guide linked below instead.

#### ❗ This is UNSUPPORTED

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
