# Installing Software

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Explain what virtual environments are and why they're useful
* List popular tools for installing software on POSIX computers
* Explain the difference between conda and mamba
* Install mamba via miniforge
* Create virtual environments with mamba
* Install software with mamba
:::

<!-- TODO: Define "computing environment" in the intro -->

This chapter will show you how to set up a computer so that it has the software
you need. You'll learn how to install software whether or not you have
administrator access.

## What's an Environment?

A **computing environment** is a collection of hardware, software, and
associated settings. Whenever you run software (including code) on your
computer, it runs in a computing environment. Being able to set up, inspect,
maintain, and document computing environments is important because:

* A computer may not have the software you need pre-installed. Often the best
  and sometimes the only solution is to install the software yourself. Doing it
  yourself gives you more control over what's installed and is typically faster
  than asking an administrator to do it for you. For your personal computer,
  you may be the sole administrator. You can use the same tools to set up and
  maintain environments on your computer as you do on remote computers.

* Differences between software versions can cause subtle bugs. Specifying a
  required software environment, with versions, can make it easier to
  collaborate on, distribute, and revisit research projects.

* Different projects may require different computing environments, and you may
  need to switch between them frequently. Switching software environments and
  settings can be quick and painless with the right tools. With modern compute
  clusters and cloud computing services, even switching hardware environments
  can be relatively easy.

* Inspecting the computing environment is the first step to diagnosing most
  computing problems. If you ask someone for help, they'll likely want to know
  which hardware, software, and settings you're using.

This chapter focuses on software environments and settings, because configuring
these is often the first thing you'll need to do, and because it's not always
possible to choose your hardware environment.

<!--
Chapter 4 addresses ways to choose the hardware environment on compute clusters
and cloud computing services.
-->


## Environment Managers

A **package manager** is a software tool that downloads and installs software
packages. If you've used R or Python, you may already be familiar with the
package managers these languages provide. Many modern operating systems also
provide a package manager, because package managers have several benefits. They
can:

* Automatically select software compatible with the computing environment
* Automatically install dependencies for software
* Update installed software, often automatically or with a single command
* In some cases, provide guarantees that software packages are not malicious

:::{note}
Most Linux distributions provide a package manager as the recommended way to
install software. Nevertheless, it is possible to install software on Linux
without a package manager. One way is to download the source code for the
software and compile it yourself; another is to download a pre-built binary.
[FlatPak][] and [AppImage][] are two popular formats for distributing pre-built
binaries.

Install software via a package manager when possible, but be aware that there
are alternatives when it's not.
:::

[FlatPak]: https://flatpak.org/
[AppImage]: https://appimage.org/


Sometimes you may need to simultaneously work with several conflicting versions
of software. For example, suppose you're working on a new project that requires
Python 3.12 or newer, but you also want to be able to examine results from an
older project that requires Python 3.9 or older. What should you do?

A lightweight solution is to use an **environment manager**, a software tool
that can create **virtual environments**. Each virtual environment behaves like
an independent software environment, so you can install conflicting software in
separate virtual environments. Many environment managers have a package manager
built in.

[Micromamba][mm] is the environment manager we recommend. Micromamba is based
on the Mamba environment manager, which is, in-turn, based on the [Conda][]
environment manager. The three tools are drop-in replacements for one another,
and it's common to refer to the virtual environments created by all three as
**conda environments**. We recommend Micromamba over Mamba and Conda because it
is substantially faster and because it eliminates some of the pitfalls of the
other two. That said, Micromamba is less mature than Mamba and Conda, so you
may occasionally encounter missing features or bugs. {numref}`micromamba` goes
into detail about how to install and use micromamba.

[mm]: https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html
[Conda]: https://docs.conda.io/
[mamba]: https://mamba.readthedocs.io/

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


:::{note}
A more complex solution is to use virtualization tools, such as [Podman][],
[Docker][], and [VirtualBox][]. These provide complete control over the
operating system and software in a computing environment, so they provide
stronger guarantees of reproducibility. The cost is that these tools are often
slower than environment managers and using them requires more technical
knowledge.

