ANSYS
=====

.. sidebar:: ANSYS
   
   :Versions: 16.1, 17.2, 18.0, 18.2
   :Dependencies: No prerequsite modules loaded. However, If using the User Defined Functions (UDF) will also need the following: For ANSYS Mechanical, Workbench, CFX and AutoDYN: Intel 14.0 or above; Compiler For Fluent: GCC 4.6.1 or above
   :URL: http://www.ansys.com 
   :Local URL: http://www.shef.ac.uk/cics/research/software/fluent


The Ansys suite of programs can be used to numerically simulate a large variety of structural and fluid dynamics problems found in many engineering, physics, medical, aeronotics and automative industry applications.


Usage
-----

Ansys can be activated using the module files::

    module load apps/ansys/16.1
    module load apps/ansys/17.2
    module load apps/ansys/18.0/binary
    module load apps/ansys/18.2/binary

The Ansys Workbench GUI executable is ``ansyswb``. ``ansyswb`` can be launched during an interactive session with X Window support (e.g. an interactive ``qsh`` session).
The Ansys exectuable is ``ansys`` and ``fluent`` is the executable for Fluent. Typing ``ansys -g`` or ``fluent -g -help`` will display a list of usage options.


Batch jobs
----------

The easiest way of running batch jobs for a particular version of Ansys (e.g. 17.2) is::
    
    module load apps/ansys/17.2
    runansys
	
Or the same for Fluent::

    module load apps/ansys/17.2
    runfluent
	
The ``runfluent`` and ``runansys`` commands submit a Fluent journal or Ansys input file into the batch system and can take a number of different parameters, according to your requirements.
Typing ``runansys`` or ``runfluent`` will display information about their usage. **Note:** ``runansys`` and ``runfluent`` are not setup for use with Ansys 18.0 and 18.2.
	
Users are encouraged to write their own batch submission scripts. The following is an example batch submission script, ``my_job.sh``, to run ``fluent`` version 16.1 and which is submitted to the queue by typing ``qsub my_job.sh``::

    #!/bin/bash
    #$ -cwd
    #$ -l h_rt=00:30:00
    #$ -l rmem=2G
    #$ -pe mpi-rsh 8

    module load apps/ansys/16.1

    fluent 2d -i flnt.inp -g -t8 -sge -mpi=intel -rsh -sgepe mpi-rsh
	
The script requests 8 cores using the MPI parallel environment ``mpi-rsh`` with a runtime of 30 mins and 2 GB of real memory per core. The Fluent input file is ``flnt.inp``. **Note:** Please use the ``mpi-rsh`` parallel environment to run MPI parallel jobs for Ansys/Fluent.

	
Installation notes
------------------

Ansys 16.1 was installed using the
:download:`install_ansys.sh </sharc/software/install_scripts/apps/ansys/16.1/install_ansys.sh>` script; the module
file is
:download:`/usr/local/modulefiles/apps/ansys/16.1 </sharc/software/modulefiles/apps/ansys/16.1>`.

Ansys 17.2 was installed using the
:download:`install_ansys.sh </sharc/software/install_scripts/apps/ansys/17.2/install_ansys.sh>` script; the module
file is
:download:`/usr/local/modulefiles/apps/ansys/17.2 </sharc/software/modulefiles/apps/ansys/17.2>`. 

Ansys 18.0 was installed using the
:download:`install_ansys_180.sh </sharc/software/install_scripts/apps/ansys/18.0/binary/install_ansys_180.sh>` script; the module
file is
:download:`/usr/local/modulefiles/apps/ansys/18.0/binary </sharc/software/modulefiles/apps/ansys/18.0/binary>`. 

Ansys 18.2 was installed using the
:download:`install_ansys_182.sh </sharc/software/install_scripts/apps/ansys/18.2/binary/install_ansys_182.sh>` script; the module
file is
:download:`/usr/local/modulefiles/apps/ansys/18.2/binary </sharc/software/modulefiles/apps/ansys/18.2/binary>`. 

The binary installations were tested by launching ``ansyswb`` and by using the above batch submission script. The ``mpi-rsh`` tight-integration parallel environment is required to run Ansys/Fluent using MPI due to password-less ssh being disabled across nodes on ShARC.
