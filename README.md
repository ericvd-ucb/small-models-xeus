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
- `content/01_finding_models.ipynb`: browse WebLLM's models, read model names, find model cards, and manage what is saved in the browser.
- `content/02_talking_to_models.ipynb`: the OpenAI-style chat request and response, system messages, multi-turn chats, and the main settings.
- `content/03_numbers_all_the_way_down.ipynb`: tokens, token counts, chunking, and embeddings.
- `content/04_memory.ipynb`: why models do not remember you, and a simple memory system with SQLite and embeddings.
- `content/05_sat_test_taker.ipynb`: a small model takes SAT-style questions and gets a report card.
- `content/06_inside_a_model.ipynb`: parameters, weights, tensors, and the files inside a WebLLM model package.
- `content/07_agents.ipynb`: build a small agent that answers questions by writing SQL, with tools, a loop, and an evaluation.

Data files used by notebooks 05 and 07 are in `content/data/`.

## How it is built

- `environment.yml` lists the packages available inside the notebooks (from emscripten-forge).
- `.github/build-environment.yml` lists the tools that build the site.
- `.github/workflows/deploy.yml` builds the site and publishes it to GitHub Pages on every push to `main`.

Edits made inside the live site stay in that browser unless they are copied back into this repository. The repository is the master copy.
