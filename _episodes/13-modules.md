---
title: "Using a cluster: Accessing software"
teaching: 20
exercises: 15
questions:
- "How do we load and unload software packages?"
objectives:
- "Understand how to load and use a software package."
keypoints:
- "Load software with `module load softwareName`"
- "The module system handles software versioning and package conflicts for you automatically."
- "When using software on any HPC system, check the software documents for details on how to use it effectively."
---

On a high-performance computing system, no software is loaded by default.
If we want to use a software package, we will need to "load" it ourselves.

Before we start using individual software packages, however, 
we should understand the reasoning behind this approach.
The two biggest factors are software incompatibilities and versioning.

Software incompatibility is a major headache for programmers.
Sometimes the presence (or absence) 
of a software package will break others that depend on it.
Two of the most famous examples are Python 2 and 3 and C compiler versions.
Python 3 famously provides a `python` command 
that conflicts with that provided by Python 2. 
Software compiled against a newer version of the C libraries 
and then used when they are not present will result in a nasty
`'GLIBCXX_3.4.20' not found` error, for instance. 

Software versioning is another common issue.
A team might depend on a certain package version for their research project - 
if the software version was to change (for instance, if a package was updated),
it might affect their results.
Having access to multiple software versions allow a set of researchers to take 
software version out of the equation if results are weird.

## Environment modules (Lmod)

Environment modules are the solution to these problems.
A module is a self-contained software package - 
it contains all of the files required to run a software packace 
and loads required dependencies.

To see available software modules, use `module avail`

```
module avail
```
{: .bash}
```
------------------------------------------------------------------------- Packages installed manually by HPRC staff -------------------------------------------------------------------------
   ABAQUS/2016                                                   LIGGGHTS-WITH-BONDS/3.3.0          myPython/3.5.2-intel-2017A
   ABAQUS/2017                                                   LS-DYNA/R8.1.0                     myPython/3.5.2-iomkl-2017A                        (D)
   ABAQUS/2018                                            (D)    LS-DYNA/R9.1.0                     myR/3.3.2-iomkl-2017A-Python-2.7.12-default-mt
   Anaconda-Jupyter/3-5.0.0.1                                    LS-DYNA/R9.2.0                     NAMD/2.12-icc-fftw-multicore-CUDA-hprc
   Anaconda/2-4.3.1                                              LS-DYNA/R10.1.0                    NAMD/2.12-intel-2017A-hprc
   Anaconda/2-5.0.1                                              LS-DYNA/R10.2.0             (D)    NAMD/2.12-iompi-2017A-fftw-hprc
   Anaconda/3-4.2.0                                              LS-OPT/5.2.1-r107348               NAMD/2.12-multicore-CUDA
   Anaconda/3-5.0.0.1                                     (D)    LS-OPT/5.2.1-r107511        (D)    NAMD/2.12-multicore
   AnsysEM/19.1                                                  LS-PREPOST/4.5                     NAMD/2.12-TCP
   AnsysEM/19.2                                           (D)    LS-PREPOST/4.6              (D)    NAMD/2.12-UDP                                     (D)
   CESM/1.2.2-intel-2017A                                        LS-TASC/3.2-110810                 OpenMPI/99999999                                  (D)
   ClusterFlow/0.5                                               LS-TASC/3.2-112069                 ORCA-HPRC-License/0
   ConvergeCFD/2.3.6                                             LS-TASC/3.2-118423          (D)    Perl_tamu/5.24.0-GCCcore-6.3.0
   curc-bench-terra/0                                            matcaffe/1.0-R2017a                PhyloNetworks/0.7.0-intel-2017A-Python-2.7.12
   devel/0                                                       matcaffe/1.0-R2017b                Python/99999999                                   (D)

   ... (too many modules) 

   D:        Default Module

     Use "module spider" to find all possible modules.
     Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".

     ****************** TAMU HPRC USERS ******************

     Please see our cluster user guides at:

       https://hprc.tamu.edu/wiki/index.php/HPRC:Systems

     Also, please see our currently defined toolchains at:

       https://hprc.tamu.edu/wiki/index.php/SW:Toolchains

     ****************** TAMU HPRC USERS ******************

```
{: .output}

## Loading and unloading software

To load a software module, use `module load`.
In this example we will use Python 3.

Intially, Python 3 is not loaded. 

