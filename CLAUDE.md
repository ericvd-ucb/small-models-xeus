# Small Models in the Browser: project brief

This repo is a JupyterLite site (built with the `jupyterlite/xeus-lite-demo` setup, deployed to GitHub Pages) that hosts a series of teaching notebooks about small language models. Every model runs **entirely in the student's browser** using WebLLM. No servers, no API keys, no installs.

The owner is Eric, who teaches data science at UC Berkeley. He is not a command-line person: explain each step briefly in plain language, and say clearly when you need him to do something.

## Extra context from Eric's past work

Use Eric's past course materials in [ds-modules/Small_Models_SP26](https://github.com/ds-modules/Small_Models_SP26/tree/main) as background context when planning or adapting notebooks for this repo, especially for the token and SAT notebooks.

## Audience and style

Students are beginners. Every notebook should follow the style of `content/webllm_starter.ipynb`:

- Numbered steps (Step 1, Step 2, ...) with sub-steps (7a, 7b, ...) when an idea needs breaking down.
- Small code cells that each do one thing, with a short markdown cell before each one saying what it does and why.
- Plain language. Define any new term (token, temperature, embedding) the first time it appears.
- Settings students are meant to change go in CAPITALS at the top of a cell (`MAX_SIZE_MB = 4000`, `CHOICE = 1`).
- Show the raw thing first, then the shortcut (for example, build a request by hand before wrapping it in `ask()`).
- Friendly messages instead of tracebacks for predictable mistakes (for example, "No model is loaded yet. Run Step 6.").
- Writing style: no emdashes, no horizontal rules, no emojis in headers.

## Technical setup

- Kernel: **xeus-python** (this repo is the xeus port of `small-models-lite`, which used Pyodide). Top-level `await` works in cells.
- Packages available in notebooks are listed in `environment.yml` (emscripten-forge). Build tools are in `.github/build-environment.yml`.
- JavaScript is reached through pyjs: `import pyjs` then `js = pyjs.js`. Do not use `pyodide.ffi`; it is not available.
- WebLLM is loaded from a CDN: `webllm = await js.eval("import('https://esm.run/@mlc-ai/web-llm')")`
- Python dicts become JavaScript objects through JSON: `js.JSON.parse(json.dumps(d))` (wrapped as `js_obj()`). This only works for plain data, so never put a Python function inside a dict passed to `js_obj()`.
- Python functions that JavaScript must call (the progress bar) are wrapped with `pyjs.create_callable(fn)`, which returns the JavaScript function and a handle.
- Create the engine with `webllm.MLCEngine.new()`, set the callback with `engine.setInitProgressCallback(...)`, then `await engine.reload(model_id)`. This avoids passing a callback inside a config object.
- Requires WebGPU (recent Chrome or Edge on a laptop). Each notebook should check for it at the start.
- Models are cached in the browser after the first download. `webllm.hasModelInCache(id)` checks; `webllm.deleteModelAllInfoInCache(id)` removes.

### No hidden helper modules

Every notebook is **self-contained**: all code a student runs is visible in the notebook itself. Do not move code into a separate `.py` file that notebooks import, because students cannot see or learn from code hidden in another file.

- Each notebook has its own setup step near the top (loading WebLLM, `js_obj`, the progress bar, loading a model, the window-size retry). Keep it as short as possible and explain what each part does in plain language.
- Only include the setup a notebook actually needs. For example, a notebook that does not download models does not need the download code.
- Repeating setup code across notebooks is fine and expected. When a fix is needed in shared setup code, apply it to **every** notebook that contains it, and tell Eric which notebooks changed.

### Problems already found and fixed

- **`WindowSizeConfigurationError`** on some models: retry loading with chat options `{"sliding_window_size": -1}`. Already handled in `start_model()`.
- **Repetition loops**: small models can repeat one line hundreds of times, which also makes answers seem very slow. Always send `max_tokens` (around 300) and `frequency_penalty` (around 0.5).
- **Thinking models** (Qwen3, Qwen3.5, DeepSeek R1 Distill, Ministral 3 Reasoning) emit `<think>` blocks and are slow. Avoid them for plain chat. `/no_think` at the end of a prompt turns thinking off for Qwen3 but output quality drops.
- **Release dates** are not in WebLLM's model list, so the starter keeps a hand-made `RELEASED` table of model families. Keep it visible in `01_finding_models.ipynb` too, and explain that students can add new families to it.
- Edits made on the live site are saved only in that browser. The repo is the master copy.

### Model recommendations

For plain chat explanations: **Gemma 3 1B** (`gemma3-1b-it-q4f16_1-MLC`) as the default, **Llama 3.2 1B Instruct** as the most predictable, **Qwen2.5 0.5B Instruct** for showing mistakes, **SmolLM2** (135M, 360M, 1.7B) and **OLMo 2 1B** as fully open models. Show only `q4f16` variants to students.

## Tasks

### Port the notebook series from small-models-lite

Port one notebook at a time from `small-models-lite/content`, following the technical setup above. `00_webllm_starter.ipynb` is done first as the test case. After Eric confirms it works on GitHub Pages and notebook.link, port 01 to 06 and the `data/` folder, then update the README notebook list.
