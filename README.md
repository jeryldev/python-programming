# Python Programming - Learning Hub

## 📚 Overview

Central directory for all Python learning projects. Organized by course/resource, with flexibility to use either single directories with multiple scripts (for exercises) or separate projects with virtual environments (for larger applications).

## 🚀 Prerequisites & Setup

### Install Python

Python 3.9+ is recommended. Check if Python is installed:

```bash
python3 --version
pip3 --version
```

**macOS:**
```bash
# Using Homebrew (recommended)
brew install python@3.12

# Or download from python.org
# https://www.python.org/downloads/
```

**Linux:**
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install python3 python3-pip python3-venv

# Fedora
sudo dnf install python3 python3-pip

# Arch
sudo pacman -S python python-pip
```

**Windows:**
- Download from [python.org](https://www.python.org/downloads/)
- **Important**: Check "Add Python to PATH" during installation
- Or use Windows Store: `winget install Python.Python.3.12`

After installation, verify:
```bash
python3 --version
pip3 --version
```

### Install Development Tools

```bash
# Install uv - The fast Python package and project manager
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or with Homebrew
brew install uv

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Verify installation
uv --version

# Essential development tools (installed with uv tool)
uv tool install black         # Code formatter
uv tool install ruff         # Fast linter  
uv tool install mypy         # Type checker
uv tool install ipython      # Enhanced Python shell

# Jupyter for interactive development
uv pip install --system notebook jupyterlab
```

### Clone & Run This Repository

```bash
# Clone the repository
git clone <your-repo-url>
cd python_programming

# Navigate to a project
cd basics

# Run a Python script
python3 01_hello_world.py

# Or make it executable
chmod +x 01_hello_world.py
./01_hello_world.py  # Requires shebang: #!/usr/bin/env python3
```

### Neovim Setup (LazyVim)

For Neovim users with LazyVim, enable the Python language extra:

```vim
:LazyExtras
```
Then select and enable `lang.python` extra. This will install:
- **pyright** or **pylsp** - Python LSP servers
- **ruff** - Fast Python linter
- **black** - Code formatter
- **debugpy** - Debug adapter
- **Treesitter** - Python syntax highlighting
- **neotest-python** - Test runner integration

The Python extra requires LSP servers to be installed:
```bash
# Mason will handle this, or install manually:
npm install -g pyright
# Or
pip install python-lsp-server
```

## 🗂️ Directory Structure

```
~/code/python_programming/                          # This current directory
├── .gitignore                                     # Git ignore file
├── README.md                                      # This file
├── basics/                                        # Basic Python concepts
│   ├── 01_hello_world.py                        # First program
│   ├── 02_variables.py                          # Variables and data types
│   ├── 03_control_flow.py                       # if/else, loops
│   ├── 04_functions.py                          # Functions
│   └── 05_classes.py                            # Object-oriented programming
├── data_structures/                              # DS & algorithms
│   ├── lists_and_tuples.py
│   ├── dictionaries.py
│   └── sets.py
├── web_development/                              # Flask/Django projects
│   ├── flask_app/
│   └── django_project/
├── data_science/                                 # NumPy, Pandas, ML projects
│   ├── numpy_basics/
│   ├── pandas_analysis/
│   └── machine_learning/
├── automation/                                   # Scripts and automation
│   ├── file_organizer.py
│   └── web_scraper.py
├── personal_projects/                           # Your own Python applications
│   └── todo_app/
│       ├── requirements.txt
│       ├── src/
│       └── tests/
└── experiments/                                 # Quick tests and explorations
    └── notebooks/                               # Jupyter notebooks
```

## 🚀 Two Workflows

### Workflow A: Simple Scripts (For Learning/Small Programs)

Perfect for learning and small scripts:

```bash
# Navigate to learning directory
cd ~/code/python_programming/basics

# Create new scripts
py_new 01_hello_world
py_new 02_variables
py_new 03_lists

# Write your code (example for 01_hello_world.py)
cat > 01_hello_world.py << 'EOF'
#!/usr/bin/env python3
"""Hello World - First Python Program"""

