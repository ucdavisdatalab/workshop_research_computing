# Going Beyond Browsing

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Describe the syntax of shell commands
* List several ways to get help with shell commands
* In the CLI:
    - Download a file
    - Extract the contents of a ZIP archive
    - Examine the contents of a file
* Explain what a file extension is
:::

The CLI is more than a mere a file browser. With the right commands, you can
download files from the internet, edit text, install new software, run
sophisticated computations, connect to remote computers, and more. This chapter
introduces a few more commands that are especially useful. It also explains the
syntax shared by most commands, and describes how to get help as you learn and
use the CLI.


## Command Syntax

Most, if not all, shell commands follow a standard syntax. Commands always
begin with the name of a particular command. So, using the `ls` command as an
example, you can write:

```none
ls
```

Some commands require or accept additional information. For instance, you can
give the `ls` command the path to a particular directory, such as `~`:

```none
ls ~
```

We call these inputs to a command **arguments**. The shell treats spaces as
delimiters between a command's name and each of its arguments.

:::{caution}
File and directory names with spaces can cause problems, because the shell will
split the name at each space. Whatever command you're trying to run will then
treat each piece as a separate and unrelated argument. This typically causes
the command to do something different from what you intended or to print an
error message.

If you need to work with a name that has spaces, there are a few ways to work
around this limitation:
* Put single (`'`) or double (`"`) quotation marks around the name. For
  example:
  ```none
  ls "My Directory"
  ```

