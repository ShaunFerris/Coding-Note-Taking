#python #skills #highlevelconcepts 

Notes in this sectiona are adapted from the [poetry documentation](https://python-poetry.org/docs/), and from the [codecamp tutorial](https://www.freecodecamp.org/news/how-to-build-and-publish-python-packages-with-poetry/) on this topic.

A python package is a collection of modules that can be easily distributed and installed on many different systems without the user needing to preconfigure mofules or other files that the product needs to work (dependancies).

Python packages generally contain a collection of functions or classes that serve a specific purpose, or work together to provide some coherent functionality. OS, sys, math, pandas and numpy are examples of python packages, with pandas and numpy being external while the other packages are part of the python standard lib. Many external packages are available for install on your system python install or into a virtual env via the python package manager, pip. When you create a python project for mass consumption, you want to prepare it in such a way that many people with many different systems can simply install it following a standard set of instructions and have it work in the intended way.

Poetry is an example of a tool you would use to prepare a package for publication on pip or any other distrubituon network. Poetry is used to manage dependencies, define package configurations, and write tests for the package.It is a command line tool that can do all of this while also managing the virtual env for the project to keep projects isolated from the system install. 

To reiterate, the vitues of using poetry for **project management** are:
-   **Dependency resolution**: It automatically manages dependencies and ensures that your package is compatible with other packages in your project.
-   **Virtual environments**: It creates a virtual environment for your project, which allows you to isolate your package and its dependencies from the rest of your system.
-   **Project scaffolding**: It provides a simple command-line interface for creating new Python projects and setting up their basic structure.
-   **Built-in building and packaging**: It provides tools for creating distributable packages in a variety of formats, including source distributions, wheel distributions, and binary distributions.
-   **Publishing to PyPI**: It makes it easy to publish your package to PyPI, allowing other developers to easily install and use your package.

## Install
To install poetry you need to have at least python 3.7 installed on your system.

Poetry is installed in it's own dedicated vitual environment to keep it isolated from your system. Install poetry on linux systems with:
```bash
curl -sSL https://install.python-poetry.org | python3 -
```

Use:
```bash
poetry --version
```
To check that the install worked correctly.

By default, Poetry is installed into a platform and user-specific directory:
-   `~/Library/Application Support/pypoetry` on MacOS.
-   `~/.local/share/pypoetry` on Linux/Unix.
-   `%APPDATA%\pypoetry` on Windows.

If you wish to change this, you may define the `$POETRY_HOME` environment variable:
```bash
curl -sSL https://install.python-poetry.org | POETRY_HOME=/etc/poetry python3 -
```

`poetry` supports generating completion scripts for Bash, Fish, and Zsh. See `poetry help completions` for full details, but ot install  on bash run the following commands:
```bash
poetry completions bash >> ~/.bash_completion
```

Note that there other aspects that can be manipulated with a more advanced install of poetry. Information on this, and information on installin poetry and it's terminal completions on windows or systems not using bash can be found [here](https://python-poetry.org/docs/).

## Basic usage of poetry - create a new project
Pulling from the official poetry docs, we will walk through how to create a basic poetry project, and what that actually does for us. Let's call our project `demo-package`. The first step is to run the following in terminal:
```bash
poetry new demo-package
```
This will create the following file structure at the file-system location we were in with the terminal:
```bash
demo-package
├── pyproject.toml
├── README.md
├── demo-package
│   └── __init__.py
└── tests
    └── __init__.py
```
The `demo-package` folder contains two files: `pyproject.toml` and `README.md`, as well as two packages: `demo-package` to store the source code files and `tests` to store the test files.

### pyproject.toml
A quick aside on toml files. TOML stands for Tom's Obvious Minimal Language, in reference to it's creator and files with the .toml extension are written in this format. It is designed as a human readable language system specifically for config files for projects. TOML is designed to map unambiguously to a hash-table, with table names in square brackets and following key-value pairs on a single line per entry.

The `pyproject.toml` file is most important in the initial file structure of our project, as it is the file that will orchestrate the project and mange dependancies.  

Initially it will look like this:
```toml
[tool.poetry]
name = "demo-package"
version = "0.1.0"
description = ""
authors = ["Shaun Ferris <slb.ferris@gmail.com>"]
readme = "README.md"
packages = [{include = "demo-package"}]

[tool.poetry.dependencies]
python = "^3.7"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api" 
```
The `pyproject.toml` file has three tables by default: `tool.poetry`, `tool.poetry.dependencies`, and `build-system`.

The `tool.poetry` table in the `pyproject.toml` file has multiple key/value pairs, with `name`, `version`, `description`, and `authors` being required while others are optional.

### tool.poetry
Poetry assumes that a package with the same name as the `tool.poetry.name` specified in the `pyproject.toml` file is located at the root of the project. But if the package location is different, the packages and their locations can be specified in the `tool.poetry.packages` key.

### tool.poetry.dependancies
```toml
[tool.poetry.dependencies]
python = "^3.11"
```
In the `tool.poetry.dependencies` table, it is mandatory to declare the Python version for which the package is compatible.

### build-system
```toml
[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```
The last table, `build-system`, has two keys – `requires` and `build-backend`. The `requires` key is a list of dependencies required to build the package, while the `build-backend` key is the Python object used to perform the build process.

TOML is Poetry's preferred configuration format, and starting from version 3.11, Python provides the `tomllib` module for parsing TOML files.

## Basic usage of poetry - initialize in a pre-existing project
We have covered how to create a new project in poetry and what the shape of it's file structure looks like, but what if we have already begun a project and now wish to use poetry to manage it and get it ready for widespread release?

Instead of creating a new project, Poetry can be used to ‘initialise’ a pre-populated directory. To interactively create a `pyproject.toml` file in directory `pre-existing-project`:
```bash
cd pre-existing-project
poetry init
```

## Poetry and virtual environment management
Poetry simplifies the creation of virtual environments for your projects. To create a virtual environment for our `demo-package` library, navigate to the project directory and run the `env use` command:
```bash
poetry env use /full/path/to/python
```

The `/full/path/to/python` specifies the full path to the Python executable.

For example, in mint:
```bash
poetry env use /usr/bin/python3.10.6
```

Poetry will detect and respect the use of pre-existing virtual envirionments in a project that are already externally activated. This feature is intended for cases where you wish to opt-out of poetrys built in environment management and take care of it yourself. To take advantage of this, simply activate a virtual environment using your preferred method or tooling, before running any Poetry commands that expect to manipulate an environment.

## Configuring dependancies



