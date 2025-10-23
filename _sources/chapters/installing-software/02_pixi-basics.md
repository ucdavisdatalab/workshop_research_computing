# Pixi Basics

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Create and organize project directories for projects
* Initialize projects with Pixi
* Install software with Pixi
:::


## Project Directories

Pixi manages virtual environments on a per-project basis. Each project must
have its own directory---called a **project directory**---where all of the
files related to the project are stored. We recommend organizing projects this
way even when you aren't using Pixi. By using a project directory to centralize
all of the files in a project, it's easier to:

* Find files, because you know where to look or to run search software
* Move or copy the project to other computers
* Share the project with collaborators, colleagues, or the public
* Create backup copies of the project to protect your work
* Access and run files with R, Python, and other tools
* Use version control software to manage different versions of project files

:::{tip}
Create a new project directory for every project, no matter how small. As you
produce or acquire new files for the project, make sure that they're stored in
the directory. Some examples of files you should store in a project directory
are:

* Documentation, such as a file manifest and instructions for use
* Code, such as notebooks and scripts
* Inputs, such as data sets and configuration files
* Outputs, such as reports, figures, and intermediate data sets
* License information (if the project will be shared with anyone)

Give the files descriptive names and create subdirectories to keep the files
well-organized. The gold standard is for a project directory to be completely
**portable**, meaning you can copy the directory to another computer, follow
included instructions to setup necessary software (such as R or Python), and
then run the code *without any modifications* to get the expected result.
:::

## Initializing a Project

You can initialize a project to use Pixi with the `pixi init` command. The
command takes one argument: a path to the project directory. If the directory
doesn't exist yet, Pixi will create it.

Open a terminal and make a directory where you can try out some commands:

```sh
mkdir pixi_workshop
cd pixi_workshop
```

Initialize a Pixi project called `my_project`:

```sh
pixi init my_project
```

```text
✔ Created /home/nick/pixi_workshop/my_project/pixi.toml
```

Navigate to the new `my_project/` directory and take a look at the files
inside:

```sh
cd my_project
ls -a
```

```text
.  ..  .gitattributes  .gitignore  pixi.toml
```

The `.gitattributes` and `.gitignore` files are files for [Git][], a version
control system. If you're not familiar with Git, it's safe to ignore these
files, and you can skip the rest of this paragraph. The `.gitattributes` file
tells Git how to handle merges for `pixi.lock`, a file Pixi uses to keep track
of installed packages. The `.gitignore` file tells Git to ignore the `.pixi/`
subdirectory, which is where Pixi installs packages.

[Git]: https://git-scm.com/

:::{seealso}
We recommend using Git for your research computing projects. To learn more,
check out the [Introduction to Version Control](ch-version-control-systems)
chapters.
:::

The `pixi.toml` file identifies the project as a Pixi project. It's also a
place where you can store metadata about the project (such as its name,
authors, and version) and where Pixi will store details about the project's
virtual environments.

Open `pixi.toml` in a text editor (such as `vim`). It will look something like
this:

```toml
[project]
authors = ["YOUR_NAME <YOUR_EMAIL>"]
channels = ["conda-forge"]
name = "my_project"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]

[dependencies]
```

The file is in [TOML][] format, a format designed to be easy for both people
and computers to read and write. TOML files consist primarily of key-value
pairs of the form `key = value`. The key-value pairs can be organized into
named tables with headers of the form `[name]`.

[TOML]: https://toml.io/en/

A `pixi.toml` file always has at least three tables:

* `project` is metadata about the project.
* `tasks` is a list of project-specific commands.
* `dependencies` is a list of required packages for the project's default
  virtual environment, which starts out empty (no packages).

When you initialize a project, Pixi fills in as much of `pixi.toml` as it can.
For instance, it gets your name and email from Git, if you have Git installed
and configured. You can edit `pixi.toml` to add or correct details.
{numref}`editing-pixi.toml` explains more about this file, but first, let's
install some packages.


## Adding & Removing Packages

You can list a project's packages with `pixi list`. Go ahead and try this for
the `my_project` project:

```sh
pixi list
```

```text
✘ No packages found in 'default' environment for 'linux-64' platform.
```

We haven't installed any packages yet, so there are none to list.

Suppose we want to install Python. Package names are always lowercase and
usually not surprising, although you can use the `pixi search` command or
search online if you're not sure about a package's name. Python is in the
`python` package.

You can install a package with the `pixi add` command. So the command to
install Python is:

```sh
pixi add python
```

```text
✔ Added python >=3.13.2,<3.14
```

By default, Pixi installs the most recent compatible version of a package. So
depending on your computer's operating system and when you run the command,
Pixi might install a different version of Python.

:::{tip}
You can use `=`, `<`, `<=`, `>`, and `>=` with `pixi add` to set constraints on
package versions. For instance, if you want the most recent version of Python
less than 3.10:

```sh
pixi add 'python<3.10'
```

When you set constraints like this, make sure to surround them with single
quotes (`' '`) or the shell will misunderstand the command.
:::

