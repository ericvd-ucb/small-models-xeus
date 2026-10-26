# Small Models in the Browser (xeus version)

This repository is a trial port of [small-models-lite](https://github.com/ericvd-ucb/small-models-lite) from the Pyodide kernel to the xeus-python kernel. Every model runs entirely in the student's browser using WebLLM, so there are no servers, no API keys, and no local installs.

The goal is one kernel everywhere: the GitHub Pages site, notebook.link, and anyone who forks the repo all run the same notebooks. If the trial works, this repo will replace the Pyodide one.

## Live site

<https://ericvd-ucb.github.io/small-models-xeus/>

## What students need

- A recent Chrome or Edge browser with WebGPU support
- A laptop or desktop computer with enough memory for small models
- Patience for the first model download, since models are cached in the browser after that

## Notebooks

- `content/00_webllm_starter.ipynb`: see which models exist, download one, load it, and chat with it.

More notebooks will be ported once the starter works.

## How it is built

- `environment.yml` lists the packages available inside the notebooks (from emscripten-forge).
- `.github/build-environment.yml` lists the tools that build the site.
- `.github/workflows/deploy.yml` builds the site and publishes it to GitHub Pages on every push to `main`.

Edits made inside the live site stay in that browser unless they are copied back into this repository. The repository is the master copy.
