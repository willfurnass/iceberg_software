Velvet
======

.. sidebar:: Velvet

   :Version:  1.2.10
   :URL: https://www.ebi.ac.uk/~zerbino/velvet/

Sequence assembler for very short reads.

Interactive Usage
-----------------

After connecting to Iceberg (see :ref:`ssh`),  :ref:`start an interactive session <sched_interactive>` then
load a specific version of Velvet using: ::

   module load apps/gcc/4.4.7/velvet/1.2.10

This makes two programs available:-

* ``velvetg`` - de Bruijn graph construction, error removal and repeat resolution
* ``velveth`` - simple hashing program

Example submission script
-------------------------

If the command you want to run is ``velvetg /fastdata/foo1bar/velvet/assembly_31 -exp_cov auto -cov_cutoff auto``,
here is an example submission script that requests 60 GB of memory ::

   #!/bin/bash
   #$ -l rmem=60G

   module load apps/gcc/4.4.7/velvet/1.2.10

   velvetg /fastdata/foo1bar/velvet/assembly_31 -exp_cov auto -cov_cutoff auto

Put the above into a text file called ``submit_velvet.sh``
and submit it to the queue with the command ``qsub submit_velvet.sh``

Velvet Performance
------------------
Velvet has got multicore capabilities but these have not been compiled in our version.
This is because there is evidence that the performance is better running in serial than in parallel.
See `<http://www.ehu.eus/ehusfera/hpc/2012/06/20/benchmarking-genetic-assemblers-abyss-vs-velvet/>`_ for details.

Installation notes
------------------
Velvet was compiled with gcc 4.4.7 ::

   tar -xvzf ./velvet_1.2.10.tgz
   cd velvet_1.2.10
   make
 
   mkdir -p /usr/local/packages6/apps/gcc/4.4.7/velvet/1.2.10
   mv * /usr/local/packages6/apps/gcc/4.4.7/velvet/1.2.10

Modulefile
----------
The module file is on the system at `/usr/local/modulefiles/apps/gcc/4.4.7/velvet/1.2.10`

Its contents are ::

   #%Module1.0#####################################################################
   ##
   ## velvet 1.2.10 module file
   ##
 
   ## Module file logging
   source /usr/local/etc/module_logging.tcl
   ##
 
   proc ModulesHelp { } {
           puts stderr "Makes velvet 1.2.10 available"
   }
 
   set VELVET_DIR /usr/local/packages6/apps/gcc/4.4.7/velvet/1.2.10
 
   module-whatis   "Makes velevt 1.2.10 available"
 
   prepend-path PATH $VELVET_DIR
