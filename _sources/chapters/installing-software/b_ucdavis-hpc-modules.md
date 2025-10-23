# Unit B: UC Davis HPC Modules

:::{admonition} Learning Goals
:class: note
After this lesson, you should be able to:

* Explain what HPC modules are
* Determine which software a module contains
* Load a module
* Unload a module
:::


On most of UC Davis' high-performance computing (HPC) clusters, the HPC Core
Facility staff maintain a collection of ready-to-use virtual environments
called **modules**. These are an alternative to setting up environments
yourself. Other clusters and servers may have a similar system, but many do
not. This unit explains the UC Davis HPC module system.

:::{seealso}
As a complement to this unit, see DataLab's [Introduction to Remote Computing
workshop reader][intro-remote].

[intro-remote]: https://ucdavisdatalab.github.io/workshop_intro_to_remote_computing/
:::


## The Module System

:::{note}
You don't have to use modules just because you're using the HPC clusters. If
you prefer to use an environment manager, that's totally fine. 
:::

You can list all of the modules available on a cluster with the `module avail`
command. For example, as of writing, the output from `module avail` on UC
Davis' Farm cluster begins:

```
----------------------------------------------------------------- /share/apps/22.04/modulefiles/spack/core -----------------------------------------------------------------
cuda/11.7.1  gcc/5.5.0  hwloc/2.9.3                      jdk/17.0.1   julia/1.9.0  julia/1.9.3      nvhpc/23.1         openjdk/16.0.2  pigz/2.7    slurm/23.02.6
gcc/4.9.4    gcc/7.5.0  intel-oneapi-compilers/2022.2.1  julia/1.8.5  julia/1.9.2  libevent/2.1.12  openjdk/11.0.17_8  openmpi/4.1.5   pmix/4.2.6  ucx/1.14.1

----------------------------------------------------------------- /share/apps/22.04/modulefiles/spack/libs -----------------------------------------------------------------
amdfftw/3.2   bzip2/1.0.8  eigen/3.4.0  gmp/6.2.1  h5z-zfp/1.1.0  hypre/2.26.0               librsvg/2.51.0  mpfr/4.1.0      netcdf-cxx4/4.3.1     nvhpc/23.1  zlib/1.2.13
boost/1.80.0  cgal/5.4.1   fftw/3.3.10  gsl/2.7.1  hdf5/1.12.2    intel-oneapi-mkl/2022.2.1  libszip/2.1.1   netcdf-c/4.9.0  netcdf-fortran/4.6.0  pigz/2.7
```

Modules are displayed in groups defined by the server administrators. Each
module has a name and a version number, separated by a slash. For instance,
`julia/1.9.3` is the `julia` module, version `1.9.3`.

To get help with a module, use the `module help` command with the module's
name and optionally its version. For example, to get help with the `julia`
module:

```sh
module help julia
```

```
-------------------------------------------------------------------
Module Specific Help for /share/apps/22.04/modulefiles/spack/core/julia/1.9.3:

The Julia Language: A fresh approach to technical computing
-------------------------------------------------------------------
```

To load a module, use the `module load` command. Again, you need to specify the
module's name and optionally its version. As an example, suppose you decide you
want to try out the [Julia][] programming language on Hive. If you try to run
`julia`, you'll see an error message saying it's not installed:

[Julia]: https://julialang.org/

```sh
julia
```

```
julia: command not found
```

You can load the `julia` module to switch to an environment where Julia is
installed:

```sh
module load julia
```

Now running `julia` will open the Julia interpreter (you can exit Julia by
typing `exit()` and pressing enter).

You can check which modules are loaded at any given time with the `module list`
command:

```sh
module list
```

```
Currently Loaded Modulefiles:
 1) slurm/23.02.6   2) openmpi/4.1.5
```

You might see different output if you've loaded or unloaded modules.

Finally, to unload a module, you can use the `module unload` command with the
module's name:

```sh
module unload julia
```

Note that when you log out, the cluster will automatically unload any modules
you loaded.


