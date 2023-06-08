# Extras Install Common Problems

This page lists common questions and problems encountered while installing SillyTavern Extras.

### Extras server can't connect to AUTOMATIC1111's Stable Diffusion Web UI

> Could not connect to remote SD backend at http://127.0.0.1:7860! Disabling SD module...

**Make sure webui-user.bat that you start Stable Diffusion with contains --api command line option in the COMMANDLINE_ARGS variable.**

Find and replace that line in your "webui-user.bat": `set COMMANDLINE_ARGS=--api`

![How it shoud look](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/a823d134-14fb-40c6-b3f1-2e174e7b1172)

If the API mode is disabled for SD Web UI, Extras server won't be able to make a connection and you won't be able to generate images!

#### Still doesn't work?

Ensure that you start everything in the proper order, waiting for every program to finish loading before proceeding to the next step:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

Extras server can't reconnect to the Stable Diffusion API if it was loaded after.

### hnswlib wheel building error when installing ChromaDB

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

Before installing the ChromaDB module you must first do `one of the following`:

* Install Visual C++ build tools: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* Install the `hnswlib` package with conda: `conda install -c conda-forge hnswlib`

---

### Error when installing Python requirements on Mac 

> ERROR: No matching distribution found for torch==2.0.0+cu117 

Mac does not support CUDA, so torch packages should be installed without CUDA support:

1. Open requirements.txt and requirements-complete.txt files with a Text Editor app.
2. Remove all `+cu117` parts from the package versions. Example: `torch==2.0.0+cu117` => `torch==2.0.0`
3. Run `pip install -r requirements.txt` again (change to `requirements-complete.txt` if needed).

---

### Missing modules?

* You must specify a list of module names in your python commandline, with the `--enable-modules` modifier.
* See [Modules](https://docs.sillytavern.app/extras/installation/#decide-which-module-to-use) section.

---

### What about mobile/Android/Termux? 🤔

* There are some folks in the community having success running Extras on their phones via Ubuntu on Termux.
* However, Extras was not made with mobile support in mind.
* No support will be provided for people running Extras on their Android devices.
* Direct all your questions to the creator of the guide linked below instead.

#### ❗ This is UNSUPPORTED

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
