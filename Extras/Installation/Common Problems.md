# Extras Install Common Problems

This page lists common questions and problems encountered while installing SillyTavern Extras.

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
3. Run `pip install -r requirements.txt` again (change to `requirements-complete.txt` if needed.

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
