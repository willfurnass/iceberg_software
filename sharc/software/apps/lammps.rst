.. _lammps_sharc:

LAMMPS
======

.. sidebar:: LAMMPS

   :Versions:  22_08_2018
   :Support Level:
   :Dependancies: gcc/4.9.4, openmpi/2.0.1
   :URL: https://lammps.sandia.gov/
   :Documentation: https://lammps.sandia.gov/doc/Manual.html

LAMMPS is a classical molecular dynamics code with a focus on materials modelling.
It's an acronym for Large-scale Atomic/Molecular Massively Parallel Simulator.

LAMMPS has potentials for solid-state materials (metals, semiconductors) and soft matter (biomolecules, polymers) and coarse-grained or mesoscopic systems.
It can be used to model atoms or, more generically, as a parallel particle simulator at the atomic, meso, or continuum scale.

LAMMPS runs on single processors or in parallel using message-passing techniques and a spatial-decomposition of the simulation domain.
Many of its models have versions that provide accelerated performance on CPUs, GPUs, and Intel Xeon Phis. The code is designed to be easy to modify or extend with new functionality.

Interactive Usage
-----------------
After connecting to ShARC (see :ref:`ssh`), :ref:`start an interactive session <sched_interactive>` then
load a specific version of LAMMPS with: ::

   module load apps/lammps/22_08_2018/gcc-4.9.4-openmpi-2.0.1

You can then run LAMMPS by entering ``lmp``

   .. code-block:: bash

      cp /usr/local/packages/apps/lammps/22_08_2018/gcc-4.9.4-openmpi-2.0.1/examples.tar.gz .
      tar -xvzf examples.tar.gz
      cd examples/indent # select indent example
      lmp -in in.indent # run indent example


Serial (one core) Batch usage
-----------------------------

Your batch script (script.sh) should contain the following commands: ::

   #!/bin/bash
   ## your email address
   #$ -M joebloggs@sheffield.ac.uk
   #$ -m eba
   ## set max runtime to 1 minute (for this test)
   #$ -l h_rt=00:01:00
   ## set max memory to 1Gb per core (default is 2G)
   #$ -l rmem=1G
   #$ -V
   #$ -cwd
   #$ -j y
   module load apps/lammps/22_08_2018/gcc-4.9.4-openmpi-2.0.1
   lmp -in in.indent

Ensure your batch script (script.sh) has unix style line endings and is executable: ::

   dos2unix script.sh
   chmod +x script.sh

Submit your job to the batch system: ::

   qsub script.sh

The output will be written to the job ``.o`` file when the job finishes.

Parallel (multi core using MPI) Batch usage
-------------------------------------------

Your batch script (``mpi_script.sh``) should contain the following commands: ::

   #!/bin/bash
   ## your email address
   #$ -M joebloggs@sheffield.ac.uk
   #$ -m eba
   ## no of cores using mpi
   #$ -pe mpi 4
   ## set max runtime to 1 minute (for this test)
   #$ -l h_rt=00:01:00
   ## set max memory to 1Gb per core (default is 2G)
   #$ -l rmem=1G
   #$ -V
   #$ -cwd
   #$ -j y
   module load apps/lammps/22_08_2018/gcc-4.9.4-openmpi-2.0.1
   mpirun -np $NSLOTS lmp -in in.indent

Ensure the ``mpi_script.sh`` has unix style line endings, and is executable using commands for serial batch (above).

Submit your job to the batch system: ::

   qsub mpi_script.sh

The output will be written to the job ``.o`` file when the job finishes.


Installation notes
------------------

LAMMPS was compiled using the
:download:`install_lammps.sh </sharc/software/install_scripts/apps/lammps/22_08_2018/gcc-4.9.4-openmpi-2.0.1/install_lammps.sh>` script.

The module file is
:download:`/usr/local/modulefiles/apps/lammps/22_08_2018/gcc-4.9.4-openmpi-2.0.1 </sharc/software/modulefiles/apps/lammps/22_08_2018/gcc-4.9.4-openmpi-2.0.1>`.

