# Extras Install Common Problems

### hnswlib wheel building error when installing ChromaDB

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

Before installing the ChromaDB module you must first do `one of the following`:

* Install Visual C++ build tools: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* Install the `hnswlib` package with conda: `conda install -c conda-forge hnswlib`

---

### Missing modules?

You must specify a list of module names to be run in the `--enable-modules` command (`caption` provided as an example). See [Modules](https://docs.sillytavern.app/extras-installation/installation/#decide-which-module-to-use) section.

---

### What about mobile/Android/Termux? 🤔

* There are some folks in the community having success running Extras on their phones via Ubuntu on Termux.
* However, Extras was not made with mobile support in mind.
* No support will be provided for people running Extras on their Android devices.
* Direct all your questions to the creator of the guide instead.

#### ❗ This is UNSUPPORTED

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
