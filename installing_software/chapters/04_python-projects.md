(chapter-reproducible)=
Making Python Projects & Environments Reproducible 
==================================================

:::{admonition} Learning Objectives
* Create and organize project directories for projects
* Create file names that are human and machine readable
* Identify when to use Python scripts versus Jupyter notebooks
* Explain different strategies for storing parameters and other configuration
  details
* Explain what version control is and why it's beneficial
* Explain what computing environments and virtual environments are
* Describe the different environment managers available for Python
* Use conda to create, manage, save, and restore virtual environments
:::

This chapter focuses on strategies and advice to make your Python projects
well-organized and **reproducible**&mdash;minimizing the obstacles for people
interested in understanding, reproducing results, contributing, or
collaborating. Doing so encourages community engagement, makes it easier for
you to expand upon or reuse parts of the project in the future, and is a
cornerstone of responsible scientific research.

Most of this chapter is broadly applicable to any computing project, including
projects which don't use Python. The second half of the chapter describes how
to use conda to manage Python and Python packages, but conda can also be used
to manage other scientific computing software.


Prerequisites
-------------

This chapter assumes you already have basic familiarity with Python. DataLab's
[Python Basics Reader][py-basics] and its accompanying workshop provide a
suitable introduction.

[py-basics]: https://ucdavisdatalab.github.io/workshop_python_basics/

This chapter also assumes you already have basic familiarity with the UNIX
command-line and shell commands. DataLab's [Introduction to the UNIX Command
Line Reader][intro-cmd] and its accompanying workshop provide a suitable
introduction.

[intro-cmd]: https://ucdavisdatalab.github.io/workshop_introduction_to_the_command_line/

To follow along, you'll need the following software versions (or newer)
installed on your computer:

* [Python][] 3.10

One way to install these is to install the [Anaconda][] Python distribution.
Anaconda is explained in {numref}`whats-an-environment`.

[Python]: https://www.python.org/
[Anaconda]: https://www.anaconda.com/


What's a Project Directory?
---------------------------

Whenever you start a new computing project, no matter how small, I recommend
that you create a new **project directory** as a centralized place to store all
of the project's files. As you produce or download new files, make sure that
they're also stored in the project directory.

Some examples of files you should store in a project directory are:

* Documentation, such as a file manifest and instructions for use
* Code, such as notebooks and scripts
* Inputs, such as data sets and configuration files
* Outputs, such as reports, figures, and intermediate data sets
* License information (if the project will be shared with anyone)

By using a project directory to centralize all of the files in a project, it's
easier to:

* Find files, because you know where to look or to run search software
* Move or copy the project to other computers
* Share the project with collaborators, colleagues, or the public
* Create backup copies of the project to protect your work
* Access and run files with Python and other command-line tools
* Use version control software to manage different versions of project files

The gold standard is for a project directory to be completely **portable**,
meaning you can copy the directory to another computer, follow included
instructions to setup necessary software (such as Python), and then run the
code *without any modifications* to get the expected result.

The following subsections describe best practices for project directories, in
approximate order from highest to lowest priority.


### Use Relative Paths!

:::{note}
This section assumes you're familiar with file paths. You can find a detailed
introduction to file paths in [this section][paths] of DataLab's Python Basics
Reader.
:::

[paths]: https://ucdavisdatalab.github.io/workshop_python_basics/chapters/01_python-basics.html#file-systems

Perhaps the most common way projects fail to be portable is by including file
paths that make assumptions about where the project directory is located. As an
example, suppose Taylor is working on a project to analyze data about the
population explosion of sea urchins along the California coast. Suppose the
project directory is at:

```text
/Users/taylor/ca_sea_urchins/
```

And the contents of the project directory are:

```text
ca_sea_urchins/
├── analysis.ipynb
├── data/
│   ├── 2021-q1_ca_urchins.csv
│   ├── 2021-q2_ca_urchins.csv
│   ├── 2021-q3_ca_urchins.csv
│   ├── 2021-q4_ca_urchins.csv
│   └── 2022-q1_ca_urchins.csv
├── LICENSE
├── README.md
└── report.docx
```

