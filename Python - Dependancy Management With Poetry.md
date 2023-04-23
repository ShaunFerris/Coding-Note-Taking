#python #skills #highlevelconcepts 

Notes in this sectiona are adapted from the [poetry documentation](https://python-poetry.org/docs/), and from the [codecamp tutorial](https://www.freecodecamp.org/news/how-to-build-and-publish-python-packages-with-poetry/) on this topic.

A python package is a collection of modules that can be easily distributed and installed on many different systems without the user needing to preconfigure mofules or other files that the product needs to work (dependancies).

Python packages generally contain a collection of functions or classes that serve a specific purpose, or work together to provide some coherent functionality. OS, sys, math, pandas and numpy are examples of python packages, with pandas and numpy being external while the other packages are part of the python standard lib. Many external packages are available for install on your system python install or into a virtual env via the python package manager, pip. When you create a python project for mass consumption, you want to prepare it in such a way that many people with many different systems can simply install it following a standard set of instructions and have it work in the intended way.

Poetry is an example of a tool you would use to prepare a package for publication on pip or any other distrubituon network. Poetry is used to manage dependencies, define package configurations, and write tests for the package.It is a command line tool that can do all of this while also managing the virtual env for the project to keep projects isolated from the system install. 