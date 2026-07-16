# Local Setup for Labs 5 & 6 (No Codespace Required)
 
These instructions are for anyone who can't use the GitHub Codespace (or the Docker/Dev Container setup in `README-vscode.md`) and needs to run **Lab 5 (Prompting for an AI Agent)** and **Lab 6 (Working with Prompts in MCP)** directly on their own Windows or Mac machine.
 
Only Labs 5 and 6 need this — they're the two labs that run real code (`agent.py`, `mcp_server.py`, `mcp_client_agent.py`) against a local LLM served by [Ollama](https://ollama.com). Labs 1–4 don't require any of this setup.
 
No Docker and no Dev Containers extension needed — just Python and Ollama installed natively.
 
## What you're installing
 
- **Python 3.10+** — to run the lab scripts.
- **Ollama** — runs the `granite4:3b` model locally and serves it on `http://127.0.0.1:11434`.
- **Python packages** from `requirements.txt` (`fastmcp`, `langchain-ollama`, `requests`, etc.).
Once these are in place, Lab 5 and Lab 6 run exactly as described in `labs.md` — just from your local terminal and editor instead of the Codespace's.
 
---
 
## macOS Setup
 
**1. Install Python 3.10+**, if you don't already have it.
 
Check what you have first:
```
python3 --version
```
If it's missing or older than 3.10, install via [Homebrew](https://brew.sh):
```
brew install python@3.12
```
or download an installer from [python.org/downloads](https://www.python.org/downloads/).
 
**2. Install Ollama.**
 
Download the macOS app from [ollama.com/download](https://ollama.com/download) and drag it to Applications, or install via Homebrew:
```
brew install ollama
```
If you installed the `.app` version, open it once — it runs in the menu bar and automatically starts serving the API on `127.0.0.1:11434`. If you installed via Homebrew (no app), start the server manually in its own terminal tab and leave it running:
```
ollama serve
```
 
**3. Pull the model used by the labs.**
 
In a terminal (a new tab is fine — the server from step 2 keeps running in its own):
```
ollama pull granite4:3b
```
This downloads a few GB the first time only.
 
**4. Clone the repo and set up the Python environment.**
```
git clone https://github.com/skillrepos/prompt-accel.git
cd prompt-accel
python3 -m venv py_env
source py_env/bin/activate
pip3 install -r requirements.txt
```
You'll need to run `source py_env/bin/activate` again in any new terminal tab you open for the labs.
 
**5. Verify everything is working.**
```
curl http://127.0.0.1:11434/api/tags
```
You should see `granite4:3b` listed. Then optionally run the repo's warmup script to pre-load the model and confirm Python can reach Ollama:
```
python3 scripts/warmup.py
```
 
You're ready — skip to [Running the Labs](#running-the-labs) below.
 
---
 
## Windows Setup
 
**1. Install Python 3.10+**, if you don't already have it.
 
Download the installer from [python.org/downloads](https://www.python.org/downloads/windows/). During install, check **"Add python.exe to PATH"** on the first screen.
 
Verify in PowerShell:
```
python --version
```
 
**2. Install Ollama.**
 
Download the Windows installer from [ollama.com/download](https://ollama.com/download) and run it. Ollama installs as a background service/tray app and automatically serves the API on `127.0.0.1:11434` — no need to manually run `ollama serve`.
 
**3. Pull the model used by the labs.**
 
Open PowerShell:
```
ollama pull granite4:3b
```
This downloads a few GB the first time only.
 
**4. Clone the repo and set up the Python environment.**
 
In PowerShell:
```
git clone https://github.com/skillrepos/prompt-accel.git
cd prompt-accel
python -m venv py_env
py_env\Scripts\Activate.ps1
pip install -r requirements.txt
```
 
If PowerShell blocks the activation script with an execution-policy error, run this once (in the same PowerShell window) and try activating again:
```
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
```
 
If you're using Command Prompt (`cmd.exe`) instead of PowerShell, activate with:
```
py_env\Scripts\activate.bat
```
 
You'll need to reactivate (`py_env\Scripts\Activate.ps1` or `activate.bat`) in any new terminal window you open for the labs.
 
**5. Verify everything is working.**
 
In PowerShell:
```
Invoke-RestMethod http://127.0.0.1:11434/api/tags
```
You should see `granite4:3b` listed. Then optionally run the warmup script:
```
python scripts/warmup.py
```
 
You're ready — continue to [Running the Labs](#running-the-labs) below.
 
---
 
## Running the Labs
 
Follow `labs.md` as written — the only difference from the Codespace is that you're working in your own terminal and editor (e.g., VS Code opened on the cloned `prompt-accel` folder) instead of the browser-based Codespace one. Any place labs.md says to open a file "in the Codespace editor," just open it in your local editor instead. Make sure your virtual environment (`py_env`) is activated in every terminal you use.
 
### Lab 5 — Prompting for an AI Agent
```
python agent.py       # macOS/Linux: python3 agent.py
```
Follow Steps 1–9 in `labs.md` exactly as written, editing the `SYSTEM` prompt in your local copy of `agent.py`.
 
### Lab 6 — Working with Prompts in MCP
This lab needs **two terminals**, both with `py_env` activated:
 
Terminal 1:
```
python mcp_server.py
```
Terminal 2:
```
python mcp_client_agent.py
```
Follow Steps 1–11 in `labs.md`, editing `mcp_server.py` locally and restarting both terminals when instructed.
 
---
 
## Troubleshooting
 
**"ollama: command not found" / not recognized** — The installer should put `ollama` on your PATH automatically. Close and reopen your terminal after installing. On Windows, also check that the Ollama tray icon is running.
 
**Port 11434 already in use** — Something else is already running Ollama (or a previous instance). On Mac, quit the Ollama menu-bar app or `killall ollama`; on Windows, check the system tray and exit Ollama, or restart the machine, then relaunch.
 
**Port 8000 already in use (Lab 6)** — `mcp_server.py` listens on `127.0.0.1:8000`. Close whatever else is using that port, or stop and restart the server after freeing it.
 
**`ModuleNotFoundError` for `fastmcp`, `langchain_ollama`, etc.** — Your virtual environment isn't activated, or `pip install -r requirements.txt` didn't complete. Re-activate `py_env` and rerun the install command.
 
**First response from the model is very slow** — Normal on CPU-only machines the first time a model loads into memory. Run `python scripts/warmup.py` before starting the labs to preload it, or just wait through the first request.
 
**Model not found errors** — Confirm the pull finished: `ollama list` should show `granite4:3b`. If it's missing, rerun `ollama pull granite4:3b`.
 
**Windows firewall prompt on first Ollama launch** — Allow access on your private network so the local API (`127.0.0.1:11434`) can be reached.