If Taylor wants to load the 2022 data set in the `analysis.ipynb` notebook,
they could use the path:

```text
/Users/taylor/ca_sea_urchins/data/2022-q1_ca_urchins.csv
```

This path is an **absolute path**, meaning it begins from the **root
directory** (the top level `/`) of Taylor's file system. The path assumes that
the project directory `ca_sea_urchins/` is in the directory `/Users/taylor/`,
but that might not always be true. If Taylor shares the project with their
colleague Sam, then Sam will probably have to edit the path to make the code
work correctly.

You can avoid this problem by making sure every path in your project is a
**relative path**, written relative to files within the project directory. For
instance, the path from `analysis.ipynb` to the 2022 data set is:

```text
data/2022-q1_ca_urchins.csv
```

This path avoids any assumptions about the location of the project directory
(and more generally, about *anything outside of the project directory*).

In summary, you can make your projects more portable by using only relative
paths.


### Naming Files

When naming files in a project directory, choose names that are both human
readable and **machine readable**.

Choosing human readable file names ensures that you and others will be able to
determine the purpose of files without needing to open or otherwise inspect
them. The key is to put descriptive, unambiguous information in the name.
Consider the audience for the project, and avoid any acronyms, abbreviations,
or jargon that won't be familiar to the entire audience.

Choosing machine readable file names ensures that paths to your files will work
in most software (such as Python) and that the file names can be parsed to
extract metadata. Some basic rules for machine readable names are:

* Don't use whitespace characters. Whitespace is not always supported by older
  software and requires special treatment (such as escape characters) in modern
  software. Instead:
    + Use dashes `-` to separate parts of a single phrase. For example,
      `sea-urchin-report` or `2022-05-02`.
    + Use underscores `_` to separate distinct phrases. For example,
      `2022-05-02_sea-urchin-report` contains two distinct phrases: a date and
      a description.
    + If you want, you can substitute a different pair of characters for dashes
      and underscores. The key is to be consistent and to make sure the
      characters are distinct and valid on most operating systems.

* Write numbers in time sequences, especially dates, from slowest to fastest.
  For example, a good format for dates is `yyyy-mm-dd`. More generally, see
  [ISO 8601][], the international standard for formatting dates and times. This
  ensures files will be sorted correctly by tools that use dictionary ordering.

* Pad numbers in sequences with leading zeroes. For instance, in a collection
  of 1000 images of trees, the name `tree0003.jpg` is preferable to
  `tree3.jpg`. Try to plan ahead, padding enough that you won't need to
  increase the number of digits later. As with the previous point, this ensures
  files will be sorted correctly by tools that use dictionary ordering.

* For Python scripts (`.py` files), make sure the name begins with a letter.
  This is especially important if you want to import code from one script into
  another, because Python's `import` keyword requires files/packages to start
  with a letter.

:::{note}
Some guides recommend using **camel case**, where parts of a phrase are
delineated by capital letters, `likeThis`. I recommend against camel case in
file names, because how acronyms should be written is ambiguous. For example,
if your project is "UC Davis Sheepmowers", should you write
`UCDavisSheepmowers` or `UcDavisSheepmowers`?
:::

For more suggestions and examples about how to name files, see:

* [This section][readme-names] of DataLab's README, Write Me! Reader
* [This chapter][turing-names] of The Turing Way
* [These slides][jennybc-slides] from Jenny Bryan

[ISO 8601]: https://en.wikipedia.org/wiki/ISO_8601
[readme-names]: https://ucdavisdatalab.github.io/workshop_how-to-data-documentation/#file-names
[turing-names]: https://the-turing-way.netlify.app/project-design/filenaming.html
[jennybc-slides]: https://speakerdeck.com/jennybc/how-to-name-files

(organizing-files)=
### Organizing Files

Use subdirectories to organize files within a project directory. Here's the
basic directory structure I recommend and use for data science projects:

```text
env.yml       Conda environment
LICENSE       License for the project
README.md     Markdown file that describes the project
data/         Data sets
docs/         Supporting documents
notebooks/    Jupyter and RMarkdown notebook files
src/          Source code (scripts)
```

