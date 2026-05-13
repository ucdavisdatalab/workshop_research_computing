# Unit D: R Packages

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Install packages for R with Pixi
* Explain the advantages and disadvantages of using Pixi with R
* Describe alternatives to Pixi for managing R packages and the R environment
:::

This unit explains how to use Pixi to manage packages in R projects, the
current state of Pixi's support for R and its limitations, and alternatives to
Pixi for R.

:::{seealso}
As a complement to this unit, see DataLab's [R Basics workshop
reader][r-basics].

[r-basics]: https://ucdavisdatalab.github.io/workshop_r_basics/
:::


## R Packages

Pixi (and Conda) were developed with Python in mind. The developers have since
expanded the scope to include other research computing software, such as R.
Support for R is still a work in progress, but Pixi is already usable for most
R projects.

:::{seealso}
To learn more about the status of R support in Pixi, see [this GitHub
issue][pixi-r].

[pixi-r]: https://github.com/prefix-dev/pixi/issues/1543
:::

To install R, use `pixi add` to add the `r` package. This will also install all
of R's dependencies.

Instead of using R's `install.packages` function, you can use `pixi add` to
install individual R packages. This has the benefit of ensuring that the
packages are recorded in your project's `pixi.toml`. Pixi will also
automatically install system-level dependencies of the package (packages for
working with other software, databases, and geospatial data often have these).
R package names begin with the prefix `r-`, so for example, the Tidyverse
package is `r-tidyverse` and the ggplot2 package is `r-ggplot2`.

Conda-forge provides many of the packages on the [Comprehensive R Archive
Network][cran] (CRAN). These are maintained semi-manually by volunteers, so a
few packages are missing, and versions sometimes lag behind CRAN. Conda-forge
is also missing many packages that aren't on CRAN but are part of the wider [R
Universe][r-univ]. 

[cran]: https://cran.r-project.org/
[r-univ]: https://r-universe.dev/

If you need to install an R package that isn't available on conda-forge, you
can:

* Install the package with R's `install.packages` function (or similar). The
  package will not be recorded in `pixi.toml`. If the package has any
  system-level dependencies, make sure to install them first with `pixi add`.
* Contribute the package to conda-forge yourself. The conda-forge
  administrators provide [instructions for contributing a
  package][conda-forge-ctb]. You don't need to be the developer of the package
  to do this, although it's polite to notify them.
* Use an R environment management package (see {ref}`sec-alternatives-to-pixi`)
  instead of or together with Pixi.

[conda-forge-ctb]: https://conda-forge.org/docs/maintainer/adding_pkgs/


:::{important}
R uses three environment variables (see {ref}`environment-variables`) to
determine where to search for installed packages: `R_LIBS`, `R_LIBS_USER`, and
`R_LIBS_SITE`. When we run R in a Pixi environment, we want R to find only the
packages that are part of that environment. To achieve this, we need to set the
three `R_LIBS` variables.

Pixi will automatically set any environment variables defined in the
`[activation.env]` table of `pixi.toml` when the environment is started. So to
set the `R_LIBS` variables, add this to `pixi.toml`:

```toml
[activation.env]
# Make sure R only searches for packages within the Pixi environment.
PIXI_R_LIBS = "$CONDA_PREFIX/lib/R/library"
R_LIBS = "$PIXI_R_LIBS"
R_LIBS_USER = "$PIXI_R_LIBS"
R_LIBS_SITE = "$PIXI_R_LIBS"
```

The `CONDA_PREFIX` variable is automatically set by Pixi when the environment
is started. It refers to the directory where the environment's packages are
installed. The `PIXI_R_LIBS` variable is just a convenience for setting the
other three---it isn't used by Pixi or R.

If you want to use [Quarto][] in a Pixi environment, it's also necessary to set
the `QUARTO_R` environment variable to ensure Quarto finds R in the environment
rather than some other copy of R on your computer. To do this, append this line
to the `[activation.env]` table:

```
QUARTO_R = "$CONDA_PREFIX/bin/R"
```

[Quarto]: https://quarto.org/
:::


## Alternatives to Pixi
