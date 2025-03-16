# Editing Configuration Files

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Explain the difference between `.bashrc`, `.profile`, and `.bash_profile`
* Create shell aliases
* Create and modify environment variables
:::

This chapter will show you how to make your computer a comfortable place to
work. You'll learn how to edit configuration files, particularly for the Bash
shell.

After installing software in your computing environment, you'll likely want to
configure it. Many software, especially command-line software, read settings
from a **configuration file** when they run. By editing configuration files,
you can customize your computing environment to make it do what you want and
make it more comfortable to use.


(bash)=
## Bash

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
### Aliases

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
### Environment Variables

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
## Shell Tips
-->

## Other Configuration Files

{numref}`bash` focuses on Bash configuration files, but software such as git,
tmux, R, Python, and Vim can also be customized through configuration files.
For software you use frequently, learning to edit the configuration file(s) is
often a worthwhile investment because allows you to tailor the software to your
specific needs and workflows.
