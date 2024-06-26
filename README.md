# Introduction

In this, I will provide you with a comprehensive step-by-step guide on how to create and host documentation for your Python package using Sphinx and Readthedocs. The topics covered in this guide include:

- Setting up Sphinx
- Configuring Sphinx extensions
- Guide to document Python package
- Create documentation modules
- Testing documentation locally
- Setting up Readthedocs
- Automatic deployment documents

Please keep in mind that the code is intended solely for demonstration purposes. With that said, let's begin.

-----

## Setting up Sphinx

Sphinx is a tool that makes it easy to create intelligent and beautiful documentation for Python projects. It is a widely used tool for documenting Python packages. To install Sphinx, run the following command:

```bash
pip install sphinx==6.2.1
```

Let's create a `docs` directory at the root of your project. This is where we will store all the documentation files. Use the following command to create the `docs` directory at the root of your project:

```bash
sphinx-quickstart docs
```

- If you don't specify docs in the above command it will create a `source` folder instead.
- Once run the above command, it will ask you a series of questions. One of the questions will be to separate the source and build directories. You can choose `n` to keep it simple. I prefer to keep all the documentation in the same directory. However, it's up to you. 

Benefits of choosing `n`: no need to specify the source directory while building the documentation. Only use the following command to build the documentation:

```bash
cd docs
make html
```
Otherwise, you need to specify the source directory while building the documentation:

```bash
cd docs
sphinx-build source build
```

- Once you have answered all the questions, it will create a `docs` directory with the following structure:

```bash
docs/
    _build/
    _static/
    _templates/
    conf.py
    index.rst
    Makefile
    make.bat
```

- `conf.py` is the main configuration file for Sphinx. It contains all the settings for the documentation. 
- `index.rst` is the main file where you will write the documentation.
- `Makefile` and `make.bat` are the files that contain the commands to build the documentation. 
- `_build` is the directory where the documentation will be built. It will contain the HTML files.
- `_static` and `_templates` are the directories that contain the static files and templates for the documentation. It will be used to store the image files, CSS files, and JavaScript files.

We will make changes to the `conf.py` and `index.rst` files in the next sections.

-----

## Configuring Sphinx extensions

Sphinx has a lot of extensions that can be used to enhance the documentation. Some of the popular extensions are mentioned in the `requirements_docs.txt` file. You can install them using the following command:

```bash
cd docs
pip install -r requirements_docs.txt
```

Most common extensions are: m2r2, myst-parser, sphinx-rtd-theme, sphinxcontrib-napoleon, sphinx-copybutton, sphinx-autodoc-typehints. Pleas keep in mind that the documentation theme is governed by the theme you provide in the `conf.py` file. In our case we will be using the `sphinx-rtd-theme`. It's included in the `requirements_docs.txt` file. The extension name should be provided in `conf.py` file. We replaced the default theme with `sphinx-rtd-theme`. Please see the details below:

In `conf.py` file, add the following lines to enable the extensions:
```python
html_theme = 'sphinx_rtd_theme'
```

Please make sure the requirements_docs.txt file is present in the `docs` directory. It will be helpful while deploy the documentation on Readthedocs.


Also, add the following lines in `conf.py` file to enable these extensions:
```python
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.autosummary',
    'sphinx.ext.duration',
    'sphinx.ext.doctest',
    "sphinx.ext.viewcode",
    "sphinx.ext.intersphinx",
    "sphinx.ext.napoleon",
    "sphinx.ext.todo",
    "sphinx.ext.coverage",
    "sphinx.ext.mathjax",
    "sphinx.ext.ifconfig",
    "sphinx_copybutton",
    "sphinx_togglebutton",
    "nbsphinx",
    'numpydoc',
    "IPython.sphinxext.ipython_console_highlighting",
    "IPython.sphinxext.ipython_directive",
    "myst_parser",
    "mdinclude",  # custom module
    "sphinx_rtd_theme",
    "sphinx_autodoc_typehints",
]
```
We have already included an extension `myst_parser` and `m2r2` which enable us to use markdown in the documentation. However, some of the configuration does not work by default therefore, we need to add the Python file named `minclude.py`. It's included in the `docs` directory. 

You need to add these lines in the `conf.py` file to include the `mdinclude.py`.
```python
import pathlib
import sys
import os

sys.path.insert(0, os.path.abspath("."))
sys.path.insert(0, os.path.abspath("../"))
sys.path.insert(1, os.path.dirname(
    os.path.abspath("../")) + os.sep + "path_to_root_directory")

# in this case path_to_root_directory is fancy_calcy
```

As of now, we installed the required extensions and configured them in the `conf.py` file. We will move to the next section to document the Python package.

-----

## Guide to document Python package

There are different Python docstring style guides that could be used. We will be using the Numpydoc style guide. It's a widely used style guide. You can find the details [here](https://numpydoc.readthedocs.io/en/latest/format.html). The package used for this is `numpydoc`. It's included in the `requirements_docs.txt` file.