def main():
    print("=== 01_hello_world ===\n")
    
    # Basic output
    print("Hello, World!")
    
    # Using f-strings (Python 3.6+)
    name = "Pythonista"
    year = 2025
    print(f"Hello, {name}! Learning Python in {year}")
    
    # Multiple ways to format
    print("Using format: {}".format(name))
    print("Using %s: %s" % ("old style", "formatting"))

if __name__ == "__main__":
    main()
EOF

# Run the script
python3 01_hello_world.py

# Or using helper function
py_run 1  # Run script #1
```

### Workflow B: UV Projects (Fast & Modern Python Management)

Using `uv` for fast virtual environment and dependency management:

```bash
cd ~/code/python_programming/personal_projects

# Initialize a new Python project with uv
uv init my_app
cd my_app

# Create and activate virtual environment
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Add dependencies
uv add requests flask
uv add --dev pytest black ruff mypy

# Or install from requirements.txt
uv pip install -r requirements.txt

# Sync dependencies (ensures exact versions)
uv pip sync requirements.txt

# Run without activating venv
uv run python src/main.py
uv run pytest

# Compile dependencies (lock versions)
uv pip compile requirements.in -o requirements.txt
```

### Workflow C: UV Tool Management (Global Python Tools)

Using `uv` to manage Python CLI tools globally:

```bash
# Install tools globally (no venv needed)
uv tool install ruff
uv tool install black
uv tool install pytest
uv tool install cookiecutter
uv tool install pre-commit

# Run tools directly
uv tool run black .
uv tool run ruff check .

# Or if added to PATH
black .
ruff check .

# List installed tools
uv tool list

# Upgrade tools
uv tool upgrade black
uv tool upgrade --all
```

## 🛠️ Shell Helper Functions

Add to your `.zshrc`:

```bash
#=============================================================================
#                     Python Programming Helper Functions
#=============================================================================

# Navigation
alias py_learn='cd ~/code/python_programming'
alias py_basics='cd ~/code/python_programming/basics'
alias py_proj='cd ~/code/python_programming/personal_projects'
alias py_data='cd ~/code/python_programming/data_science'

# Python version management
alias py='python3'
alias pip='pip3'

# List all Python files in current directory
py_list() {
    echo "Available Python scripts:"
    ls -1 *.py 2>/dev/null | sed 's/\.py$//' | nl
}

# Run by name or number
py_run() {
    if [ -z "$1" ]; then
        py_list
        echo "\nUsage: py_run <name or number>"
        return 1
    fi

    local file
    if [[ "$1" =~ ^[0-9]+$ ]]; then
        file=$(ls -1 *.py 2>/dev/null | sed -n "${1}p" | sed 's/\.py$//')
        [ -z "$file" ] && echo "No file #$1" && return 1
    else
        file="$1"
    fi

    echo "Running $file.py..."
    echo "---"
    python3 "$file.py"
}

# Create new Python file with template
py_new() {
    [ -z "$1" ] && echo "Usage: py_new <filename>" && return 1

    local filename="${1%.py}.py"

    cat > "$filename" << 'EOF'
#!/usr/bin/env python3
"""
Module description goes here
"""

def main():
    """Main function"""
    print(f"=== ${1%.py} ===\n")
    
    # Your code here
    

if __name__ == "__main__":
    main()
EOF

    chmod +x "$filename"
    echo "Created: $filename"
    echo "Run with: python3 $filename or py_run ${filename%.py}"
}

# Create test file
py_test_new() {
    [ -z "$1" ] && echo "Usage: py_test_new <name>" && return 1

    local filename="test_${1%.py}.py"

    cat > "$filename" << 'EOF'
#!/usr/bin/env python3
"""Tests for ${1%.py} module"""

import pytest


def test_example():
    """Example test"""
    assert 2 + 2 == 4


def test_${1%.py}_functionality():
    """Test ${1%.py} specific functionality"""
    # Your test here
    result = True
    assert result is True


if __name__ == "__main__":
    pytest.main([__file__])
EOF

    echo "Created: $filename"
    echo "Run with: pytest $filename"
}

# UV virtual environment helpers
py_venv_create() {
    uv venv ${1:-.venv}
    echo "Created virtual environment: ${1:-.venv}"
    echo "Activate with: source ${1:-.venv}/bin/activate"
}