```
which python3
```
{: .bash}
```
/usr/bin/which: no python3 in /usr/bin/which: no python3 in (/opt/mvapich2/intel/18.0/2.3/bin:/usr/local/gnu/7.3.0/bin:/opt/intel/itac/2018.3.022/bin:/opt/intel/advisor_2018/bin
64:/opt/intel/vtune_amplifier_2018/bin64:/opt/intel/inspector_2018/bin64:/opt/intel/compilers_and_libraries_2018.3.222/linux/bin/intel64:/opt/torque/bin:/usr/lib64/qt-3.3/bin:/opt/osc/bin:/opt/moab/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/ibutils/bin:/opt/ddn/ime/bin:/opt/pup
petlabs/bin)
```
{: .output}

We can load the `Python/3.7.0-intel-2018b` command with `module load`.

```
module load Python/3.7.0-intel-2018b
which python3
```
{: .bash}
```
/sw/eb/sw/Python/3.7.0-intel-2018b/bin/python3
```
{: .output}

So what just happened?

To understand the output, first we need to understand the nature of the 
`$PATH` environment variable.
`$PATH` is a special ennvironment variable that controls where a UNIX system looks for software.
Specifically `$PATH` is a list of directories (separated by `:`)
that the OS searches through for a command before giving up and telling us it can't find it.
As with all environment variables we can print it out using `echo`.

```
echo $PATH
```
{: .bash}
```
/sw/eb/sw/Python/3.7.0-intel-2018b/bin:/sw/eb/sw/XZ/5.2.4-GCCcore-7.3.0/bin:/sw/eb/sw/SQLite/3.24.0-GCCcore-7.3.0/bin:/sw/eb/sw/Tcl/8.6.8-GCCcore-7.3.0/bin:/sw/eb/sw/libreadline/7.0-GCCcore-7.3.0/bin:/sw/eb/sw/ncurses/6.1-GCCcore-7.3.0/bin:/sw/eb/sw/bzip2/1.0.6-GCCcore-7.3.0/bin:/sw/eb/sw/imkl/2018.3.222-iimpi-2018b/mkl/bin:/sw/eb/sw/imkl/2018.3.222-iimpi-2018b/bin:/sw/eb/sw/impi/2018.3.222-iccifort-2018.3.222-GCC-7.3.0-2.30/bin64:/sw/eb/sw/ifort/2018.3.222-GCC-7.3.0-2.30/compilers_and_libraries_2018.3.222/linux/bin/intel64:/sw/eb/sw/icc/2018.3.222-GCC-7.3.0-2.30/compilers_and_libraries_2018.3.222/linux/bin/intel64:/sw/eb/sw/binutils/2.30-GCCcore-7.3.0/bin:/sw/eb/sw/GCCcore/7.3.0/bin:/sw/local/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/lpp/mmfs/bin:/home/pingluo/.local/bin:/home/pingluo/bin
```
{: .output}

You'll notice a similarity to the output of the `which` command. 
In this case, there's only one difference:
the `/sw/eb/sw/Python/3.7.0-intel-2018b/bin` directory at the beginning.
When we ran `module load python/3.5.2`, 
it added this directory to the beginning of our `$PATH`.
Let's examine what's there:

```
ls /sw/eb/sw/Python/3.7.0-intel-2018b/bin
```
{: .bash}
```
2to3        cygdb      easy_install      idle3    nosetests      pip     pydoc3    python3           python3.7m         pyvenv      tabulate
2to3-3.7    cython     easy_install-3.7  idle3.7  nosetests-3.7  pip3    pydoc3.7  python3.7         python3.7m-config  pyvenv-3.7  virtualenv
chardetect  cythonize  f2py              netaddr  pbr            pip3.7  python    python3.7-config  python3-config     runxlrd.py
```
{: .output}

Taking this to it's conclusion, `module load` will add software to your `$PATH`.
It "loads" software.
A special note on this - 
depending on which version of the `module` program that is installed at your site,
`module load` will also load required software dependencies.
To demonstrate, let's use `module list`.
`module list` shows all loaded software modules.

```
module list
```
{: .bash}
```
Currently Loaded Modules:
Currently Loaded Modules:
  1) GCCcore/7.3.0                        6) impi/2018.3.222-iccifort-2018.3.222-GCC-7.3.0-2.30  11) zlib/1.2.11-GCCcore-7.3.0      16) XZ/5.2.4-GCCcore-7.3.0
  2) binutils/2.30-GCCcore-7.3.0          7) iimpi/2018b                                         12) ncurses/6.1-GCCcore-7.3.0      17) GMP/6.1.2-GCCcore-7.3.0
  3) icc/2018.3.222-GCC-7.3.0-2.30        8) imkl/2018.3.222-iimpi-2018b                         13) libreadline/7.0-GCCcore-7.3.0  18) libffi/3.2.1-GCCcore-7.3.0
  4) ifort/2018.3.222-GCC-7.3.0-2.30      9) intel/2018b                                         14) Tcl/8.6.8-GCCcore-7.3.0        19) Python/3.7.0-intel-2018b
  5) iccifort/2018.3.222-GCC-7.3.0-2.30  10) bzip2/1.0.6-GCCcore-7.3.0                           15) SQLite/3.24.0-GCCcore-7.3.0 
```
{: .output}


