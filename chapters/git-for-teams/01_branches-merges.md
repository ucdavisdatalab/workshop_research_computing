# Branches & Merges

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Explain what a branch is
* Describe some ways in which branches are useful
* Explain how local branches and remote branches are different
* List the branches in a repository
* Switch between branches in a repository
* Explain what a merge is
* Merge one branch into another
* Explain what a merge conflict is
* Describe some ways to avoid merge conflicts
* Resolve a merge conflict
:::



<!--
We usually teach this workshop immediately after the Intro to Command Line and
Intro to Version Control workshops. At that point, many learners are relatively
new to programming and might not be able to imagine ways in which concepts like
branches, issues, pull requests, and .gitignore will apply to their projects.
They're also probably not working on a project with hundreds or thousands of
collaborators (graduate research tends to be solitary or within a single lab).
So to reach the most learners, the focus should probably be on how these
concepts can benefit small projects with 1-3 contributors. That is, if I'm
working alone or on a small team, for what would I use issues? Pull requests?
Etc.

A natural starting point is merge conflicts, because they can arise any time
there are multiple copies of a repository (or even branches within a
repository). Beginners can even create merge conflicts with themselves.

Merges might be easier to understand in the context of branches, because if you
understand branches then you're already part of the way to understanding
diverging histories (and branches are very visual). On the other hand,
beginners probably aren't going to be using branches frequently, so in practice
they're more likely to encounter a merge conflict when they pull from a remote.

We could follow the example in Intro to Version Control: have the learners work
in pairs, each committing to a single repository, and lead them to a merge
conflict. This is nice for continuity between workshops and -
-->

<!--
Example flow:

* Clone sandbox repo
* Switch to `demo`
* Make changes
* Switch to `main`, discarding changes

* Switch to `demo`
* Create branch `xx-foo`
* Switch to `xx-demo`
* Make changes, add, commit
* Switch to `demo`
* Merge `xx-demo` into `demo`

* Create merge conflict
* Resolve merge conflict

* Create a fork of sandbox repo
* Switch to `main`
* Create branch `xx-response`
* Add and commit a response to the prompt
* Add fork to the remotes
* Push `xx-response` to the fork
* Make a pull request from fork to original
-->

## What's a Branch?

As you make commits to a Git repository, the commits become that repository's
history. You can explore the history with commands like `git log` and `git
restore` (see {ref}`sec-exploring-history`). For many repositories, the history
is linear---each commit has one commit before it and possibly one after it, as
in {numref}`fig-history-linear`.

:::{figure} /images/git-for-teams/history_linear.svg
:name: fig-history-linear

Visualization of a Git repository with a linear history. There is exactly one
branch, named `a`. Solid arrows show the order of commits. Dashed arrows show
where the next commit will be on each branch. Each circle represents a commit.
:::

A **branch** is a distinct path through a repository's history, usually formed
by branching off of or splitting the history at a particular commit, as in
{numref}`fig-history-branched`. When a repository's history is completely
linear, the repository has just one branch.

