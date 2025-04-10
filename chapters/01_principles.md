(introduction)=
# Principles of Reproducibility

A research project is **reproducible** if a different researcher can carry out
the same analysis with the same data and produce the same overall result. A
reproducible project is transparent about the research process and facilitates
independent verification, the gold standard for research. Best practices for
reproducible research can also make it easier to collaborate and to distribute
and reuse products of research.

:::{tip}
You always have at least one collaborator: future you! Work you did in the past
can be as challenging to navigate as work from someone else. So even if you
don't plan to collaborate with anyone else, it can still be helpful to adopt
good practices for reproducible and collaborative research.
:::

Almost all contemporary research projects involve computing---whether that
means creating and storing digital documents, developing code for small
analyses that run for a few minutes on a laptop, or developing code for large
analyses that run for a few days on a high-performance computing cluster.
Consequently, this reader focuses on practices related to computing. Many are
relevant regardless of whether your research involves writing lots of code.

One way to think about reproducibility is in terms of five principles:

1. **Documentation**: Get in the habit of writing documentation as you work.
   Most details should be documented, including:

    + The scope of the project, in terms of goals, milestones, and a timeline
    + Meetings---especially new tasks, decisions, and deferred discussions
    + Your thought process as you work: what references did you find, what
      approaches did you try, what results did you obtain, and what do you want
      to try in the future?
    + Data you collect, especially aspects which are not clearly documented
      elsewhere
    + Code you write (use comments!)
    + Any other files and directories you create

   Documentation can be for you, your collaborators, or other researchers
   interested in your work. Remember that not all documentation has to be for
   all of these people, and that even if some information is only for you, it's
   usually still a good idea to document it---it's easy to be too optimistic
   about what you'll remember later.

```{figure} /img/phd_scratch.gif
---
name: phd-scratch
alt:
---
"Scratch" from ["Piled Higher and Deeper"][phd] by Jorge Cham.
```

[phd]: https://www.phdcomics.com/

2. **Artifact preservation**: Every file you produce is an **artifact** of your
   research. Keep records so that you can revisit old results
   or even "rewind" a project to an earlier state. Some things you might want
   records of include:

   + Changes you make to (or different versions of) documents, code, and data
   + Settings and other inputs you use to run computations such as analyses,
     experiments, and simulations
   + Outputs from computations---especially outputs that are easy to overlook,
     such as diagnostic information printed while a computation runs

   You never know what you'll need later, and digital file storage is cheap.
   Keeping records is a big task, but there are many software tools available
   to automate most of the work.

```{figure} /img/xkcd_documents.png
---
figclass: margin
name: xkcd-documents
alt: "A comic of a person exclaiming, &quot;Oh my god,&quot; while looking at a
long list of &quot;untitled&quot; documents on another person's computer.
Caption: never look in someone else's documents folder."
---
"Documents" from ["xkcd"][xkcd] by Randall Munroe ([license][xkcd-license]).
```

[xkcd]: https://xkcd.com/
[xkcd-license]: https://xkcd.com/license.html

3. **Project organization**: Establish conventions for how you'll organize a
   project. Document these and then make sure to follow them. They should
   address details such as:

    + How you'll name things like files, functions, and modules
    + Which directories you'll create and which kinds of files belong in each
      directory
    + Whether your code should be in notebooks, scripts, or a combination of
      both
    + How you'll organize code using functions, classes, modules, and other
      programming abstractions
    + Which structures and file formats you'll use to store data
    + Whether you'll package your code or data for widespread distribution

    Try to cover the most common cases rather than every case, and leave some
    flexibility for ambiguous cases. Good conventions establish single,
    specific "correct" places for things to be, and encourage descriptive,
    self-documenting names. This makes data, code, and results easier to find,
    in contrast to the example in {numref}`Figure %s<xkcd-documents>`. Finding
    things quickly becomes especially important when collaborating with other
    people. Good conventions also encourage using programming abstractions,
    such as functions and modules, to make code easier to understand, verify,
    and reuse.

4. **Workflow automation**: Even when a project is well-documented and
   organized, reproducing particular result can be complicated: it may be
   necessary to run several different steps in a specific order and with
   specific settings. You can use programming languages and other workflow
   automation tools to automate a collection of steps, or **workflow**, so that
   it's easy to run. You can also use workflow automation tools to avoid doing
   tedious, repetitive tasks by hand and to run commands every time you take a
   particular action. For example, you might want to run tests on your code
   each time you upload it to share with collaborators. By using programming
   languages and other workflow automation tools, you create an unambiguous
   record of the steps in each computation and make it easier to reuse parts of
   a project.

5. **Environment management**: Make the hardware and software requirements, or
   **environment**, for your project explicit and as easy as possible to
   recreate. Environment management tools are especially helpful here, since
   they allow you to install different versions of software for different
   projects simultaneously, can record the software you install as you install
   it, and can recreate recorded environments. It's much easier to start
   working on a new machine, bring a new collaborator on board, or assist other
   people interested in using your project when you can deploy the required
   computing environment in just one or two commands.

All of these principles are important. You can make sure your projects are
reproducible by adopting various practices that each address one or more of
these principles. The principles are often symbiotic: a good practice for one
may also be a good practice for others.

Many different practices and tools exist, so it can be difficult to determine
which ones are the most important. In this reader, they're divided into three
groups:

1. **Primary practices** are the ones you should always adopt for every
   project. For example, you should always keep notes (documentation).
2. **Secondary practices** are the ones that are generally a good idea, but
   aren't relevant to every project. For example, if your project includes a
   lot of Python code, you should use an environment manager, but if your
   project doesn't include any code, you might not need one.
3. **Case-by-case practices** are the ones that have a high cost-to-benefit
   ratio, so you should consider the details of your project carefully before
   adopting them. For certain kinds of projects, they can be very important,
   but for others, not so much. For example, turning your code into a
   redistributable package is a great way to share it with others, but there's
   a cost in additional effort and technical complexity. It might not be worth
   it unless you expect to have many users.

Each of the next three chapters addresses one of these groups. The practices
were sorted into these groups based on the author's and DataLab's research
projects and experiences.

:::{note}
Most of the practices related to managing code are also software engineering
best practices and originated there. This reader attempts to translate the
software engineering jargon and perspective into something academic researchers
can understand and appreciate, while skipping over the parts that aren't
relevant to most research projects.
:::


<!--
On Collaboration
----------------
-->
