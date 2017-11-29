Abaqus
======

.. sidebar:: Abaqus
   
   :Versions: 2017, 6.14-2
   :Dependencies: Intel compiler 15.0.7 and Foxit modules loaded. User subroutines need Intel compiler 2011 or above, GCC 4.1.2 or above. 
   :URL: http://www.3ds.com/products-services/simulia/products/abaqus/ 
   :Documentation: https://www.3ds.com/support/documentation/users-guides/
   :Local URL: https://www.shef.ac.uk/wrgrid/software/abaqus


Abaqus is a software suite for Finite Element Analysis (FEA) developed by Dassault Systèmes.


Usage
-----

Abaqus version 2017 or version 6.14-2 can be activated using the module files::

    module load apps/abaqus/2017/binary
    module load apps/abaqus/6.14-2/binary
	
Type ``abaqus cae`` to launch the Abaqus GUI from an interactive session with X Window support (e.g. an interactive ``qsh`` session).
Type ``abaqus`` for the command line interface. Typing ``abaqus -help`` will display a list of usage options.

The PDF viewer ``foxit`` can be launched to view the PDF documentation for Abaqus 6.14-2 located at ``/usr/local/packages/apps/abaqus/6.14-2/binary/Documentation/docs/v6.14/pdf_books``.

Abaqus 2017 documentation in HTML format is located at ``/usr/local/packages/apps/abaqus/2017/binary/Documentation/DSSIMULIA_Established_homepage_English.htm``.


**Note:** When using hardware-accelerated graphics rendering for Abaqus 6.14-2 on Sharc, i.e., during a ``qsh-vis`` interactive session, please run ``abq6142 cae`` to launch the GUI. When using a general compute node for Abaqus 2017 on Sharc, please run ``abaqus cae -mesa`` or ``abq2017 cae -mesa`` to launch the GUI without support for hardware-accelerated graphics rendering. The option ``-mesa`` disables hardware-accelerated graphics rendering within Abaqus's GUI.


Abaqus example problems
-----------------------

Abaqus contains a large number of example problems which can be used to become familiar with Abaqus on the system.
These example problems are described in the Abaqus documentation and can be obtained using the Abaqus ``fetch`` command.
For example, after loading the Abaqus module enter the following at the command line to extract the input file for test problem s4d::

    abaqus fetch job=s4d
	
This will extract the input file ``s4d.inp`` to run the computation defined by the commands and batch submission script below.


Batch jobs
----------

The easiest way of running a batch job for a particular version of Abaqus (e.g. 6.14-2) is::
    
    module load apps/abaqus/6.14-2/binary
    runabaqus
	
The ``runabaqus`` command submits an Abaqus input file into the batch queuing system and can take a number of different parameters according to your requirements.
Typing ``runabaqus`` will display information about its usage. **Note:** ``runabaqus`` is not setup for use with Abaqus 2017.

Users are encouraged to write their own batch submission scripts. The following is an example batch submission script, ``my_job.sh``, to run the executable ``abq6142`` and which is submitted to the queue by typing ``qsub my_job.sh``::

    #!/bin/bash
    #$ -cwd
    #$ -l h_rt=00:30:00
    #$ -l rmem=2G
    #$ -pe smp 4

    module load apps/abaqus/6.14-2/binary

    abq6142 job=my_job input=s4d.inp scratch=$TMPDIR memory="2gb" interactive mp_mode=threads cpus=$NSLOTS
	
The above script requests 4 cores using the OpenMP parallel environment ``smp`` with a runtime of 30 mins and 2 GB of real memory per core. The Abaqus input file is ``s4d.inp``.

**User subroutines:** The script below is an example of a batch submission script for a single core job with a runtime of 30 mins, 8 GB of real memory and with user subroutine ``umatmst3.f`` and input file ``umatmst3.inp``::

    #!/bin/bash
    #$ -cwd
    #$ -l h_rt=00:30:00
    #$ -l rmem=8G

    module load apps/abaqus/6.14-2/binary
    
    abq6142 job=my_user_job input=umatmst3.inp user=umatmst3.f scratch=$TMPDIR memory="8gb" interactive

The input file ``umatmst3.inp`` and the Fortran user subroutine ``umatmst3.f`` are obtained by typing ``abaqus fetch job=umatmst3*``.
Note that the module ``dev/intel-compilers/15.0.7``, required for user subroutines, is automatically loaded when the module for Abaqus is loaded.  

**Important information:** Please note that at present Abaqus will not run on more than one node when using MPI on ShARC. The SGE option ``-l excl=true`` can be used to request that an MPI job runs on one compute node only.


Installation notes
------------------

Abaqus 2017 was installed using the
:download:`install_abaqus_2017.sh </sharc/software/install_scripts/apps/abaqus/2017/binary/install_abaqus_2017.sh>` script; the module
file is
:download:`/usr/local/modulefiles/apps/abaqus/2017/binary </sharc/software/modulefiles/apps/abaqus/2017/binary>`. 

Abaqus 6.14-2 was installed using the
:download:`install_abaqus.sh </sharc/software/install_scripts/apps/abaqus/6.14-2/binary/install_abaqus.sh>` script; the module
file is
:download:`/usr/local/modulefiles/apps/abaqus/6.14-2/binary </sharc/software/modulefiles/apps/abaqus/6.14-2/binary>`. 

The binary installations were tested by launching ``abaqus cae`` and by using the above batch submission scripts.
Abaqus at present does not run on more than one node when using MPI due to password-less ssh being disabled across nodes on ShARC.