For most research projects, using an environment manager provides adequate
flexibility and reproducibility.
:::

[Podman]: https://podman.io/
[Docker]: https://www.docker.com/
[VirtualBox]: https://www.virtualbox.org/


(micromamba)=
## Micromamba

### Installing Micromamba

The Micromamba documentation includes [official installation
instructions][mm-install]. We recommend the "install script" method for
installing Micromamba, which we describe below with slight changes. You can use
these instructions to install Micromamba on a server or on your personal
computer.

[mm-install]: https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html

:::{important}
Before following the instructions below, make sure to review the [official
installation instructions][mm-install], as they may have changed since this was
written. At the time of writing, the official installation command was:

```sh
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)
```

This command downloads the install script and immediately runs it. In the
instructions below, we separate the download and install steps, to make it
easier to troubleshoot problems and because it's generally a good idea to read
a script you downloaded before running it, to make sure it doesn't do anything
malicious.
:::

:::{note}
On Windows, we recommend installing Micromamba in [Git Bash][git] or the
[Windows Subsystem for Linux][wsl].

While it is possible to install Micromamba in Windows' native `CMD.exe` or
PowerShell terminals, these terminals do not support Unix shell commands, so we
recommend against using them for research computing.

[git]: https://git-scm.com/
[wsl]: https://learn.microsoft.com/en-us/windows/wsl/about
:::


The first step is to download the install script. The official installation
instructions suggest using the `curl` command line tool to do this. Since
`curl` is not pre-installed on some servers, we've also provided instructions
for how to do it with the `wget` command line tool. In a terminal, run:

::::{tab-set}

:::{tab-item} curl

```sh
curl -O -L micro.mamba.pm/install.sh
```

The `-O` flag saves the file with the same name it had on the server
(`install.sh`), while the `-L` flag ensures `curl` follows redirects.

If you see a message like `curl: command not found`, try the `wget` version
instead.
:::

:::{tab-item} wget

```sh
wget micro.mamba.pm/install.sh
```

If you see a message like `wget: command not found`, try the `curl` version
instead.
:::

::::

If the command was successful, you'll have a file `install.sh` in your working
directory. Take a moment to inspect the script with your preferred text editor.
If everything looks okay, make the script executable:

```sh
chmod +x install.sh
```

Then run the script:

```sh
./install.sh
```

The script will prompt you about several installation details:

* `Micromamba binary folder`: where the `micromamba` tool will be installed.
  For most computers, it's fine to accept the default by pressing `Enter`
  without typing anything.
* `Init shell`: whether to configure the shell to find the `micromamba` tool.
  Enter `Y` here unless you know how to configure this yourself.
* `Configure conda-forge`: whether to default to installing packages from the
  conda-forge package repository, which provides additional
  community-maintained packages. Enter `Y` here.
* `Prefix location`: where environments and packages will be installed. Once
  again, it's fine to accept the default by pressing `Enter` without typing
  anything.

Once the script finishes, you must reload your shell's settings before running
Micromamba. To do so, run this command in the terminal:

```sh
source ~/.bashrc
```

{numref}`bash` provides more details about what this command is doing.

Then check that your shell can find Micromamba. In the terminal, run:

```sh
micromamba --version
```

If the installation was successful, you should see a version number like
`1.5.3`. It's okay if you have a different version. If you see an error message
like `micromamba: command not found`, try running `source ~/.bashrc` and then
try the command above again. If you still see an error message, some possible
fixes are to run the install script again or to edit the shell configuration
files (and check the `PATH` environment variable).


### Creating Environments

The [Micromamba documentation][mm] is intended for users already familiar with
Conda. If you're new to environment management or the Conda family of tools,
you may find the [Conda documentation][conda-docs] more helpful for
understanding what commands do and how to use them. There's also a [Conda cheat
sheet][conda-cheat].

[conda-cheat]: https://docs.conda.io/projects/conda/en/latest/_downloads/843d9e0198f2a193a3484886fa28163c/conda-cheatsheet.pdf
[conda-docs]: https://docs.conda.io/projects/conda/en/stable/commands/index.html

