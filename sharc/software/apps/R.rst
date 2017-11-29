.. _sharc_r:

R
=

.. sidebar:: R

   :URL: http://www.r-project.org/
   :Documentation: http://www.r-project.org/

R is a statistical computing language.

Interactive Usage
-----------------
After connecting to ShARC, start an interactive session with the :ref:`qrshx` command.

The latest version of R can be loaded with ::

        module load apps/R

Alternatively, you can load a specific version of R using one of the following ::
        
        module load apps/R/3.4.0/gcc-4.8.5
        module load apps/R/3.3.3/gcc-4.8.5
        module load apps/R/3.3.2/gcc-4.8.5

R can then be run with ::

        $ R

Serial (one CPU) Batch usage
----------------------------
Here, we assume that you wish to run the program ``my_code.r`` on the system. With batch usage it is recommended to load a specific version of R, for example ``module load apps/R/3.4.0``, to ensure the expected output is achieved.

First, you need to write a batch submission file. We assume you'll call this ``my_job.sge`` ::

  #!/bin/bash
  #$ -cwd                                # Run job from current directory
  #$ -l rmem=4G                          # Request 4 gigabytes of memory

  module load apps/R/3.4.0/gcc-4.8.5     # Recommended to load a specific version of R

  R CMD BATCH my_code.r my_code.r.o$JOB_ID

Note that R must be called with both the ``CMD`` and ``BATCH`` options which tell it to run an R program, in this case ``my_code.r``. If you do not do this, R will attempt to open an interactive prompt.

The final argument, ``my_code.r.o$JOBID``, tells R to send output to a file with this name. Since ``$JOBID`` will always be unique, this ensures that all of your output files are unique. Without this argument R sends all output to a file called ``my_code.Rout``.

Ensuring that ``my_code.r`` and ``my_job.sge`` are both in your current working directory, submit your job to the batch system ::

	qsub my_job.sge

Replace ``my_job.sge`` with the name of your submission script.

Graphical output
----------------
By default, graphical output from batch jobs is sent to a file called `Rplots.pdf`

Installing additional packages
------------------------------

As you will not have permissions to install packages to the default folder, additional R packages can be installed to your home folder `~/`. To create the appropriate folder, install your first package in R in interactive mode. Load an interactive R session as described above, and install a package with ::

        install.packages()

You will be prompted to create a personal package library. Choose yes. The package will download and install from a CRAN mirror (you may be asked to select a nearby mirror, which you can do simply by entering the number of your preferred mirror).

Once the chosen package has been installed, additional packages can be installed either in the same way, or by creating a .R script. An example script might look like ::

        install.packages("dplyr")
        install.packages("devtools")

Call this using `source()`. For example if your script is called `packages.R` and is stored in your home folder, source this from an interactive R session with ::

        source("~/packages.R")

These additional packages will be installed without prompting to your personal package library.

To check your packages are up to date, and update them if necessary, run the following line from an R interactive session ::

        update.packages(lib.loc = "~/R/x86_64-unknown-linux-gnu-library/3.4/")

The folder name after :code:`~/R/` will likely change, but this can be completed with tab autocompletion from the R session. Ensure ``lib.loc`` folder is specified, or R will attempt to update the wrong library.

.. warning::
    Notice that the personal package library path includes the version of R:
    if after installing some packages you switch to using a different `major or minor version <http://semver.org/>`_ of R
    then you will need then to install those package *for this new version*.

R Packages that require external libraries
------------------------------------------
Some R packages require external libraries to be installed before you can install and use them. Since there are so many, we only install those libraries that have been explicitly requested by users of the system.

The associated R packages are not included in the system install of R, so you will need to install them yourself to your home directory following the instructions linked to below.

* :ref:`geos_sharc` This is the library required for the ``rgeos`` package.
* :ref:`gdal_sharc` and :ref:`proj_sharc` These are the libraries required for the ``rgdal`` package.