This is loosely based on [Cookiecutter Data Science][cookiecutter], a
standardized directory structure for data science projects.

Every project is different and has different needs; you should tailor the
directory structure to meet them. For example, some directories I occasionally
include in projects are:

* `R/` to store R code (because the R community prefers this over `src/`)
* `output/` to store output files
* `config/` or `models/` to store configuration files or model descriptions
* `log/` to store logs from runs of the code

What's most important is that you choose descriptive names for subdirectories
and use them in a consistent way. It's also a good idea to include a **file
manifest** in your project's documentation (typically in the `README` file)
that describes the purpose of each file and subdirectory, similar to the
listing at the beginning of this subsection.

For more suggestions and examples about how to organize files, see:

* [This section][readme-dir] of DataLab's README, Write Me! Reader
* [This chapter][turing-dir] of The Turing Way
* [This chapter][hitch-dir] of The Hitchhiker's Guide to Python

[readme-dir]: https://ucdavisdatalab.github.io/workshop_how-to-data-documentation/#project-directory-structure
[turing-dir]: https://the-turing-way.netlify.app/project-design/project-repo/project-repo-advanced.html
[cookiecutter]: https://drivendata.github.io/cookiecutter-data-science/
[hitch-dir]: https://docs.python-guide.org/writing/structure/


### Documentation

A project directory should always include a **README**, a plain text file that
serves as an introduction to and overview of your project. At a minimum, the
README should include:

* The title of the project
* A description of the project
* The name of the project maintainer
* A way to contact the project maintainer
* The names of all other project contributors

It's also common for READMEs to include:

* A file manifest
* Hardware and software requirements
* Instructions for running the project
* Instructions for contributing to the project

The [Markdown][] formatting language is a popular way to format README files.

[Markdown]: https://learnxinyminutes.com/docs/markdown/

In addition to a README, include documentation in your code. I recommend that
you:

* Put a comment at the beginning of each script to describe the purpose of
  the script. Where appropriate, you can also include inputs, outputs, and
  usage information. In notebooks, make a note at the beginning rather than a
  comment.

* Use comments to frame and explain code, particularly any code that feels
  complicated. Writing the comments *before* you the write code can also be a
  good way to guide development.

* Write Python **docstrings** for functions defined in your code. A docstring
    is a string at the beginning of a function (the first line after `def`)
    that describes how the function works.  At a minimum, you should provide a
    brief description of what the function does. It's also common to describe
    the parameters (inputs), describe the return value (output), and provide
    examples of use. Docstrings are accessible through Python's `help`
    function. Here's an example of a function with a docstring:

    ```python
    def central_indexes(n, k = 5):
        """Compute the k central indexes for an array with n elements.
    
        Parameters
        ----------
        n : int
            The length of the array.
        k : int
            The number of indexes to compute.

        Returns
        -------
        A list of indexes.
        """
        if n <= k:
            return list(range(n))
    
        span = (k - 1) / 2
        below = int(np.ceil(span))
        above = int(span)
        midpoint = int(np.ceil((n - 1) / 2))
        return list(range(midpoint - below, midpoint + above + 1))
    ```

For more suggestions and examples of how to write documentation, see:

* DataLab's [README, Write Me! Reader][readme]
* [This chapter][hitch-docs] of The Hitchhiker's Guide to Python
* [Write the Docs][wtd], a global community dedicated to writing good
  documentation

[readme]: https://ucdavisdatalab.github.io/workshop_how-to-data-documentation/
[hitch-docs]: https://docs.python-guide.org/writing/documentation/
[wtd]: https://www.writethedocs.org/


### Scripts vs. Notebooks

There are two common ways to store Python code:

* Python scripts (`.py` files).
* [**Jupyter notebooks**][jupyter] (`.ipynb` files)

[jupyter]: https://jupyter.org/

Notebooks provide a way to develop code **interactively**, meaning you can run
code in small chunks, quickly switching between writing code and running code.
Notebooks also support formatted text, images, and a selection of other
programming languages. As a result, notebooks are well-suited for:

