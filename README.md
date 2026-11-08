# Restaurant Order Agent

A command-line food-ordering agent built with LangGraph and Groq. It extracts
one dish and quantity, checks the sample menu, handles unavailable/partial
orders, and simulates cooking and serving outcomes.

## Setup

Use Python 3.12 for the project environment. The existing `venv` was created
with Python 3.15, which may force pip to compile dependencies such as
`zstandard` from source on Windows. If the Python install manager reports that
no 3.12 runtime is installed, install it first. Run each command separately in
PowerShell from the `AIAGENT` project directory:

```powershell
py install 3.12
```

After that finishes, create a separate `.venv312`; this leaves the existing
`venv` untouched:

```powershell
py -3.12 -m venv .venv312
```

Only after that command succeeds, install the packages and start the app:

```powershell
.\.venv312\Scripts\python.exe -m pip install --upgrade pip
.\.venv312\Scripts\python.exe -m pip install -r requirements.txt
```

Set your Groq API key in `.env` (copy `.env.example` to `.env` and replace the
placeholder), or set it in the current PowerShell session:

```powershell
$env:GROQ_API_KEY = "your-groq-api-key"
.\.venv312\Scripts\python.exe main.py
```

The application uses Groq with the `openai/gpt-oss-120b` model configured in
`main.py`.

The `MENU` mapping in `main.py` is sample data. Replace its dish names and
integer inventory counts with the restaurant's menu. A dish absent from the
mapping, or with a count of zero, is unavailable.

Cooking and serving each have a 60% success probability by default. `simulate`
accepts scripted user inputs, cook outcomes, and serve outcomes for repeatable
scenario checks. The graph state tracks order, cook, and serve retry budgets;
the first cook and first serve do not use a retry. A failed serve requires both
a remaining serve retry and cook retry before another cook-and-serve cycle.