The other most popular style guide is the Google style guide. You can find the details [here](https://google.github.io/styleguide/pyguide.html#doc-function-args) and [here](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)

You could also see how the function and classes have been documented in the `fancy_calcy` package. It's included in the `fancy_calcy` directory. It also includes some mathematical formulas and equations. Also, it provided usage of linking one module inside another module as a reference.


-----


## Create documentation modules
Now, we have completed the basic setup and also Python code is well-documented using the Numpydoc style guide. Let's move to the next section to create the module-level documentation in the `docs` directory.

By default, the `index.rst` file contains the following content:
```rst
.. fancy-calcy documentation master file, created by
   sphinx-quickstart on Wed Feb 28 23:12:53 2024.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to fancy_calcy's documentation!
=======================================
.. toctree::
   :maxdepth: 2
   :caption: Contents:



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```

Consider this as the table of contents for the documentation. You can add the modules in the `toctree` directive. Sphinx looks for this file to build the documentation. It will include the modules mentioned in the `toctree` directive. If a module is not mentioned in the `toctree` directive, it will not be included in the documentation.

Let's create some markdown files such as `installation.md`, `about.md`, and `codeofconduct.md`. Now add the following lines in the `index.rst` file to include these markdown files in the documentation:
```rst
.. only new lines are shown here:

.. toctree::
   :name: Getting Started
   :caption: Getting Started
   :maxdepth: 1
   :hidden:

   installation

.. toctree::
   :name: Community
   :caption: Community
   :maxdepth: 1
   :hidden:

   codeofconduct
   about
```

As of now, we have not done anything to add the docstrings to the Python files in the documentation. We have on option, we create an `api` folder inside the `docs` directory and add all modules and submodules as defined in the Python package.

Like, we have three modules in the `fancy_calcy` package, namely `basics`, `advanced` and `triangle`. Now, let's folder with the same names in the `api` directory. Each module inside this folder will contain the `index.rst` file and `.rst` files for each function and class.

Let's take an example of the `basics` module. It contains one Python c; We will create the following files in the `api/basics` directory with two files, namely `index.rst` and `BasicOperations.rst`.

The `docs/api/basics/index.rst` file will contain the following content:
```rst
.. -*- mode: rst -*-

.. currentmodule:: fancy_calcy.basics

Basic Operations
=================

This module provides some basic functionalities for calculation.

=================================== =========================================================================
 Functions                            Description
=================================== =========================================================================
:class:`BasicOperations`              Provide class methods for addition, substraction etc.
=================================== =========================================================================

.. toctree::
   :maxdepth: 1
   :hidden:

   BasicOperations
```
This file will used in the main `index.rst` placed inside `docs/` file to include the `basics` module in the documentation. Similarly, we will create the `docs/api/advanced` and `docs/api/triangle` directories with the `index.rst` and `.rst` files for each function and class.

Once these modules are created, you can add these modules in the `docs/index.rst` file to include them in the documentation. You can also add the markdown files in the `index.rst` file to include them in the documentation.
```rst
.. only new lines are shown here:

.. toctree::
   :name: API Docs
   :caption: API Docs
   :maxdepth: 1
   :hidden:

 
   api/basics/index
   api/advanced/index
   api/triangle/index
```

Now, let's add Python examples to the documentation. We create an `examples.rst` file in the `api` directory. It will contain the following content references to the various Python examples included in the `advanced_example.rst` and `simple_example.rst` files. The actual Python files are present in the `examples` directory at the root of the project. Now, include the `examples.rst` file in the `index.rst` file to include it in the documentation.
```rst
.. only new lines are shown here:

.. toctree::
   :name: Examples
   :caption: Examples
   :maxdepth: 1
   :hidden:

   api/examples
```

We are all set to build the documentation. We will move to the next section to test the documentation locally.

## Testing documentation locally
You can run the following command to build the documentation locally:
```bash
cd docs
make clean
make html
```

Go to the `_build` directory and open the `index.html` file in the browser. You will see the documentation with the modules mentioned in the `toctree` directive.

Check the HTML-rendered documentation, navigate through the different modules, and see if the documentation is rendered correctly. If you see any issues, you can fix them and rebuild the documentation using the above command.


## Setting up Readthedocs

We would like to host the documentation on Readthedocs. It's a widely used platform to host the documentation. It's free and open source. You can create an account on Readthedocs and import the repository to host the documentation.

Please follow the following steps to create an account on [Readthedocs](https://readthedocs.org/).

Readthedocs uses the `readthedocs.yml` file to build the documentation. It contains the configuration for the documentation. You can create a `.readthedocs.yml` file at the root of the project. It will contain the following content:

```yaml
# .readthedocs.yaml
# Read the Docs configuration file
# See https://docs.readthedocs.io/en/stable/config-file/v2.html for details

# Required
version: 2

# Set the OS, Python version and other tools you might need
build:
  os: ubuntu-22.04
  tools:
    python: "3.10"
    # You can also specify other tool versions:
    # nodejs: "19"
    # rust: "1.64"
    # golang: "1.19"

# Build documentation in the "docs/" directory with Sphinx
sphinx:
  configuration: docs/conf.py

# Optionally build your docs in additional formats such as PDF and ePub
formats:
   - pdf
#    - epub

# Optional but recommended, declare the Python requirements required
# to build your documentation
# See https://docs.readthedocs.io/en/stable/guides/reproducible-builds.html
python:
   install:
   - requirements: docs/requirements_docs.txt
```


1. Create a Readthedocs account using your GitHub account.
2. Click on the `Import a Project` button.
3. Select the repository you would like to import.
5. Select the `branch` you would like to build the documentation from, generally, it's `main`.
6. Finally `Build the documentation`.

We followed the same approach, now the documentation is hosted on Readthedocs. You can access the documentation using the following link: [fancy_calcy](https://docs-demo-2024.readthedocs.io/en/latest/index.html).

## Automatic deploy documents
Now, you have hosted the documentation on Readthedocs. Nevertheless, the documents get updated from time to time. It's a good practice to automate the deployment of the documentation. You can use the following steps to automate the deployment of the documentation:

You have selected the `main` branch to build the documentation. Therefore, any changes made to the `main` branch will trigger the build of the documentation. You can also select the `tags` to build the documentation. It's up to you.
