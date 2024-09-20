Case-by-case Core Practices
===========================

This chapter covers case-by-case core practices for reproducibility. These are
practices for which the benefits outweigh the costs, but which don't apply to
every project. For instance, projects that will never be distributed do not
necessarily need a license, so choosing a license is listed here. DataLab
adopts these practices for all projects for which they are relevant, and we
recommend you do too.


Documentation
-------------

### Document the Data

In a perfect world, every data set would come with detailed documentation or
**metadata** about how the data were collected, what assumptions were made,
what biases might be present, any ethical concerns, the overall structure, what
each observation means, what each feature means, and more. Good data
documentation guides researchers towards appropriate, responsible use of the
data.

Collecting data as part of a project gives you and your collaborators control
over how the data are documented, so you can ensure there are no gaps. If your
project uses data collected earlier or by someone else, it's a good practice to
fill gaps in the existing documentation with your own. Thorough documentation
isn't just beneficial to other researchers, it's also beneficial to future
you---small details you notice and document about features could be important
later in the project.

:::{seealso}
See DataLab's [README, Write Me! workshop reader][datalab-readme] for more
about how to document data.
:::

[datalab-readme]: https://ucdavisdatalab.github.io/workshop_how-to-data-documentation/


(document-every-experiment)=
### Document Every Experiment

:::{note}
An "experiment" doesn't necessarily have to be an experiment done in a lab.
Doing an experiment could mean running a simulation, testing a model, or
something else.
:::

Keep a record of every experiment that you do. Document parameters, the
results, and anything else you think is relevant for:

* Repeating experiments in the future. Research projects are often
  iterative---you may need to repeat an experiment to make adjustments to a few
  parameters or because you discovered a bug in your code.

* Comparing across experiments. Think of the documentation as a high-level
  overview of your project. Use it to get a sense of which experiments had
  promising results, which didn't, and which you might want to try next.

* Avoiding redundant work. By keeping track of what's already been done, you
  can avoid accidentally and unnecessarily repeating an experiment that you or
  your collaborators have already done.

Get in the habit of recording experiments as soon as you start to plan them,
and recording the results as soon as they finish. Assign a unique name or
number to each experiment so that you can reference specific experiments in
your other notes and files. If you have collaborators, make sure they can
access the documentation and understand how to use it.

:::{tip}
Spreadsheets are often a good format for this kind of documentation, since
they're convenient for data entry.
:::


### Choose a License

A **license** is a legal document that grants others permission to do, use, or
possess something. If you plan to make your project widely or publicly
available, it's a good idea to select a license so that you can retain some
control over how it's used and protect yourself against certain kinds of legal
claims.

Different kinds of licenses are appropriate for different kinds of content.
There are licenses designed specifically for software and code, as well as
licenses for other media.

An **open-source** license is one that mandates access to or distribution of
original sources for any derivative products. For software, this means that the
original source code must be accessible. Open-source licenses ensure that
software can continue to be maintained and extended even if the original
developers cease development. They also benefit transparency and collaboration,
since anyone with the software can inspect and modify the code.

DataLab uses open-source licenses for a majority of our projects, whether they
consist of software or other materials (like this reader).

:::{seealso}
For licensing software, see [choosealicense.com][gh-cal] (maintained by GitHub)
or the Open Source Initiative's [FAQ answer about which license to
choose][osi-cal].

For licensing data, writing, art, or other materials, see Creative Commons'
[Choose a License page][cc-cal].
:::

[gh-cal]: https://choosealicense.com/
[osi-cal]: https://opensource.org/faq/#which-license
[cc-cal]: https://creativecommons.org/choose/


Artifact Preservation
---------------------

### Use Configuration Files

A **configuration file** is a file that stores accessory parameters for a
computation. Configuration files are distinct from data sets in that the data
they contain is usually under your control and not of primary interest. For
example, a configuration file for code that trains a statistical model might
specify where to find training data, where to find test data, which features to
include in the model, and where to save predictions. All of these could be
embedded in the code, but using a configuration file makes the code more
flexible.

Configuration files also have another benefit: they serve as a record of your
parameter settings. When parameter settings are embedded in code, then you have
to edit the code each time you want to run it with different settings. Unless
you're meticulous about version control, it may be difficult to determine which
settings you used for past computations. Configuration files eliminate this
problem: when you want to use a new settings, create a new configuration file
(possibly by copying one you already have). Then you can easily find the
settings for any computation in its respective configuration file.

<!-- also hard to share with collaborators and may introduce bugs -->

:::{tip}
If you also {ref}`document-every-experiment`, include the name of the
configuration file(s) for each experiment in your documentation.
:::

Plain-text formats are a good choice for configuration files because they can
be edited by anyone with a text editor. At DataLab, we particularly like to use
[TOML][] and [YAML][] formats for configuration files because they're
widely-used, human-readable, and there are well-supported functions for reading
them in R, Python, and Julia.

[TOML]: https://toml.io/
[YAML]: https://yaml.org/


### Use Version Control for Data

(databases)=
#### Databases

:::{seealso}
See DataLab's [An Overview of Databases and Data Storage workshop
reader][datalab-db] for an introduction to the topic. Then see DataLab's
[Introduction to SQL for Querying Databases workshop reader][datalab-sql] to
learn how to query data in databases.
:::

[datalab-db]: https://ucdavisdatalab.github.io/workshop_intro_to_databases/
[datalab-sql]: https://ucdavisdatalab.github.io/workshop_intro_to_sql/


#### Data Version Control Systems


Project Organization
--------------------

### Save Clean Data



:::{seealso}
See DataLab's [Intermediate R: Cleaning Data & Automating Tasks workshop
reader][datalab-r-clean] to learn how to clean data in R.
:::