py_venv_activate() {
    local venv_path="${1:-.venv}"
    if [ -d "$venv_path" ]; then
        source "$venv_path/bin/activate"
    else
        echo "No virtual environment found at $venv_path"
        echo "Create one with: uv venv"
    fi
}

# Quick project setup with uv
py_project_init() {
    [ -z "$1" ] && echo "Usage: py_project_init <project_name>" && return 1
    
    # Use uv to initialize project
    uv init "$1"
    cd "$1"
    
    # Create virtual environment
    uv venv
    source .venv/bin/activate
    
    # Create additional structure
    mkdir -p src tests docs
    touch src/__init__.py
    
    # Add common dev dependencies
    uv add --dev pytest black ruff mypy pre-commit
    
    # Create pre-commit config
    cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.9
    hooks:
      - id: ruff
      - id: ruff-format

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.8.0
    hooks:
      - id: mypy
        additional_dependencies: [types-all]
EOF

    # Initialize pre-commit
    uv run pre-commit install
    
    echo "Created project: $1 with uv"
    echo "Virtual environment activated"
    echo "Install packages with: uv add <package>"
    echo "Install dev packages with: uv add --dev <package>"
}

# UV-specific project initialization with Python version
py_uv_init() {
    local name="$1"
    local python_version="${2:-3.12}"
    
    [ -z "$name" ] && echo "Usage: py_uv_init <project_name> [python_version]" && return 1
    
    uv init --python "$python_version" "$name"
    cd "$name"
    uv venv
    source .venv/bin/activate
    
    echo "Created $name with Python $python_version"
}

# Install packages with uv
py_add() {
    if [ -z "$1" ]; then
        echo "Usage: py_add <package> [package2...]"
        return 1
    fi
    uv add "$@"
}

# Install dev packages with uv
py_add_dev() {
    if [ -z "$1" ]; then
        echo "Usage: py_add_dev <package> [package2...]"
        return 1
    fi
    uv add --dev "$@"
}

# Run command in uv environment without activation
py_uv_run() {
    uv run "$@"
}

# Sync dependencies from requirements.txt
py_sync() {
    if [ -f "requirements.txt" ]; then
        uv pip sync requirements.txt
    elif [ -f "pyproject.toml" ]; then
        uv sync
    else
        echo "No requirements.txt or pyproject.toml found"
    fi
}

# Compile dependencies (pin versions)
py_lock() {
    if [ -f "requirements.in" ]; then
        uv pip compile requirements.in -o requirements.txt
        echo "Compiled requirements.in -> requirements.txt"
    elif [ -f "pyproject.toml" ]; then
        uv lock
        echo "Created uv.lock file"
    else
        echo "No requirements.in or pyproject.toml found"
    fi
}

# Run with auto-reload (for development)
py_watch() {
    [ -z "$1" ] && echo "Usage: py_watch <script>" && return 1
    
    # Check if watchdog is installed
    if ! python3 -c "import watchdog" 2>/dev/null; then
        echo "Installing watchdog..."
        pip install watchdog
    fi
    
    watchmedo auto-restart --patterns="*.py" --recursive -- python3 "$1.py"
}

# Format Python code
py_format() {
    if command -v black &> /dev/null; then
        black ${1:-.}
    elif command -v autopep8 &> /dev/null; then
        autopep8 --in-place --recursive ${1:-.}
    else
        echo "No formatter found. Install with: pip install black"
    fi
}

# Lint Python code
py_lint() {
    if command -v ruff &> /dev/null; then
        ruff check ${1:-.}
    elif command -v flake8 &> /dev/null; then
        flake8 ${1:-.}
    elif command -v pylint &> /dev/null; then
        pylint ${1:-.}
    else
        echo "No linter found. Install with: pip install ruff"
    fi
}

# Type check
py_type() {
    if command -v mypy &> /dev/null; then
        mypy ${1:-.}
    else
        echo "mypy not found. Install with: pip install mypy"
    fi
}

# Run tests
py_test() {
    if [ -f "pytest.ini" ] || [ -f "setup.cfg" ] || [ -f "pyproject.toml" ]; then
        pytest ${1:-.}
    elif [ -d "tests" ]; then
        python3 -m pytest tests/
    else
        python3 -m pytest ${1:-.}
    fi
}

# Start Jupyter notebook
py_notebook() {
    jupyter notebook ${1:-.}
}

