# Template: Workshop Reader

This repository is a template for Python-specific workshop readers for the UC 
Davis DataLab. It uses [Jupyter Book][jb] to knit the reader. You can also 
optionally use **Conda** to manage packages (instructions at the bottom).

To get started, create a new repo on GitHub from this template
([instructions][gh]), then `git clone` your new repo.

[gh]: https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template
[jb]: https://jupyterbook.org/en/stable/intro.html

Once you've cloned the repo, here's a checklist of things to do to prepare it:

1. **Conda** (optional): To create a new virtual environment with Conda, open 
   a command line interface (such as Terminal) and run the following:

   ```sh
   conda create --name WORKSHOP_TITLE python=YOUR.DESIRED.VERSION
   ```

   Then activate the environment with:

   ```sh
   activate WORKSHOP_TITLE
   ```

   You'll need to install Jupyter Book before doing anything else. 

   ```sh
   conda install -c conda-forge jupyter-book
   conda install -c conda-forge ghp-import
   ```

   You can skip this step if you're not going to use **Conda**.

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
   ghp-import -n -p -f _build/html
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
_Instructor: YOUR NAME <<YOUR_EMAIL@ucdavis.edu>>_  
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
    and match filetypes). Put all chapter filess in the `chapters/` directory.
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

6.  Run the command `ghp-import -n -p -f _build/html` in a shell at the top
    level of the repo to update the `gh-pages` branch of the repo. This uses
    the [`ghp-import` Python package][ghp-import], which you will need to
    install first (`pip install ghp-import`). The live web page will update
    automatically after 1-10 minutes.

[ghp-import]: https://github.com/c-w/ghp-import


## Setup

### Python Packages

We recommend using [conda][] to manage Python dependencies. The `env.yaml` file
in this repo contains a list of packages necessary to build the reader. You can
create a new conda environment with all of the packages listed in that file
with this shell command:

```sh
conda env create --file env.yaml
```

[conda]: https://docs.conda.io/en/latest/