:::{tip}
In almost all cases, Conda commands are the same as Micromamba commands---just
replace `conda` with `micromamba`.
:::

:::{caution}
If you find a different Conda cheat sheet online, make sure it's up-to-date!
There are a few very old cheat sheets that rank highly in search engines but
list commands that will not work in Micromamba and recent versions of Conda.
:::

To practice using Micromamba, let's create a new virtual environment and
install a few useful command line tools. You can create a new virtual
environment with the `create` subcommand. Run this command in the terminal
to create a new virtual evironment called `utils`:

```sh
micromamba create --name utils
```

```
Empty environment created at prefix: /home/USERNAME/micromamba/envs/utils
```

When the `create` subcommand runs successfully, it prints out `Empty
environment created at prefix:` and a path. The path is the location where
packages installed in the environment will be stored.

:::{tip}
Creating a new virtual environment for each project you work on is a good way
to avoid software version conflicts and keep track of the software each project
requires. For some projects, you may need to create multiple virtual
environments to accommodate distinct research directions and collections of
software.

Virtual environments are easy to create and remove (aside from the time it
takes to download and install software), so experiment until you find a
workflow that works well for you.
:::

You can list all of the virtual environments Micromamba detects with the `env
list` subcommand. Try running this command in the terminal:

```sh
micromamba env list
```

```
  Name      Active  Path
────────────────────────────────────────────────────────────────
  utils             /home/USERNAME/micromamba/envs/utils
```

In order to use an environment and its installed packages, you must first
**activate** the environment. You can activate an environment with the
`activate` subcommand. Go ahead and activate the `utils` environment:

```sh
micromamba activate utils
```

By default, the name of the active virtual environment (if any) is displayed as
a prefix on the shell prompt. So after running the `activate` subcommand above,
you should see `(utils)` at the beginning of the shell prompt. The environment
will remain active as long as your terminal is open. If you close the terminal
(or log out, for SSH connections) the environment will be deactivated.

:::{note}
You can also deactivate the currently active environment by running `micromamba
deactivate`. It is *not* necessary to do this before activating a different
environment.
:::

Let's install a file search tool, [ripgrep][rg], in the `utils` environment.
The ripgrep (`rg`) tool searches for files which contain a given text string
(or regular expression). You can use the `install` subcommand to install
packages. The default is to install packages into the active environment. You
can list any number of packages after `install`. For instance, to install
ripgrep:

```sh
micromamba install ripgrep
```

The subcommand will list the packages that will be installed and prompt you to
confirm. If you see more than just the requested packages in the listing, it's
because the requested packages have dependencies.

:::{caution}
Whenever you run the `install` subcommand, it will also try to update packages
that are already installed. This is good for keeping your software up-to-date,
but a problem if your project requires specific versions. You can use the
`--freeze-installed` flag to prevent updates to packages that are already
installed.
:::

:::{tip}
You can install a specific version of a package by quoting the name of the
package and using `=`, `<`, `<=`, `>`, or `>=` to indicate the version. For
instance, to install Python 3.9:

```sh
micromamba install 'python=3.9'
```
:::


After the installation is complete, try out one of the commands. For example,
create a file called `foo.txt` with the text `hello world`, and then try
finding it with `rg`:

```sh
echo "hello world" > foo.txt
rg hello
```

You can list all of the packages installed in a virtual environment with the
`list` subcommand. The default is to list packages in the active environment:

```sh
micromamba list
```

```
List of packages in environment: "/home/nick/micromamba/envs/utils"

  Name           Version  Build        Channel
────────────────────────────────────────────────────
  _libgcc_mutex  0.1      conda_forge  conda-forge
  _openmp_mutex  4.5      2_gnu        conda-forge
  libgcc-ng      13.2.0   h807b86a_5   conda-forge
  libgomp        13.2.0   h807b86a_5   conda-forge
  ripgrep        14.1.0   he8a937b_0   conda-forge
```

The list might differ slightly on your computer because of different operating
systems and new versions of packages released since this was written. However,
you should see ripgrep in the list. All of the other packages---which we did
not request directly---are dependencies.

