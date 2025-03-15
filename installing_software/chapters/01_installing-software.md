Setting Up Software
===================

<!-- TODO: Define "computing environment" in the intro -->

This chapter will show you how to set up a computer so that it has the software
you need and it's a comfortable place to work. Along the way, you'll learn two
fundamental skills:

* How to install software, whether or not you have administrator access
* How to edit configuration files, particularly for the Unix shell

The examples in this chapter focus on remote computers, but these skills are
also useful for configuring laptops and desktops.


:::{admonition} Learning Goals
+ Use UC Davis HPC's module system
+ List popular tools for installing software on POSIX computers
+ Explain what virtual environments are and why they're useful
+ Explain the difference between conda and mamba
+ Install mamba via miniforge
+ Create virtual environments with mamba
+ Install software with mamba
+ Explain the difference between `.bashrc`, `.profile`, and `.bash_profile`
+ Create shell aliases
+ Create and modify environment variables
:::


What's an Environment?
----------------------

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


Installing Software
-------------------

### Package & Environment Managers

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


### UC Davis HPC Modules

:::{important}
This section is specific to UC Davis HPC clusters. While some other servers and
clusters may have a similar module system, many do not.
:::

On most of UC Davis' HPC clusters, the HPC Core Facility staff maintain a
collection of ready-to-use virtual environments called **modules**. These are
an alternative to setting up environments yourself.

:::{note}
After learning to use Micromamba or another environment manager, you may find
it more convenient and less of a cognitive burden to just manage environments
yourself rather than use modules.
:::

You can list all of the modules available on a cluster with the `module avail`
command. For example, as of writing, the output from `module avail` on UC
Davis' Farm cluster begins:

```
----------------------------------------------------------------- /share/apps/22.04/modulefiles/spack/core -----------------------------------------------------------------
cuda/11.7.1  gcc/5.5.0  hwloc/2.9.3                      jdk/17.0.1   julia/1.9.0  julia/1.9.3      nvhpc/23.1         openjdk/16.0.2  pigz/2.7    slurm/23.02.6
gcc/4.9.4    gcc/7.5.0  intel-oneapi-compilers/2022.2.1  julia/1.8.5  julia/1.9.2  libevent/2.1.12  openjdk/11.0.17_8  openmpi/4.1.5   pmix/4.2.6  ucx/1.14.1

----------------------------------------------------------------- /share/apps/22.04/modulefiles/spack/libs -----------------------------------------------------------------
amdfftw/3.2   bzip2/1.0.8  eigen/3.4.0  gmp/6.2.1  h5z-zfp/1.1.0  hypre/2.26.0               librsvg/2.51.0  mpfr/4.1.0      netcdf-cxx4/4.3.1     nvhpc/23.1  zlib/1.2.13
boost/1.80.0  cgal/5.4.1   fftw/3.3.10  gsl/2.7.1  hdf5/1.12.2    intel-oneapi-mkl/2022.2.1  libszip/2.1.1   netcdf-c/4.9.0  netcdf-fortran/4.6.0  pigz/2.7
```

Modules are displayed in groups defined by the server administrators. Each
module has a name and a version number, separated by a slash. For instance,
`julia/1.9.3` is the `julia` module, version `1.9.3`.

To get help with a module, use the `module help` command with the module's
name and optionally its version. For example, to get help with the `julia`
module:

```sh
module help julia
```

```
-------------------------------------------------------------------
Module Specific Help for /share/apps/22.04/modulefiles/spack/core/julia/1.9.3:

The Julia Language: A fresh approach to technical computing
-------------------------------------------------------------------
```

To load a module, use the `module load` command. Again, you need to specify the
module's name and optionally its version. As an example, suppose you decide you
want to try out the [Julia][] programming language on Hive. If you try to run
`julia`, you'll see an error message saying it's not installed:

[Julia]: https://julialang.org/

```sh
julia
```

```
julia: command not found
```

You can load the `julia` module to switch to an environment where Julia is
installed:

```sh
module load julia
```

Now running `julia` will open the Julia interpreter (you can exit Julia by
typing `exit()` and pressing enter).

You can check which modules are loaded at any given time with the `module list`
command:

```sh
module list
```

```
Currently Loaded Modulefiles:
 1) slurm/23.02.6   2) openmpi/4.1.5
```

You might see different output if you've loaded or unloaded modules.

Finally, to unload a module, you can use the `module unload` command with the
module's name:

```sh
module unload julia
```

Note that when you log out, the cluster will automatically unload any modules
you loaded.


(micromamba)=
### Micromamba

#### Installing Micromamba

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


#### Creating Environments

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


#### Exporting Environments

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


#### Managing Storage

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


Editing Configurations
----------------------

After installing software in your computing environment, you'll likely want to
configure it. Many software, especially command-line software, read settings
from a **configuration file** when they run. By editing configuration files,
you can customize your computing environment to make it do what you want and
make it more comfortable to use.


