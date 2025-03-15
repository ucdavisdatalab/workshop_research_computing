# Template: Workshop Reader

This repository is a template for Python-specific workshop readers for the UC
Davis DataLab. It uses [Jupyter Book][jb] to generate the reader.

To get started, create a new repo on GitHub from this template
([instructions][gh]), then `git clone` your new repo.

[gh]: https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template
[jb]: https://jupyterbook.org/en/stable/intro.html

Once you've cloned the repo, here's a checklist of things to do to prepare it:

1. Environment management: @nick-ulle recommends that you use [pixi][], a fast
   package manager based on the conda ecosystem. A few minutes spent on proper
   environment management now will save you time (and suffering) later! To
   install pixi, follow [the official instructions][pixi].

   Once you've installed pixi, open a terminal and navigate to your local copy
   of this repo. To initialize pixi for this repo, run:

   ```sh
   pixi init
   ```

   This creates a new empty virtual environment. Metadata about the environment
   and project are recorded in the `pixi.toml` file. Verify and correct the
   `pixi.toml` file. In particular, make sure the platforms include `linux-64`,
   `osx-64`, and `win-64`.

   Next, install the jupyter-book and ghp-import packages, which you'll need
   for development:

   ```sh
   pixi add jupyter-book ghp-import
   ```

   You can also install other packages at this point. The packages you install
   will be recorded in `pixi.toml` and specific versions of *every* installed
   package will be recorded in `pixi.lock`. Don't forget to commit both
   `pixi.toml` and `pixi.lock`!

   To open a shell in the virtual environment:

   ```sh
   pixi shell
   ```

   See the note at the end of this file if you're using Git Bash.


2. `README.md`: Replace the all-caps text with your workshop details.
   + Title
   + Quarter & year
   + Author's name and email
   + Helpers' names and email (optional)
   + Reader URL
   + Event URL
   + Description, learning goals, & prerequisites

3. `_config.yml`: Replace the all-caps text with your workshop details.
   + Title
   + Author
   + Date (year only)
   + URL

4. `chapters/index.md`: You can write chapters as Markdown (`.md`) files,
   Jupyter Notebook (`.ipynb`) files, or a mix of both. The template defaults
   to Markdown files. If you want to use a Jupyter notebook for the front page,
   delete `index.md` and use Jupyter to create `index.ipynb` instead.

5. `_toc.yml`: This file is the table of contents for the book. Any chapters
   that are not registered here will not appear in the book. The `index.md` (or
   `index.ipynb`) and `01_example.md` chapters are already registered. Note
   that you should not specify the file extension in the table of contents. If
   you rename or add any chapters, you must update the table of contents in
   order for them to appear in the book.

6. Compile your book with:

   ```sh
   jupyter-book build .
   ``` 

   This will generate a new `_build/` directory, which will contain HTML 
   versions of your reader. This should not be added to Git (a `.gitignore` 
   file is already in the template).

7. `git add` all of the changed files, then `git commit` and `git push`.

8. This template is set up to serve the reader from the `gh-pages` branch of
   the repository rather than a directory in the `main` branch (in contrast to
   our Bookdown template). Once you've committed your files, you need to run
   one more command, which will automatically push the rendered HTML files to
   the `gh-pages` branch on GitHub:

   ```sh
   ghp-import -nop _build/html
   # Or equivalently:
   # ghp-import --no-jekyll --no-history --push _build/html
   ```

   Make sure the GitHub repo is configured to serve pages from the `gh-pages`
   branch by going to `Settings/Pages` on GitHub. Select the `gh-pages` branch
   if it isn't selected already. _You must run the `ghp-import ...` step every
   time you wish to push updates to the live site on GitHub._

9. `README.md`: Remove these template instructions, which end at this step, 
   and, if you'd like, `git add` this file and `commit`/`push` it.

# Workshop: WORKSHOP TITLE

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