* **Escape** each space by putting a backslash (`\`) in front of it. For
  example:
  ```
  ls My\ Directory
  ```

These workarounds can be a bit of a pain to type over and over. For this
reason, people who use the command line in their daily work tend to avoid
spaces altogether when naming files and directories. At DataLab, we use
underscores (`_`) and dashes (`-`) rather than spaces in names.

If you want to learn more about naming conventions, see [this
section][dl-readme] of DataLab's README, Write Me! workshop reader.

[dl-readme]: https://ucdavisdatalab.github.io/workshop_how-to-data-documentation/#file-names
:::

Many commands also accept special arguments called **flags** that modify
something about what the command does. Flags begin with a dash (`-`), usually
followed by a single letter. For example, the `ls` command's `-a` flag makes
the command print out all files in a directory, even files that are hidden:

```
ls -a
```

Some flags can also be written in long form, as a word, to make it clearer how
they modify the command. In long form, flags conventionally begin with two
dashes (`--`). For the `ls` command, the long form of `-a` is `--all`. So
another way to print all files in a directory is:

```none
ls --all
```

Without the `-a` or `--all` flag, the `ls` command does not print any files or
directories whose names begin with a dot (`.`). Because of this, we call these
files **hidden files** or **dotfiles**.

Hidden files typically contain settings or metadata for your computer's
operating system, for the shell, or for other programs. Sometimes you might
want to edit a hidden file to configure your computer or even create a new
hidden file for your own use.

Does your home directory contain any hidden files? First print the visible
files:

```none
ls ~
```

Then try prining all of the files, for comparison:

```none
ls -a ~
```

<!--
### Example: More About Files

As another example, if you want to *list* the files and folders in the current
directory, you can run the `ls` command:

```none
ls
```

```none
backups  data.csv  jupyter_notebook.ipynb  project_folder
```

You can also use `ls` on a subfolder to show its contents:

```none
ls project_folder
```
```none
file_1.txt  file_2.txt
```

Want some more information about your files and folders? You can modify the
base functionality of the `ls` command -- and in fact you can do this for many
such commands -- with **flags**. We do this by adding a dash (`-`) and then a
letter, or a combination of letters, directly after the dash.

```none
ls -lha
```

...will print out information about the permissions, size, and date of the
current directory's contents, as well as the user(s) who created them:

```none
ls -lha
```
```none
.
..
.config_file
drwxrwxr-x 2 tshoe staff 4.0K Jun 21 23:30 backups
-rw-rw-r-- 1 tshoe staff  28K Oct  4 20:42 data.csv
-rw-rw-r-- 1 tshoe staff 6.2K Aug 27 22:44 jupyter_notebook.ipynb
drwxrwxr-x 8 root  root  4.0K Oct  4 21:44 project_folder
```

It's worth walking through this output, because it demonstrates how we can
bring important information into view with just a few keystrokes. For
beginners, the most relevant portions of `ls -lha` are likely to be toward the
end of the output, where the file sizes are listed along with the date each
file was last modified. To the left of this information are listings for the
file owner/file group (respectively, `tshoe`/`staff` and `root`/`root`). These
listings help us make some sense of the most confusing part of the above
output: those strings of `r`'s, `w`'s, and `x`'s. These represent the various
**permissions** for each file. Permissions are specific file attributes that
control which users can read (`r`), change (`w`), or execute (`x`) a file.
Depending on what level of permissions you, or the user group to which you
belong, have been assigned, you may or may not be able to work with certain
files (if you're on your personal computer, you likely have full permissions).

Besides outputting file information, `ls -lha` also shows us three things in
our folder that were otherwise hidden with just `ls`: `.`, `..`, and
`.config_file`. The first two are special notation for navigating your
computer, which we'll discuss in a later section. The last, `.config_file`, is
a **dotfile**. Dotfiles (named for the `.` that prefaces them) are hidden by
default on your computer. They often contain various configuration settings
that people use to customize their computers. Some will also log the history of
commands you've sent via a CLI (for example, `.bash_history` or
`.python_history`). We can't discuss these files in depth, but for the moment
it's worth recognizing that they exist and that you'll occasionally encounter
them when working on the command lie.

Depending on your particular system and/or CLI, all of the above might look
slightly different on your computer. However, the basic presentation and
functionality as described above will be the same. Typing is the predominant
way of using a CLI, and the results of sending in commands with a CLI are
text-only representations of your computer and your files.
-->


## Getting Help

Many commands have a `--help` flag that makes them print out some basic
documentation. For example, to get help with the `pwd` command:

```none
pwd --help
```

```none
pwd: pwd [-LP]
    Print the name of the current working directory.

    Options:
      -L	print the value of $PWD if it names the current working
    		directory
      -P	print the physical directory, without any symbolic links

    By default, `pwd' behaves as if `-L' were specified.

    Exit Status:
    Returns 0 unless an invalid option is given or the current directory
    cannot be read.
```

Most commands also provide detailed documentation in the form of a manual page.
On macOS and Linux, you can use the `man` command to open the manual page for
another command. For instance:

```none
man ls
```

```none
LS(1)                                               User Commands                                               LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about the FILEs (the current directory by default).  Sort entries alphabetically if none of
       -cftuvSUX nor --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
              with -l, print the author of each file

       -b, --escape
              print C-style escapes for nongraphic characters

       --block-size=SIZE
              with -l, scale sizes by SIZE when printing them; e.g., '--block-size=M'; see SIZE format below

       -B, --ignore-backups
              do not list implied entries ending with ~

       -c     with -lt: sort by, and show, ctime (time of last modification of file  status  information);  with  -l:
              show ctime and sort by name; otherwise: sort by ctime, newest first

       -C     list entries by columns

       --color[=WHEN]
              colorize the output; WHEN can be 'always' (default if omitted), 'auto', or 'never'; more info below

       -d, --directory
              list directories themselves, not their contents
[...]
```

As opposed to the usually mute, minimalist disposition of a CLI, here you can
see thorough documentation for a given command and its various flags.

:::{note}
On Windows, Git Bash does not include the `man` command. Instead, you can look
up manual pages for commands by searching online. Try searching for `man` and
the name of the command.
:::

Sometimes it can also be helpful to know what version of a command you're
using, as commands themselves can be updated. You can find this information for
some commands with `--version` or `-v`:

```none
file -v
```

```none
file-5.46
magic file from /usr/share/file/misc/magic
seccomp support included
```

Still unclear about what a command does? Look it up with your favorite search
engine, or visit Stack Exchange and search there. CLIs are widely used
software, and chances are incredibly good that someone else has had the same
question you want to ask.


## When Problems Arise

Error messages offer another, admittedly less pleasant way to learn about how a
command works. When you're first starting out with any kind of console-based
software (whether it be a CLI or writing code), one of the most important
skills you can learn is how to read an error message. For the most part, such
messages are quite clear; they're intended to help you debug your problem and
thus attempt to supply you with information about what might be going wrong.

As an example, if you're at your home directory, and it looks like the
following:

```none
ls
```

```none
backups  data.csv  jupyter_notebook.ipynb  project_folder
```

And you decide to `ls` a folder within this directory that doesn't exist:

```none
ls no_name
```

You'll see this error message:

```none
ls: cannot access 'no_name': No such file or directory
```

This tells you that, while you've sent in a valid `ls` command, it can't find
what you're looking for.

Likewise, forgetting a space:

```none
echo"hello"
```

Leads to:

```none
-bash: echohello: command not found
```

In other words, you've sent in a command that's either invalid or is
unavailable. These error messages are both fairly clear, but if you're ever
confused, or if you simply want to find out more about an error, a search
engine is once again your friend. Sometimes simply copy/pasting the error
message and searching on that alone will lead directly to information about
what went wrong.

That all said, sometimes you need to stop a CLI process immediately. Did you,
for example, do something that causes your computer to print a million lines on
screen? Did you decide you don't want a file copied to a new location, and it's
still in the midst of transferring? You can **interrupt** any command with
`Ctrl` + `c` (on macOS: `Cmd` + `c`). This will stop whatever current process
is running in your interface.

While you can stop a command, for the most part *it isn't possible to undo a
command*. Please take care to know exactly what you're running and what you're
running it on, especially when it comes to modifying or deleting things on your
computer.


(sec-example-downloading-inspecting-files)=
## Example: Downloading & Inspecting Files

To practice using the CLI and learn some new commands, let's download a file
from the Internet.

The file we'll download is called `example-files.zip`. It's an **archive**,
meaning it contains several other files. Specifically, it's a **ZIP archive**
(`.zip`), a format that uses compression to minimize the file size. This makes
ZIP files great for exchanging data online, although other compressed archive
formats (such as `.tar.gz`) are also popular. The file contains several of the
files from the Ada and Charles example ({numref}`sec-file-systems`).

To prepare, change the working directory to the `2026_intro-cmd/` directory you
created in {numref}`sec-making-removing-directories`:

```none
cd ~/2026_intro-cmd
```

Rather than downloading the file with a web browser, we'll download it with the
CLI. To do this, you can use the `curl` command together with the URL for the
file. The file is at this URL:

```none
https://ucdavis.box.com/shared/static/8nybft8ysh90vuqureczugueruhfm1lp.zip
```

And this is the command to download the file:

```none
curl -L -o example-files.zip https://ucdavis.box.com/shared/static/8nybft8ysh90vuqureczugueruhfm1lp.zip
```

```none
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0   0     0   0     0     0     0  --:--:-- --:--:-- --:--:--     0
  0     0   0     0   0     0     0     0  --:--:-- --:--:-- --:--:--     0
100     5   0     5   0     0     4     0  --:--:--  0:00:01 --:--:--     0
100 71957 100 71957   0     0 49205     0   0:00:01  0:00:01 --:--:-- 49205
```

The `-L` flag tells `curl` to follow redirects and is necessary because Box,
the website where the file is hosted, does not provide direct links to files.
The `-o` flag tells `curl` where to save the downloaded file; in this case, we
save it to `example-files.zip`.

Confirm that the file is in the working directory with the `ls` command:

```none
ls
```

```none
data  example-files.zip
```

In order to use the contents of the file, we have to decompress it. You can
decompress ("unzip") a ZIP archive with the `unzip` command:

```none
unzip example-files.zip
```

```none
Archive:  example-files.zip
   creating: example-files/
  inflating: example-files/README.md
   creating: example-files/charles/
  inflating: example-files/charles/cool_hair_selfie.jpg
   creating: example-files/ada/
  inflating: example-files/ada/analysis.ipynb
  inflating: example-files/ada/cats.csv
```

Unzipping the file creates a new directory called `example-files` in the
working directory. Change to the `example-files` directory and list the files:

```none
cd example-files
ls
```

```none
ada  charles  README.md
```

Archives and projects often include a file named `README`, or some variation of
this, intended as documentation to be read before doing anything else. You can
print out the first 10 lines of a file with the `head` command. Take a look at
the `README.md` file:

```none
head README.md
```

```none
# README

This archive contains some of the files from the Ada and Charles example in
DataLab's [Introduction to the Command Line][cmd] workshop reader.

[cmd]: https://ucdavisdatalab.github.io/workshop_research_computing/chapters/command-line/
```

From the output, we can see that this file contains text. In this case, the
file is written in [Markdown][], a formatting language designed to be easy to
read but convenient for adding formatting like italics and hyperlinks to text.

[Markdown]: https://commonmark.org/help/

:::{note}
The `cat` ("concatenate") command is similar to `head`, but prints out an
entire file rather than just the first 10 lines. Use the `cat` command with
care, since some files are very large (and thus take a long time to print).

If you want to view the contents of a large file, it's better to use a text
editor, which we introduce in the next chapter.
:::

The README file is named `README.md`. The part after the dot, `md`, is called a
**file extension**. Extensions are a convention for describing the formats of
files and are usually, but not always, 1-3 characters long. In this case, `.md`
is the standard extension for a Markdown file. A few other common extensions
include `.txt` for text files, `.zip` for ZIP archives, and `.png` for PNG
image files.

:::{tip}
Some operating systems hide extensions in their GUI file browsers by default.
We recommend configuring your GUI file browser to always display extensions. If
you're not sure how, try searching for instructions online.
:::

Sometimes the extension for a file will be missing or incorrect. If, while
working with the CLI, you need to get a sense of what kind of data is stored in
a file, you should use the `file` command. Let's take a look at the output from
the command for a few files:

```none
file README.md
```

```none
README.md: ASCII text
```

The `file` command recognizes that the `README.md` file contains text. The ZIP
archive doesn't:

```none
file ../example-files.zip
```

```none
../example-files.zip: Zip archive data, made by v4.6 UNIX, extract using at least v2.0, last modified, last modified Sun, Jan 07 2026 03:59:54, uncompressed size 0, method=store
```

For this file, the command indicates that it's a ZIP archive and prints out
some metadata about how and when it was created.

Use the `file` command and the commands for browsing files from
{numref}`sec-browsing-files` to inspect the files in the `example-files`
directory and its subdirectories.


## Reference: Common Commands

The commands introduced in this chapter, as well as a few other common
commands, are listed in the following table:

:::{list-table}
:header-rows: 1

* - Command
  - Description
  - Examples
* - `man`
  - Opens the manual page for another command.
  - `man ls`
* - `curl`
  - Downloads a file from the internet.
  - `curl https://datalab.ucdavis.edu/`
* - `tar`
  - Compresses/decompresses files into/from a `.tar` or `.tar.gz` file.
  - `tar -xf my_file.tar`
* - `zip`
  - Compresses files into a `.zip` file.
  - `zip my_files.zip my_files/`
* - `unzip`
  - Decompresses ("unzips") files from a `.zip` file.
  - `unzip my_file.zip`
* - `head`
  - Prints the first 10 lines of a file to the screen.
  - `head README.md`
* - `cat`
  - Prints the entirety of a file to the screen.
  - `cat README.md`
* - `file`
  - Prints the type of a file.
  - `file my_dataset.csv`
:::

<!--
* - `mv`
  - Moves files.
  -
* - `cp`
  - Copies files.
  -
* - `rm`
  - Deletes files.
-->
<!--
A more complete list of Unix Commands may be found on Valerie Henderson
Summet's [Unix Cheat Sheet][]. That said, 

[Unix Cheat Sheet]: cheatsheet.pdf
-->

There are dozens of shell commands, each with their own set of flags, and it's
possible to install even more. We can't go over everything in one workshop, but
by knowing the shell command syntax and how to get help, you'll be able to
figure out how to use new commands.
