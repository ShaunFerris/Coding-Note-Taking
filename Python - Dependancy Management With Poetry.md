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

