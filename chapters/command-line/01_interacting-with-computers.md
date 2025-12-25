(ch-interacting-with-computers)=
# Interacting with Computers

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Explain what a graphical user interface is
* Explain what a command line interface is
* Describe the components of a command line interface
* Explain what a prompt is
:::


## Computer Interfaces

Most people interact with their computer through a **graphical user interface**
(GUI), which allows them to use a mouse, keyboard, and graphical elements on
screen (such as file menus, pictures of folders and files, etc.) as they do
their work. We tend to conflate our operating systems and their GUIs because
computer hardware and software manufacturers tightly pack these two things
together as a convenience to users. But the operating system that makes your
computer work (Windows, macOS, etc.) and the GUI that you interact with are, in
fact, completely different and separable software packages. It is possible to
use different methods/software to interact with your computer other than the
stock GUI that launches automatically when you turn it on.

:::{figure} /images/command-line/x_window_system.png
The X Window System, a graphical user interface first released in 1984, with
several windows open, including a command line interface. Image from
[Wikipedia][wiki-x].

[wiki-x]: https://en.wikipedia.org/wiki/Shell_(computing)#/media/File:X-Window-System.png
:::

One such method, the **command line interface** (CLI), offers a text-only means
of interacting with your computer. The CLI acts somewhat like a typewriter
rather than a *desktop* with *windows* (as the prevailing metaphors for GUIs
go). You can think of this interface as having three main parts:

1. A **terminal**: a window (or originally, a [teleprinter][]) with which to
   send/receive text to/from a computer.
2. A **shell**: a program that interprets and executes commands you enter into
   the terminal (common shells include Bash, Fish, and Zsh).
3. A **command line** or **prompt**: a line in the terminal where you'll type
   in commands for the shell to interpret and execute.

[teleprinter]: https://en.wikipedia.org/wiki/Teleprinter

In the early days of computing, all user interaction with computers happened at
the command line. But during the 1980s, computer manufacturers---with Apple at
the lead---made a big push to convert their machines to the windowing systems
we know today. A CLI, they felt, was too difficult for users to understand, and
this, in turn, would hamper computer sales. Nowadays there are few viable (and
at present, almost no commercially available) programs that can compete with
the predominant systems of Macs and PCs. While this is less the case for Linux
machines, people are by and large locked into the GUIs that come pre-installed
on their machines. CLIs are, however, available, and some are even installed on
popular computer hardware.


## To the Command Line: A Mentality Shift

The following chapters are a hands-on introduction to the CLI. They explain how
to open the CLI and cover a host of different commands that will help you in
your day-to-day work on a computer. That said, knowing the pragmatics of using
the CLI is only one part of a broader change of mentality in interacting with
computers that we hope to instill.

In order to use the CLI effectively, you'll need to understand more about how
computers work than you would if you only used a GUI. As such, we'll cover
several concepts and conventions in computing that generalize beyond the CLI.
Thus learning this material is in part about preparing yourself to learn more
advanced computing skills in the future, such as tracking changes to files with
version control and writing code.
