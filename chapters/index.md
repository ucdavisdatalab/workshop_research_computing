# Preface

This collection of workshops provides an introduction to the concepts,
workflows, and tools fundamental to reproducible computational research. The
workshops are:

* **Reproducibility Principles and Practices** (one 1-hour session)

  A research project is **reproducible** if a different researcher can carry
  out the same analysis with the same data and produce the same overall result.
  To do so, they need transparent, detailed documentation about _all_ of the
  steps in the research process and access to the tools---especially
  code---with which the steps were carried out. Reproducibility enables
  independent verification, a touchstone for all research.

  There are myriad practices, often accompanied by software tools, that can
  help ensure research projects are reproducible. This overview workshop will
  help you decipher which to adopt and when to adopt them. The workshop also
  highlights additional benefits many of these practices confer, such as making
  it easier to collaborate with others. As an overview, this workshop is
  relatively non-technical, but provides technical references, including other
  DataLab workshops, for all of the practices covered.

  This workshop is intended for learners at all experience levels, and may
  benefit learners at different experience levels in different ways.

  :::{admonition} Learning Goals
  :class: note dropdown
  After completing this workshop, learners should be able to:

  + Describe widely-used practices and tools for ensuring that research projects
    are reproducible, such as:
       - Writing documentation
       - File and directory naming conventions
       - Version control systems
       - Software environment management
       - Build systems
       - Packaging
       - Software testing
       - Virtualization and containerization
  + Explain the advantages and disadvantages of these reproducibility practices
  + Evaluate whether a given reproducibility practice is relevant to their
    research project
  + Identify references they can consult to learn technical details about
    reproducibility practices
  + Explain the ways in which reproducibility practices can facilitate
    collaboration for active research projects
  :::

  :::{important}
  We use [this slide deck][slides] when we present this workshop.

  [slides]: https://docs.google.com/presentation/d/1uez0jDi5itswL6La3hj9DUucjNR30EKLCeh8KIzK2WA/edit?usp=sharing

  The entire workshop is summarized by [this cheat sheet][cheat].

  [cheat]: https://docs.google.com/document/d/1Ris4HHFZz_3yPJxvVw8vt_qS6WFxV9SlFQ86oznece8/edit?usp=sharing
  :::

* **Introduction to the Command Line** (one 2-hour session)

  Learn and practice how to talk directly to your computer via the command
  line. The command line is a powerful tool for using scientific software,
  working with large data sets, and controlling remote servers. It is primarily
  used to manage files and run programs, and it allows for automation of
  repetitive tasks. This workshop is a prerequisite for many of DataLab's other
  workshops, including all of the following workshops in this list.

  :::{admonition} Learning Goals
  :class: note dropdown
  After completing this workshop, learners should be able to:
  + Explain the directory structure of their computers
  + Navigate across and within files and directories
  + Create, copy, move, and delete files
  + Use command line tools to edit files
  + Identify where to go for help and to learn more
  :::

* **Installing Software with Pixi** (one 2-hour session)

  Learn how to install and manage open-source software packages for research
  computing. Installing software is often tricky due project-specific
  requirements for package versions---which can conflict with other
  projects---and inconsistent or incomplete install documentation. We’ll focus
  on using [Pixi][], a package manager for [the conda ecosystem][conda-eco], to
  create independent, reproducible software environments and install software
  with ease. We’ll also briefly discuss the unique advantages of using pixi for
  Python projects and how pixi compares to other package managers.

  [pixi]: https://pixi.sh/
  [conda-eco]: https://conda.org/

  :::{admonition} Learning Goals
  :class: note dropdown
  After completing this workshop, learners should be able to:

  + Explain what computing environments are
  + Explain what virtual environments are and why they're useful
  + List popular tools for installing software on POSIX computers
  + Create and organize project directories for projects
  + Initialize projects with Pixi
  + Install software with Pixi

  The second half of this workshop is modular, and which modules are covered is
  up to the instructor. Each module lists specific learning goals.
  :::

* **Introduction to Version Control** (one 2-hour session)

  This workshop covers the fundamentals of using version control systems for
  reproducible research. Topics covered include what version control is, key
  concepts and terminology, how to install the [Git][] version control system,
  how to create a repository, how to save versions of files, how to restore old
  versions of files, and how to use hosting services for Git repositories to
  share and collaborate on projects.

  [Git]: https://git-scm.com/

  :::{admonition} Learning Goals
  :class: note dropdown
  After completing this workshop, learners should be able to:

  + Explain the purpose of using a version control system (VCS)
  + Explain the difference between centralized and distributed version control
  + Explain what a repository is
  + Explain what Git is
  + Initialize a Git repository
  + Check the status of a Git repository
  + Explain what the Git working tree and staging area are
  + Inspect and stage changes to a Git repository
  + Commit changes to a Git repository
  + View the history of commits in a Git repository
  + Restore an old version of a file from a commit
  + Explain what [GitHub][] is and how it relates to Git
  + Create an SSH key in order to authenticate with GitHub
  + Explain the difference between a local and remote repository
  + Clone a remote repository to your computer
  + Push changes to a remote repository
  + Pull changes from a remote repository

  [GitHub]: https://github.com/
  :::

* **Git for Teams** (one 2-hour session)

  This workshop goes beyond the basics of Git: it explains how to customize Git
  to suit your preferred workflow and how to take full advantage of Git and
  GitHub's collaborative features. Topics include why and how to use branches,
  how to merge branches even when there are conflicts, how to use GitHub's
  project management features, and ways to configure Git to be more convenient.
  This workshop also prepares learners to use Git to contribute to open-source
  projects.

  :::{admonition} Learning Goals
  :class: note dropdown
  After completing this workshop, learners should be able to:

  + Explain what a branch is
  + Describe some ways in which branches are useful
  + Explain how local branches and remote branches are different
  + List the branches in a repository
  + Switch between branches in a repository
  + Explain what a merge is
  + Merge one branch into another
  + Explain what a merge conflict is
  + Describe some ways to avoid merge conflicts
  + Resolve a merge conflict
  + Explain what an issue is
  + Create an issue
  + Explain what a fork is
  + List a repository's remotes
  + Add a remote to a repository
  + Remove a remote from a repository
  + Explain what a pull request is
  + Create a pull request
  + Merge a pull request
  + Use GitHub Flow to contribute to projects
  + Create a `.gitignore` file to make Git ignore specific files
  + Describe some popular Git configuration changes
  + Create Git aliases
  + Describe some strategies to fix problems with repositories
  + Name some references to turn to for help with Git
  :::

Open-source tools are an integral part of many research projects. Contributing
to these projects ensures they continue to be sustainable, and releasing
research-related code under open-source licenses ensures computational research
is reproducible. These workshops are part of the [University of California Open
Source Program Office Network][ucospo] and their development was funded in part
by a grant from the Alfred P. Sloan Foundation.

[ucospo]: https://ucospo.net/