* Data exploration and open-ended analyses
* Testing potential solutions to problems
* Learning how use unfamiliar packages or code
* Creating reports or presentations
* Teaching

The primary way view and edit notebooks is through a web browser, which makes
them inconvenient if you want to work at the command-line (on a server, for
example) or with command-line tools (such as git). The [Jupytext
project][jupytext] provides a partial remedy by making it easy to convert
between notebooks and plain text formats.

[jupytext]: https://jupytext.readthedocs.io/

Scripts tend to be more useful than notebooks for code that doesn't need to run
interactively. Scripts are well-suited for:

* Creating programs designed to run with minimal intervention (for example, on
  a server)
* Creating programs to use at the command-line
* Creating **modules** that contain reusable code. A module is a `.py` file
  designed to be imported into other Python scripts with the `import` keyword.
  Modules usually contain function and class definitions, but can also contain
  other code. Creating modules is a good way to organize your code, especially
  if you have multiple scripts that all use a few common functions or classes.
* Creating Python packages

Data science projects often include a mix of notebooks and scripts. Whenever
you start working on a new task, consider which format is more appropriate for
what you're trying to do.

:::{tip}
If you use the directory structure described in {numref}`organizing-files` and
create Python modules, it can be difficult to figure out how to import the
modules for code that exists outside of the `src/` directory, such as notebooks
(in `notebooks/`). Adding this code snippet to the beginning of a notebook or
script makes it possible to import modules from the `src/` directory:

```python
import os
import sys

# Assemble a path to `../src/`
module_path = os.path.abspath(os.path.join("..", "src"))
# Append the path to the module search list
if module_path not in sys.path:
    sys.path.append(module_path)
```

This assumes `../src/` is the relative path to the `src/` directory from your
notebook or script. If that's not the correct you'll have to edit the path in
the `module_path` variable.
:::

You can learn more about how to create Python modules and packages in [this
chapter][hitch-module] of The Hitchhiker's Guide to Python.

[hitch-module]: https://docs.python-guide.org/writing/structure/#modules

### Parameters & Configuration Files

Most code, especially in scientific computing, includes a collection of
adjustable parameters. For instance, code to run a simulation might allow for
different initial conditions and code to fit a statistical model might allow
for different formulations of the model.

Here are a few ways to create a parameterized script:

* Make the parameters global variables within the script. Then users can
  set the parameters by editing the variables. In this case, it's good practice
  to put the parameter definitions at the beginning of the script, write the
  parameter names in `ALL CAPS`, and use comments to document each parameter.

*   Create a `main` function in the script that runs the rest of the code.
    Call the `main` function in guard condition that checks whether the script
    was deliberately executed (rather than `import`ed):

    ```python
    if __name__ == "__main__":
        main()
    ```

    The parameters of the main function are the parameters of the script. Then
    users can set the parameters by editing the call to `main` (or more
    generally, the code in the guard condition).

* Set the parameters with command-line arguments to the script. The `argv`
  object in Python's built-in `sys` module is the list of command-line
  arguments. Python's built-in [`argparse`][argparse] module provides a richer
  set of functions for parsing command-line arguments.

* Set the parameters with a configuration file read by the script. Two options
  are:
    + Python's built-in [`json`][json-module] module provides a way to read
      settings from a **JavaScript Object Notation** (JSON) file. JSON is a
      plain text format for storing lists and dictionaries of numbers, strings,
      and other data.
    + The [`tomli`][tomli] package provides a way to read settings from a
      **Tom's Obvious Minimal Language** (TOML) file. The [TOML format][toml]
      has more features and is easier to read and write than the [JSON
      format][json]. Moreover, [`tomli` may soon be built-in to
      Python][pep680].


The options above are listed from lowest to highest complexity, but also from
lowest to highest reproducibility. Using separate configuration files (the last
option) is the best way to ensure that your project is reproducible, because
you can create a new configuration file to record the parameters for each run
of the code.

[configparser]: https://docs.python.org/3/library/configparser.html

