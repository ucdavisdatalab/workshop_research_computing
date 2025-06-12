# Environment Managers

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Explain what computing environments are
* Explain what virtual environments are and why they're useful
* List popular tools for installing software on POSIX computers
:::


## What's an Environment?

A **computing environment** is a collection of hardware, software, and
associated settings. Whenever you run software (including code) on your
computer, it runs in a computing environment. Being able to set up, inspect,
maintain, and document computing environments is important because:

* A computer might not have the software you need pre-installed. Often the best
  and sometimes the only solution is to install the software yourself. Doing it
  yourself gives you more control over what's installed and is typically faster
  than asking an administrator to do it for you. For your personal computer,
  you're probably the sole administrator. You can use the same tools to set up
  and maintain environments on your computer as you do on remote computers.

* Different projects may require different computing environments, and you may
  need to switch between them frequently. Switching software environments and
  settings can be quick and painless with the right tools. With modern compute
  clusters and cloud computing services, even switching hardware environments
  can be relatively easy.

* Specifying a required software environment, with versions, can make it easier
  to collaborate on, distribute, revisit, and reproduce research projects.
  Differences in environment can cause major errors or subtle bugs.

* Inspecting the computing environment is the first step to diagnosing most
  computing problems. If you ask someone for help, they'll likely want to know
  which hardware, software, and settings you're using.

In a high-level programming language like R or Python, details of the hardware
are mostly hidden away. That is, hardware has limited influence on how you
write code (with exceptions for a few specific use cases, such as GPU
computing). Hardware affects how quickly your code runs, but usually not the
final result. For many research computing projects, hardware is less of a
concern than software, so this chapter focuses on software environments.

<!--
Chapter 4 addresses ways to choose the hardware environment on compute clusters
and cloud computing services.

---

One of the major advantages of Python over other programming languages is the
massive number of packages developed and published by members the community. As
Python and Python packages are updated, code designed for older versions may
need to be edited to continue to work correctly with the newer versions. So for
most Python projects, keeping track of the software environment means keeping
track of the specific versions of Python and Python packages for which the code
was designed.
-->


## Environment Managers

A **package manager** is a tool that can download, install, update, and remove
software packages. If you've used R or Python, you might already be familiar
with the package managers they provide. Many modern operating systems also
provide a package manager, because package managers have several benefits. They
can:

* Automatically select packages compatible with the computing environment
* Automatically install dependencies for packages
* Update installed packages, often automatically or with a single command
* In some cases, provide guarantees that packages are not malicious

:::{note}
Most Linux distributions provide a package manager as the recommended way to
install software. Nevertheless, it's possible to install software on Linux
without a package manager. One way is to download the source code for the
software and compile it yourself; another is to download a pre-built binary.
[FlatPak][] and [AppImage][] are two popular formats for distributing pre-built
binaries.

Install software via a package manager when possible, but be aware that there
are alternatives when it's not.

[FlatPak]: https://flatpak.org/
[AppImage]: https://appimage.org/
:::

Some, but not all, package managers can create **virtual environments**:
self-contained environments that can coexist alongside others, even if they
contain conflicting packages. You can think of a virtual environment as being
like a terrarium for a collection of packages.

Virtual environments make it easier to work on projects with different software
requirements simultaneously. For example, suppose one of your projects requires
Python 3.13 or newer, but another uses a package that hasn't been updated since
Python 3.9. You can work on either project as needed if you create two virtual
environments and switch between them: one with Python 3.13 and one with Python
3.9.

In the strictest sense, an **environment manager** is a tool that can create,
modify, and delete virtual environments. There are environment managers that
are not package managers (and vice-versa), but from here on we'll use the terms
somewhat interchangeably.

[Pixi][pixi] is the environment manager we recommend and use. Pixi is related
to the popular environment manager [Conda][]: both install conda packages from
[conda-forge][], a community-led repository of packages for research computing.
There are packages on conda-forge for R and Python, as well as other
programming languages and tools. Pixi can also install packages from other
sources and repositories (most notably, from the [Python Package Index][pypi]).

[pixi]: https://pixi.sh/latest/
[Conda]: https://conda.org/
[conda-forge]: https://conda-forge.org/
[pypi]: https://pypi.org/

We recommend Pixi over Conda because Pixi creates environments that are fully
reproducible, takes a project-centric approach to environments, is noticeably
faster, and lacks many of Conda's quirks and pitfalls. That said, Pixi is
relatively new, so you might occasionally encounter missing features or bugs.

:::{important}
Pixi is available for Windows, macOS, and Linux, and generally doesn't require
administrator privileges to install.

Install Pixi by following [the official instructions][pixi].
:::

:::{note}
Examples of other popular package and environment managers are:

* [Homebrew][] for macOS and Linux
* [Chocolatey][] for Windows
* Advanced Packaging Tool (APT) for Debian-based Linux distributions
* [Nix][] for Linux and macOS
* [Spack][] for Linux and macOS, focused on high-performance computing
* [EasyBuild] for Linux, focused on research computing

[Homebrew]: https://brew.sh/
[Chocolatey]: https://chocolatey.org/
[Spack]: https://spack.io/
[Nix]: https://nixos.org/
[EasyBuild]: https://easybuild.io/
:::

:::{note}
Virtualization tools, such as [Podman][], [Docker][], and [VirtualBox][] are a
different way to create isolated computing environments. They provide complete
control over the operating system and software in an environment, so they
provide stronger guarantees of reproducibility. The cost is that these tools
are often slower than environment managers and using them requires more
technical knowledge.

For most research projects, using an environment manager provides adequate
flexibility and reproducibility.
:::

[Podman]: https://podman.io/
[Docker]: https://www.docker.com/
[VirtualBox]: https://www.virtualbox.org/