The `create`, `activate`, and `install` subcommands are fundamental to using
Micromamba. There are many other subcommands, which you can learn about by
reading the [Conda documentation][conda-docs].

:::{tip}
The ripgrep search tool is worth knowing about if you work in the command line
frequently. It can help you find files quickly. It's a faster, modernized
versions of the classic `grep` tool.
:::


### Exporting Environments

One of the benefits of using conda environments is that they can be exported
and then recreated later, possibly by other people or on other computers.
You can export a conda environment with the `env export` subcommand. In most
cases, you should also use the `--from-history` flag, which exports only the
packages you installed directly. For example, run this command in the `utils`
environment:

```sh
micromamba env export --from-history
```

```yaml
name: utils
channels:
- conda-forge
- pkgs/main
dependencies:
- ripgrep
```

The subcommand prints out the environment specification in [YAML][yaml], a
human-readable markup language. The `dependencies` section lists the packages
as you installed them, so each package's version is omitted unless you
installed a specific version. The `channels` section lists the channels
(package repositories) from which you downloaded the packages.

[yaml]: https://en.wikipedia.org/wiki/YAML

You'll probably want to save the environment specification rather than just
print it out. You can use the shell `>` command to pipe the output to a file,
to save or share with others. It's a good idea to include the environment name
in the name of the file, and to use a `.yml` or `.yaml` extension. So for the
`utils` environment:

```sh
micromamba env export --from-history > utils.yml
```

After running the command, there will be a `utils.yml` file in your working
directory. You can use a text editor to check its contents.

You can recreate an environment from a specification file with the `env create`
subcommand and the `--file` flag. Let's remove the `utils` environment from
Micromamba, so that we can recreate it from the `utils.yml` file. To remove the
environment, run this command:

```sh
micromamba env remove --name utils
```

Then recreate the environment from `utils.yml`:

```sh
micromamba env create --file utils.yml
```

After the packages finish installing, activate the environment and test that
ripgrep (`rg`) is there.

Keep environment specification files with the code for the project to which
they belong. For instance, if you version your code with git, version your
environment file(s) too. Share the environment specification files with anyone
who needs to work on your project, so that you can make sure their environment
is set up with the same software as yours.

The environment specification files produced by `env export` with the
`--from-history` flag do not include version info for most packages. This is by
design: the format is meant to be cross-platform, and sometimes specific
versions are not available on all platforms. The tradeoff is that when the
environment is recreated, Micromamba might install newer versions of packages,
which might cause compatibility issues. If you know your project depends on
specific versions of packages, make sure to specify the versions when you
install the packages. Alternatively, you can export to a specification format
that has complete version information but is *not* cross-platform by omitting
the `--from-history` flag. The command to recreate an environment from a
specification file in this format is the same.

:::{note}
Environment export is still under development in both the Conda and Mamba
(which includes Micromamba) projects, and there's no consensus yet. As of
writing:

* The [conda-lock][] subproject is a relatively mature tool for exporting
  environments.
* The Conda developers [might add an option to include version information in
  the cross-platform format][conda-issue-10345].
* The Mamba developers [might create a new format][mamba-issue-1209].
:::

[conda-lock]: https://github.com/conda/conda-lock
[conda-issue-10345]: https://github.com/conda/conda/issues/10345
[mamba-issue-1209]: https://github.com/mamba-org/mamba/issues/1209


### Managing Storage

Depending on which packages you install, conda environments can potentially use
10s of gigabytes of storage. Micromamba automatically caches installed packages
and shares them between environments when possible, but there are also a few
things you can do to keep storage usage under control.

First, make sure you use `env remove` to remove old environments that you no
longer use. If you export these environments before removing them, you can
recreate them if you need them again later.

Second, periodically clean up Micromamba's cache to remove old, unused
packages. To do this, run:

```sh
micromamba clean --all
```

This will remove all packages from the cache that are not installed in any
environments. Unless you install packages via symlinks, which is beyond the
scope of this reader, then this is a safe command and will not break your
environments.
