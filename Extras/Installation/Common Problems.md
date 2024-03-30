---
label: Common Problems
icon: bug
---

# Extras Install Common Problems

This page lists common questions and problems encountered while installing SillyTavern Extras.

---
Local installation of Extras can be difficult or impossible on your OS (especially Termux).

#### Use the [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* Simple to setup
* Free to use
* No Colab GPU credits required (use the `use_cpu` options)
* See the [Colab Guide Page](https://docs.sillytavern.app/extras/running-extras-in-colab/) for details.

---

### Error: Could not import the 'talkinghead' module on Linux

It requires the installation of an additional package because it's not installed automatically due to incompatibility with Colab. Run this after you install other requirements:

`pip install wxpython`

### Extras server can't connect to AUTOMATIC1111's Stable Diffusion Web UI

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**Make sure webui-user.bat that you start Stable Diffusion with contains --api command line option in the COMMANDLINE_ARGS variable.**

Find and replace that line in your "webui-user.bat": `set COMMANDLINE_ARGS=--api`

![How it shoud look](/static/extensions/sd-user.png)

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
* See [Modules](https://docs.sillytavern.app/extras/installation/#decide-which-module-to-use) section.

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