```
module unload Python/3.7.0-intel-2018b
module list
```
{: .bash}
```
Currently Loaded Modules:
  1) GCCcore/7.3.0                        6) impi/2018.3.222-iccifort-2018.3.222-GCC-7.3.0-2.30  11) zlib/1.2.11-GCCcore-7.3.0      16) XZ/5.2.4-GCCcore-7.3.0
  2) binutils/2.30-GCCcore-7.3.0          7) iimpi/2018b                                         12) ncurses/6.1-GCCcore-7.3.0      17) GMP/6.1.2-GCCcore-7.3.0
  3) icc/2018.3.222-GCC-7.3.0-2.30        8) imkl/2018.3.222-iimpi-2018b                         13) libreadline/7.0-GCCcore-7.3.0  18) libffi/3.2.1-GCCcore-7.3.0
  4) ifort/2018.3.222-GCC-7.3.0-2.30      9) intel/2018b                                         14) Tcl/8.6.8-GCCcore-7.3.0
  5) iccifort/2018.3.222-GCC-7.3.0-2.30  10) bzip2/1.0.6-GCCcore-7.3.0                           15) SQLite/3.24.0-GCCcore-7.3.0
```
{: .output}

So using `module unload` "un-loads" a module along with it's dependencies.
If we wanted to unload everything at once, we could run `module purge` (unloads everything).


## Software versioning

So far, we've learned how to load and unload software packages.
This is very useful.
However, we have not yet addressed the issue of software versioning.
At some point or other, 
you will run into issues where only one particular version of some software will be suitable.
Perhaps a key bugfix only happened in a certain version, 
or version X broke compatibility with a file format you use.
In either of these example cases, 
it helps to be very specific about what software is loaded.

Let's examine the output of `module avail` more closely.

```
module avail
```
{: .bash}
```
------------------------------------------------------------------------- Packages installed manually by HPRC staff -------------------------------------------------------------------------
   ABAQUS/2016                                                   LIGGGHTS-WITH-BONDS/3.3.0          myPython/3.5.2-intel-2017A
   ABAQUS/2017                                                   LS-DYNA/R8.1.0                     myPython/3.5.2-iomkl-2017A                        (D)
   ABAQUS/2018                                            (D)    LS-DYNA/R9.1.0                     myR/3.3.2-iomkl-2017A-Python-2.7.12-default-mt
   Anaconda-Jupyter/3-5.0.0.1                                    LS-DYNA/R9.2.0                     NAMD/2.12-icc-fftw-multicore-CUDA-hprc
   Anaconda/2-4.3.1                                              LS-DYNA/R10.1.0                    NAMD/2.12-intel-2017A-hprc
   Anaconda/2-5.0.1                                              LS-DYNA/R10.2.0             (D)    NAMD/2.12-iompi-2017A-fftw-hprc
   Anaconda/3-4.2.0                                              LS-OPT/5.2.1-r107348               NAMD/2.12-multicore-CUDA
   Anaconda/3-5.0.0.1                                     (D)    LS-OPT/5.2.1-r107511        (D)    NAMD/2.12-multicore
   AnsysEM/19.1                                                  LS-PREPOST/4.5                     NAMD/2.12-TCP
   AnsysEM/19.2                                           (D)    LS-PREPOST/4.6              (D)    NAMD/2.12-UDP                                     (D)
   CESM/1.2.2-intel-2017A                                        LS-TASC/3.2-110810                 OpenMPI/99999999                                  (D)
   ClusterFlow/0.5                                               LS-TASC/3.2-112069                 ORCA-HPRC-License/0
```
{: .output}

