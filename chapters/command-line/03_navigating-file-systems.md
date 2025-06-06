# Navigating File Systems

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Describe a file system, directory, and working directory
* Write paths to files or directories
* Get or set the working directory
* Move files and directories
:::

Due to the nature of its stripped down display, the command line interface can
at times feel a bit static. But it offers much of the same functionality that
modern GUIs offer, including the ability to move around your computer. With
GUIs, we tend to navigate with our mouses; on the command line, we'll use our
keyboard. This chapter is intended to give you a working sense of how your
files are structured.


## File Systems

Your computer's **file system** consists of **files** (chunks of data) and
**directories** (or "folders") to organize those files. For instance, the file
system on a computer shared by [Ada][ada] and [Charles][chuck], two pioneers of
computing, might look like this:

[ada]: https://en.wikipedia.org/wiki/Ada_Lovelace
[chuck]: https://en.wikipedia.org/wiki/Charles_Babbage

:::{figure} /images/command-line/filesystem.png
:alt: Directories and files laid out in a hierarchical structure. At the top, the root directory, denoted by a forward slash, contains all other directories. Some of the directories below the root directory contain other directories or files.
:height: 24em
An example of a file system.
:::

Don't worry if your file system looks a bit different from the picture.

File systems have a tree-like structure, with a top-level directory called the
**root directory**. On Ada and Charles' computer, the root is called `/`, which
is also what it's called on all macOS and Linux computers. On Windows, the root
is usually called `C:/`, but sometimes other letters, like `D:/`, are also used
depending on the computer's hardware.

A **path** is a list of directories that leads to a specific file or directory
on a file system (imagine giving directions to someone as they walk through the
file system). Use forward slashes `/` to separate the directories in a path,
rather than commas or spaces. The root directory includes a forward slash as
part of its name, and doesn't need an extra one.

For example, suppose Ada wants to write a path to the file `cats.csv`. She can
write the path like this:

```none
/Users/ada/cats.csv
```

You can read this path from left-to-right as, "Starting from the root
directory, go to the `Users` directory, then from there go to the `ada`
directory, and from there go to the file `cats.csv`." Alternatively, you can
read the path from right-to-left as, "The file `cats.csv` inside of the `ada`
directory, which is inside of the `Users` directory, which is in the root
directory."

As another example, suppose Charles wants a path to the `Programs` directory.
He can write:

```none
/Programs/
```

The `/` at the end of this path is reminder that `Programs` is a directory, not
a file. Charles could also write the path like this:

```none
/Programs
```

This is still correct, but it's not as obvious that `Programs` is a directory.
In other words, when a path leads to a directory, including a **trailing
slash** is optional, but makes the meaning of the path clearer. Paths that lead
to files never have a trailing slash.

