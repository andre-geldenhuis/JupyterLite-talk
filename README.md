# JupyterLite talk — interactive Python in the browser

[![lite-badge](https://jupyterlite.rtfd.io/en/latest/_static/badge.svg)](https://andre-geldenhuis.github.io/JupyterLite-talk/)

A talk on JupyterLite + xeus, presented as a live slide deck (`jupyterlab-deck`) and
deployed as a static site to GitHub Pages. Everything runs in your browser — no server.

## ✨ Try it in your browser ✨

**Live site:** https://andre-geldenhuis.github.io/JupyterLite-talk/

**Straight into the talk deck:** https://andre-geldenhuis.github.io/JupyterLite-talk/lab/index.html?path=talk.ipynb

> To present: open the deck link, run **Run → Run All Cells**, then start deck mode
> (the slideshow/deck button in the toolbar, or command palette → *Start Deck*).

## ≠ How does it compare to the Pyodide kernel?

#### Pyodide kernel:

- Is based on [Pyodide](https://github.com/pyodide/pyodide)
- Uses [IPython](https://github.com/ipython/ipython) for the code execution (access to IPython magics, support for the inline Matplotlib backend, *etc*)
- Provides a way to dynamically install packages with ``piplite`` (**e.g.** ``await piplite.install("ipywidgets")``)
- **Does not support** sleeping with ``from time import sleep``
- **Does not support** pre-installing packages

#### jupyterlite-xeus-python:

- Is based on [xeus-python](https://github.com/jupyter-xeus/xeus-python)
- Uses [IPython](https://github.com/ipython/ipython) for the code execution (access to IPython magics, support for the inline Matplotlib backend, *etc*)
- **Does not provide** a way to dynamically install packages (yet. We are working on building a ``mamba`` package manager for WASM)
- **Supports** sleeping with ``from time import sleep``
- **Supports** pre-installing packages from ``emscripten-forge`` and ``conda-forge``, by providing an ``environment.yml`` file defining the runtime environment

## 💡 How to make your own deployment

![Deploy your own](deploy.gif)

Then your site will be published under https://{USERNAME}.github.io/{DEMO_REPO_NAME}

## 📦 How to install extra packages

You can pre-install extra packages for xeus-python by adding them to the ``environment.yml`` file.

For example, if you want to create a JupyterLite deployment with NumPy and Matplotlib pre-installed, you would need to edit the ``environment.yml`` file as following:

```yml
name: xeus-python-kernel
channels:
  - https://repo.mamba.pm/emscripten-forge
  - conda-forge
dependencies:
  - xeus-python
  - numpy
  - matplotlib
```

Only ``no-arch`` packages from ``conda-forge`` and packages from ``emscripten-forge`` can be installed.
- **How do I know if a package is ``no-arch`` on ``conda-forge``?** ``no-arch`` means that the package is OS-independent, usually pure-python packages are ``no-arch``. To check if your package is ``no-arch`` on ``conda-forge``, check if the "Platform" entry is "no-arch" in the https://beta.mamba.pm/channels/conda-forge?tab=packages page. If your package is not ``no-arch`` but is a pure Python package, then you should probably update the feedstock to turn your package into a ``no-arch`` one.
![](noarch.png)
- **How do I know if my package is on ``emscripten-forge``?** You can see the list of packages pubished on ``emscripten-forge`` [here](https://beta.mamba.pm/channels/emscripten-forge?tab=packages). In case your package is missing, or it's not up-to-date, feel free to open an issue or a PR on https://github.com/emscripten-forge/recipes.
