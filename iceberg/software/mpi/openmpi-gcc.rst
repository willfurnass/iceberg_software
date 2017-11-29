.. _openmpi_gcc_iceberg:

OpenMPI (gcc version)
=====================

.. sidebar:: OpenMPI (gcc version)

   :Latest Version: 1.10.1
   :Dependancies: gcc
   :URL: http://www.open-mpi.org/

The Open MPI Project is an open source Message Passing Interface implementation that is developed and maintained by a consortium of academic, research, and industry partners. Open MPI is therefore able to combine the expertise, technologies, and resources from all across the High Performance Computing community in order to build the best MPI library available. Open MPI offers advantages for system and software vendors, application developers and computer science researchers.

Module files
------------
The latest version is made available using ::

   module load mpi/gcc/openmpi

Alternatively, you can load a specific version using one of ::

   module load mpi/gcc/openmpi/1.10.1
   module load mpi/gcc/openmpi/1.10.0
   module load mpi/gcc/openmpi/1.8.8
   module load mpi/gcc/openmpi/1.8.3
   module load mpi/gcc/openmpi/1.6.4
   module load mpi/gcc/openmpi/1.4.4
   module load mpi/gcc/openmpi/1.4.3

Installation notes
------------------
These are primarily for administrators of the system.

**Version 1.10.1**

Compiled using gcc 4.4.7.

* :download:`install_openMPI_1.10.1.sh </iceberg/software/install_scripts/mpi/gcc/openmpi/install_gcc_openMPI_1.10.1.sh>` Downloads, compiles and installs OpenMPI 1.10.0 using the system gcc.
* :download:`Modulefile 1.10.1 </iceberg/software/modulefiles/mpi/gcc/openmpi/1.10.1>` located on the system at ``/usr/local/modulefiles/mpi/gcc/openmpi/1.10.1``

**Version 1.10.0**

Compiled using gcc 4.4.7.

* :download:`install_openMPI_1.10.0.sh  </iceberg/software/install_scripts/mpi/gcc/openmpi/install_gcc_openMPI_1.10.0.sh>` Downloads, compiles and installs OpenMPI 1.10.0 using the system gcc.
* :download:`Modulefile 1.10 </iceberg/software/modulefiles/mpi/gcc/openmpi/1.10.0>` located on the system at ``/usr/local/modulefiles/mpi/gcc/openmpi/1.10.0``

**Version 1.8.8**

Compiled using gcc 4.4.7.

* :download:`install_openMPI.sh  </iceberg/software/install_scripts/mpi/gcc/openmpi/install_gcc_openMPI_1.8.8.sh>` Downloads, compiles and installs OpenMPI 1.8.8 using the system gcc.
* :download:`Modulefile </iceberg/software/modulefiles/mpi/gcc/openmpi/1.8.8>` located on the system at ``/usr/local/modulefiles/mpi/gcc/openmpi/1.8.8``
