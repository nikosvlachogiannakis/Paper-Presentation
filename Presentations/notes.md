<!-- night_gold.css Theme -->

# Notes

Nikolaos Vlachogiannakis, 2025

---

# Table of Contents

--

<div style="display: flex; text-align: left; width: 70vw; margin-left: -10vw;">

<div style="flex: 1;">

1. [Connect Remotely with SSH](#/2)
    - [Password](#/2/1)
    - [Passwordless](#/2/2)
2. [Create Repository & Git Push](#/3)
    - [Manually](#/3/1)
    - [Using gh](#/3/2)
    - [Push Changes on GitHub](#/3/3)
    - [LazyGit](#/3/4)
    - [Check GitHub Connection](#/3/5)
    - [Additional Information](#/3/6)
      - [What is SSH?](#/3/7)
      - [What is gh?](#/3/7)
      - [What is LazyGit?](#/3/8)
3. [Create a Project and use Poetry](#/4)
    - [Step-by-Step Process](#/4/1)
    - [Additional Information](#/4/5)
      - [What is Poetry?](#/4/6)
      - [TOML File](#/4/7)

</div>

<div style="flex: 1;">

4. [Advanced Workflow](#/5)
    - [Step By Step Process](#/5/1)
    - [Create Documentation using Sphinx](#/5/10)
    - [Additional Information](#/5/14)
      - [What is pre-commit?](#/5/15)
      - [Why use it?](#/5/16)
      - [Common Tools](#/5/17)
      - [How it works](#/5/18)
      - [Python REPL](#/5/19)
      - [YAML File](#/5/20)
      - [Traceback](#/5/21)
5. [Advanced Workflow <span style="color:lime;">Better</span>](#/6)
    - [Pre-commits](#/6/1)
    - [Install & Set Up](#/6/2)
    - [Usage](#/6/3)
    - [Linters & Formatters](#/6/4)
    - [Numpy Docstrings](#/6/6)
    - [Generate Documentation](#/6/8)
    - [Additional Information](#/6/15)
      - [.rst File](#/6/16)

</div>

<div style="flex: 1;">

6. [Reveal.js Presentation](#/7)
    - [Install Node.js](#/7/1)
    - [Get a Template](#/7/2)
    - [Change Theme](#/7/4)
    - [Slides Transition](#/7/5)
    - [Change Presentations](#/7/6)
    - [Load Slides Locally](#/7/7)
    - [Hosting Slides on GitHub Pages](#/7/8)
    - [Public vs Private Repos and Ways to Share Slides](#/7/10)
    - [Managing Large Files with Git LFS](#/7/13)
    - [Additional Information](#/7/16)
      - [What is Reveal.js?](#/7/17)
      - [HTML File](#/7/18)
      - [CSS File](#/7/19)
      - [Markdown File](#/7/20)
      - [JSON File](#/7/21)

</div>

</div>

--

## Extra

<div style="display: flex; gap: 4em; text-align: left;">

<div style="flex: l;">

7. [Git Commands](#/8)
    - [Git Cheatsheet](#/8/1)
    - [Version Control](#/8/3)
    - [Branching & Merging](#/8/14)
    - [Bug Tracking & Fixing](#/8/25)
    - [Git Hooks (Automation)](#/8/36)

8. [Store images as PDFs](#/9)
    - [On Windows](#/9/1)
    - [On Linux](#/9/2)

</div>

<div style="flex: l">

9. [Cheat Sheet](#/10)
    - [File & Directory Management](#/10/1)
    - [File Viewing & Editing](#/10/2)
    - [Search & Navigation](#/10/3)
    - [System Utilities](#/10/4)
    - [Package Management(APT)](#/10/5)
    - [Python & pyenv](#/10/6)
    - [SSH & Remote Access](#/10/7)
    - [FTP & File Transfer](#/10/8)
    - [Shortcuts](#/10/9)
    - [Git Essentials - Part 1](#/10/10)
    - [Git Essentials - Part 2](#/10/11)

</div>

</div>

---

# How to connect to a Remote Machine?

--

# With Password

<pre><code class="language-bash" data-trim>
ssh user@remote-server
</code></pre>

--

# Passwordless Login

--

## 1. Generate an SSH key(If you don't have one)

Locally, on your computer, open Terminal(for Windows, Git Bash):

<pre><code class="language-bash" data-trim>
ssh-keygen -t ed25519 -C "your_email@example.com"
</code></pre>

- Press Enter to accept the default path: C:\Users\YourName\.ssh\id_ed25519

- Choose no passphrase to enable passwordless login

## 2. Copy the Public Key to the Remote Server

<pre><code class="language-bash" data-trim>
ssh-copy-id user@remote-server
</code></pre>

--

## 3. Use SSH Config File
Create or edit:
<pre><code class="language-bash" data-trim>
~/.ssh/config
</code></pre>
Add:
<pre><code class="language-bash" data-trim>
Host myserver
  HostName REMOTE_SERVER
  User USERNAME
  IdentityFile ~/.ssh/id_ed25519
</code></pre>
Now you can simply run:
<pre><code class="language-bash" data-trim>
ssh myserver
</code></pre>

## 4. Test the Passwordless SSH Login
<pre><code class="language-bash" data-trim>
ssh user@remote-server
</code></pre>
You should be logged in without a password.

### Log Out

Press <span style="color:skyblue">Ctrl + d</span>

---

# Create a Repository on GitHub (3 Options)
# & Push Changes

--

# 1. Create a repo on GitHub and clone it locally

- Create a Repository on GitHub(either public or private) with a ReadMe file

- Create a folder:
<pre><code class="language-bash" data-trim>
mkdir folder_name
</code></pre>
- Go to its path:
<pre><code class="language-bash" data-trim>
cd folder_name
</code></pre>
- Copy the link of the SSH key(ssh_key) from your repository in GitHub and write the 
following in terminal:
<pre><code class="language-bash" data-trim>
git clone ssh_key
</code></pre>
- You can open the file we cloned in VScode and work on it.

--

# 2. Create a repo locally and push to GitHub

- Go to GitHub and create a repo. Name it the same as your local repo and do not create 
a README.md (you already have one locally). You want your repo to be completely empty.
- Once the the repo is created, go to Code → SSH and copy the repo SSH link.
- Locally, on your cmd/bash do:
<pre><code class="language-bash" data-trim>
git remote add origin ssh_link-you-copied
git branch -M main
git push -u origin main
</code></pre>

Your commits should be pushed to GitHub and your local repo is connected to the GitHub 
repo.

--

# 3. Using GitHub CLI (gh)

- Install [GitHub CLI](https://github.com/cli). 
After successful installation of gh(install shell repo and gh will be installed too), 
asks for a token. Follow the link and create a token with the proposed permissions. 
After this action you can use gh to do things in Github. The next step creates a new 
ssh key and uploads it to Github using the gh tool. Instead of copying the contents of 
the pub file, going to Github settings and creating a new key entry, this is done 
automatically using gh.
- Connect it to you GitHub with 
<code class="language-bash" style="display: inline-block;background: #222;" data-trim>gh
 auth login</code> and follow the promts
- To create a new repo in the current folder, do:
<pre><code class="language-bash" data-trim>
git init
git add .
git commit -m "Initial commit"
gh repo create REPO_NAME --public --source=. --push
</code></pre>
- Clone a repo:
<pre><code class="language-bash" data-trim>
gh repo clone arampatzis/shell
</code></pre>
- See all your repos in Github:
<pre><code class="language-bash" data-trim>
gh repo list
</code></pre>

--

# Push changes on GitHub

<pre><code class="language-bash" data-trim>
cd repository_name
</code></pre>

<pre><code class="language-bash" data-trim>
git status
</code></pre>
With this command we can see all changed files which are 
<span style="color:red">red</span>.

So, we use:
<pre><code class="language-bash" data-trim>
git add (all files on red separated by comma)
</code></pre>
<span style="color:skyblue">OR</span>
<pre><code class="language-bash" data-trim>
git add .
</code></pre>
Then:
<pre><code class="language-bash" data-trim>
git commit -m "This is a message explaining what changes we made"
</code></pre>
Finally:
<pre><code class="language-bash" data-trim>
git push
</code></pre>
Now, all the changes you made are uploaded in your repository on Github!

--

# Otherwise, Use LazyGit

- Inside the folder you want to push, write:
<pre><code class="language-bash" data-trim>
lazygit
</code></pre>
- A new window will appear where, in the upper left side it will be all the files that 
have changes and in order to include each file in your push Press 
<span style="color:skyblue">Spacebar</span>.

- After selecting all the files we want to push, Press 
<span style="color:skyblue">c</span>
 and write what changes you have made and then Press 
 <span style="color:skyblue">Enter</span>.

- To push these changes in your GitHub Press <span style="color:skyblue">Shift+P</span>
- To Exit LazyGit Press <span style="color:skyblue">q</span>

--

# Important Note

- When you have large files(e.g. .mp4 files) that are stored in git-lfs and try to clone
 a repository from GitHub, you have to run 
<code class="language-bash " data-trim style="color:green;">git lfs pull</code> in 
rder to get the actual video and not a pointer file to it.

- Otherwise, if you want to replace the video, make sure to do it before running git 
add, using a proper .mp4 file (e.g., downloaded from the web)

--

# GitHub Connection

If we want to check if GitHub is connected with our machine using ssh:
<pre><code class="language-bash" data-trim>
ssh -T git@github.com
</code></pre>
and you should get a message like:
<pre><code class="language-bash" data-trim>
Hi username!You've successfully authenticated, but GitHub does not provide shell access.
</code></pre>

--

# Additional Information

## What is SSH?

<span style="color:skyblue">SSH (Secure Shell)</span> is a cryptographic network 
protocol that allows you to securely connect to remote computers or servers over an 
unsecured network. It's commonly used for accessing and managing servers, transferring 
files, and authenticating with services like GitHub. SSH works by using a key pair: a 
private key stored on your device and a public key shared with the remote service. When 
you connect, SSH verifies your identity using these keys, ensuring encrypted, secure 
communication without needing to send passwords.

## What is gh?

<span style="color:skyblue">gh</span> is Github's command line interface (CLI) that can 
be used to manage your github without leaving the command line and the keyboard.

--

## What is LazyGit?

<span style="color:skyblue">Lazygit</span> is a simple, fast, terminal-based UI for Git.

It lets you manage your Git repositories more easily than typing out commands - you can 
stage, commit, push, pull, resolve conflicts, and browse logs interactively, all from a 
clean text interface.

It’s especially useful when you want the power of Git without memorizing all the 
commands, and it works entirely in your terminal without needing a graphical IDE.

## IDE(Integrated Development Environment)

<span style="color:skyblue">IDE</span> is a software application that provides a set of 
tools for programmers to write, test, and debug their code more efficiently - all in one
 place. 

Examples of IDEs: PyCharm, Visual Studio, Eclipse.

---

# Create a Project with Poetry

--

## 1. Open Terminal and Create a new Poetry Project

<pre><code class="language-bash" data-trim>
poetry new my_project
</code></pre>

This creates:

<pre><code class="language-markdown" data-trim>
my_project/
├── my_project/
│   └── __init__.py
├── tests/
│   └── __init__.py
│   └── test_my_project.py
├── pyproject.toml
</code></pre>

## 2. Move Into Your Project Directory

<pre><code class="language-bash" data-trim>
cd my_project
</code></pre>

## 3. Open in VS Code

<pre><code class="language-bash" data-trim>
code .
</code></pre>

--

## 4. Set Up the Python Environment

Inside the project folder:

<pre><code class="language-bash" data-trim>
poetry shell
</code></pre>

This activates the virtual environment.

If you want to just install dependencies without activating the shell:

<pre><code class="language-bash" data-trim>
poetry install
</code></pre>

--

## 5. Configure VS Code to Use Poetry's Environment

VS Code needs to know about Poetry’s virtual environment.

### Automatically:

When you run <span style="color:skyblue">poetry shell</span>, then start VS Code using 
<span style="color:skyblue">code .</span> from the same shell - it usually detects it.

### Manually:

1. Press <span style="color:skyblue">Ctrl+Shift+P</span> (Command Pallete)
2. Select: <span style="color:skyblue">Python: Select Interpreter</span>
3. Choose the one inside <span style="color:skyblue">.venv</span> or the one Poetry 
created in cache.

--

## 6. Add Dependencies

Add a package:

<pre><code class="language-bash" data-trim>
poetry add requests
</code></pre>

Add a dev dependency (e.g. pytest):

<pre><code class="language-bash" data-trim>
poetry add --dev pytest
</code></pre>


--

# Additional Information

--

## What is Poetry?

<span style="color:skyblue">Poetry</span> is a tool for managing Python projects. It 
helps you declare, install, and resolve dependencies, build your package, and publish 
it. Unlike using pip and virtualenv separately, Poetry handles both dependency 
management and virtual environments seamlessly.

It’s widely used because it creates a pyproject.toml file (modern Python standard), 
ensures reproducible builds, and makes it easy to develop and distribute Python 
packages.

--

## TOML File

A <span style="color:skyblue">.toml</span> file (short for Tom’s Obvious, Minimal 
Language) is a simple configuration file format that is easy for humans to read and 
write.

In Python projects - especially when using tools like Poetry - the pyproject.toml file 
is used to define the project’s metadata, dependencies, build settings, and tool 
configurations.

---

# Advanced Workflow

Create a well-organized Python project using Poetry, adding tools to improve code 
quality, debugging, interactivity and documentation, while keeping dependencies cleanly 
grouped into dev and docs environments

--

## Inside the project path

1. Download and add <span style="color:skyblue">pre-commit</span> - a tool that 
automatically runs checks like formatting or linting before you commit code - into your 
project using Poetry. The <span style="color:skyblue">--group dev</span> flag tells 
Poetry not to include pre-commit in your project's main dependencies. Instead, it places
 it under <span style="color:skyblue">[tool.poetry.group.dev.dependencies]</span> in 
your project.toml (pre-commit = "^x.y.z"). This means it's used only during 
development - not needed by users who install your package in production.

<pre ><code class="language-bash" data-trim>
poetry add --group dev pre-commit
</code></pre>

### Why use this?

- Keeps your production environment clean
- Makes it easy to separate tools like formatters, linters and test runners
- Supports installing only specific groups:
  - <span style="color:skyblue">poerty install --with dev</span> &rArr; includes dev 
  tools
  - <span style="color:skyblue">poertry install --without dev</span> &rArr; skips them 
  for lightweight builds

--

2. Then we activate the Poetry virtual environment, where we can run installed packages 
(like pre-commit) directly.

<pre><code class="language-bash" data-trim>
poetry shell
</code></pre>

# OR

Activate the same virtual environment manually. This is redundant if you've already 
used poetry shell.

<pre><code class="language-bash" data-trim>
.../project-...-py3.11/bin/activate
</code></pre>

--

3. Install <span style="color:skyblue">bpython</span>, a fancy interactive Python REPL 
(alternative to python), as a dev tool

<pre><code class="language-bash" data-trim>
poetry add --group dev bpython
</code></pre>

--

4. Install <span style="color:skyblue">pretty-errors</span>, a package that makes Pyhton
 tracebacks more readable.

<pre><code class="language-bash" data-trim>
poetry add --group dev pretty-errors
</code></pre>

--

5. Activate pretty-errors for the current terminal session. Makes Python errors and 
tracebacks prettier.

<pre><code class="language-bash" data-trim>
python3 -m pretty_errors
</code></pre>

In the Options that will appear Press on this first the number 1 and for the second the 
number 2.

--

6. Install the Git hook (.git/hooks/pre-commit). After this, every git commit will 
automatically run the checks defined in .pre-commit-config.yaml

<pre><code class="language-bash" data-trim>
pre-commit install
</code></pre>

--

7. List the pre-commit Git hook file, confirming it has been installed. 
(<span style="color:skyblue">Optional</span>)

<pre><code class="language-bash" data-trim>
ls .git/hooks/pre-commit
</code></pre>

--

8. Show the contents of the pre-commit hook file (a shell script that runs pre-commit)

<pre><code class="language-bash" data-trim>
cat .git/hooks/pre-commit
</code></pre>

--

9. Run all configured pre-commit hooks on all files, not just changed ones. It is useful
 for validating the entire project upfront.

<pre><code class="language-bash" data-trim>
pre-commit run --all-files
</code></pre>

--

# Create Documentation using Sphinx

<span style="color:skyblue">Sphinx</span> is a documentation generator, most famous in 
the Python ecosystem.

<span style="color:skyblue">What it does:</span> You write your docs in reStructuredText (.rst) or Markdown, and Sphinx converts them into HTML websites, PDFs, ePubs, man pages, etc.

--

10. Install documentation-related tools:
    - <span style="color:skyblue">myst-parser</span>: for Markdown support in Sphinx
    - <span style="color:skyblue">numpydoc</span>: parses NumPy-style docstrings
    - <span style="color:skyblue">sphinx</span>: the main documentation generator
    - <span style="color:skyblue">sphinx-math-dollar</span>: support for math in 
    Markdown using $...$
    - <span style="color:skyblue">sphinx-rtd-theme</span>: the Read the Docs theme for 
    docs

<pre><code class="language-bash" data-trim>
poetry add --group docs myst-parser numpydoc sphinx sphinx-math-dollar sphinx-rtd-theme
</code></pre>

--

11. Enter the docs/ directory

<pre><code class="language-bash" data-trim>
cd docs
</code></pre>

--

12. Build the Sphinx documentation in HTML format using a custom Makefile target 
(html-all).
If this is a custom Makefile, the command could:
- Build all doc pages
- Run validation
- Include Markdown and reStructuredText files

<pre><code class="language-bash" data-trim>
make html-all
</code></pre>

--

# Additional Information

--

# What is pre-commit?

<span style="color:skyblue">pre-commit</span> is a framework for managing and running 
Git hooks, especially the <span style="color:skyblue">pre-commit</span> hook - a script 
that runs right before you make a commit. It helps enforce code quality by running tools
 (like linters, formatters, or security checks) automatically before code is committed 
to your repository.

--

# Why use pre-commit?

## Benefits:

- Auto Formatting: Automatically runs tools like 
<span style="color:skyblue">black</span>, <span style="color:skyblue">isort</span> or 
<span style="color:skyblue">prettier</span> before commits.

- Catch Errors Early: Run linters like <span style="color:skyblue">flake8</span>, 
<span style="color:skyblue">mypy</span>, or <span style="color:skyblue">eslint</span> 
before code gets into version control.

- Consistent Code Style: Enforces uniform code style across your team or projects.

- Avoid Forgetting: Automates things you might forget to do manually (e.g., remove debug
 prints, fix import order).

- Prevent Bad Commits: Can block commits that violate rules (e.g., large files, TODOs 
left in code).

--

# Common Tools Managed with pre-commit

- <span style="color:skyblue">black</span> – auto-format Python code

- <span style="color:skyblue">flake8</span> – lint Python code

- <span style="color:skyblue">mypy</span> – static type checker

- <span style="color:skyblue">isort</span> – sort imports

- <span style="color:skyblue">detect-secrets</span> – prevent committing secrets

- <span style="color:skyblue">trailing-whitespace</span>, 
<span style="color:skyblue">end-of-file-fixer</span>, etc. – small but useful 
consistency checks

--

# How pre-commit works

When you install and configure <span style="color:skyblue">pre-commit</span>, it creates
 a Git hook at <span style="color:skyblue">.git/hooks/pre-commit</span>. When you run 
<span style="color:skyblue">git commit</span>, it automatically:

1. Runs configured tools on your changed files

2. Fails the commit if any tool fails

3. Lets the commit succeed if everything passes

--

## What is Python REPL (Read-Eval-Print Loop)

It's an interactive Python shell you get when you run:

<pre><code class="language-bash" data-trim>
python
</code></pre>

You can:

- Type code line by line
- See results instantly
- Test, debug etc.

### Better REPLs:

- bpython - colorful, autocompletion
- ipython - powerful, rich features

--

# YAML File

A <span style="color:skyblue">.yaml</span> file is a human-readable text file used to 
store configuration or structurred data. It uses indentation to represent hierarchy and 
is common in tools like Docker, GitHub Actions and pre-commit. It's simple, clean and 
easy for both humans and machines to read.

--

# What is Traceback?

A <span style="color:skyblue">traceback</span> is the error message Python shows when 
your code crashes. It includes a list of function calls that led to the error, showing 
where and why it happened. It helps you debug by pointing to the exact line and file 
that caused the exception.

---

# Advanced Workflow Better

--

# Pre-commits

Pre-commits are scripts that run automatically before a commit is made. These can be 
used to ensure code quality by running linters, formatters, and other checks on the 
codebase.

--

# Install & Set Up

Inside your project:

<pre><code class="language-bash" data-trim>
poetry add pre-commit
pre-commit install
</code></pre>

This adds the .pre-commit-config.yaml file, where you define the hooks you want to run.

# Example Configuration

Example of a .pre-commit-config.yaml file:

<pre><code class="language-yaml" data-trim>
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
  - repo: https://github.com/psf/black
    rev: 21.12b0
    hooks:
      - id: black
</code></pre>

--

# Usage

- Stage files you want to commit
- Run all hooks against all files: <code class="language-bash" style="display: inline-block; background: #222;">
pre-commit run --all-files</code>

- Run hooks only on changed files: <code class="language-bash" style="display: inline-block; background: #222;">
pre-commit run</code>

- Update pre-commit hooks: <code class="language-bash" style="display: inline-block; background: #222;">
pre-commit autoupdate</code>

- If you run git commit with pre-commits and files are modified by hooks, commit will be
 aborted. You need to:
  - Stage them again
  - Commit them

--

# Linters

Analyze the code for syntax, style and potential bugs. Catch issues like:

- Unused variables
- Wrong import order
- Missing docstrings

# Formatters

Automatically reformat code to follow a consistent style.

- Identation
- Quote style
- Line breaks
- Comma placements

Linters and formatters get their config from 
<code class="language-bash" style="display: inline-block; background: #222;">
pyproject.toml</code>

--

# Example Configuration

<pre><code style="max-height: none; overflow: visible;" class="language-toml" data-trim>
[tool.ruff]
exclude = ["docs"]
line-length = 88
target-version = "py311"

[tool.ruff.lint]
ignore = [
  # pylint
  # No pylints
  # ruff
  "RUF001",
  # flake8-bandit
  "S101",
  ...
  "ISC001",
  "N806",
  "N803",
  "SLF001"
]
select = ["ALL"]

[tool.ruff.lint.flake8-quotes]
inline-quotes = "double"

[tool.ruff.lint.pydocstyle]
convention = "numpy"
</code></pre>

--

# NumPy Docstrings

NumPy docstrings follow a specific format to ensure consistency and clarity. Here's a 
basic template for writing NumPy-style docstrings:

<pre><code class="language-python" data-trim>
def example_function(param1, param2):
    """
    Brief summary of the function.

    Extended description of the function, which can cover
    multiple lines and provide more detailed information.

    Parameters
    ----------
    param1 : int
        Description of the first parameter.
    param2 : str
        Description of the second parameter.

    Returns
    -------
    bool
        Description of the return value.

    Raises
    ------
    ValueError
        If `param1` is not a positive integer.

    See Also
    --------
    related_function : Description of related function.
    
    Notes
    -----
    Additional notes about the function, its usage, or implementation details.

    Examples
    --------
    >>> example_function(3, 'test')
    True
    """

    if param1 <= 0:
        raise ValueError("param1 must be a positive integer")
    # Example function logic
    return True
</code></pre>

--

# Key Sections

- **Summary**: A one-line summary that starts with a capital letter and ends with a 
period.

- **Parameters**: A list of parameters with types and descriptions.

- **Returns**: Description of the return value, including type information.

- **Raises**: Any exceptions that the function may raise and the conditions under which 
they occur.

- **See Also**: References to related functions or methods within the module.

- **Notes**: Additional information about the function, which may include implementation
 details.

- **Examples**: Usage examples with the expected output, helpful for understanding 
function behavior.

--

# Generating Documentation

Assuming you already have numpy docstrings in your code.

--

1. Add necessary packages

<pre><code class="language-bash" data-trim>
poetry add --group docs numpydoc sphinx sphinx-rtd-theme sphinx-math-dollar myst-parser
</code></pre>

- numpydoc: Parses NumPy-style docstrings for Sphinx.

- sphinx: Turns reStructuredText or Markdown + docstrings into HTML, LaTeX, PDF, etc.

- sphinx-math-dollar: Allows you to write math equations with LaTeX dollar signs 
(`$...$` for inline, `$$...$$` for block).

- sphinx-rtd-theme: The [ReadTheDocs theme](https://readthedocs.io/)

- myst-parser: Enables writing docs in Markdown .md instead of .rst

--

2. Set up sphinx in the docs folder of your repo

<pre><code class="language-bash" data-trim>
poetry run sphinx-quickstart docs
</code></pre>

Follow prompts on the terminal.

--

3. Configure <span style="color:skyblue">conf.py</span> For example

<pre><code style="max-height: none; overflow: visible;" class="language-bash" data-trim>
# -- General configuration ---------------------------------------------------

extensions = [
    "sphinx.ext.autosummary",
    "sphinx.ext.autodoc",
    "numpydoc",
    "sphinx_math_dollar",
    "sphinx.ext.mathjax",
    "sphinx_rtd_theme",
    "myst_parser",  # enables Markdown support via MyST
]

templates_path = ["_templates"]
exclude_patterns = []

autosummary_generate = True
numpydoc_class_members_toctree = False

# -- Options for HTML output -------------------------------------------------

html_theme = "sphinx_rtd_theme"

# -- Options for napoleon extension ------------------------------------------
napoleon_google_docstring = False
napoleon_numpy_docstring = True
</code></pre>

--

4. Autogenerate <span style="color:skyblue">.rst</span> files for your code

<pre><code class="language-bash" data-trim>
sphinx-apidoc -o docs/source your_module
</code></pre>

--

5. Include modules into <span style="color:skyblue">index.rst</span> Include something 
like:

<pre><code class="language-yaml" data-trim>
.. toctree::
   :maxdepth: 2
   :caption: API Reference

   your_module # don't forget the indent
</code></pre>

--

6. Build your documentation

<pre><code class="language-bash" data-trim>
sphinx-build -b html docs/source docs/build
</code></pre>

--

# Additional Information

--

# .rst File

An <span style="color:skyblue">.rst</span> file (reStructuredText) is a plain-text 
markup format commonly used in Python projects for writing documentation.

- Works seamlessly with Sphinx, which converts <span style="color:skyblue">.rst</span> 
into HTML, PDF, ePub, etc.
- More powerful than Markdown for large docs: supports cross-references, indexes, 
advanced tables, math, and extensible directives
- Standard for many Python libraries because PyPI supports .rst for project descriptions

### Example

<pre><code class="language-typescript" data-trim>
Project Title
=============

Features
--------

* Fast
* Easy

.. code-block:: python

   print("Hello, World!")
</code></pre>

---

# Reveal.js Presentation

--

## 1. Install Node.js

You need Node.js (which includes npm) installed on your system.

**Windows / macOS / Linux:**

- Visit: [nodejs](https://nodejs.org/)
- Download the LTS version and install it

- To verify:
<pre><code class="language-bash" data-trim>
node -v
npm -v
</code></pre>

--

## 2. Get a Template

- Go to a template on GitHub
- Click Code -> Download ZIP
- Extract it on your computer
- Rename the folder as you wish

--

## Editing Your Slides

Slides are written in slides.md using Markdown.

Use **---** to separate horizontal slides, and **--** for vertical slides:

<pre><code class="language-markdown" data-trim>
# Welcome

&#45;&#45;&#45;

## Slide 1

Some text

&#45;&#45;

- Vertical slide 1

&#45;&#45;

- Vertical slide 2
</code></pre>

--

# Customizing Theme

Reveal.js themes are in the dist/theme folder.

To change the theme, edit index.html and modify this line:

<pre><code class="language-html" data-trim>
<link rel="stylesheet" href="dist/theme/black.css" id="theme">
</code></pre>

You can try other themes like white.css, 
night.css, moon.css,etc.

--

# Transition Between Slides

### Option 1: Set Globally in the Reveal Configuration (index.html)

<pre><code class="language-html" data-trim>
<script>
  Reveal.initialize({
    transition: 'fade', // or 'slide', 'convex', 'concave', 'zoom', 'none'
    transitionSpeed: 'default' // 'fast', 'slow', or 'default'
  });
</script>
</code></pre>

### Option 2: Per Slide with data-transmition in the Markdown File

<pre><code class="language-bash" data-trim>
&lt;!-- .slide: data-transition="zoom" --&gt;
</code></pre>

### Or for vertical slides

<pre><code class="language-bash" data-trim>
&lt;!-- .slide: data-transition="fade-in slide-out" --&gt;
</code></pre>

--

# Change Markdown File

Inside the file that contains the template, you can create a folder called 
<span style="color:skyblue">Presentations</span> for example. There you can store all 
Markdown files for every presentation you make.

#### To change the presentation, you can do the following:

- Go to index.html
- Find:
<pre><code class="language-markdown" data-trim>
data-markdown="slides.md"
</code></pre>
- Change it to:
<pre><code class="language-markdown" data-trim>
data-markdown="Presentations/notes.md"
</code></pre>

- Now, the presentation written in the notes.md file 
will appear.

--

# Load Slides Locally

- Inside the folder, open a terminal and run:
<pre><code class="language-bash" data-trim>
npm install
</code></pre>
This installs local development tools like the server (gulp).

- Run the Presentation:
<pre><code class="language-bash" data-trim>
npm start
</code></pre>
- Open your browser at:
<pre><code class="language-bash" data-trim>
http://localhost:8000
</code></pre>
You should see your slides rendered.

--

# Hosting Slides on GitHub Pages

You can publish your presentation online for free using GitHub Pages.

#### **Step-by-Step:**

1. Make sure your repository includes:
    - index.html
    - slides.md
    - dist/ and plugin/ folders from Reveal.js
    - empty .nojekyll file(prevents GitHub from using Jekyll)
2. Go to your GitHub repository -> Settings -> Pages
3. Under Build and Deployment, set:
- Source: Deploy from branch
- Branch: main
- Folder: / (root)
- Click Save

4. Wait 1-10 minutes. GitHub will show a <span style="color:green">green message</span> 
like the following:
<pre><code class="language-markdown" data-trim style="color:green;">
Your site is live at https://your-username.github.io/repo-name/
</code></pre>

--

## If the green message does 
## <span style="color:red">NOT</span> appear

- Add a .nojekyll file(empty file with that name)
- Make a small commit to trigger a rebuild:
<pre><code class="language-markdown" data-trim>
git commit --allow-empty -m "Trigger rebuild"
git push
</code></pre>

- Visit your slides live in the browser!

--

# Public vs Private Repos and Ways to Share Slides

--

### ✅ Option 1: GitHub Pages (Public Repo)

- If your repository is public, GitHub Pages will host your presentation at:
<pre><code class="language-markdown" data-trim>
https://your-username.github.io/repo-name/
</code></pre>
- This is the <span style="color:green">easiest</span> and 
<span style="color:green">recommended</span> way to publish Reveal.js slides.

### 🚫 Option 2: GitHub Pages (Private Repo)

- GitHub Pages do <span style="color:red">not</span> work for private repos unless you
 are on a GitHub Enterprise plan.

- You will see a 404 or the site won’t build.

--

### <span style="color:blue">🛠</span> Option 3: Local Server (for Private Sharing)

- You can run your slides locally with:
<pre><code class="language-bash" data-trim>
npm start
</code></pre>

- Then access them at:
<pre><code class="language-bash" data-trim>
http://localhost:8000/
</code></pre>

- This works for development or sharing over a local network.

### 📤 Option 4: Export to PDF

- Reveal.js can export slides to PDF:
<pre><code class="language-bash" data-trim>
npm install -g decktape
decktape reveal http://localhost:8000 slides.pdf
</code></pre>

- Useful for email, offline viewing, or submissions.

--

# 📦 Managing Large Files with Git LFS

If you need to include large binary files in your presentation (e.g. .mp4 videos, .pdf 
documents), it is recommended to use Git Large File Storage (LFS) instead of committing 
them directly to the repository.

--

## ✅ Install Git LFS

- Ubuntu/Debian:
<pre><code class="language-bash" data-trim>
sudo apt install git-lfs
</code></pre>

- Windows:  
Download and install from [git-lfs.com](https://git-lfs.com/)

--

### ✅ Set Up Git LFS in Your Repo

1. Run this ince per machine:
<pre><code class="language-bash" data-trim>
git lfs install
</code></pre>
2. Track the file types you want to store with LFS:
<pre><code class="language-bash" data-trim>
git lfs track "*.mp4"
git lfs track "*.pdf"
</code></pre>
3. Add and commit:
<pre><code class="language-bash" data-trim>
git add .gitattributes
git add media/my-video.mp4
git commit -m "Add video using Git LFS"
git push
</code></pre>

### ℹ️ Notes

- GitHub’s free LFS plan includes 1 GB of storage and 1 GB/month bandwidth.
- You can view tracked LFS files using: <span style="color:lime">
<code class="language-bash" data-trim>git lfs ls-files</code></span>

To avoid LFS entirely, you can also upload media externally (e.g. Google Drive or a web 
server) and link to them in your slides.

--

# Additional Information

--

## What is Reveal.js?
<span style="color:skyblue">Reveal.js</span> is an open-source framework for creating 
HTML-based presentations using Markdown or HTML. Instead of writing slides in a visual 
editor (like Keynote or PowerPoint), you write them in plain text - and Reveal.js turns 
them into a modern, interactive slideshow in the browser.

--

## HTML File

- The <span style="color:skyblue">HTML</span> file is the main entry point of the 
presentation.

- It defines the page structure, loads Reveal.js and its plugins, configures settings, 
and optionally contains the slides directly if not using Markdown.

--

## CSS File

- The <span style="color:skyblue">CSS</span> file defines the styling of your 
presentation.

- It customizes the look and feel - fonts, colors, layout - either by overriding the 
default Reveal.js theme or adding new styles.

--

## Markdown File

- The <span style="color:skyblue">Markdown (.md)</span> file contains the slide content 
written in simple, readable Markdown syntax.

- It’s loaded by the HTML file (via the Reveal.js markdown plugin), making it easy to 
edit slides without touching HTML.

--

## JSON File

- A <span style="color:skyblue">JSON</span> file is optional and is used to store extra 
data or configuration for the presentation.

- It can define metadata, timing, or slide-related data that JavaScript in the HTML file
 can read and use.

---

# Git Commands

--

<div style="display: flex; gap: 0.5em; text-align: left;">
<img src="figures/git_cheatsheet_page_1.jpg" alt="Git Page 1" width="550" height="650">
<img src="figures/git_cheatsheet_page_2.jpg" alt="Git Page 2" width="550" height="650">
</div>

--

<div style="display: flex; gap: 0.5em; text-align: left;">
<img src="figures/git_cheatsheet_page_3.jpg" alt="Git Page 3" width="550" height="650">
<img src="figures/git_cheatsheet_page_4.jpg" alt="Git Page 4" width="550" height="650">
</div>

--

## I. Version Control

--

### 1. `git init`

Initializes a new Git repository in the current directory.

<pre><code class="language-bash" data-trim> 
git init
</code></pre>

**Use**: Start tracking version history for a new or existing project.

--

### 2. `git clone`

Copies a repository from a remote source.

<pre><code class="language-bash" data-trim>
git clone https://github.com/user/repo.git
</code></pre>

**Use**: Download a full copy of a repository.

--

### 3. `git status`

Displays the state of the working directory and staging area.

<pre><code class="language-bash" data-trim>
git status
</code></pre>

**Use**: Check which files are staged, modified, or untracked.

--

### 4. `git add`

Stages changes to be committed.

<pre><code class="language-bash" data-trim>
git add file.txt
# or add all
git add .
</code></pre>

**Use**: Prepare changes for committing.

--

### 5. `git commit`

Saves staged changes to the repository history.

<pre><code class="language-bash" data-trim>
git commit -m "Add login functionality"
</code></pre>

**Use**: Record a snapshot of your changes.

--

### 6. `git log`

Shows the commit history.

<pre><code class="language-bash" data-trim>
git log
# or condensed
git log --oneline
</code></pre>

**Use**: View past commits.

--

### 7. `git diff`

Shows changes between working directory and index.

<pre><code class="language-bash" data-trim>
git diff
# for staged changes
git diff --staged
</code></pre>

**Use**: See what has been modified.

--

### 8. `git config`

Sets user config values.

<pre><code class="language-bash" data-trim>
git config --global user.name "Your Name"
git config --global user.email "your@example.com"
</code></pre>

**Use**: Identify the author of commits.

--

### 9. `git rm`

Removes a file from the working directory and staging area.

<pre><code class="language-bash" data-trim>
git rm oldfile.txt
</code></pre>

**Use**: Track file deletion in version history.

--

### 10. `git mv`

Renames or moves a file.

<pre><code class="language-bash" data-trim>
git mv oldname.txt newname.txt
</code></pre>

**Use**: Maintain history when renaming files.

--

## II. Branching and Merging

--

### 11. `git branch`

Lists, creates, or deletes branches.

<pre><code class="language-bash" data-trim>
git branch
# create a new branch
git branch feature-x
</code></pre>

**Use**: Manage different lines of development.

--

### 12. `git checkout`

Switches branches or restores files.

<pre><code class="language-bash" data-trim>
git checkout feature-x
# or create and switch
git checkout -b new-feature
</code></pre>

**Use**: Move between branches.

--

### 13. `git switch`

A simpler alternative to `checkout` for switching branches.

<pre><code class="language-bash" data-trim>
git switch main
</code></pre>

**Use**: Modern method for changing branches.

--

### 14. `git merge`

Merges changes from one branch into another.

<pre><code class="language-bash" data-trim>
git checkout main
git merge feature-x
</code></pre>

**Use**: Integrate completed features.

--

### 15. `git rebase`

Reapplies commits on top of another base branch.

<pre><code class="language-bash" data-trim>
git checkout feature-x
git rebase main
</code></pre>

**Use**: Clean linear history.

--

### 16. `git stash`

Temporarily shelves changes.

<pre><code class="language-bash" data-trim>
git stash
# apply them later
git stash apply
</code></pre>

**Use**: Save unfinished changes without committing.

--

### 17. `git tag`

Creates tags for marking releases or important commits.

<pre><code class="language-bash" data-trim>
git tag v1.0.0
</code></pre>

**Use**: Mark specific points in history.

--

### 18. `git cherry-pick`

Applies a commit from one branch to another.

<pre><code class="language-bash" data-trim>
git cherry-pick abc1234
</code></pre>

**Use**: Apply specific changes without merging full branch.

--

### 19. `git pull`

Fetches and integrates from a remote repository.

<pre><code class="language-bash" data-trim>
git pull origin main
</code></pre>

**Use**: Update your branch with the latest changes.

--

### 20. `git push`

Uploads local commits to a remote repository.

<pre><code class="language-bash" data-trim>
git push origin main
</code></pre>

**Use**: Share your changes with others.

--

## III. Bug Tracking and Fixing

--

### 21. `git blame`

Shows which commit and author last modified each line.

<pre><code class="language-bash" data-trim>
git blame index.js
</code></pre>

**Use**: Trace the origin of code changes.

--

### 22. `git revert`

Creates a new commit that undoes changes from a previous one.

<pre><code class="language-bash" data-trim>
git revert abc1234
</code></pre>

**Use**: Safely undo commits.

--

### 23. `git reset`

Resets HEAD and optionally working directory/index.

<pre><code class="language-bash" data-trim>
git reset --soft HEAD~1
# or hard reset
git reset --hard HEAD~1
</code></pre>

**Use**: Undo local commits.

--

### 24. `git bisect`

Binary search to find which commit introduced a bug.

<pre><code class="language-bash" data-trim>
git bisect start
git bisect bad
git bisect good abc1234
</code></pre>

**Use**: Locate the commit that caused a regression.

--

### 25. `git show`

Displays information about a specific commit.

<pre><code class="language-bash" data-trim>
git show abc1234
</code></pre>

**Use**: Examine commit details and diff.

--

### 26. `git reflog`

Records changes to HEAD for recovery.

<pre><code class="language-bash" data-trim>
git reflog
</code></pre>

**Use**: Restore lost commits or branches.

--

### 27. `git clean`

Removes untracked files or directories.

<pre><code class="language-bash" data-trim>
git clean -fd
</code></pre>

**Use**: Clean up working directory.

--

### 28. `git log -p`

Shows each commit along with the patch.

<pre><code class="language-bash" data-trim>
git log -p
</code></pre>

**Use**: See exactly what was changed.

--

### 29. `git diff <branch1>..<branch2>`

Shows differences between two branches.

<pre><code class="language-bash" data-trim>
git diff main..feature-x
</code></pre>

**Use**: Compare changes between branches.

--

### 30. `git shortlog`

Summarizes commit history by author.

<pre><code class="language-bash" data-trim>
git shortlog -sn
</code></pre>

**Use**: See who contributed and how much.

--

## IV. Git Hooks (Automation)

Git hooks are scripts triggered by Git actions like commit, push, or merge. They help 
automate tasks such as linting, testing, or enforcing policy.

--

### 31. `pre-commit`

Runs before a commit is finalized.

<pre><code class="language-bash" data-trim>
#!/bin/sh
echo "Running pre-commit checks..."
</code></pre>

**Use**: Lint or test code before committing.

--

### 32. `prepare-commit-msg`

Edits the default commit message before the editor is launched.

<pre><code class="language-bash" data-trim>
#!/bin/sh
echo "[AUTO-LOG] " >> $1
</code></pre>

**Use**: Add default messages or tags.

--

### 33. `commit-msg`

Validates or modifies the commit message.

<pre><code class="language-bash" data-trim>
#!/bin/sh
if ! grep -qE 'JIRA-[0-9]+' "$1"; then
  echo "ERROR: Commit message must include a JIRA ID"
  exit 1
fi
</code></pre>

**Use**: Enforce commit message rules.

--

### 34. `post-commit`

Executes after a successful commit.

<pre><code class="language-bash" data-trim>
#!/bin/sh
echo "Commit successful!"
</code></pre>

**Use**: Notifications or logging.

--

### 35. `pre-push`

Runs before pushing to a remote.

<pre><code class="language-bash" data-trim>
#!/bin/sh
npm test || exit 1
</code></pre>

**Use**: Prevent broken code from being pushed.

--

### 36. `post-merge`

Runs after a merge completes.

<pre><code class="language-bash" data-trim>
#!/bin/sh
echo "Merged successfully! Run build..."
</code></pre>

**Use**: Rebuild project or install dependencies.

--

### 37. Installing a Git Hook

Git hooks live in the `.git/hooks/` directory. You must make them executable:

<pre><code class="language-bash" data-trim>
chmod +x .git/hooks/pre-commit
</code></pre>

**Use**: Enable and manage automation workflows.

---

# Store screenshot as PDF instead of PNG to NOT lose quality

--

## Windows

### Option 1

- Take a screenshot using <span style="color:skyblue">Win + Shift + S</span>
- <span style="color:skyblue">Paste</span> it into Paint or Word or a 
<span style="color:skyblue">browser</span>
- Got to <span style="color:skyblue">File</span> > 
<span style="color:skyblue">Print</span> > choose <span style="color:skyblue">Microsoft 
Print to PDF</span> as the printer
- <span style="color:skyblue">Save</span> the PDF

### Option 2 (PowerShell)

<pre><code class="language-bash" data-trim>
Start-Process -FilePath "screenshot.png" -Verb PrintTo -ArgumentList "Microsoft Print to PDF"
</code></pre>

--

## Linux

### Option 1

- Use <span style="color:skyblue">PrtSc</span> or a screenshot tool like 
<span style="color:skyblue">Flameshot</span>
- Open the image in <span style="color:skyblue">LibreOffice Draw</span> or 
<span style="color:skyblue">GIMP</span> or a <span style="color:skyblue">browser</span>
- <span style="color:skyblue">Export</span> or print to PDF:
  - In LibreOffice: <span style="color:skyblue">File</span> > 
  <span style="color:skyblue">Export As</span> > 
  <span style="color:skyblue">Export as PDF</span>
  - In GIMP: <span style="color:skyblue">File</span> > 
  <span style="color:skyblue">Export As</span> > 
  <span style="color:skyblue">select PDF</span>

### Option 2 (Terminal)

<pre><code class="language-bash" data-trim>
convert screenshot.png screenshot.pdf
</code></pre>

---

# Cheat Sheet

--

## 📁 File & Directory Management
| Command | Description |
| --- | --- |
| ls | List files in current directory |
| ls -la | List all files with details |
| cd folder_name | Change directory |
| cd | Go to home directory |
| cd .. | Go up one directory |
| cd - | Go to previous directory |
| mkdir folder_name | Create a new folder |
| rm file | Delete file |
| rm -rf folder | Force delete folder and contents |
| cp source dest/ | Copy file/folder |
| mv old new | Move or rename file/folder |
| touch file.txt | Create empty file |

--

## 📄 File Viewing & Editing
| Command | Description |
| --- | --- |
| cat file.txt | Show file contents |
| less file.txt | Scrollable view of file |
| nano file.txt | Edit file in nano editor |
| code . | Open folder in VS Code (if installed) |

--

## 🔍 Search & Navigation
| Command | Description |
| --- | --- |
| find . -name '*.py' | Find all `.py` files |
| grep "pattern" file.txt | Search for text in a file |
| grep -r "text" folder/ | Recursive search in folder |
| pwd | Show current directory |
| history | View command history |

--

## 🧰 System Utilities
| Command | Description |
| --- | --- |
| whoami | Show current user |
| df -h | Show disk usage |
| free -h | Show memory usage |
| top / htop | Process monitor (install `htop`) |
| uptime | System uptime |

--

## 📦 Package Management (APT)
| Command | Description |
| --- | --- |
| sudo apt update | Refresh package index |
| sudo apt upgrade | Upgrade installed packages |
| sudo apt install pkgname | Install package |
| sudo apt remove pkgname | Remove package |

--

## 🐍 Python & pyenv
| Command | Description |
| --- | --- |
| python3 --version | Show system Python version |
| pyenv install 3.11.13 | Install specific Python version |
| pyenv global 3.11.13 | Set global Python version |
| pyenv local 3.11.13 | Set version only for current folder |
| pyenv versions | List installed versions |
| pyenv which python | Show path to Python binary |
| poetry init | Create new Poetry project |
| poetry env use $(pyenv which python) | Link Poetry to pyenv Python |
| poetry env list | Lists all exiting environments |
| poetry env remove <env> | Removes poetry environment |
| poetry add <package> | Add dependency |
| poetry install | Install all libraries in toml |
| poetry shell | Activate project virtual environment |

--

## 🔐 SSH & Remote Access
| Command | Description |
| --- | --- |
| ssh user@host | Connect to remote machine |
| ssh -i ~/.ssh/id_rsa user@host | Use specific SSH key |
| ssh-keygen -t ed25519 | Generate new SSH key |
| cat ~/.ssh/id_ed25519.pub | Show public key to add to GitHub/remote |
| scp file user@host:/remote/path | Copy file to remote server |
| scp user@host:/remote/file ./local/ | Copy file from remote server |

--

## 📡 FTP & File Transfer

| Command | Description |
|--------|-------------|
| ftp host | Connect to an FTP server |
| sftp user@host | Connect securely via SFTP |
| scp file user@host:/path | Copy file to remote server |
| scp user@host:/file ./ | Copy file from remote server |
| rsync -avz file user@host:/path | Sync file/directory over SSH |
| rsync -avz user@host:/path ./ | Download file/directory via rsync |
| mput *.txt | Upload multiple files in FTP |
| mget *.txt | Download multiple files in FTP |
| put file.txt | Upload file in FTP |
| get file.txt | Download file in FTP |
| bye | Exit FTP session |

--

## 🧠 Shortcuts
| Command | Description |
| --- | --- |
| Ctrl + C | Cancel current command |
| Ctrl + D | Exit terminal or virtual env |
| Ctrl + L | Clear terminal screen |
| ↑ / ↓ | Navigate command history |
| Tab | Autocomplete files or commands |
| !! | Repeat last command |
| Ctrl+ | Move by word instead of character |

--

## 🌿 Git Essentials - Part 1
| Command | Description |
| --- | --- |
| git init | Initialize a new Git repository |
| git config --global user.name/email | Set global Git username/email |
| git clone <url> | Clone a remote repository |
| git status | Check status of working directory |
| git add <file> | Stage changes for commit |
| git add . | Stage all changes |
| git diff | Shows unstaged changes |
| git diff --staged | Shows staged changes |
| git diff HEAD | Shows changes since last commit |
| git diff commit1 commit2 | Shows changes between commits |
| git diff branch1 branch2 | Shows changes between branches |
| git commit -m 'msg' | Commit staged changes with message |
| git commit --amend | Edit and replace the most recent commit. |

--

## 🌿 Git Essentials - Part 2

| Command | Description |
| --- | --- |
| git log | View commit history |
| git branch | List branches |
| git checkout <branch> | Switch to branch |
| git checkout -b <branch> | Create and switch to new branch |
| git merge <branch> | Merge branch into current |
| git pull | Fetch and merge from remote |
| git push | Push local commits to remote |
| git remote -v | List remote connections |
| git restore  <file> | Restore a file to its last commited state |
| git reset --soft HEAD~1 | Undo the last commit, keep changes staged |
| git stash | Temporarily save uncommited changes |
| git stash pop | Apply and remove the most recent stash |

---

# That is all!
