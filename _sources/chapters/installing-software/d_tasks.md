# Unit D: Tasks

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Install packages from PyPI with Pixi
* Explain the purpose of `pyproject.toml`
* Create a `pyproject.toml` file
* Use Pixi with `pyproject.toml` instead of `pixi.toml`
:::

## Tasks

In addition to being a package and environment manager, Pixi is also a simple
**task runner**: a tool that can run other commands in a convenient way. Tasks
make the functionality or workflow of your project easier for other people to
discover---especially when paired with detailed documentation---and lower the
cognitive burden on you and your collaborators to remember the syntax for
specific commands.

You can create tasks with the `pixi task` command or by editing the `[tasks]`
table in `pixi.toml`. In `pixi.toml`, tasks take this form:

```toml
task_name = { cmd = "commands_to_run", description = "Help for the task." }
```

The `description` field is optional and can be omitted, but it's a good habit
to document your tasks.

As an example, we use these tasks in the project for this reader:

```toml
[tasks]
build = { cmd = "jupyter-book build .", description = "Build the reader." }
publish = { cmd = "ghp-import --no-jekyll --no-history --push _build/html", description = "Publish the reader to the `gh-pages` branch on GitHub." }
clean = { cmd = "rm -rf _build/", description = "Remove the build directory." }
rebuild = { depends-on = ["clean", "build"], description = "Remove the build directory and build the reader." }
```

The `build` task generates the reader from source files, the `publish` task
uploads the reader to the web, and the `clean` task deletes the built reader
(in case we want to rebuild it). The `rebuild` task is special: instead of a
command to run, it specifies that it `depends-on` the `clean` and `build`
tasks. So Pixi will run both of those tasks when you run `rebuild`.

You can run tasks with the `pixi run` command and the name of the task. So in
the project for this reader, we can run the `build` task with this command:

```sh
pixi run build
```

:::{caution}
Be careful not to give tasks names that conflict with other tools you use. For
instance, `python` is probably not a good name for a task.
:::