# Python REPL with common imports
py_repl() {
    python3 -i -c "
import os, sys, json, re
from pathlib import Path
from datetime import datetime, timedelta
from collections import defaultdict, Counter
from itertools import chain, combinations, permutations
from functools import partial, reduce
import math

print('Common modules imported!')
print('Available: os, sys, json, re, Path, datetime, collections, itertools, functools, math')
"
}

# Install common data science packages
py_ds_setup() {
    pip install numpy pandas matplotlib seaborn scikit-learn jupyter ipython
    echo "Installed: numpy, pandas, matplotlib, seaborn, scikit-learn, jupyter"
}

# Install common web development packages
py_web_setup() {
    pip install flask django fastapi uvicorn requests beautifulsoup4
    echo "Installed: flask, django, fastapi, uvicorn, requests, beautifulsoup4"
}
```

## ⚡ Quick Commands

### Shell Commands

```bash
# Navigation
py_learn             # Go to main Python directory
py_basics            # Go to basics directory
py_proj              # Go to personal projects
py_data              # Go to data science directory

# Working with scripts
py_list              # Show all Python scripts (numbered)
py_run 1             # Run script #1
py_run hello         # Run by name
py_new 05_loops      # Create new script
py_test_new utils    # Create test file
py_watch script      # Auto-reload on changes

# UV Project management
py_project_init app  # Create new project with uv
py_uv_init app 3.11  # Create with specific Python version
py_venv_create       # Create virtual environment with uv
py_venv_activate     # Activate virtual environment
py_add requests      # Add package with uv
py_add_dev pytest    # Add dev package with uv
py_sync              # Sync dependencies
py_lock              # Lock/compile dependencies
py_uv_run python app.py  # Run without activating venv

# Code quality
py_format            # Format code with black
py_lint              # Lint with ruff
py_type              # Type check with mypy
py_test              # Run tests with pytest

# Interactive
py_repl              # Python with common imports
py_notebook          # Start Jupyter notebook

# Setup helpers
py_ds_setup          # Install data science packages
py_web_setup         # Install web dev packages
```

### Common uv Commands

```bash
# Project management
uv init project_name         # Initialize new project
uv venv                      # Create virtual environment
uv add package               # Add dependency
uv add --dev package         # Add dev dependency
uv remove package            # Remove dependency
uv sync                      # Sync all dependencies
uv lock                      # Create lock file

# Running code
uv run python script.py      # Run with uv environment
uv run pytest               # Run tests
uv run black .              # Format code

# Tool management
uv tool install ruff        # Install tool globally
uv tool run ruff check      # Run tool
uv tool list               # List installed tools
uv tool upgrade --all      # Upgrade all tools

# Pip compatibility
uv pip install package      # Install package
uv pip compile requirements.in  # Compile requirements
uv pip sync requirements.txt    # Sync exact versions
```

## 📖 Example: Starting Your Python Journey

### Quick Start with Scripts

```bash
# 1. Navigate to Python programming directory
cd ~/code/python_programming

# 2. Create basics directory
mkdir basics
cd basics

# 3. Create first programs
py_new 01_hello_world
py_new 02_variables
py_new 03_data_types

# 4. Run programs
py_run 01_hello_world
py_list              # See all programs
py_run 1             # Run by number
```

### Creating a Project with uv

```bash
# 1. Create a new project with uv
cd ~/code/python_programming/personal_projects
py_project_init my_first_app  # or: uv init my_first_app

# 2. Project is created and venv activated
# Add dependencies
py_add requests rich  # or: uv add requests rich
py_add_dev pytest black ruff  # or: uv add --dev pytest black ruff

# 3. Create your application
cat > src/main.py << 'EOF'
#!/usr/bin/env python3
"""My First UV-managed Python App"""

from rich.console import Console
from rich.table import Table
import requests

console = Console()

def fetch_python_info():
    """Fetch Python package info from PyPI"""
    response = requests.get("https://pypi.org/pypi/uv/json")
    data = response.json()
    return data["info"]

