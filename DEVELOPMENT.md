# Setting up a Development Environment

This document details how to set up a local development environment that will allow you to contribute changes to the project!

## Install Pre-Requisites

* **Operating System**: macOS or Linux; Windows users need to develop under WSL.
* **`git`**: The project uses the ubiquitous `git` tool for change control.
  * The project is hosted on GitHub, so your `git` enviroment should have appropriate credentials to GitHub.
* **`make`**: The project uses `make` to coordidinate packaging.
* **`uv`**: This project uses `uv` (`>=0.4`), a Python package and project manager from Astral. Install instructions at https://docs.astral.sh/uv/getting-started/installation/.
* **`npm`**: The frontend files are built with Node.js (`v22.12 LTS`) and `npm` (`v10.9`). Install instrucations at https://nodejs.org/en/download/package-manager.
  * Windows (WSL) users: ensure `npm` is installed within WSL environment; `which npm` should resolve to a Linux location, not a Windows location.

Optionally, you may wish to install the Microsoft VS Code IDE https://code.visualstudio.com/ .

## Set up Git Repository

You will push changes to a fork of the Langflow repository, and from there create a Pull Request into the project repository.

Fork the [Langflow GitHub repository](https://github.com/langflow-ai/langflow/fork), and follow the instructions to create a new fork.

On your new fork, click the "<> Code" button to get a URL to clone using your preferred method, and clone the repostory; for example using `https`:

```bash
git clone https://github.com/<your username>/langflow.git
```

Finally, add the Project repository as `upstream`:

```bash
cd langflow
git remote add upstream https://github.com/langflow-ai/langflow.git
git remote set-url --push upstream no_push
```

> **Windows/WSL Users**: You may find that files "change", specifically the file mode e.g. "changed file mode 100755 → 100644". You can workaround this problem with `git config core.filemode false`. 

## Initial Environment Validation

Setup and validate the initial environment by running:

```bash
make init
```

This will set up the development environment by installing backend and frontend dependencies, building the frontend static files, and initializing the project. It runs `make install_backend`, `make install_frontend`, `make build_frontend`, and finally `uv run langflow run` to start the application.

Once the application is running, the command output should look similar to:

```
╭───────────────────────────────────────────────────────────────────╮
│ Welcome to ⛓ Langflow                                             │
│                                                                   │
│                                                                   │
│ Collaborate, and contribute at our GitHub Repo 🌟                 │
│                                                                   │
│ We collect anonymous usage data to improve Langflow.              │
│ You can opt-out by setting DO_NOT_TRACK=true in your environment. │
│                                                                   │
│ Access http://127.0.0.1:7860                                      │
╰───────────────────────────────────────────────────────────────────╯
```

At this point, validate you can access the UI by opening the URL shown.

This is how the application would normally run: the (static) front-end pages are compiled, and then this "frontend" is served by the FastAPI server; the "backend" APIs are also serviced by the FastAPI server.

However, as a developer,  you will want to proceed to the next step. Shutdown Langflow by hitting `Control (or Command)-C`.

## Completing Development environment Setup

There are a few other steps to take before you are ready to begin development. Install pre-commit hooks by running the following commands:

```bash
uv sync --dev
uv run pre-commit install
```

These pre-commit hooks will help keep your changes clean and well-formatted.

## Run Langflow in "Development" Mode

With the above validation, you can now run the backend (FastAPI) and frontend (Node) services in a way that will "hot-reload" your changes. In this mode, the FastAPI server requires a Node.js server to serve the frontend pages rather than serving them directly. 

> ***Note***: You will likely have multiple terminal sessions active in the normal development workflow. These will be annotated as *Backend Terminal*, *Frontend Terminal*, *Documentation Terminal*, and *Build Terminal*. 

### Start the Backend Service

The backend service runs as a FastAPI service on Python, and is responsible for servicing API requests. In the *Backend Terminal*, start the backend service:

```bash
make backend
```

You will get output similar to:

```
INFO:     Will watch for changes in these directories: ['/home/phil/git/langflow']
INFO:     Loading environment from '.env'
INFO:     Uvicorn running on http://0.0.0.0:7860 (Press CTRL+C to quit)
INFO:     Started reloader process [22330] using WatchFiles
Starting Langflow ...
```

At which point you can check http://127.0.0.1:7860/ in a browser; when the backend service is ready it will return a document like:

```json
{"detail":"Not Found"}
```

### Start the Frontend Service

The frontend (User Interface) is, in shipped code (i.e. via `langflow run`), statically-compiled files that the backend FastAPI service provides to clients via port `7860`. In development mode, these are served by a Node.js service on port `3000`. In the *Frontend Terminal*, start the frontend service:

```bash
make frontend
```

You will get output similar to:

```
  VITE v5.4.11  ready in 552 ms

  ➜  Local:   http://localhost:3000/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

At this point, you can navigate to http://localhost:3000/ in a browser and access the Langflow User Interface.

### Build and Display Documentation

If you are contributing changes to documentation (always welcome!), these are built (using [Docusaurus](https://docusaurus.io/)) and served separately, also using Node.js. 

In the *Documentation Terminal* (from the project root directory), run the following:

```bash
cd docs
npm install
npm run start
```

If the frontend service is running on port `3000` you might be prompted `Would you like to run the app on another port instead?`, in which case answer "yes". You will get output similar to:

```
[SUCCESS] Docusaurus website is running at: http://localhost:3001/
```

At which point you can navigate to http://localhost:3001/ in a browser and view the documentation. Documentation updates will be visible as they are saved, though sometimes the browser page will also need to be refreshed.

## Making and Testing Changes

> The directory structure and other similar considerations are out of scope of this document.

When you are ready to commit, and before you commit, you should consider the following:

* `make lint` 
* `make format_backend` and `make format_frontend` will run code formatters on their respective codebases
* `make tests` runs tests on the backend, while `make tests_frontend` runs them on the frontend
  * You can run a subset of backend tests with `make unit_tests`, `make integration_tests`, and `make coverage`.

Once these changes are ready, it is helpful to rebase your changes on top of `upstream`'s `main` branch, to ensure you have the latest code version!

## Committing, Pushing, and Pull Requests

Once you are happy your changes are complete, commit them and push the changes to your own fork (this will be `origin` if you followed the above instructions). You can then raise a Pull Request into the Project repository on the GitHub interface.

---

**TODO**: LANGFLOW_LOG_LEVEL, LANGFLOW_LOG_FILE, LANGFLOW_CONFIG_DIR, LANGFLOW_SAVE_DB_IN_CONFIG_DIR ?

**TODO**: Tips about managing the browser cache? i.e. if I make a change to a "base" or other component, do I need to reload the browser to ensure the components are up-to-date? 