(bash)=
### Bash

[Bash][] is a **shell**: a programming language and prompt that serves as a
command line interface for computers. Bash is widely used, and is the default
shell for most Linux distributions. Knowing how to configure Bash can make it
much easier to work in a terminal.

:::{important}
This section is about configuring Bash, but some POSIX computers default to
other shells (such as the [Z Shell (Zsh)][zsh] or [Fish][fish]). These shells
can also be configured, but the files and commands needed to do so may be
different; see their documentation for details.

You can check the default shell on a POSIX computer with this command:

```
echo $SHELL
```

On macOS, the default shell for versions 10.3 through 10.14 is Bash. Beginning
with version 10.15, the default shell is Zsh.
:::

[Bash]: https://en.wikipedia.org/wiki/Bash_(Unix_shell)
[zsh]: https://en.wikipedia.org/wiki/Z_shell
[fish]: https://en.wikipedia.org/wiki/Fish_(Unix_shell)

Three files in your home directory are especially important for configuring
Bash. Each one is a script, which means they contain commands for Bash (`bash`)
or its predecessor, the [Bourne shell][sh] (`sh`). The files are:

[sh]: https://en.wikipedia.org/wiki/Bourne_shell

* `~/.bash_profile` runs every time you log in (to a server, for example).
* `~/.profile` also runs every time you log in, but only if `.bash_profile`
  doesn't exist. It can only contain `sh` commands, not `bash` commands.
* `~/.bashrc` runs every time you open a shell when you're *already logged in*.

When these files don't exist or are empty, Bash uses default settings. If they
alredy exist on your computer and contain commands, they may have been set up
by your operating system or by your computer's administrator. Leave them intact
until you have enough experience to know what the commands do---they might be
important. You can still add your own settings to the files.

:::{note}
You can learn more about how Bash reads configuration files at startup in the
[Bash Startup Files][bash-startup] section of the Bash manual.
:::

[bash-startup]: https://www.gnu.org/software/bash/manual/bash.html#Bash-Startup-Files

:::{note}
Configuration files are also called **dotfiles**, because they usually have
names that begin with a dot.
:::

We recommend setting up the Bash configuration files so that `.bash_profile`
always runs `.profile` and `.bashrc`. Then use `.profile` to set environment
variables ({numref}`environment-variables`) and use `.bashrc` for everything
else. This approach ensures that your environment variables are loaded whenever
you use any `sh`-compatible shell and that your Bash configuration is loaded
whenever you use Bash. To set this up, use a text editor (such as `vi`) to add
these lines to your `.bash_profile`:

```bash
#
# .bash_profile
#
# This file is runs on login to a Bash shell.

[[ -r ~/.profile ]] && source ~/.profile
[[ -r ~/.bashrc  ]] && source ~/.bashrc
```

These commands check that `.profile` and `.bashrc` exist, and run them if they
do. Leave the rest of your `.bash_profile` blank.

(aliases)=
#### Aliases

While learning about Micromamba, did you ever feel like typing out `micromamba`
for every command was a little bit tedious? One way you can customize the shell 
is by creating **aliases** for commands that are long or hard to remember.
Let's create an alias `mm` for the `micromamba` command, so that you can just
type `mm` instead of typing `micromamba`.

The command to create an alias is `alias`, followed by a space, the name of the
alias, an equals sign, and then the aliased command in quotes.

For example, try adding this code to your `.bashrc`:

```bash
alias mm='micromamba'
```

:::{important}
Be careful to match the spacing and quotes exactly. In Bash, spacing often
matters and single quotes have a distinct meaning from double quotes.
:::

After adding the `alias` command to `.bashrc`, save and close the file. You can
make Bash "reload" the settings in `.bashrc` with this command:

```sh
source ~/.bashrc
```

Alternatively, you can restart or open a new terminal, since `.bashrc` runs
every time you open a new interactive shell. Then try using the alias to
list your conda environments:

```sh
mm env list
```

This should print a list of environments. Notice that you can add arguments as
you would normally after the aliased command.

:::{note}
If the alias doesn't work, or the shell prints `mm: command not found`,
double-check that your saved the `alias` command in `~/.bashrc` and that you
ran the `source` command.
:::

If you ever decide you don't like an alias, you can use the `unalias` command
to unset aliases in your current shell (for example, `unalias mm`). Make sure
to remove the alias from `.bashrc` as well, or else it will return next time
you open a new shell.

:::{tip}
For many shell commands, you can set an argument several times and only the
last setting applies. As a result, you can use aliases to set your preferred
defaults for a command, and then override them as needed.

For example, to make the `ls` command color-code file names and display file
sizes in human-readable units by default, add this alias to your `.bashrc`:

```bash
alias ls='ls --color=auto --human-readable'
```
:::

You can find many examples of helpful aliases online; try searching for `bash
aliases` or `bash dotfiles`.


