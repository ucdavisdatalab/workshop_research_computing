# Editing Files

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Explain the difference between binary and plain text files
* Explain what a text editor is
* In the CLI:
    * Edit files with a text editor
    * Copy, move, and delete files
:::

The shell commands we've covered so far give you ways to navigate between
directories and inspect the contents of files. You can also use the CLI to edit
files directly. This chapter explains one way to edit files with the CLI. It
also introduces the final piece of the file browsing experience: how to copy,
move, and delete files.


## Text Editors

A **text editor** is a computer program designed to edit files that contain
**plain text**, free of images and other kinds of data. This reflects the fact
that text editors originated in the CLI, although now text editors for GUIs are
also available.

Text editors display the contents of a file faithfully and efficiently, with
minimal processing. They stand in contrast to word processors (such as
Microsoft Word), where you can freely mix text with graphical formatting,
images, and other data, but what you see on the screen is a heavily processed
version of what's actually stored in the file. This minimalisim makes text
editors useful for editing files that do not normally include formatting, such
as code, and for examining the true contents of files, especially if their
format is intended to be human-readable.

:::{admonition} Plain Text vs. Binary
:class: note

Ultimately, computers represent all data as numbers. They use a **binary**
number system, where numbers are made up of the **bits** 0 and 1 rather than
the digits 0 through 9 in the familiar decimal number system.

To represent text on a computer, we can **encode** each symbol as a specific
number; the number represents the symbol. People have come up with many
different standards for doing this. For instance, in the [ASCII][] standard,
which was designed for languages that use the Latin alphabet, 65 represents the
letter `A`, 66 represents the letter `B`, 97 represents the letter `a`, and so
on. Nowadays, most people use the [UTF-8][] standard, a superset of ASCII
designed to support all languages.

[ASCII]: https://en.wikipedia.org/wiki/ASCII
[UTF-8]: https://en.wikipedia.org/wiki/UTF-8

When we say that files or data are "plain text," we mean that they can be
decoded with some well-known standard into text that's intelligible. We refer
to files and data that cannot be decoded this way as "binary." Note that this
is a reuse of the word in a closely related but distinct way from how we
defined it previously.

Examples of plain text files include text files (`.txt`), Markdown files
(`.md`), source code files for most programming languages (`.R`, `.py`, and
many more), and human-readable data files (`.csv`, `.json`, and more). Plain
text files can be opened in any text editor.

Examples of binary files include image files (`.jpg`, `.png`, and more), audio
and video files (`.mp3`, `.mp4`, and more), data files (`.rds`, `.pickle`,
`.hd5`, and more), and executable programs (`.exe`, `.o`, `.so`, and more). To
open a binary file, you generally need to have special software to decode the
contents into something meaningful.

You can see how all of this works if you try to open a binary file with a text
editor. For example, if you open the `cool_hair_selfie.jpg` image included with
the files from {numref}`sec-example-downloading-inspecting-files` in a text
editor, you'll see something like this:

