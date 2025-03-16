# Python Projects

This chapter focuses on strategies and advice to make your Python projects
well-organized and **reproducible**&mdash;minimizing the obstacles for people
interested in understanding, reproducing results, contributing, or
collaborating. Doing so encourages community engagement, makes it easier for
you to expand upon or reuse parts of the project in the future, and is a
cornerstone of responsible scientific research.

Most of this chapter is broadly applicable to any computing project, including
projects which don't use Python.


## Virtual Environments

Python's built-in package manager is [`pip`][pip]. Even if you use an
alternative package manager, it's good to know the basics of pip in order to
troubleshoot problems with package installations.

[pip]: https://pip.pypa.io/en/stable/


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