:::{warning}
On Windows computers, the components of a path are usually separated with
backslashes ```\``` instead of forward slashes `/`.

Git Bash is an exception: the shell commands you've learned so far expect and
understand paths separated with forward slashes. If you instead use Windows'
built-in terminal, you'll need to use paths separated with backslashes (and a
different set of commands).
:::


(sec-absolute-relative-paths)=
### Absolute & Relative Paths

A path that starts from the root directory, like all of the ones we've seen so
far, is called an **absolute path**. The path is "absolute" because it
unambiguously describes where a file or directory is located. The downside is
that absolute paths usually don't work well if you share your code.

For example, suppose Ada uses the path `/Programs/ada/cats.csv` to load the
`cats.csv` file in her code. If she shares her code with another pioneer of
computing, say [Gladys][gladys], who also has a copy of `cats.csv`, it might
not work. Even though Gladys has the file, she might not have it in a directory
called `ada`, and might not even have a directory called `ada` on her computer.
Because Ada used an absolute path, her code works on her own computer, but
isn't portable to others.

[gladys]: https://en.wikipedia.org/wiki/Gladys_West

On the other hand, a **relative path** is one that doesn't start from the root
directory. The path is "relative" to an unspecified starting point, which
usually depends on the context.

For instance, suppose Ada's code is saved in the file `analysis.ipynb`, which
is in the same directory as `cats.csv` on her computer. Then instead of an
absolute path, she can use a relative path in her code:

```none
cats.csv
```

The context is the location of `analysis.ipynb`, the file that contains the
code. In other words, the starting point on Ada's computer is the `ada`
directory. On other computers, the starting point will be different, depending
on where the code is stored.

Now suppose Ada sends her corrected code in `analysis.ipynb` to Gladys, and
tells Gladys to put it in the same directory as `cats.csv`. Since the path
`cats.csv` is relative, the code will still work on Gladys' computer, as long
as the two files are in the same directory. The name of that directory and its
location in the file system don't matter, and don't have to be the same as on
Ada's computer. Gladys can put the files in a directory
`/Users/gladys/from_ada/` and the path (and code) will still work.

Relative paths can include directories. For example, suppose that Charles wants
to write a relative path from the `Users` directory to a cool selfie he took.
Then he can write:

```none
charles/cool_hair_selfie.jpg
```

You can read this path as, "Starting from wherever you are, go to the `charles`
directory, and from there go to the `cool_hair_selfie.jpg` file." In other
words, the relative path depends on the context of the code or program that
uses it.

:::{tip}
When you use paths in code, they should almost always be relative paths. This
ensures that the code is portable to other computers, which is an important
aspect of reproducibility. Another benefit is that relative paths tend to be
shorter, making your code easier to read (and write).
:::

When you write paths, there are three shortcuts you can use. These are most
useful in relative paths, but also work in absolute paths:

* `.` means the current directory.
* `..` means the directory above the current directory.
* `~` means the **home directory**. Each user has their own home directory,
  whose location depends on the operating system and their username. Home
  directories are typically found inside `C:/Users/` on Windows, `/Users/` on
  macOS, and `/home/` on Linux.

As an example, suppose Ada wants to write a (relative) path from the `ada`
directory to Charles' cool selfie. Using these shortcuts, she can write:

```none
../charles/cool_hair_selfie.jpg
```

Read this as, "Starting from wherever you are, go up one directory, then go to
the `charles` directory, and then go to the `cool_hair_selfie.jpg` file." Since
`/Users/ada` is Ada's home directory, she could also write the path as:

```none
~/../charles/cool_hair_selfie.jpg
```

This path has the same effect, but the meaning is slightly different. You can
read it as "Starting from your home directory, go up one directory, then go to
the `charles` directory, and then go to the `cool_hair_selfie.jpg` file."

The `..` and `~` shortcut are frequently used and worth remembering. The `.`
shortcut is included here in case you see it in someone else's code. Since it
means the current directory, a path like `./cats.csv` is identical to
`cats.csv`, and the latter is preferable for being simpler. There are a few
specific situations where `.` is necessary, but they fall outside the scope of
this text.


## The Working Directory

{ref}`sec-absolute-relative-paths` explained that relative paths have a
starting point that depends on the context where the path is used. The
**working directory** is the starting point the shell uses for relative paths.
Think of the working directory as the directory the shell is currently "at" or
watching.

The shell provides commands to manipulate the working directory. The command
`pwd` returns the absolute path for the current working directory, as a string.
It doesn't require any arguments:

```none
pwd
```

```none
/home/nick/
```

On your computer, the output from `pwd` will likely be different. This is a
very useful function for getting your bearings when you write relative paths.
If you write a relative path and it doesn't work as expected, the first thing
to do is check the working directory.

The related `cd` function changes the working directory. It takes one argument:
a path to the new working directory. Here's an example:

```none
cd ..
pwd
```

```none
/home/
```

<!--
:::{warning}
Generally, you should avoid using calls to `os.chdir` in your Jupyter notebooks
and Python scripts. Calling `os.chdir` makes your code more difficult to
understand, and can always be avoided by using appropriate relative paths. If
you call `os.chdir` with an absolute path, it also makes your code less
portable to other computers. It's fine to use `os.chdir` interactively (in the
Python console), but avoid making your saved code dependent on it.
:::
-->
 
Another command that's useful for dealing with the working directory and file
system is `ls`. The `ls` command returns the names of all of the files and
directories inside of a directory. It accepts a path to a directory as an
argument, or assumes the working directory if you don't pass a path. For
instance:

```none
ls /
```

```none
bin   dev  etc   lib    lost+found  opt   root  sbin  swapfile  tmp  var
boot  efi  home  lib64  mnt         proc  run   srv   sys       usr  windows
```

```none
ls
```

```none
archive  depot  go  haven  mill  notes.md  wharf  woods  Zotero
```

As usual, since you have a different computer, you're likely to see different
output if you run this code. If you run `ls` with an invalid path, the shell
emits an error:

```none
ls /this/path/is/fake/
```

```none
"/this/path/is/fake/": No such file or directory (os error 2)
```


## Moving Data Around

Beyond helping you navigate, file paths also enable you to move information
around on your computer. Doing so works very much like the above.

To move a file, we use `mv`, which has the following syntax:

```none
mv [location/of/file] [location/where/you/want/to/move/the/file]
```

If we're in a directory that looks like so:

```none
ls
```

```none
file.txt  subdirectory
```

...and we want to move `file.txt` into `subdirectory`, we can use:

```none
mv ./file.txt ./subdirectory/file.txt
```

Note here that we've used `./` to specify that `file.txt` is in the *current*
directory. Note too that we wrote out the *full filename* of `file.txt` after
`subdirectory`. If you didn't write out the full filename after `subdirectory`
or use `./`, your CLI is usually smart enough to infer what you meant and would
move `file.txt` to `subdirectory`:

```none
mv file.txt subdirectory
cd subdirectory
ls
```
```none
file.txt
```

However, it's better to be as specific as possible so as to ensure you've moved
your file exactly where you want it. Many a file has gotten lost on computers
because of inexact commands.