```none
ÿØÿà^@^PJFIF^@^A^A^A^D°^D°^@^@ÿþ^@^SCreated with GIMPÿÛ^@C^@^C^B^B^C^B^B^C^C^C^C^    D^C^C^D^E^H^E^E^D^D^E
^G^G^F^H^L
^L^L^K
^K^K^M^N^R^P^M^N^Q^N^K^K^P^V^P^Q^S^T^U^U^U^L^O^W^X^V^T^X^R^T^U^TÿÛ^@C^A^C^D^D^E^D    ^E▷^E^E▷ ^T^M^K^M^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T    ^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^T^TÿÂ^@^Q^H^Bw^AÃ^C^A^Q^@^B^Q^A^C^Q^AÿÄ^@^\^@^@^    B^C^A^A^A^A^@^@^@^@^@^@^@^@^@^B^C^A^D^E^F^@^G^HÿÄ^@^X^A^@^C^A^A^@^@^@^@^@^@^@^@^@    ^@^@^@^@^A^B^C^DÿÚ^@^L^C^A^@^B^P^C^P^@^@^Aø¶W^C<94>*<84>1^R<85>(^BcSv<94>é+Ä¾ª%^P    *ß<99>^S/L4j<89>ò^\é<93>0ß¥<90><96>;6ê¥,(<97>'6áµ^H^PÊg^HÓ]<8c>^KPï§KD¬ä<82>F<9a>    ^BÝ¼<87>'êuÃR^DQ<83>yØÑÖ<90>&@$^VÆ[<98>^nÖ­^PNQ-<94><9e>°Ósj<88><86>EÑ0^A²NkÉ<9d>    ¶hV<97>(ò<98>³Ù«2>ê«^ZÅ#Ò<9a><9f><98>Z^A<8a>0³uêe<8c><9c>¯hêS<9b>g<98>Cn<8d>^Y<8f    >ÎHxºAh^B^Nd-<9b>`<97><91>^R<84>^[»^DÇ9$6<98>Êrs½BG<94><82>@Ø!<92>7J^hÁì¬Ü6É<95>k    P¦ä6^N$
```

It's a complete mess! It looks like this because the text editor does its best
to decode the file as text, but since (most of) the file isn't text, most of
the decoded symbols actually represent something else. As a result, it's
generally not a good idea to edit binary files with a text editor.

As a final note, word processors, like Microsoft Word, add an interesting twist
to all of the above. Word documents are actually comprised of a number of
different, associated files under the hood. It's technically possible to alter
the text of such files with a text editor, but finding the right place to make
a change is difficult, and it's more convenient to use a program made to to
interact with them.
:::

Many different text editors exist, including several free and open-source
options. A few popular CLI text editors are:

* nano (and its predecessor Pico), a straightforward text editor with few
  features beyond displaying and editing text.
* Vim (and its predecessor vi), a text editor designed to be customizable and
  extensible. It has many features that make it easier to edit code, such as
  search-and-replace, keyboard shortcuts, and syntax highlighting. Vim is also
  infamous for being difficult to learn because its interface consists of
  several different "modes."
* Emacs, another text editor designed to be customizable and extensible. It has
  many of the same features as Vim, and is also somewhat difficult to learn,
  although its interface is more like a word processor.

For the remainder of this reader, we'll focus on nano (and Pico).


### Nano & Pico

:::{important}
On Windows, nano is included with Git Bash.

On macOS, some versions have nano and some versions have Pico. You can use
either one to follow along with the subsequent sections.

On Linux, nano is included with most distributions. If it's missing, you can
install it with your distribution's package manager.
:::

Let's use nano to create a new text file in the `2026_intro-cmd` directory you
created in {numref}`sec-making-removing-directories`. To get started, change
the working directory to that directory:

```none
cd ~/2026_intro-cmd
```

We'll make a file called `hello.txt`. You can open a file with nano by putting
the name of or path to the file after the `nano` command. This will work even
for files that don't exist yet. Most other CLI text editors also work this way.
Go ahead and open `hello.txt` with nano:

```none
nano hello.txt
```

Once nano opens the file, you can type in or delete text, and use the arrow
keys to move the cursor, as you would in a word processor. For this example,
type in `Hello world!` on the first line of the file.

When you're done editing, you can press `Ctrl` + `o` to save the file. nano
will ask you to confirm that you want to save the file by pressing `Enter` or
`Return`. After saving the file, you can press `Ctrl` + `x` to exit nano.

:::{tip}
nano displays all of its keyboard shortcuts at the bottom of the screen. In the
dislpay, it uses `^` to mean the `Ctrl` key. The keyboard shortcuts in nano are
not case-sensitive.
:::

Back at the CLI, use the `head` or `cat` command to check that the new
`hello.txt` file contains what you wrote:

```none
cat hello.txt
```

```none
Hello world!
```

As another example, let's look at the `README.md` file included in
`example-files.zip` ({numref}`sec-example-downloading-inspecting-files`). To
open the file in nano:

```none
nano example-files/README.md
```

The contents should look familiar from when you printed the file with `head`,
but now you can edit the file if you'd like.

A text editor is an essential tool for anyone working with the CLI. It not only
provides a way to edit text files, but also a way to inspect files whether or
not they contain text. After file browsing commands, your text editor is likely
the CLI command you will use the most.

:::{tip}
If you want a text editor that's full-featured but not much harder to learn
than nano, check out [Micro][].

[Micro]: https://micro-editor.github.io/
:::


## Moving & Deleting Files

{numref}`sec-browsing-files` explained how to use the CLI to browse through
files and directories on your computer, but we left out one important detail:
how to copy, move, and delete things. We'll cover commands for all of those in
this section.

Let's make a copy of the `hello.txt` file you just created. You can use the
`cp` command to make a copy. It requires two arguments: the path to the
original file and the path to the copy (which the command will create). For the
copy of `hello.txt`, let's use the name `another_hello.txt`. So the command to
make the copy is:

```none
cp hello.txt another_hello.txt
```

Check with `ls` that there's now a new file in the directory called
`another_hello.txt`:

```none
ls
```

```none
another_hello.txt  data  example-files  example-files.zip  hello.txt
```

If you open `another_hello.txt` with a text editor or print it with `head` or
`cat`, you should see that it's exactly the same as `hello.txt`.
Congratulations, you've made a copy!

The `mv` command moves a file to a different place. Note that renaming a file
is equivalent to moving the file, so this command is also useful for renaming
files. The syntax of the `mv` command is similar to the `cp` command. It
requires two arguments: the path to the original file and the path to the
destination. Try moving `another_hello.txt` to `a_sincere_hello.txt`:

```none
mv another_hello.txt a_sincere_hello.txt
```

Now the directory looks like this:

```none
ls
```

```none
a_sincere_hello.txt  data  example-files  example-files.zip  hello.txt
```

If you want, you can also inspect `a_sincere_hello.txt` to make sure that its
contents are the same.

Finally, let's clean up by removing the `a_sincere_hello.txt` file. You can
remove a file with the `rm` command. Be careful with this command, as there is
no undo---removing a file is permanent and irreversible. Go ahead and remove
`a_sincere_hello.txt`:

```none
rm a_sincere_hello.txt`
```

Take one last look at the contents of the directory:

```none
ls
```

```none
data  example-files  example-files.zip  hello.txt
```

The `a_sincere_hello.txt` file is gone.

:::{caution}
Like many shell commands, there's no undo for the `cp`, `mv`, and `rm`
commands. They're also relatively mute, printing nothing or little to indicate
what they're doing. These characteristics make it easy to accidentally
overwrite or delete a file.

If you're worried about making mistakes, you can put these commands in
interactive mode by passing the `-i` or `--interactive` flag. In interactive
mode, the commands will prompt you with a yes/no question any time they might
overwrite or delete a file.
:::

:::{note}
The `cp`, `mv`, and `rm` commands can also copy, move, and remove directories.
The `mv` command can do this by default. For the `cp` and `rm` commands, you
have to set the `-r` or `--recursive` flag in order for them to operate on
directories.

Unlike `rmdir`, the `rm -r` command can delete directories even if they contain
files.
:::


## Reference: Editing Commands

Here's a summary of the commands we covered in this chapter:

:::{list-table}
:header-rows: 1

* - Command
  - Description
  - Examples
* - `nano`
  - Opens the nano text editor.
  - `nano my_file.txt`
* - `cp`
  - Makes a copy of a file or directory.
  - `cp my_file.txt my_copy.txt`
* - `mv`
  - Moves (or renames) a file or directory.
  - `mv my_file.txt my_file_renamed.txt`
* - `rm`
  - Removes a file or directory.
  - `rm my_file.txt`
:::


## A Final Note

Although we've only scratched the surface of what you can do with the command
line, the commands and underlying concepts we've discussed here should prepare
you to continue using and learning about the CLI. In doing so, our hope is that
you'll become a more confident and experienced computer user. If questions do
arise, DataLab offers support across all levels of experience. See [our
website][dl] for details about office hours, as well as other resources and
events for learners. Happy scripting!

[dl]: https://datalab.ucdavis.edu/office-hours/