(environment-variables)=
#### Environment Variables

You can assign variables in your shell just like in most other programming
languages. Typically, some variables are assigned automatically when the shell
starts.

For example, the `PATH` variable contains a colon-separated list of paths where
the shell should search for commands (software programs). Without `PATH`, you'd
have to type the full path to every command.

:::{tip}
You can use the `which` command to check where the shell found a command. For
instance, to check where the `git` command is:

```sh
which git
```
:::

In the shell, the `echo` command prints things, and you can get the value of a
variable by putting a dollar sign `$` in front of its name. Put these two ideas
together to print the value of `PATH`:

```sh
echo $PATH
```
```
/home/nick/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/bin
```

You should see a colon-separated list of directories, like the one above. Some
of the directories listed will probably be different on your computer.


You can assign a variable by typing a name, an equals sign (`=`), and then a
value. The equals sign *must not* have spaces around it, and if the value
contains spaces, it must be quoted. By convention, names of shell variables
should be all uppercase. Here's how to assign a variable `FOO` with the value
`Hello world!`:

```sh
FOO='Hello world!'
```

Try printing `FOO` with `echo`.

:::{note}
You can delete a variable with the `unset` command. For instance, to delete
`FOO`, run:

```sh
unset FOO
```

If you try this, make sure to reassign `FOO` again, as we'll use it in a
subsequent example.
:::


An **environment variable** is a shell variable that's shared with other
programs (including other shells). Environment variables are typically used to
configure the computing environment. As it turns out, `PATH` is an environment
variable.

You can get a list of all environment variables with the `env` command. Here's
an example of a few lines of output from the command:

```sh
env
```

```
SHELL=/usr/bin/bash
SESSION_MANAGER=local/zen:@/tmp/.ICE-unix/579,unix/zen:/tmp/.ICE-unix/579
R_PROFILE_USER=/home/nick/.config/R/rprofile
LESSHISTFILE=/home/nick/.config/less/history
COLORTERM=truecolor
```

On your computer, you'll probably see some environment variables with different
names and values.

Shell variables are not automatically environment variables. Notice that the
variable `FOO`, created above, is not in the output from `env`. To assign an
environment variable, you can use the `export` command. To make `FOO` an
environment variable, run:

```sh
export FOO
```

Now `FOO` is an environment variable and will appear in the output of `env`.

Variables you assign interactively will be deleted when you close the shell. If
you want to automatically assign or modify a variable every time you open a
shell, put the assignment in your `~/.profile` file.

As an example, let's make an executable script that prints `Hello, world!` and
add it to the `PATH`. To start, create and navigate to a directory called
`~/scripts`:

```sh
mkdir ~/scripts
cd ~/scripts
```

Open `hello.sh` in a text editor (for example, `nano hello.sh`), enter the
following, and save the file:

```sh
#!/bin/sh
echo Hello world!
```

Then make the script executable:

```sh
chmod +x hello.sh
```

Try running the script to make sure it works:

```
./hello.sh
```

If you want to run the script from any other directory, you'll have to tell the
shell where to find the script by typing the full path `~/scripts/hello.sh`.

For instance, navigate back to your home directory and try running `hello.sh`
without the full path:

```sh
cd ~
hello.sh
```

```
-bash: hello.sh: command not found
```

The shell is unable to find `hello.sh`. You can tell the shell where to look by
adding `~/scripts` to the `PATH`. The command is:

```sh
export PATH="$HOME/scripts:$PATH"
```

This prepends `~/scripts` (`$HOME/scripts`) and a colon (`:`) to the `PATH`.

:::{caution}
Be careful not to overwrite the `PATH`, as this can break your shell. Instead,
prepend or append directories to the `PATH`.


If you do accidentally overwrite the `PATH`, you can usually reset it by
closing and reopening the shell. On a server, you can do this by logging out
and then logging in again.
:::

Now try running `hello.sh` again:

```sh
hello.sh
```
```
Hello world!
```

As it stands, `~/scripts` will only stay on the `PATH` until the shell is
closed. You can make the change persistent by adding the `export` command above
to `~/.profile`. Open `~/.profile` in a text editor and add this line:

```sh
export PATH="$HOME/scripts:$PATH"
```

Now `~/scripts` will be on the `PATH` even if you close and reopen the shell.

Besides the `PATH`, there are many other environment variables you might want
to customize. For instance, you can assign `EDITOR` to specify your preferred
text editor or assign `R_LIBS_USER` to control where R stores packages. Check
the documentation for your favorite software to learn more about how they use
environment variables.


<!--
### Shell Tips
-->

### Other Configuration Files

{numref}`bash` focuses on Bash configuration files, but software such as git,
tmux, R, Python, and Vim can also be customized through configuration files.
For software you use frequently, learning to edit the configuration file(s) is
often a worthwhile investment because allows you to tailor the software to your
specific needs and workflows.