_[UC Davis DataLab](https://datalab.ucdavis.edu/)_  
_QUARTER YEAR_  
_Instructor: YOUR NAME_  
_Maintainer: MAINTAINER'S NAME <<MAINTAINER_EMAIL@ucdavis.edu>>_  

* [Reader](https://ucdavisdatalab.github.io/YOUR_REPOSITORY/)
* [Event Page](https://datalab.ucdavis.edu/eventscalendar/YOUR_EVENT/)

YOUR DESCRIPTION, LEARNING GOALS, PREREQUISITES, ETC

## Contributing

The course reader is a live webpage, hosted through GitHub, where you can enter
curriculum content and post it to a public-facing site for learners.

To make alterations to the reader:
	  
1.  Check in with the reader's current maintainer and notify them about your 
    intended changes. Maintainers might ask you to open an issue, use pull 
    requests, tag your commits with versions, etc.

2.  Run `git pull`, or if it's your first time contributing, see
    [Setup](#setup).

3.  Edit an existing chapter file or create a new one. Chapter files may be 
    either Markdown files (`.md`) or Jupyter Notebook files (`.ipynb`). Either 
    is fine, but you must remain consistent across the reader (i.e. don't mix 
    and match filetypes). Put all chapter files in the `chapters/` directory.
    Enter your text, code, and other information directly into the file. Make 
    sure your file:

    - Follows the naming scheme `##_topic-of-chapter.md/ipynb` (the only 
      exception is `index.md/ipynb`, which contains the reader's front page).
    - Begins with a first-level header (like `# This`). This will be the title
      of your chapter. Subsequent section headers should be second-level
      headers (like `## This`) or below.

    Put any supporting resources in `data/` or `img/`.

4.  Run the command `jupyter-book build .` in a shell at the top level of the
    repo to regenerate the HTML files in the `_build/`.

5.  When you're finished, `git add`:
    - Any files you edited directly
    - Any supporting media you added to `img/`

    Then `git commit` and `git push`. This updates the `main` branch of the
    repo, which contains source materials for the web page (but not the web
    page itself).

6.  Run the following command in a shell at the top level of the repo to update
    the `gh-pages` branch:
    ```
    ghp-import -n -p -f _build/html
    ```
    This uses the [`ghp-import` Python package][ghp-import], which you will
    need to install first (`pip install ghp-import`). The live web page will
    update automatically after 1-10 minutes.

[ghp-import]: https://github.com/c-w/ghp-import


## Setup

We *strongly recommend* using [pixi][], a fast package manager based on the
conda ecosystem, to install the packages required to build this reader. To
install pixi, follow [the official instructions][pixi]. If you prefer not to
use pixi, it's also possible to manually install the packages using conda or
mamba.

[pixi]: https://pixi.sh/

The `pixi.toml` file in this repo lists required packages, while the
`pixi.lock` file lists package versions for each platform. When the lock file
is present, pixi will attempt to install the exact versions listed. Deleting
the lock file allows pixi to install other versions, which might help if
installation fails (but beware of inconsistencies between package versions).

To install the required packages, open a terminal and navigate to this repo's
directory. Then run:

```sh
pixi install
```

This will automatically create a virtual environment and install the packages.

To open a shell in the virtual environment, run:

```sh
pixi shell
```

You can run the `pixi shell` command from the repo directory or any of its
subdirectories. Use the virtual environment to run any commands related to
building the reader. When you're finished using the virtual environment, you
can use the `exit` command to exit the shell.

> [!NOTE]
> If you're using Windows and Git Bash, the `pixi shell` command is [not yet
> supported][pixi-shell-win]. Instead, you can use the `pixi run` command to
> run commands in the virtual environment. See [the pixi
> documentation][pixi-basics] for examples of how to use `pixi run`.

[pixi-shell-win]: https://github.com/prefix-dev/pixi/issues/417
[pixi-basics]: https://pixi.sh/latest/basic_usage/
