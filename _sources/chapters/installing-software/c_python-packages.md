# Unit C: Python Packages

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Install packages from PyPI with Pixi
* Explain the purpose of `pyproject.toml`
* Create a `pyproject.toml` file
* Use Pixi with `pyproject.toml` instead of `pixi.toml`
:::

This unit covers some specific features to be aware of if you plan to use Pixi
to install software for Python projects.

:::{seealso}
As a complement to this unit, see DataLab's [Python Basics workshop
reader][py-basics].

[py-basics]: https://ucdavisdatalab.github.io/workshop_python_basics/
:::


## PyPI Packages

Python's built-in package manager is [`pip`][pip]. It installs packages from
the [Python Package Index][pypi] (PyPI) rather than conda-forge. Because `pip`
is built into Python and is much older than Conda, most packages are released
on PyPI before conda-forge, and some are never released on conda-forge.
Fortunately, Pixi can install packages from PyPI.

[pip]: https://pip.pypa.io/en/stable/
[pypi]: https://pypi.org/

To install a package from PyPI rather than conda-forge, use `pixi add --pypi`
instead of `pixi add`. For instance, to install `numpy` from PyPI:

```sh
pixi add --pypi numpy
```

:::{caution}
Packages don't always go by the same name on PyPI and conda-forge.
:::

If you’re planning to package your project for distribution on PyPI, it’s
currently best to add only PyPI dependencies, since PyPI and `pip` are not
Conda-aware. In the future, Pixi will add support for packaging projects, and
will likely provide a way to generate PyPI packages even for projects that
depend on packages on conda-forge.

If you're not going to distribute your project on PyPI, it’s generally better
to use packages from conda-forge, since a wider variety of packages (such as
non-Python packages) are available.


## Using `pyproject.toml`

Python [officially specifies][pyproject] that metadata about Python projects
and packages should be placed in a `pyproject.toml` file. Pixi supports using
`pyproject.toml` rather than `pixi.toml` so that you don't have to maintain
both files for a project.

[pyproject]: https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

When you initialize a Pixi project, use `pixi init --format pyproject` instead
of `pixi init` if you want to use `pyproject.toml` rather than `pixi.toml.` For
example, try creating a new project called `my_project2`:

```sh
pixi init --format pyproject my_project2
```

Open the generated `pyproject.toml` file in a text editor:

```toml
[project]
authors = [{name = "YOUR_NAME", email = "YOUR_EMAIL"}]
dependencies = []
name = "my_project2"
requires-python = ">= 3.11"
version = "0.1.0"

[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[tool.pixi.project]
channels = ["conda-forge"]
platforms = ["linux-64"]

[tool.pixi.pypi-dependencies]
my_project2 = { path = ".", editable = true }

[tool.pixi.tasks]
```

The format is slightly different from `pixi.toml`, with the main difference
being that Pixi-specific tables have the prefix `tool.pixi.`.

When you use `pyproject.toml`, Pixi assumes that you're developing a Python
package. Because of this, it automatically installs Python and adds a PyPI
dependency on the project itself. Any `.py` files you place in the project's
`src/project_name/` directory will be treated as modules in the package, and
you can `import` them into Python in the project's virtual environment. This is
convenient even for projects you don't intend to publish as a package.