[argparse]: https://docs.python.org/3/library/argparse.html
[json-module]: https://docs.python.org/3/library/json.html
[tomli]: https://github.com/hukkin/tomli
[toml]: https://toml.io/
[json]: https://en.wikipedia.org/wiki/JSON
[pep680]: https://peps.python.org/pep-0680/


Version Control
---------------

A **version control system** (VCS) is software that tracks the changes you make
to files, so that you can go back to a previous version at any time. You might
already be familiar with the version control systems built-in to Microsoft Word
and Google Docs. Many different version control systems exist for code, but the
most popular one is [**git**][git].

[git]: https://git-scm.com/

I recommend that you use version control for every project. By using
version control, particularly git, you can:

* Access older versions of files at any time
* Back up your project to the cloud
* Share your project with collaborators or the public
* Merge changes when you and a collaborator both edit the same file

Learning to use a version control system can be difficult, but it's well-worth
the time and effort. DataLab's [Introduction to Version Control
Reader][intro-vcs] and workshop are a good way to learn more.

[intro-vcs]: https://ucdavisdatalab.github.io/workshop_introduction_to_version_control/

:::{note}
Most version control systems are designed for versioning text or code rather
than data. However, versioning data is important for many projects, and the
growth of data science as a discipline has led to multiple efforts to develop
version control systems for data.

One VCS for data that looks promising is [`dvc`][dvc], which is designed to
coexist with git and provides a similar interface.
:::

[dvc]: https://dvc.org/


(whats-an-environment)=
What's an Environment?
----------------------

A **computing environment** is a collection of hardware and software used to
run code. Whether code runs correctly or at all depends on the computing
environment, so an important part of making a project reproducible is
documenting the environment where the code was originally developed and tested.

In a high-level programming language like Python, details of the hardware are
mostly hidden away. That is, hardware has little to no impact on how you write
Python code, with the exception of a few specific applications such as GPU
computing. Hardware may affect how quickly your Python code runs, but usually
not the final result. As a consequence, for Python projects (and many
scientific computing projects in general) the hardware environment is generally
less of a concern than the software environment.

One of the major advantages of Python over other programming languages is the
massive number of packages developed and published by members the community. As
Python and Python packages are updated, code designed for older versions may
need to be edited to continue to work correctly with the newer versions. So for
most Python projects, keeping track of the software environment means keeping
track of the specific versions of Python and Python packages for which the code
was designed.

A **package manager** is a program that can install, update, and remove
packages. Python's built-in package manager is [`pip`][pip]. Even if you use an
alternative package manager, it's good to know the basics of pip in order to
troubleshoot problems with package installations.

[pip]: https://pip.pypa.io/en/stable/


### Virtual Environments

A **virtual environment** is a computing environment with specific software
versions that can coexist alongside other virtual environments with different
software versions. Virtual environments make it possible to work on several
different projects at once, even if they require different computing
environments.

There are several options for managing Python virtual environments:

* [`venv`][venv] is Python 3's built-in module for managing virtual
  environments, based on virtualenv.
* [`virtualenv`][virtualenv] is the most popular tool for managing virtual
  environments for Python 2, and also supports Python 3. It provides more
  features than venv.
* [`pipenv`][pipenv] is an integrated environment and package manager.
* [`poetry`][poetry] is a relatively new integrated environment and package
  manager. It also provides tools to create and publish packages.
* [`conda`][conda] is an integrated environment and package manager originally
  designed with data science projects in mind.

Unlike the other tools listed here, conda can manage environments for a variety
of programming languages, not just Python. This is its standout feature, and
the reason why the rest of this chapter will focus on conda rather than any of
the others.

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io/
[pipenv]: https://pipenv.pypa.io/
[poetry]: https://python-poetry.org/
[conda]: https://docs.conda.io/projects/conda/



Managing Environments with conda
--------------------------------

There are two major ways to get conda:

* Install [Anaconda][]. Anaconda is a Python distribution designed to be
  ready-to-use for data science. It includes over 1,500 packages and as a
  consequence, it requires at least 3 GB of hard disk space to install.