Software with multiple versions will be marked with a (D). The best way to be sure you understand how certain software is
used on our clusters is to look at the software page on our website. [https://hprc.tamu.edu/wiki/SW](https://hprc.tamu.edu/wiki/SW).

> ## Using software modules in scripts
>
> Create a job that is able to run `python3 --version`.
> Remember, no software is loaded by default!
> Running a job is just like logging on to the system 
> (you should not assume a module loaded on the login node is loaded on a compute node).
{: .challenge}

## Installing software of our own

>The following tutorial is from our HOWTO guide on our website. We have many more useful guides there, take 
>a look at [https://hprc.tamu.edu/wiki/SW](https://hprc.tamu.edu/wiki/SW).
>
{: .callout}

Most HPC clusters have a pretty large set of preinstalled software.
Nonetheless, it's unlikely that all of the software we'll need will be available.
Sooner or later, we'll need to install some software of our own. 

Though software installation differs from package to package,
the general process is the same:
download the software, read the installation instructions (important!),
install dependencies, compile, then start using our software.

### Getting Started
Before installing your software, you should first prepare a place for it to live. We recommend the following directory structure, which you should create in the top-level of your scratch directory:

```
    local
    |-- src
    |-- share
        `-- lmodfiles
```
{: .bash}

Each directory serves a specific purpose:

| directory | description |
| --- | --- |
|local  |Gathers all the files related to your local installs into one directory, rather than cluttering your home directory. Applications will be installed into this directory with the format "appname/version". This allows you to easily store multiple versions of a particular software install if necessary.|
|local/src | Stores the installers -- generally source directories -- for your software. Also, stores the compressed archives ("tarballs") of your installers; useful if you want to reinstall later using different build options.|
|local/share/lmodfiles  |The standard place to store module files, which will allow you to dynamically add or remove locally installed applications from your environment.|

You can create this structure with one command.

After navigating to where you want to create the directory structure, run:

```
    mkdir -p $SCRATCH/local/src $SCRATCH/local/share/lmodfiles
```
{: .bash}


### Installing Software
Now that you have your directory structure created, you can install your software. For demonstration purposes, we will install a local copy of Git.

First, we need to get the source code onto the HPC filesystem. The easiest thing to do is find a download link, copy it, and use the wget tool to download it on the HPC. We'll download this into $SCRATCH/local/src:

```
    cd $SCRATCH/local/src
    wget https://github.com/git/git/archive/v2.9.0.tar.gz
```
{: .bash}


Now extract the tar file:

```
    tar zxvf v2.9.0.tar.gz
```
{: .bash}

Next, we'll go into the source directory and build the program. Consult your application's documentation to determine how to install into `$SCRATCH/local/"software_name"/"version"`. Replace "software_name" with the software's name and "version" with the version you are installing, as demonstrated below. In this case, we'll use the configure tool's --prefix option to specify the install location.

You'll also want to specify a few variables to help make your application more compatible with our systems. We recommend specifying that you wish to use the Intel compilers and that you want to link the Intel libraries statically. This will prevent you from having to have the Intel module loaded in order to use your program. To accomplish this, add `CC=icc CFLAGS=-static-intel` to the end of your invocation of configure. If your application does not use configure, you can generally still set these variables somewhere in its Makefile or build script.

Then, we can build Git using the following commands:

```
    cd git-2.9.0
    autoconf # this creates the configure file
    ./configure --prefix=$SCRATCH/local/git/2.9.0 CC=icc CFLAGS=-static-intel
    make && make install
```
{: .bash}

We've successfully installed our first piece of software!

Now create a module file for your software 
```
cd $SCRATCH/local/share/lmodfiles
mkdir git
touch git/2.9.0.lua
```
{: .bash}
Copy the following content to `git/2.9.0.lua`.
```
help([[
This module loads the git software environment.
]])

whatis([[Description: git software environment]])

local root = pathJoin(os.getenv("SCRATCH"),"/local/git/2.9.0")
local bin = pathJoin(root, "/bin")

if not isloaded("intel/2018b") then
load("intel/2018b")
end

prepend_path("PATH", bin)
```
{: .output}

Add your moduel path to the LMOD module search path.
```
module use "$SCRATCH/local/share/lmodfiles
```
{: .bash}

Now you can load the module you just created and use git you just built.
```
module load git/2.9.0
module list
```
{: .bash}
```
Currently Loaded Modules:
  1) GCCcore/7.3.0                 3) icc/2018.3.222-GCC-7.3.0-2.30     5) iccifort/2018.3.222-GCC-7.3.0-2.30                   7) iimpi/2018b                   9) intel/2018b
  2) binutils/2.30-GCCcore-7.3.0   4) ifort/2018.3.222-GCC-7.3.0-2.30   6) impi/2018.3.222-iccifort-2018.3.222-GCC-7.3.0-2.30   8) imkl/2018.3.222-iimpi-2018b  10) git/2.9.0
```
{: .output}
