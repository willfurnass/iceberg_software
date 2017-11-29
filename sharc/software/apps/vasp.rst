VASP
====

.. sidebar:: VASP

   :Version: 5.4.1 (05Feb16)
   :Dependencies: Fortran and C compilers, an implementation of MPI, numerical libraries BLAS, LAPACK, ScaLAPACK, FFTW. Modules for Intel compiler 17.0.0, Open MPI 2.0.1 and Intel MKL 2017.0 loaded.
   :URL: https://www.vasp.at/
   :Documentation: https://www.vasp.at/index.php/documentation


The Vienna Ab initio Simulation Package (VASP) is a computer program for atomic scale materials modelling, e.g. electronic structure calculations and quantum-mechanical molecular dynamics, from first principles.


Usage
-----

VASP 5.4.1 can be activated using the module file::

    module load apps/vasp/5.4.1.05Feb16/intel-17.0.0-openmpi-2.0.1

The VASP 5.4.1 executables are ``vasp_std``, ``vasp_gam`` and ``vasp_ncl``.

**Important note:** Only licensed users of VASP are entitled to use the code; refer to VASP's website for license details: https://www.vasp.at/index.php/faqs. Access to VASP on ShARC is restricted to members of the unix group ``vasp``.
To be added to this group, please contact ``research-it@sheffield.ac.uk`` and provide evidence of your eligibility to use VASP.


Batch jobs
----------

Users are encouraged to write their own batch submission scripts. The following is an example batch submission script, ``my_job.sh``, to run ``vasp_std`` and which is submitted to the queue by typing ``qsub my_job.sh``. ::

    #!/bin/bash
    #$ -cwd
    #$ -l h_rt=00:30:00
    #$ -l rmem=2G
    #$ -pe mpi 4

    module load apps/vasp/5.4.1.05Feb16/intel-17.0.0-openmpi-2.0.1
    
    mpirun vasp_std

The script requests 4 cores using the Open MPI parallel environment ``mpi`` with a runtime of 30 mins and 2 GB of real memory per core.


Installation notes
------------------

VASP 5.4.1 (05Feb16) was installed using the
:download:`install_vasp.sh </sharc/software/install_scripts/apps/vasp/5.4.1.05Feb16/intel-17.0.0-openmpi-2.0.1/install_vasp.sh>` script;
the module file is 
:download:`/usr/local/modulefiles/apps/vasp/5.4.1.05Feb16/intel-17.0.0-openmpi-2.0.1 </sharc/software/modulefiles/apps/vasp/5.4.1.05Feb16/intel-17.0.0-openmpi-2.0.1>`.

The VASP 5.4.1 installation was tested by running a batch job using the ``my_job.sh`` batch script, above, and the input for the "O atom" example (https://cms.mpi.univie.ac.at/wiki/index.php/O_atom) from the online VASP tutorials (https://cms.mpi.univie.ac.at/wiki/index.php/VASP_tutorials). 
