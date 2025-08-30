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
# Virtual environment (usually included with Python 3.3+)
python3 -m pip install --upgrade pip

# Essential development tools
pip3 install --user pipx  # For installing Python applications
pipx ensurepath

# Code quality tools
pipx install black         # Code formatter
pipx install ruff         # Fast linter
pipx install mypy         # Type checker
pipx install poetry       # Dependency management

# Jupyter for interactive development
pip3 install --user notebook ipython
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

### Workflow B: Virtual Environment Projects (For Real Applications)

For substantial projects with dependencies:

```bash
cd ~/code/python_programming/personal_projects

# Create new project with virtual environment
mkdir my_app && cd my_app

# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install requests flask pytest

# Save dependencies
pip freeze > requirements.txt

# Create project structure
mkdir src tests docs
touch src/__init__.py src/main.py
touch tests/test_main.py
touch README.md

# Deactivate when done
deactivate
```

### Workflow C: Poetry Projects (Modern Dependency Management)

Using Poetry for better dependency management:

```bash
cd ~/code/python_programming/personal_projects

# Create new Poetry project
poetry new my_modern_app
cd my_modern_app

# Add dependencies
poetry add requests
poetry add --group dev pytest black ruff

# Run within Poetry environment
poetry run python src/main.py
poetry run pytest

# Activate shell
poetry shell
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

# Virtual environment helpers
py_venv_create() {
    local name="${1:-venv}"
    python3 -m venv "$name"
    echo "Created virtual environment: $name"
    echo "Activate with: source $name/bin/activate"
}

py_venv_activate() {
    local name="${1:-venv}"
    if [ -d "$name" ]; then
        source "$name/bin/activate"
    else
        echo "No virtual environment found at $name"
    fi
}

# Quick virtual environment setup
py_project_init() {
    [ -z "$1" ] && echo "Usage: py_project_init <project_name>" && return 1
    
    mkdir -p "$1" && cd "$1"
    python3 -m venv venv
    source venv/bin/activate
    
    # Create project structure
    mkdir -p src tests docs
    touch src/__init__.py
    touch src/main.py
    touch tests/__init__.py
    touch tests/test_main.py
    touch requirements.txt
    touch requirements-dev.txt
    touch README.md
    touch .gitignore
    
    # Basic .gitignore
    cat > .gitignore << 'EOF'
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
env/
ENV/

# IDE
.vscode/
.idea/
*.swp
*.swo

# Testing
.pytest_cache/
.coverage
htmlcov/

# Distribution
dist/
build/
*.egg-info/
EOF

    # Basic README
    cat > README.md << EOF
# $1

## Setup

\`\`\`bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
\`\`\`

## Development

\`\`\`bash
pip install -r requirements-dev.txt
pytest
\`\`\`
EOF

    echo "Created project: $1"
    echo "Virtual environment activated"
    echo "Install packages with: pip install <package>"
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

# Project management
py_project_init app  # Create new project with venv
py_venv_create       # Create virtual environment
py_venv_activate     # Activate virtual environment

# Code quality
py_format            # Format code with black
py_lint              # Lint with ruff/flake8
py_type              # Type check with mypy
py_test              # Run tests with pytest

# Interactive
py_repl              # Python with common imports
py_notebook          # Start Jupyter notebook

# Setup helpers
py_ds_setup          # Install data science packages
py_web_setup         # Install web dev packages
```

### Common pip Commands

```bash
pip install package           # Install package
pip install -r requirements.txt  # Install from file
pip freeze > requirements.txt    # Save dependencies
pip list                      # List installed packages
pip show package              # Show package info
pip uninstall package         # Remove package
pip install --upgrade package # Update package
```

## 📖 Example: Starting Your Python Journey

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
py_new 04_control_flow
py_new 05_functions

# 4. Edit and enhance the hello world program
cat > 01_hello_world.py << 'EOF'
#!/usr/bin/env python3
"""Hello World - Demonstrating Python basics"""

def greet(name: str, language: str = "Python") -> str:
    """Generate a greeting message"""
    return f"Hello, {name}! Welcome to {language} programming!"

def main():
    print("=== 01_hello_world ===\n")
    
    # Basic output
    print("Hello, World!")
    
    # Variables and f-strings
    name = "Developer"
    year = 2025
    print(f"Hello, {name}! Learning Python in {year}")
    
    # Using functions
    message = greet("Alice")
    print(message)
    
    # List and loop
    languages = ["Python", "Rust", "C", "Zig"]
    for lang in languages:
        print(f"  - {lang} is awesome!")
    
    # Dictionary
    info = {
        "language": "Python",
        "version": "3.12",
        "type": "interpreted"
    }
    print(f"\nUsing {info['language']} {info['version']}")

if __name__ == "__main__":
    main()
EOF

# 5. Run and test
py_list              # See all programs
py_run 1             # Run first program
py_run 01_hello_world   # Or run by name

# 6. Create and run a test
py_test_new 01_hello_world
pytest test_01_hello_world.py

# 7. Format and lint
py_format 01_hello_world.py
py_lint 01_hello_world.py
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

1. **Use virtual environments** - Keep dependencies isolated
2. **Follow PEP 8** - Python style guide
3. **Write docstrings** - Document your functions
4. **Use type hints** - Improve code clarity
5. **Test your code** - pytest makes it easy
6. **Use f-strings** - Modern string formatting
7. **List comprehensions** - Pythonic and efficient
8. **Learn the stdlib** - Python's batteries included

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