<style type="text/css">
    .blue, .yellow, .green, .gray, .white {
        display: inline-block;
        vertical-align: sub;
        width: 1em;
        height: 1em;
        border: 1px solid;
    }
    .blue   { background-color: #7dbee0 }
    .yellow { background-color: #f3d06f }
    .green  { background-color: #b8c7a8 }
    .gray   { background-color: #b3b3b3 }
    .white  { background-color: #fff }
</style>

:::{figure} /images/git-for-teams/history_branch.svg
:name: fig-history-branched

Visualization of a Git repository with two branches, `a` and `b`. Solid arrows
show the order of commits. Dashed arrows show where the next commit will be on
each branch. Each circle represents a commit. Commits only on branch `a` are
blue <span class="blue"></span>, and commits only on branch `b` are yellow
<span class="yellow"></span>. Commits on both branches before the merge are
gray <span class="gray"></span>.
:::

Branches are really useful! With branches, you can:

* **Try something that might not work out.** Suppose you get an idea for a new
  feature to add to your lab's analysis package, but developing it will take
  time and might temporarily break the package. You also need to use the
  package for your day-to-day work. By developing the new feature on a branch,
  you can switch between the stable branch and the development branch whenever
  you need. This makes it easy to keep using the package while you develop the
  new feature, to test the new feature, and to pause or abandon the new feature
  if it doesn't work out (just stop working on the branch).
* **Temporarily store some work.** If you start working on something but get
  interrupted, you can store your work on a branch until you're ready to come
  back to it.
* **Explore an earlier version of the repository.** If you need to look at an
  earlier version of a single file, `git restore` and `git show` are great. But
  what if you want to know what the entire repository was like in the past or
  don't know which file you need? You can make a branch at the commit in which
  you're interested, switch to the branch, and then explore the repository
  without need for any special tools or Git commands.
* **Control when conflicting changes are resolved.** When multiple people work
  on a repository, sharing work frequently makes it easier to get feedback and
  coordinate everyone's efforts. By having each person work on a separate
  branch, they can share their work at any time, even if changes they made to a
  particular file conflict with changes someone else made. Resolving the
  conflicts can wait until everyone is ready to combine their work.
* **Present a repository differently to different audiences.** For instance, if
  your repository contains a package, you can use the default branch for files
  users of the package need and a separate development branch for files
  contributors to the package need.

These are just a few ways to use branches. Git wasn't the first version
control system to support branches, but it does make using them especially
quick and easy.


## Switching Branches

Let's explore Git's commands for working with branches. Along the way, we'll
also review some of the fundamental Git commands we explained in
{ref}`sec-git-basics`. As an example, we'll use DataLab's [Git sandbox
repository][dl-git-sandbox].

[dl-git-sandbox]: https://github.com/ucdavisdatalab/sandbox_git

To get started, clone a copy of the repository from GitHub to your computer.
Open a terminal and navigate to your home directory:

```none
cd
```

Go to the repository's page on GitHub, click the green "Code" button, and copy
the URL under the SSH tab. Then go back to the terminal, type the `git clone`
command, and paste the URL after the command. The full command should look like
this:

```none
git clone git@github.com:ucdavisdatalab/sandbox_git.git
```

Run the command, wait for Git to clone the repository, and then change
directories into it:

```none
cd sandbox_git
```

Take a moment to explore the files in the repository.

At any given time, the working tree (the collection of actual files and
directories) in a repository corresponds to a single branch. We call this the
**current branch**, and say that the current branch is **checked out** or that
the repository is "on" the current branch. When you clone a repository, Git
automatically checks out the default branch.

The `git status` command lists the current branch in its output:

```none
git status
```
```none
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

The repository is on the `main` branch, which makes sense you just cloned the
repository, and `main` is the default branch.

On GitHub, the repository has two branches: the default branch `main` and
another branch called `demo`. You can use `git branch` to list the branches in
a repository:

```none
git branch
```
```none
* main
```

The active branch (`main` in this case) is marked with a star `*`. But where's
the `demo` branch? By default, `git branch` only lists branches you create or
switch to locally. Each of these is called a **local branch**. The `demo`
branch exists on the remote (GitHub), but you've never switched to a local
copy, so it's a **remote branch**. You can use the `-a` flag (short for
`--all`) to make the `git branch` command show all branches, local and remote:


```none
git branch -a
```
```none
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/demo
  remotes/origin/main
```

<!-- FIXME: explain HEAD -->
In the output, remote branches are prefixed with `remotes/` and the name of the
remote (in this case, `origin`). You should see one local branch, `main`, and
three remote branches.

:::{tip}
You can use the `-v` flag (short for `--verbose`) to make the `git branch`
command show more information about each branch.
:::

Let's take a look at the `demo` branch. To switch to a different branch, use
the `git switch` command with the name of the branch you want:

```none
git switch demo
```
```none
branch 'demo' set up to track 'origin/demo'.
Switched to a new branch 'demo'
```

<!-- TODO: explain that Git automatically creates a local branch -->

Open the `README.md` file in a text editor. You should now see a message in the
file that wasn't there on the `main` branch:

```markdown
**You're probably on the `demo` branch!**

What's your favorite color?
```

Answer the question in `README.md` and save the file. Don't worry about
committing your answer. Git will now say that you've made some changes to
`README.md`:

```none
git status
```
```none
On branch demo
Your branch is up to date with 'origin/demo'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Try switching back to the `main` branch:

```none
git switch main
```
```none
error: Your local changes to the following files would be overwritten by checkout:
        README.md
Please commit your changes or stash them before you switch branches.
Aborting
```

Git refuses to switch branches because doing so would overwrite the changes you
made to `README.md`. You haven't committed the changes, so there wouldn't be
any way to restore them later. There are several different ways to handle this:

<!-- FIXME: maybe encourage --force instead of -f? -->

* **Keep the changes on the `demo` branch.** To do this, add the changes to the
  staging area, make a commit, and then switch branches.
* **Move the changes to the `main` branch.** To do this, switch branches with
  the `-m` flag (short for `--merge`).
* **Discard the changes.** To do this, switch branches with the `-f` flag
  (short for `--force`).

For the sake of this example, take the third option and throw away the changes:

```none
git switch -f main
```
```none
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

Check that the changes you made to `README.md` are gone:

```none
git status
```
```none
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Then switch back to the `demo` branch:

```none
git switch demo
```
```none
Switched to branch 'demo'
Your branch is up to date with 'origin/demo'.
```

Notice that your changes are gone on this branch too:

```none
git status
```
```none
On branch demo
Your branch is up to date with 'origin/demo'.

nothing to commit, working tree clean
```

Since we didn't commit the changes, Git has no record of them.

:::{danger}
Only use `git switch` with `-f` when you're absolutely sure you want to discard
your changes. There's no undo.
:::


## Creating a Branch

When you start working on something new in a repository, like a new feature or
a bug fix, it's usually a good idea to create a new branch and do the work
there. Then you can switch back to the original branch any time you need to use
the repository in the state it was in before you started working. Working on a
branch also makes collaboration easier (see {ref}`sec-whats-a-pull-request`)
and can make a repository's history easier to understand.

Branch names should be short but descriptive enough to serve as a reminder
about the purpose of the branch. Git allows a wide variety of characters in
branch names, but using only lowercase letters, numbers, underscores (`_`), and
dashes (`-`) ensures that your branch name is safe for Git-related tools and
easy to type. Branch names cannot contain spaces or begin with a dash. See
[this StackOverflow post][git-branch-names] for the exact rules.

[git-branch-names]: https://stackoverflow.com/a/3651867

:::{tip}
For branches where you'll work mostly or entirely alone, we recommend names of
the form `XX-DESCRIPTION`, where `XX` are your initials and `DESCRIPTION` is a
short description of the branch. For instance, `nu-fix-indexing` is a good
branch name for a branch about fixing an indexing bug.
:::

You can also use the `git branch` command to create a new branch. Branches
almost always start from an existing commit in the repository's history; the
default is the most recent commit on the current branch. Make sure that you're
on the `demo` branch, and then create a new branch called `my-first-branch`:

```none
git branch my-first-branch
```

The `git branch` command doesn't print anything when it successfully creates a
branch. Switch to the new branch:

```none
git switch my-first-branch
```
```none
Switched to branch 'my-first-branch'
```

Examine `README.md` and the most recent commit in the Git log on this branch.
They're identical to `demo`, since this is a branch off of the most recent
commit to `demo`.

Open `README.md` and answer the question again. For instance, you might answer:

```markdown
My favorite color is blue.
```

Save the file, then add the changes to the staging area:

```none
git add README.md
```

Then commit the changes, making sure to write a descriptive commit message:

```none
git commit
```

With your changes committed, you can safely switch back to the `demo` branch:

```none
git switch demo
```
```none
Switched to branch 'demo'
Your branch is up to date with 'origin/demo'.
```

You can switch back to `my-first-branch` any time you want to work on that
version of the repository.


## What's a Merge?

After working on a branch for a while, you might reach a point where you feel
like your work is ready to copy back to the original branch. You can do this
with a **merge**, which applies commits from one branch onto another. 

To practice merging, let's merge the `my-first-branch` branch into the `demo`
branch. You can use `git merge BRANCH` to merge the branch `BRANCH` into the
current branch. Make sure the repository is on the `demo` branch, and then run:

```none
git merge my-first-branch
```
```none
Updating d734c25..7d96756
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

When you run `git merge`, Git will try to merge the commits automatically. An
automatic merge is possible as long as there are not changes to a file on one
branch that conflict with changes to that file on the other. There are two
kinds of automatic merges (also see {numref}`fig-merge-vs-ff`):

* A regular merge, for which Git creates a **merge commit** that combines the
  changes from each branch.
* A **fast-forward merge**, where only one branch has new commits, so Git can
  apply the new commits without any need for a merge commit.

When we merged `my-first-branch` into `demo`, Git did a fast-forward merge,
since we didn't make any commits to `demo` after creating `my-first-branch`.
Git notes this in the output. This means that Git did not need to create a
merge commit.

:::{figure} /images/git-for-teams/merge-vs-fast-forward.svg
:name: fig-merge-vs-ff

Visualization of merging and fast-forward merging branch `b` into `a`. Solid
arrows show the order of commits. Dashed arrows show where the next commit will
be on each branch. Each circle represents a commit. Commits only on branch `b`
are yellow <span class="yellow"></span>. Commits on both branches before the
merge are gray <span class="gray"></span>. Commits on both branches after the
merge are green <span class="green"></span>. The merge commit is white <span
class="white"></span>.
:::

In a regular merge, Git will create a new commit and give you a chance to enter
a commit message (similar to when you run `git commit`). Git will automatically
fill in a commit message describing the merge, and in most cases it's best to
accept the default message.

::::{admonition} Merge versus Rebase
:class: note

There's another way to apply commits from one branch onto another: a **rebase**
updates a collection of commits so that they appear as if they were made after
any new commits on the original branch, and then applies them. Rebasing makes a
repository's history linear (and thus simpler) even if most work was done on
branches. On the other hand, rebasing can make the actual development process
harder to fathom, since commits are not necessarily in chronological order and
information about branches is discarded. You can rebase one branch onto another
with the `git rebase` command.

:::{figure} /images/git-for-teams/merge-vs-rebase.svg
:name: fig-merge-vs-rebase

Visualization of merging branch `b` into `a` and rebasing branch `a` onto `b`.
Solid arrows show the order of commits. Dashed arrows show where the next
commit will be on each branch. Each circle represents a commit. Commits only on
branch `a` are blue <span class="blue"></span>, and commits only on branch `b`
are yellow <span class="yellow"></span>. Commits on both branches before the
rebase are gray <span class="gray"></span>. Commits on both branches after the
rebase are green <span class="green"></span>.
:::

We recommend merging over rebasing when you're dealing with multiple branches.
That said, learning to use `git rebase` can be beneficial for managing commits
within a single branch---you can use the command to reorganize or combine
commits *before* you share them with anyone else, so that the history of your
work is well-organized even if your development process is messy.
::::


## Merge Conflicts

Branches can have commits that change files in different ways. For example, you
might answer the favorite color question with "red" on the `demo` branch and
save and commit your work. Later, you switch to `my-first-branch`, think about
how your favorite color is actually more of an "orange", and answer the
question again, saving and committing your new answer to the branch. The commit
on `demo` changes `README.md` to mention "red", while the commit on
`my-first-branch` changes `README.md` to mention "orange".

The example is a bit silly, but in a repository with many files, it's easy to
accidentally change a file in different ways on different branches. You might
also do it intentionally, perhaps because the branches originally had separate
purposes. When you're working collaboratively, you and your collaborator might
both decide to make changes to the same file, unaware of what the other is
doing.

We say branches like the two in the example have **diverged**: a file is
changed in a different way by the commits on each branch. Git can't merge
divergent branches automatically. Instead, when you run `git merge`, Git will
report that there's a **merge conflict** and ask you to resolve the conflict
manually. In other words, it's up to you to decide what the contents of the
file should be after the merge. That might mean keeping the changes from one
branch and discarding the changes from the other, or it might mean combining
the changes from both. What's correct depends on the file, the conflicting
changes, and the reason for the merge. Git can't guess your intent, so you have
to resolve the conflict.

To practice resolving a merge conflict, let's deliberately create the one in
the example. Make sure the repository is on the `demo` branch. Open `README.md`
with a text editor and edit it so that the last line is:

```markdown
My favorite color is red.
```

Save the file, then `git add` and `git commit` the changes.

Next, switch to the `my-first-branch` branch:

```none
git switch my-first-branch
```

Once again, open `README.md` with a text editor. This time, edit it so that the
last line is:

```markdown
I really like orange.
```

Save the file, then `git add` and `git commit` the changes.

The `demo` and `my-first-branch` branches have now diverged. Each has different
changes to `README.md`. Let's merge `demo` into `my-first-branch` so that we
can resolve the merge conflict. Make sure the repository is on
`my-first-branch`, then run:

```none
git merge demo
```
```none
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

As expected, Git reports that there's a merge conflict in `README.md`. At this
point, the branches are partially merged, but Git is waiting for you to fix the
conflict. You can see more information about this by checking the repository's
status:

```none
git status
```
```none
On branch my-first-branch
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

The "unmerged paths" mentioned in the output are the files with conflicts,
where Git was unable to merge the changes automatically. To resolve the
conflicts and complete the merge, edit each file with a text editor, then `git
add` and `git commit` your changes. To cancel the merge instead, run `git merge
--abort`.

Let's fix the conflict. Open `README.md` with a text editor. The last few lines
should look something like this:

```none
What's your favorite color?

<<<<<<< HEAD
I really like orange.
=======
My favorite color is red.
>>>>>>> demo
```

When you run `git merge` and there are merge conflicts, Git adds markers to the
associated files to indicate the conflicts. The markers have this form:

```none
<<<<<<< a

=======

>>>>>>> b
```

The `<<<<<<< a` line shows where the conflict begins and the `>>>>>>>` line
shows where it ends. Each also names one of the two branches being merged. The
`=======` line separates the conflicting changes on each branch. So Git puts
changes only on branch `a` between these markers:

```none
<<<<<<< a

=======
```

And puts changes only on branch `b` between these markers:

```none
=======

>>>>>>> b
```

Everything outside of conflict markers is the same on both branches.

In the `README.md` file, the first part of the conflict is:

```none
<<<<<<< HEAD
I really like orange.
=======
```

`HEAD` is a special name Git uses to refer to the current branch. The current
branch is `my-first-branch`, since we switched to it before starting the merge.
So this part shows the conflicting changes on `my-first-branch`. And sure
enough, `I really like orange.` is what we committed there.

The second part of the conflict is:

```none
=======
My favorite color is red.
>>>>>>> demo
```

This part shows the conflicting changes on `demo`. That's where we committed
`My favorite color is red.`.

To fix a conflict, delete the conflict markers and edit the associated lines to
be the way you want. For instance, we can edit the last few lines of
`README.md` to be:

```markdown
What's your favorite color?

I really like orange, but I also really like red.
```

This combines the changes from each branch nicely. Save the file and `git add`
the changes. Take a look at the status now:

```none
On branch my-first-branch
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   README.md
```

There are no longer any unmerged paths, meaning there are no more conflicts to
resolve. With all of the conflicts resolved, we can complete the merge with:

```none
git commit
```
```none
[my-first-branch fbadd8f] Merge branch 'demo' into my-first-branch
```

Because this is a merge, Git creates a merge commit and automatically provides
a commit message. Congratulations, you've just resolved your first merge
conflict! 🎉


## Merging Remote Branches

You've probably used the `git pull` command to pull changes from a repository.
The command is actually just a shortcut for running two simpler commands:

1. `git fetch` to download branches from a remote
2. `git merge` to merge a remote branch onto the current local branch

For each local branch, the remote branch that `git pull` merges is called the
**upstream** or **tracking** branch. You can set or change a local branch's
upstream branch by running `git pull` or `git branch` with the `-u` (short for
`--set-upstream`) flag and the name of a remote branch. You can also configure
Git to automatically set an upstream branch for each local branch your create.

Understanding how `git pull` works might help you simplify your mental model of
Git: every copy of a repository is just a collection of branches, and getting
commits from a remote just means downloading a branch---often with the same
name as a local branch---and merging it.
