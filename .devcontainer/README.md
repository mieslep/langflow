# Langflow Demo Codespace Readme

These instructions will walk you through the process of running a Langflow demo via GitHub Codespaces.

If you want a faster demo experience with Langflow, DataStax Langflow is a hosted environment with zero setup: [Sign up for a free account.](https://astra.datastax.com/signup?type=langflow)

## Create a Codespace in GitHub

To setup the demo:

1. Navigate to the Langflow repo
2. On the "Code <>" button, select the "Codespaces" tab
3. Click the "+" icon tho create a new Codespace

## Wait for everything to install

After the codespace is opened, there will be two phases to the process. It will take ≈5-10 minutes to complete.

* **Phase 1**: Building Container; you can click on the "Building Codespace" link to watch the logs
* **Phase 2**: Building Langflow; the terminal will now show `Running postCreateCommand...` and the terminal will close

You can now create a new terminal window 

you should see a new Terminal window in VS Code where langflow is installed (You may need to click ). Once the install completes, `langflow` will launch the webserver and your application will be available via devcontainer port. 

Note: VS Code should prompt you with a button to push once the port is available.

## Start up the Service

Open a Terminal, and type `uv run langflow run`.