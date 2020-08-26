R
=

.. sidebar:: R

   :Dependencies: BLAS
   :URL: http://www.r-project.org/
   :Documentation: http://www.r-project.org/

R is a statistical computing language.

Interactive Usage
-----------------
After connecting to Iceberg (see :ref:`ssh`), :ref:`start an interactive session <sched_interactive>` then
load a specific version of R using one of the following: ::

    module load apps/R/3.3.1
    module load apps/R/3.3.0
    module load apps/R/3.2.4
    module load apps/R/3.2.3
    module load apps/R/3.2.2
    module load apps/R/3.2.1
    module load apps/R/3.2.0
    module load apps/R/3.1.2

R can then be run with ::

   $ R

Serial (one CPU) Batch usage
----------------------------

Here, we assume that you wish to run the program :code:`my_code.R` on the system.

First, you need to write a batch submission file.
We assume you'll call this ``my_job.sge``: ::

   #!/bin/bash
   #$ -cwd                                # Run job from current directory
   #$ -l rmem=4G                          # Request 4 gigabytes of memory

   module load apps/R/3.3.1

   R CMD BATCH my_code.r my_code.r.o$JOB_ID

Note that R must be called with both the ``CMD`` and ``BATCH`` options which tell it to run an R program,
in this case ``my_code.R``.
If you do not do this, R will attempt to open an interactive prompt.

The final argument, ``my_code.R.o$JOBID``,
tells R to send output to a file with this name.
Since ``$JOBID`` will always be unique,
this ensures that all of your output files are unique.
Without this argument R sends all output to a file called ``my_code.Rout``.

Ensuring that ``my_code.R`` and ``my_job.sge`` are both in your current working directory,
submit your job to the batch system ::

   qsub my_job.sge

Replace ``my_job.sge`` with the name of your submission script.

.. warning::
   By default, R will save variables in the workspace in the current directory
   (in a file called ``.RData``)
   when a script exits and reload them when it starts from the same directory.
   To disable this, add the ``--no-save`` and ``--no-restore`` options to your command
   e.g. ``R CMD BATCH --no-save --no-restore my_code.r my_code.r.o$JOB_ID``.

Graphical output
----------------

By default, graphical output from batch jobs is sent to a file called ``Rplots.pdf``.

Installing additional packages
------------------------------

As you will not have permissions to install packages to the default folder,
additional R packages can be installed to your home folder ``~/``.
To create the appropriate folder,
install your first package in R in interactive mode.
Load an interactive R session as described above, and install a package with: ::

   install.packages()

You will be prompted to create a personal package library.
Choose 'yes'.
The package will download and install from a CRAN mirror
(you may be asked to select a nearby mirror,
which you can do simply by entering the number of your preferred mirror).

Once the chosen package has been installed,
additional packages can be installed either in the same way,
or by creating a ``.R`` script.
An example script might look like: ::

   install.packages("dplyr")
   install.packages("devtools")

Call this using ``source()``.
For example if your script is called ``packages.R`` and is stored in your home folder,
source this from an interactive R session with: ::

   source("~/packages.R")

These additional packages will be installed without prompting to your personal package library.

To check your packages are up to date, and update them if necessary,
run the following line from an R interactive session ::

   update.packages(lib.loc = "~/R/x86_64-unknown-linux-gnu-library/3.3/")

The folder name after ``~/R/`` will likely change,
but this can be completed with tab autocompletion from the R session.
Ensure ``lib.loc`` folder is specified, or R will attempt to update the wrong library.

.. warning::
    Notice that the personal package library path includes the version of R:
    if after installing some packages you switch to using a different `major or minor version <http://semver.org/>`_ of R
    then you will need then to install those package *for this new version*.

R Packages that require external libraries
------------------------------------------

Some R packages require external libraries to be installed before you can install and use them.
Since there are so many, we only install those libraries that have been explicitly requested by users of the system.

The associated R packages are not included in the system install of R,
so you will need to install them yourself to your home directory following the instructions linked to below.

* :ref:`geos` This is the library required for the ``rgeos`` package.
* :ref:`gdal_iceberg` and :ref:`proj_iceberg` These are the libraries required for the ``rgdal`` package.
* :ref:`jags` This is the library required for the ``rjags`` and ``runjags`` packages

.. warning::
   To install R packages that require external libraries, the libraries need to be loaded prior to installing the R packages.
   E.g. to install package **rgeos** you would need to load ``geos``, enter an interactive R session and then install ``rgeos`` ::

      module load libs/geos/3.6.1/gcc-4.9.4
      R
      install.packages("rgeos")

   You may also need to ``module load`` those dependencies each time you *use* your R package.

   See :ref:`here <Libraries>` more information on the available external libraries

Using the Rmath library in C Programs
-------------------------------------

The Rmath library allows you to access some of R's functionality from a C program.
For example, consider this C program: ::

   #include <stdio.h>
   #define MATHLIB_STANDALONE
   #include "Rmath.h"

   main(){
      double shape1,shape2,prob;

      shape1 = 1.0;
      shape2 = 2.0;
      prob = 0.5;

      printf("Critical value is %lf\n",qbeta(prob,shape1,shape2,1,0));
   }

This makes use of R's ``qbeta`` function.
You can compile and run this on a worker node as follows.

Start an :ref:`interactive session <sched_interactive>` then load a version of of R using a module file: ::

   module load apps/R/3.3.0

Assuming the program is called ``test_rmath.c``, compile with ::

   gcc test_rmath.c -lRmath -lm -o test_rmath

For full details about the functions made available by the Rmath library,
see section 6.7 of the document `Writing R extensions <https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Numerical-analysis-subroutines>`_

Accelerated version of R
------------------------