:::{caution}
The `pixi add` command installs packages. Don't confuse it with the `pixi
install` command, which installs entire virtual environments. You usually won't
need to run `pixi install`, because Pixi runs it automatically as needed.
:::


The `python` package is now listed in the environment, along with all of its
dependencies:

```sh
pixi list
```

```text
Package           Version    Build               Size       Kind   Source
_libgcc_mutex     0.1        conda_forge         2.5 KiB    conda  _libgcc_mutex
_openmp_mutex     4.5        2_gnu               23.1 KiB   conda  _openmp_mutex
bzip2             1.0.8      h4bc722e_7          246.9 KiB  conda  bzip2
ca-certificates   2025.1.31  hbcca054_0          154.4 KiB  conda  ca-certificates
ld_impl_linux-64  2.43       h712a8e2_4          655.5 KiB  conda  ld_impl_linux-64
libexpat          2.6.4      h5888daf_0          71.6 KiB   conda  libexpat
libffi            3.4.6      h2dba641_0          52.2 KiB   conda  libffi
libgcc            14.2.0     h767d61c_2          828 KiB    conda  libgcc
libgcc-ng         14.2.0     h69a702a_2          52.5 KiB   conda  libgcc-ng
libgomp           14.2.0     h767d61c_2          449.1 KiB  conda  libgomp
liblzma           5.6.4      hb9d3cd8_0          108.7 KiB  conda  liblzma
libmpdec          4.0.0      h4bc722e_0          87.9 KiB   conda  libmpdec
libsqlite         3.49.1     hee588c1_2          897.1 KiB  conda  libsqlite
libuuid           2.38.1     h0b41bf4_0          32.8 KiB   conda  libuuid
libzlib           1.3.1      hb9d3cd8_2          59.5 KiB   conda  libzlib
ncurses           6.5        h2d0b736_3          870.7 KiB  conda  ncurses
openssl           3.4.1      h7b32b05_0          2.8 MiB    conda  openssl
python            3.13.2     hf636f53_101_cp313  31.7 MiB   conda  python
python_abi        3.13       5_cp313             6.1 KiB    conda  python_abi
readline          8.2        h8c095d6_2          275.9 KiB  conda  readline
tk                8.6.13     noxft_h4845f30_101  3.2 MiB    conda  tk
tzdata            2025a      h78e105d_0          120 KiB    conda  tzdata
```

Packages you installed explicitly are printed in bold (although the bold
doesn't show up in this reader). All of the other packages are dependencies.
You can use `pixi add` multiple times to install whatever packages you need.

Once you've installed some packages in a virtual environment, you'll probably
want to use them. You can run a command in the virtual environment with the
`pixi run` command. So to run `python`:

```sh
pixi run python
```

This will open a Python prompt in the virtual environment. You can use Python
as you would normally at this point.

:::{note}
On most operating systems, you can use `which` to check which program a command
will run. For instance, try:

```sh
pixi run which python
```

Compare the output to:

```sh
which python
```
The output should be different. The `which` command is a Unix shell tool, not
part of Pixi.
:::

If you no longer need a package for your project, you can uninstall it with
`pixi remove`. Let's remove Python from our project:

```sh
pixi remove python
```

```text
✔ Removed python
```

The environment is once again empty:

```sh
pixi list
```
```text
✘ No packages found in 'default' environment for 'linux-64' platform.
```

With Python uninstalled, let's install another major language of data science:
R. The package for R is called `r`. We'll also install R's popular ggplot2
package for data visualization. In conda-forge, R packages have the prefix
`r-`, so ggplot2 is `r-ggplot2`. We can install both packages in a single `pixi
add` command:

```sh
pixi add r r-ggplot2
```

When you need to run multiple commands in a virtual environment, typing `pixi
run` in front of each one is inconvenient and tedious. Instead, you can use
`pixi shell` to launch a subshell in the virtual environment. Any commands you
enter in the subshell run in the virtual environment.

:::{caution}
If you're using Windows and Git Bash, the `pixi shell` command is [not yet
supported][pixi-shell-win].

[pixi-shell-win]: https://github.com/prefix-dev/pixi/issues/417
:::

Open a subshell:

```sh
pixi shell
```

Now try running `R`. You can use R normally, exit, and reopen R again without
leaving the virtual environment. When you're finished with the subshell, enter
`exit` to return to your original shell.


(editing-pixi.toml)=
## Editing `pixi.toml`

When you install a package with `pixi add`, Pixi adds the package's name and a
version constraint to the `pixi.toml` file. Likewise, when you uninstall a
package with `pixi remove`, Pixi removes the package's name from the file. The
file is a description of the project and the virtual environment(s) it
requires.

Open `pixi.toml` in a text editor again:

```toml
[project]
authors = ["YOUR_NAME <YOUR_EMAIL>"]
channels = ["conda-forge"]
name = "my_project"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]