def main():
    console.print("[bold blue]My First UV App![/bold blue]\n")
    
    info = fetch_python_info()
    
    table = Table(title="UV Package Info")
    table.add_column("Field", style="cyan")
    table.add_column("Value", style="green")
    
    table.add_row("Name", info["name"])
    table.add_row("Version", info["version"])
    table.add_row("Summary", info["summary"])
    table.add_row("Author", info["author"])
    
    console.print(table)

if __name__ == "__main__":
    main()
EOF

# 4. Run without activating venv
uv run python src/main.py
# Or if venv is activated
python src/main.py

# 5. Run tests
uv run pytest

# 6. Format and lint
uv run black src/
uv run ruff check src/

# 7. Lock dependencies for reproducible builds
uv lock  # Creates uv.lock file
```

## 🎯 Learning Path

### Beginner

1. **Basics** - Variables, data types, operators
2. **Control Flow** - if/else, loops, comprehensions
3. **Functions** - Parameters, return values, lambdas
4. **Data Structures** - Lists, tuples, dicts, sets
5. **File I/O** - Reading/writing files, JSON, CSV

### Intermediate

1. **OOP** - Classes, inheritance, polymorphism
2. **Modules & Packages** - Importing, creating packages
3. **Error Handling** - try/except, custom exceptions
4. **Regular Expressions** - Pattern matching
5. **Decorators & Generators** - Advanced functions

### Advanced

1. **Async Programming** - asyncio, async/await
2. **Type Hints** - Static typing, mypy
3. **Testing** - unittest, pytest, mocking
4. **Web Development** - Flask, Django, FastAPI
5. **Data Science** - NumPy, Pandas, scikit-learn

### Specializations

**Web Development:**
- Flask/Django for web apps
- FastAPI for APIs
- SQLAlchemy for databases

**Data Science:**
- NumPy for numerical computing
- Pandas for data analysis
- Matplotlib/Seaborn for visualization
- scikit-learn for machine learning

**Automation:**
- Selenium for web automation
- Beautiful Soup for web scraping
- Schedule for task automation

## 🔗 Resources

- [Official Python Tutorial](https://docs.python.org/3/tutorial/)
- [Python.org Documentation](https://docs.python.org/3/)
- [Real Python](https://realpython.com/)
- [Python Crash Course](https://ehmatthes.github.io/pcc/)
- [Automate the Boring Stuff](https://automatetheboringstuff.com/)
- [Python for Data Analysis](https://wesmckinney.com/book/)
- [Full Stack Python](https://www.fullstackpython.com/)

## 💡 Tips

1. **Use uv for speed** - 10-100x faster than pip
2. **Always use virtual environments** - Keep dependencies isolated
3. **Follow PEP 8** - Python style guide
4. **Write docstrings** - Document your functions
5. **Use type hints** - Improve code clarity
6. **Test your code** - pytest makes it easy
7. **Use f-strings** - Modern string formatting
8. **Lock dependencies** - Use `uv lock` for reproducible builds

## 🚄 Why uv?

**uv** is a blazing-fast Python package manager written in Rust that replaces pip, pip-tools, pipx, poetry, pyenv, and virtualenv. Key benefits:

- **⚡ Speed** - 10-100x faster than pip
- **🔒 Reliable** - Consistent dependency resolution
- **📦 All-in-one** - Replaces multiple tools
- **🎯 Drop-in replacement** - Compatible with pip commands
- **🔄 Reproducible** - Lock files for exact versions
- **🐍 Python management** - Install and manage Python versions
- **🛠️ Tool management** - Install CLI tools globally without conflicts

## 🐍 Pythonic Principles

- **Readability counts** - Code is read more than written
- **Explicit is better than implicit** - Be clear
- **Simple is better than complex** - KISS principle
- **Flat is better than nested** - Avoid deep nesting
- **Errors should never pass silently** - Handle exceptions
- **There should be one obvious way** - The Zen of Python

Run `python3 -c "import this"` to see the complete Zen of Python.

## ⚠️ Common Pitfalls

- **Mutable default arguments** - Don't use `[]` or `{}` as defaults
- **Late binding closures** - Be careful with lambdas in loops
- **Modifying lists while iterating** - Use copy or list comprehension
- **Using `is` for equality** - Use `==` for values, `is` for identity
- **Not using context managers** - Use `with` for files and resources
- **Global variables** - Avoid or use sparingly

---

_Happy Learning! 🐍_