[datalab-r-clean]: https://ucdavisdatalab.github.io/workshop_intermediate_r/string-date-processing.html


### Use File Formats Effectively

Store data sets in file formats that:

* Are accessible to you, your collaborators, and other researchers across a
  variety of software

* Preserve the data set's structure and types (including categories, dates, and
  times)

* Can be read and written efficiently

Plain-text formats---such as comma-separated values (CSV)---satisfy the
accessibility requirement, and also have the advantage of being human-readable,
but usually fail on preservation and efficiency. Binary formats usually fare
better for preserving data set structure and types, as well as for fast read
and write times.

For tabular data sets, both [Apache Arrow][arrow] (also known as [Feather][])
and [Apache Parquet][parquet] satisfy all three requirements. Arrow and Parquet
are binary formats, so they aren't human-readable, but they are open standards,
so support for them can be implemented in any software. R, Python, and Julia
all already support both through various packages. The Arrow standard is
simpler but changes more frequently than the Parquet standard. As a result,
Arrow is faster to read and write and a good choice for short-term storage.
Parquet is a better choice for long-term storage because the standard is more
stable.

[arrow]: https://arrow.apache.org/
[feather]: https://arrow.apache.org/docs/python/feather.html
[parquet]: https://parquet.apache.org/



Workflow Automation
-------------------

### Orchestrate with Shell Scripts

Even if you {ref}`organize-the-code` carefully, it's likely you'll end up with
workflows that consist of running several commands in a specific order and with
specific settings. It's also likely you'll need to re-run these workflows many
times as you incrementally improve, extend, and adapt the code. To make this
more convenient and ensure that you don't accidentally skip commands or use the
incorrect settings, it's a good idea to automate your most frequently used
workflows.

:::{note}
A **Unix shell** is a standardized way of interacting with a computer via text
commands---that is, via a command line. Unix shells are widely-used for
research computing. 

See DataLab's [Introduction to the Unix Command Line workshop
reader][datalab-shell] to get started with the command line and Unix shells.
:::

If you run your workflows in a Unix shell, one way you can automate them is by
writing **shell scripts**. A shell script is a plain-text file with a sequence
of Unix shell commands (akin to scripts for other languages, such as R or
Python). You can create separate shell script for each workflow that you run
frequently. The technical effort is low, since this just amounts to writing
down the commands that you already run in a text file. Besides making it easier
to run specific workflows, you can also share shell scripts with your
collaborators or other researchers. This helps to ensure that they also run the
workflow correctly.

:::{seealso}
See DataLab's and DIB Lab's [Automating Your Analyses and Executing
Long-running Analyses on Remote Computers workshop
reader][datalab-shell-scripts] for the technical details of writing shell
scripts.
:::

[datalab-shell]: https://ucdavisdatalab.github.io/workshop_introduction_to_the_command_line/
[datalab-shell-scripts]: https://ngs-docs.github.io/2021-august-remote-computing/automating-your-analyses-and-executing-long-running-analyses-on-remote-computers.html


Environment Management
----------------------

### Use an Environment Manager

A **computing environment** is a collection of hardware and software used to
run computations. Whether a computation runs correctly or at all depends on the
computing environment, so an important part of making a project reproducible is
documenting the environment where the code was originally developed and tested.

In high-level programming languages like R and Python, details of the hardware
are mostly hidden away. That is, hardware has little to no impact on how you
write code, with only a few exceptions. Hardware may affect how quickly your
code runs, but usually not the final result. As a consequence, for many
research computing projects, the hardware environment is of less concern than
the software environment.

One of the major advantages of the open-source and open-science ecosystem is
the trove of packages and other software developed and published by members of
the community. As these are updated, projects that rely on older versions may
cease to work correctly unless they are also updated. So for most projects,
keeping track of the software environment means keeping track of the specific
versions of packages and other software with which the project was carried out.

A **virtual environment** is a computing environment with specific software
versions that can coexist alongside other virtual environments with different
software versions. An **environment manager** is a software tool that can
create virtual environments. Environment managers can usually also install,
update, and remove software.

```{figure} ../img/xkcd_python_environment.png
---
name: xkcd-python
alt:
---
"Python Environment" from ["xkcd"][xkcd] by Randall Munroe
([license][xkcd-license]).
```

[xkcd]: https://xkcd.com/
[xkcd-license]: https://xkcd.com/license.html

Many different open-source environment managers exist. Among those explicitly
designed for research computing, [conda][] is the de facto standard. Conda was
originally developed to manage Python environments, but now supports other
languages such as R and Julia.

[conda]: https://docs.conda.io/

:::{note}
If you exclusively use R and prefer to use an environment manager developed
specifically for R, check out the [renv][] package.
:::

[renv]: https://rstudio.github.io/renv/

By using an environment manager like conda, you can ensure that the versions of
packages and software your projects depend on are recorded. Generally you
should create a new virtual environment for each project, and switch between
them as needed. Environment managers provide tools to save a description of a
virtual environment to a file and to recreate an environment from such a file.
This way you and your collaborators can set up a project on a new computer and
make sure it has the correct software environment.


:::{seealso}
See DataLab's [Making Python Projects & Environments Reproducible workshop
reader][datalab-conda] for a technical introduction to conda. Then see
DataLab's and DIB Lab's [Installing Software on Remote Computers with Conda
workshop reader][datalab-conda-remote] for even more technical details.
:::

[datalab-conda]: https://ucdavisdatalab.github.io/workshop_intermediate_python/chapters/02_reproducible.html
[datalab-conda-remote]: https://ngs-docs.github.io/2021-august-remote-computing/installing-software-on-remote-computers-with-conda.html