[dependencies]
r = ">=4.4,<4.5"
r-ggplot2 = ">=3.5.1,<4"
```

The packages we installed in the default virtual environment with `pixi add`
are listed in the `dependencies` table, along with their version constraints.
In the constraints, the lower bound comes from the version of the package that
was actually installed. The upper bound is one minor version higher. We say
these packages are **pinned** because there are specific constraints on their
versions. Pixi makes sure constraints on pinned packages are satisfied unless
you explicitly override them.

In addition to or instead of using `pixi add` and `pixi remove` to manage
packages, you can do so by editing `pixi.toml` directly. For instance, if you
want to remove a package, just remove its line from `pixi.toml`. Pixi will
automatically uninstall the package the next time you run a `pixi` command.
Similarly, you can add packages by adding a line to `pixi.toml`, and they'll be
installed the next time you run a command.

Take a look at the files in the `my_project/` directory again now that we've
added and removed some packages:

```sh
ls -a
```

```
.  ..  .gitattributes  .gitignore  .pixi  pixi.lock  pixi.toml
```

There's a new directory called `.pixi/`. This is where Pixi installs all of a
project's virtual environments and packages.

:::{tip}
If you know you won't work on a project again for a while (or ever), you can
safely delete `.pixi/` to get back some storage space. All of the information
needed to reconstruct the virtual environment(s) is in `pixi.toml` and
`pixi.lock`.
:::

There's also a new file `pixi.lock`, called a **lockfile**. The lockfile lists
the exact version and source of every package, including dependencies,
installed in the project's virtual environments. It's a complete specification,
in contrast to the flexible specification in `pixi.toml`. Both are useful: the
lockfile is for reproducing environments exactly, while the `pixi.toml` file is
for producing compatible environments---as defined by the version
constraints---that might differ slightly from the originals.

:::{important}
If you use Git, make sure to commit both `pixi.toml` and `pixi.lock`.
:::

By default, Pixi will only compute the lockfile for your computer's operating
system. If you plan to use other operating systems---or share your project with
people who do---it's a good idea to have Pixi compute the lockfile for all of
them. You can do this by adding the operating systems to the `platforms` key in
`pixi.toml`. Some valid operating systems are `linux-64`, `macos-64`,
`osx-arm64` (for M1, M2, ...), and `win-64`. Computing the lockfile for all of
the operating systems where your project might be used can help you catch
problems with virtual environments early, such as packages that are not
available for some operating systems. For example, if you want to include all
three operating systems, change the `platforms` key to:

```toml
platforms = ["linux-64", "osx-64", "osx-arm64", "win-64"]
```

Pixi searches for and installs packages from the conda-forge repository by
default. In the jargon of Pixi (and Conda), package repositories are called
**channels**. You can specify other channels Pixi should search for packages in
`pixi.toml`'s `channels` key. The channels have highest to lowest priority from
left to right.

:::{seealso}
There are many other useful things you can do by editing `pixi.toml`, such as
setting up multiple virtual environments or specifying minimum hardware
requirements for a project.

See [the Pixi documentation][pixi] for more details.
:::

[pixi]: https://pixi.sh/


## Updating Packages

You can update packages in an environment with the `pixi update` command. This
command respects the version constraints in `pixi.toml`, so it will not upgrade
any packages to versions that don't satisfy the constraints. If you only want
to update one package, pass the command the name of the package. For example,
to update the R package:

```none
pixi update r
```

In contrast, the `pixi upgrade` command upgrades packages to the newest
available version that's compatible with the other packages in the environment.
This command ignores the constraints in `pixi.toml` and will even change them
to match the new versions.

:::{tip}
It's a good idea to update your Pixi environments occasionally, since anyone
else who installs the environment will get the newest versions of the packages
(within the constraints in `pixi.toml`). Minor versions of packages usually
contain bug and security fixes rather than breaking changes.

On the other hand, be cautious about *upgrading* your Pixi environments.
There's a chance that new versions of packages will break your code---major
versions of packages often introduce breaking changes.
:::



## Global Installs

Occasionally, you might come across a tool that's broadly useful but not
required by any particular project. For example, ripgrep (`rg`) is a tool to
search for files that contain a given text string (or regular expression). It's
a faster, modernized version of the classic `grep` tool, and worth knowing
about if you use the command line frequently, because it can help you find
files quickly. 

You can use the `pixi global install` command to install a package globally, so
that it's available in all shells without any need to use `pixi run` or `pixi
shell`. Try installing `ripgrep` this way:

```sh
pixi global install ripgrep
```

```text
Global environments as specified in '/home/nick/.pixi/manifests/pixi-global.toml'
└── ripgrep: 14.1.1 (installed)
    └─ exposes: rg
```

Pixi creates a new virtual environment for each package you install this way,
in order to prevent conflicts. You can use `pixi global list` to see which
packages you've installed globally:

```sh
pixi global list
```

```text
Global environments as specified in '/home/nick/.pixi/manifests/pixi-global.toml'
└── ripgrep: 14.1.1
    └─ exposes: rg
```

If you want to uninstall a globally-installed package, use `pixi global
uninstall`. For example, to uninstall `ripgrep`:

```sh
pixi global uninstall ripgrep
```

```text
✔ Removed environment ripgrep.
```