* Install [Miniconda][]. Miniconda is a Python distribution that only includes
  Python, conda, and a few packages conda depends on. Miniconda is much smaller
  than Anaconda, but leaves it up to you to install the packages you want to
  use.

[Miniconda]: https://docs.conda.io/en/latest/miniconda.html

I recommend Miniconda because it only uses hard disk space for the packages you
actually want. Moreover, because you have to install the packages yourself,
you'll quickly become familiar with conda's basic commands.

Both Anaconda and Miniconda provide detailed installation instructions on their
respective websites.


### Creating Environments

Once you have conda installed, get a list of your conda environments with the
shell command:

```sh
conda env list
```

The default environment for conda is called `base`. It's generally recommended
to avoid installing packages in `base` because it's the environment where conda
itself is installed. Any problems you create in the `base` environment can
potentially break your entire conda installation.

You can create a new environment with the command `conda create`. For example,
the command to create a new environment called `datasci` is:

```sh
conda create --name datasci
```

You can get help with most conda commands by appending `--help`. For instance,
you can see all of th options for `conda create` with the command:

```sh
conda create --help
```

You can switch to or **activate** a specific conda environment with the command
`conda activate`. Go ahead and activate the `datasci` environment:

```sh
conda activate datasci
```

Once an environment is activated, you can install software with the command
`conda install`. For example, this command installs Python:

```sh
conda install python
```

You can request a specific version of software by appending `=`, `>=`, `>`,
`<=`, or `<` and a version number after the name, and then enclosing the name
and version in single quotes. For example, this command specifically requests
the latest version of Python 2:

```sh
conda install 'python<3'
```

Say no at the prompt so that your environment still has Python 3.

By default, conda installs packages from Anaconda's package repository. The
packages in Anaconda's package repository tend to work well together, but can
be slightly out of date. The community-maintained `conda-forge` package
repository often has newer versions and also has many packages that are not
available through Anaconda. You can specify that you want to install a package
from conda-forge by including `-c conda-forge` in the `conda install` command.
For instance, to install `numpy` from conda-forge:

```sh
conda install -c conda-forge numpy
```

:::{tip}
A common complaint about conda is that it takes a long time for conda to
install new packages. [**Mamba**][mamba] is a drop-in replacement for conda
that's designed to be substantially faster. You can install Mamba in a conda
environment with the command:

```
conda install -c conda-forge mamba
```

Then you can replace `conda` with `mamba` in any conda command.
:::

[mamba]: https://github.com/mamba-org/mamba

You can see a list of installed software in a conda environment with the
command:

```sh
conda list
```

You can also use the commands `conda update` and `conda uninstall` to update
and uninstall packages within an environment.

When you're finished using a conda environment, you can deactivate it with the
command:

```sh
conda deactivate
```

Finally, you can delete an environment with the command `conda env remove`. Use
the `--name` option to provide the name of the environment.


### Saving & Restoring Environments

In order to make a project reproducible, you should include information about
the conda environment used to develop and test the code. You can export the
details of a conda environment to a file with the command `conda env export`.
For example, to export the details of the `datasci` environment:

```sh
conda env export --name datasci > env.yml
```

Then anyone conda and a copy of your project directory can install the
environment on their machine with the command:

```sh
conda env create --file env.yml
```

Note that you can include `--name NAME` in this command to explicitly set the
name of the new environment to `NAME`.


### Additional References

The [conda cheatsheet][conda-cheat] provides a list of the most common conda
commands.

[conda-cheat]: https://docs.conda.io/projects/conda/en/latest/_downloads/843d9e0198f2a193a3484886fa28163c/conda-cheatsheet.pdf

For more suggestions and examples of how to use conda, see:

* [The conda documentation][conda-docs]
* [This chapter][remote-conda] of DataLab's Introduction to Remote Computing
  Reader

[conda-docs]: https://docs.conda.io/projects/conda/
[remote-conda]: https://ngs-docs.github.io/2021-august-remote-computing/installing-software-on-remote-computers-with-conda.html