There is an experimental, accelerated version of R installed on Iceberg that makes use of the :ref:`Intel compilers <iceberg_intel_compilers>` and the Intel MKL.
See :ref:`Intel R` for details.

Installation Notes
------------------
These notes are primarily for administrators of the system.

version 3.3.1
^^^^^^^^^^^^^

* `What's new in R version 3.3.1 <https://stat.ethz.ch/pipermail/r-announce/2016/000604.html>`_

Compiled from source with gcc 4.4.7 and with ``--enable-R-shlib`` enabled.
This build required several external modules including :ref:`xzutils`, :ref:`curl`, :ref:`bzip2` and :ref:`zlib`

* :download:`Installation script (install.sh) </iceberg/software/install_scripts/apps/R/3.3.1/install.sh>`: downloads, compiles, tests and installs R 3.3.1 and the ``Rmath`` library.
* Install log files, including the output of the ``make check`` tests are available on the system at ``/usr/local/packages6/R/3.3.1/install_logs``
* :download:`module file </iceberg/software/modulefiles/apps/R/3.3.1>` installed as ``/usr/local/modulefiles/apps/R/3.3.1``

version 3.3.0
^^^^^^^^^^^^^

* `What's new in R version 3.3.0 <https://stat.ethz.ch/pipermail/r-announce/2016/000602.html>`_

This was a scripted install. It was compiled from source with gcc 4.4.7 and with ``--enable-R-shlib`` enabled. You will need a large memory ``qrshx`` session in order to successfully run the build script. I used ``qrshx -l rmem=8G -l mem=8G``

This build required several external modules including :ref:`xzutils`, :ref:`curl`, :ref:`bzip2` and :ref:`zlib`

* :download:`install_R_3.3.0.sh </iceberg/software/install_scripts/apps/R/install_R_3.3.0.sh>` Downloads, compiles, tests and installs R 3.3.0 and the ``Rmath`` library.
* :download:`R 3.3.0 Modulefile </iceberg/software/modulefiles/apps/R/3.3.0>` located on the system at ``/usr/local/modulefiles/apps/R/3.3.0``
* Install log-files, including the output of the ``make check`` tests are available on the system at ``/usr/local/packages6/R/3.3.0/install_logs``

Version 3.2.4
^^^^^^^^^^^^^

* `What's new in R version 3.2.4 <https://cran.r-project.org/bin/windows/base/old/3.2.4/NEWS.R-3.2.4.html>`_

This was a scripted install. It was compiled from source with gcc 4.4.7 and with ``--enable-R-shlib`` enabled. You will need a large memory ``qrshx`` session in order to successfully run the build script. I used ``qrshx -l rmem=8G -l mem=8G``

This build made use of new versions of :ref:`xzutils` and :ref:`curl`

* :download:`install_R_3.2.4.sh </iceberg/software/install_scripts/apps/R/install_R_3.2.4.sh>` Downloads, compiles, tests and installs R 3.2.4 and the ``Rmath`` library.
* :download:`R 3.2.4 Modulefile </iceberg/software/modulefiles/apps/R/3.2.4>` located on the system at ``/usr/local/modulefiles/apps/R/3.2.4``
* Install log-files, including the output of the ``make check`` tests are available on the system at ``/usr/local/packages6/R/3.2.4/install_logs``

Version 3.2.3
^^^^^^^^^^^^^

* `What's new in R version 3.2.3 <https://cran.r-project.org/bin/windows/base/old/3.2.3/NEWS.R-3.2.3.html>`_

This was a scripted install. It was compiled from source with gcc 4.4.7 and with ``--enable-R-shlib`` enabled. You will need a large memory ``qrsh`` session in order to successfully run the build script. I used ``qrsh -l rmem=8G -l mem=16G``

* :download:`install_R_3.2.3.sh </iceberg/software/install_scripts/apps/R/install_R_3.2.3.sh>` Downloads, compiles, tests and installs R 3.2.3 and the ``Rmath`` library.
* :download:`R 3.2.3 Modulefile </iceberg/software/modulefiles/apps/R/3.2.3>` located on the system at ``/usr/local/modulefiles/apps/R/3.2.3``
* Install log-files, including the output of the ``make check`` tests are available on the system at ``/usr/local/packages6/R/3.2.3/install_logs``

Version 3.2.2
^^^^^^^^^^^^^

* `What's new in R version 3.2.2 <https://stat.ethz.ch/pipermail/r-announce/2015/000589.html>`_

This was a scripted install. It was compiled from source with gcc 4.4.7 and with ``--enable-R-shlib`` enabled. You will need a large memory ``qrsh`` session in order to successfully run the build script. I used ``qrsh -l rmem=8G -l mem=16G``

* :download:`install_R_3.2.2.sh </iceberg/software/install_scripts/apps/R/install_R_3.2.2.sh>` Downloads, compiles and installs R 3.2.2 and the ``Rmath`` library.
* :download:`R 3.2.2 Modulefile </iceberg/software/modulefiles/apps/R/3.2.2>` located on the system at ``/usr/local/modulefiles/apps/R/3.2.2``
* Install log-files were manually copied to ``/usr/local/packages6/R/3.2.2/install_logs`` on the system. This step should be included in the next version of the install script.

Version 3.2.1
^^^^^^^^^^^^^

This was a manual install. It was compiled from source with gcc 4.4.7 and with ``--enable-R-shlib`` enabled.

* :download:`Install notes </iceberg/software/install_scripts/apps/R/R-3.2.1.md>`
* :download:`R 3.2.1 Modulefile </iceberg/software/modulefiles/apps/R/3.2.1>` located on the system at ``/usr/local/modulefiles/apps/R/3.2.1``

Older versions
^^^^^^^^^^^^^^

Install notes for older versions of R are not available.