.. warning::
	To install r packages that require external libraries, the libraries need to be loaded prior to installing the r packages. 
	E.g. to install package **rgeos** you would need to load ``geos``, enter an interactive R session and then install ``rgeos`` ::
	
		module load libs/geos/3.6.1/gcc-4.9.4
		R
		install.packages("rgeos")
		
	See :ref:`here <Libraries>` more information on the available external libraries
				

Using the Rmath library in C Programs
-------------------------------------
The Rmath library allows you to access some of R's functionality from a C program. For example, consider the C-program below ::

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

This makes use of R's ``qbeta`` function. You can compile and run this on a worker node as follows.

Start a session on a worker node with ``qrsh`` or ``qrshx`` and load the R module ::

    module load apps/R/3.4.0/gcc-4.8.5

Assuming the program is called ``test_rmath.c``, compile with ::

    gcc test_rmath.c -lRmath -lm -o test_rmath

For full details about the functions made available by the Rmath library, see section 6.7 of the document `Writing R extensions <https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Numerical-analysis-subroutines>`_

Versions of R with faster linear algebra
----------------------------------------
We have compiled versions of R using the Intel Compiler suite and the Intel MKL. These can be faster than this 'standard' version in some cases. For more details see :ref:`Intel R (Sharc)`

Installation Notes
------------------
These notes are primarily for administrators of the system.

**version 3.4.0**

* `What's new in R version 3.4.0 <https://stat.ethz.ch/pipermail/r-announce/2017/000612.html>`_ 

This was a scripted install. It was compiled from source with gcc 4.8.5 and with ``--enable-R-shlib`` enabled. It was run in batch mode.

* :download:`install_r_3.4.0_gcc4.8.5.sh </sharc/software/install_scripts/apps/R/3.4.0/gcc-4.8.5/install_r_3.4.0_gcc4.8.5.sh>` Downloads, compiles, tests and installs R 3.4.0 and the ``Rmath`` library.
* :download:`R 3.4.0 Modulefile </sharc/software/modulefiles/apps/R/3.4.0/gcc-4.8.5>` located on the system at ``/usr/local/modulefiles/apps/R/3.4.0/``
* Install log-files, including the output of the `make check` tests are available on the system at `/usr/local/packages/apps/R/3.4.0/gcc-4.8.5/install_logs/`

**version 3.3.3**

* `What's new in R version 3.3.3 <https://stat.ethz.ch/pipermail/r-help//2017-March/445277.html>`_

This was a scripted install. It was compiled from source with gcc 4.8.5 and with ``--enable-R-shlib`` enabled. It was run in batch mode.

* :download:`install_r_3.3.3_gcc4.8.5.sh </sharc/software/install_scripts/apps/R/3.3.3/gcc-4.8.5/install_r_3.3.3_gcc4.8.5.sh>` Downloads, compiles, tests and installs R 3.3.3 and the ``Rmath`` library.
* :download:`R 3.3.3 Modulefile </sharc/software/modulefiles/apps/R/3.3.3/gcc-4.8.5>` located on the system at ``/usr/local/modulefiles/apps/R/3.3.3/``
* Install log-files, including the output of the `make check` tests are available on the system at `/usr/local/packages/apps/R/3.3.3/gcc-4.8.5/install_logs/`

**version 3.3.2**

* `What's new in R version 3.3.2 <https://stat.ethz.ch/pipermail/r-announce/2016/000608.html>`_

This was a scripted install. It was compiled from source with gcc 4.8.5 and with ``--enable-R-shlib`` enabled. It was run in batch mode.

* :download:`install_r_3.3.2_gcc4.8.5.sh </sharc/software/install_scripts/apps/R/3.3.2/gcc-4.8.5/install_r_3.3.2_gcc4.8.5.sh>` Downloads, compiles, tests and installs R 3.3.2 and the ``Rmath`` library.
* :download:`R 3.3.2 Modulefile </sharc/software/modulefiles/apps/R//3.3.2/gcc-4.8.5>` located on the system at ``/usr/local/modulefiles/apps/R/3.3.2/``
* Install log-files, including the output of the `make check` tests are available on the system at `/usr/local/packages/apps/R/3.3.2/gcc-4.8.5/install_logs/`
