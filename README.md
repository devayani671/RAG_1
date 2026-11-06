# RAG_1


# Python Project Setup Using UV (Windows)

## 0. Download UV manager for windows

## 1. Install UV

If uv is not installed:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Verify:

```bash
uv --version
```

## 2. Create a project

Open your project folder in VS Code.

For a new project, run:

```bash
uv init --python 3.11
```

This creates the project configuration, including:

- `pyproject.toml` – project details and dependencies
- `.python-version` – Python version
- `main.py` – starter Python file
- `README.md` – project description

**Note:** If the project already has a `pyproject.toml`, don't initialize it again.

## 3. Create a virtual environment

```bash
uv venv --python 3.11
```

The virtual environment keeps project dependencies separate from other Python projects.

## 4. Activate the virtual environment (optional)

For Windows CMD:

```cmd
.venv\Scripts\activate
```

You should see `(.venv)` in your terminal.

**Note:** You don't have to activate the environment if you use `uv run` or explicitly specify the Python interpreter.

## 5. Install dependencies

### Option A: You already have requirements.txt (recommended migration to uv)

Suppose your requirements.txt contains:

```text
fastapi
uvicorn
python-dotenv
pandas
numpy
```

Import these dependencies into your uv project:

```bash
uv add -r requirements.txt
```

This installs the dependencies and records them in `pyproject.toml`. It also updates `uv.lock`.

Then run:

```bash
uv sync
```

Now your project is managed by uv.


**Important:** Don't run `uv pip install -r requirements.txt` and then `uv sync` as consecutive steps unless the dependencies are also declared in your project configuration. `uv sync` follows `pyproject.toml` and `uv.lock` and may remove packages that aren't declared there.

## 6. Understand your project structure

After setup:

```text
my_project/
│
├── .venv/
│
├── pyproject.toml
│
├── uv.lock
│
├── .python-version
│
├── requirements.txt
│
├── main.py
│
└── README.md
```

- `.venv` – virtual environment
- `pyproject.toml` – project dependencies and configuration
- `uv.lock` – exact resolved dependency versions
- `.python-version` – Python version
- `requirements.txt` – optional dependency list for pip-compatible workflows

## 7. Select the Python interpreter in VS Code

1. Press `Ctrl + Shift + P`.
2. Search for `Python: Select Interpreter`.
3. Select the Python interpreter inside your project:

```text
my_project/.venv/Scripts/python.exe
```

This ensures VS Code runs your code using the correct environment.

## 8. Run your Python file

You can run:

```bash
uv run main.py
```

Or, if your virtual environment is activated:

```bash
python main.py
```

For FastAPI:

```bash
uv run uvicorn main:app --reload
```

## 9. Add new dependencies later

Whenever you need another package:

```bash
uv add langchain
```

For example:

```bash
uv add chromadb
uv add sentence-transformers
uv add requests
```

uv automatically updates your project configuration and lockfile.

## 10. Recreate your environment

If you delete your `.venv` folder or move your project to another computer:

```bash
uv sync
```

uv recreates the environment using the dependencies recorded in `pyproject.toml` and `uv.lock`.

## 11. Verify your environment

Check the Python version:

```bash
python --version
```

Check installed packages:

```bash
uv pip list
```

Check dependency compatibility:

```bash
uv pip check
```

Expected result:

```text
All installed packages are compatible
```

Test FastAPI:

```bash
uv run python -c "import fastapi; print(fastapi.__version__)"
```

## 12. Common problems

### FastAPI import error

Error:

```text
ModuleNotFoundError: No module named 'fastapi'
```

Solution:

```bash
uv add fastapi
```

Then select the correct `.venv` interpreter in VS Code.

### Wrong Python environment

Check:

```bash
python -c "import sys; print(sys.executable)"
```

It should point to:

```text
my_project/.venv/Scripts/python.exe
```

### Dependency conflicts

Run:

```bash
uv lock
uv sync
uv pip check
```

If the conflict remains, inspect the incompatible package version requirements before changing versions.

---

# My Recommended UV Workflow

For every new project:

```bash
# 1. Initialize project
uv init --python 3.11

# 2. Create virtual environment
uv venv --python 3.11

# 3. Import existing requirements (if available)
uv add -r requirements.txt

# 4. Synchronize environment
uv sync

# 5. Run project
uv run main.py
```

If there is no requirements.txt, skip step 3 and use `uv add package-name` for each dependency.

**Remember:** You only need to initialize a project once. After that, use `uv add` to install new dependencies and `uv sync` to recreate or synchronize the environment.

You can use this workflow for RAG, LangChain, LangGraph, FastAPI, Django and other Python projects.









uv run create_structure.py