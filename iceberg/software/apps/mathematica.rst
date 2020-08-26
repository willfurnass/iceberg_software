Mathematica
===========

.. sidebar:: Wolfram Mathematica

   :Dependancies: None
   :URL: http://www.wolfram.com/mathematica/
   :Latest version: 11.0.1

Mathematica is a technical computing environment and programming language with strong symbolic and numerical abilities.

Single Core Interactive Usage
-----------------------------
After connecting to iceberg (see :ref:`ssh`),
:ref:`start an interactive session <sched_interactive>` then
load a specific version of Mathematica using one of: ::

   module load apps/binapps/mathematica/11.0.1
   module load apps/binapps/mathematica/10.3.1
   module load apps/binapps/mathematica/10.2

Mathematica can then be started using: ::

   mathematica

Multicore Interactive Usage
---------------------------

Mathematica has extensive parallel functionality.
To use it, you should request a parallel interactive session.
For example, to request 4 cores: ::

   qrshx -pe openmp 4

Load and launch Mathematica: ::

   module load apps/binapps/mathematica/11.0.1
   mathematica

In Mathematica, let's time how long it takes to calculate the first 20 million primes on 1 CPU core: ::

   AbsoluteTiming[primelist = Table[Prime[k], {k, 1, 20000000}];]

When I tried this, I got 78 seconds.
Your results may vary greatly.
Now, let's launch 4 Parallel Kernels and redo the calculation in parallel: ::

   LaunchKernels[4]
   AbsoluteTiming[primelist =
   ParallelTable[Prime[k], {k, 1, 20000000},
   Method -> "CoarsestGrained"];]

When I tried this, I got 29 seconds (around 2.7 times faster).
This illustrates a couple of points:

* You should always request the same number of kernels as you requested when you started your interactive session (in this case, 4).
  If you request more, you will damage performance for yourself and other users of the system.
* N kernels doesn't always translate to N times faster.

Batch Submission
----------------

Unfortunately, it is not possible to run Mathematica notebook ``.nb`` files directly in batch.
Instead, you need to create a simple text file that contains all of the Mathematica commands you want to execute.
Typically, such files are given the extension ``.m``.
Let’s run the following simple Mathematica script as a batch job: ::

   (*Find out what version of Mathematica this machine is running*)
   Print["Mathematica version is " <> $Version]

   (*Find out the name of the machine we are running on*)
   Print["The machine name is " <> $MachineName]

   (*Do a calculation*)
   Print["The result of the integral is "]
   Print [ Integrate[Sin[x]^2, x]]

Copy and paste the above into a text file called ``very_simple_mathematica.m``.

An example batch submission script for this file is: ::

   #!/bin/bash
   # Request 4 gigabytes of real memory
   #$ -l rmem=4G

   module load apps/binapps/mathematica/10.3.1

   math -script very_simple_mathematica.m

Copy and paste the above into a file called ``run_job.sh`` and submit with ::

   qsub run_job.sh

Once the job has successfully completed, the output will be in a file named like ``run_job.sh.o396699``.
The number at the end refers to the job ID given to this job by the system and will be different for you.
Let's take a look at the contents of this file ::

   more run_job.sh.o396699

   Mathematica version is 10.2.0 for Linux x86 (64-bit) (July 28, 2015)
   The machine name is node131
   The result of the integral is
   x/2 - Sin[2*x]/4

Installation notes
------------------
These are primarily for administrators of the system.

**For Version 11.0.1** ::

   mkdir -p /usr/local/packages6/apps/binapps/mathematica/11.0.1
   chmod +x ./Mathematica_11.0.1_LINUX.sh
   ./Mathematica_11.0.1_LINUX.sh

The installer is interactive. Here's the session output: ::

   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                                        Wolfram Mathematica 11 Installer
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

   Copyright (c) 1988-2016 Wolfram Research, Inc. All rights reserved.

   WARNING: Wolfram Mathematica is protected by copyright law and international treaties. Unauthorized reproduction or distribution may result in severe civil and criminal penalties and will be
   prosecuted to the maximum extent possible under law.

   Enter the installation directory, or press ENTER to select /usr/local/Wolfram/Mathematica/11.0:
   > /usr/local/packages6/apps/binapps/mathematica/11.0.1

   Now installing...

   [*********************************************************************************************************************************************************************************************************]

   Type the directory path in which the Wolfram Mathematica script(s) will be created, or press ENTER to select /usr/local/bin:
   > /usr/local/packages6/apps/binapps/mathematica/11.0.1/scripts

   Create directory (y/n)?
   > y


   WARNING: No Avahi Daemon was detected so some Kernel Discovery features will not be available. You can install Avahi Daemon using your distribution's package management system.

   For Red Hat based distributions, try running (as root):

   yum install avahi

   Installation complete.

Copy the license from the previous version.
It is a network license so you need to ensure that the network license server has been updated. ::

   cp /usr/local/packages6/apps/binapps/mathematica/10.2/Configuration/Licensing/mathpass /usr/local/packages6/apps/binapps/mathematica/11.0.1/Configuration/Licensing/

**For Version 10.3.1** ::

   mkdir -p /usr/local/packages6/apps/binapps/mathematica/10.3.1
   chmod +x ./Mathematica_10.3.1_LINUX.sh
   ./Mathematica_10.3.1_LINUX.sh

The installer is interactive. Here's the session output ::

   --------------------------------------------------------------------------------
                         Wolfram Mathematica 10.3 Installer
   --------------------------------------------------------------------------------

   Copyright (c) 1988-2015 Wolfram Research, Inc. All rights reserved.

   WARNING: Wolfram Mathematica is protected by copyright law and international
   treaties. Unauthorized reproduction or distribution may result in severe
   civil and criminal penalties and will be prosecuted to the maximum extent
   possible under law.

   Enter the installation directory, or press ENTER to select
   /usr/local/Wolfram/Mathematica/10.3:
   > /usr/local/packages6/apps/binapps/mathematica/10.3.1

   Now installing...

   [*****************************************************************************]

   Type the directory path in which the Wolfram Mathematica script(s) will be
   created, or press ENTER to select /usr/local/bin:
   > /usr/local/packages6/apps/binapps/mathematica/10.3.1/scripts

   Create directory (y/n)?
   > y


   WARNING: No Avahi Daemon was detected so some Kernel Discovery features will
   not be available. You can install Avahi Daemon using your distribution's
   package management system.

   For Red Hat based distributions, try running (as root):

   yum install avahi

   Installation complete.

Install the University network ``mathpass`` file at ``/usr/local/packages6/apps/binapps/mathematica/10.3.1/Configuration/Licensing``

**For Version 10.2** ::

   mkdir -p /usr/local/packages6/apps/binapps/mathematica/10.2
   chmod +x ./Mathematica_10.2.0_LINUX.sh
   ./Mathematica_10.2.0_LINUX.sh

The installer is interactive. Here's the session output ::

   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                                       Wolfram Mathematica 10.2 Installer
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

   Copyright (c) 1988-2015 Wolfram Research, Inc. All rights reserved.

   WARNING: Wolfram Mathematica is protected by copyright law and international treaties. Unauthorized reproduction or distribution may result in severe civil and criminal penalties and will be
   prosecuted to the maximum extent possible under law.

   Enter the installation directory, or press ENTER to select /usr/local/Wolfram/Mathematica/10.2:
   >

   Error: Cannot create directory /usr/local/Wolfram/Mathematica/10.2.

   You may need to be logged in as root to continue with this installation.

   Enter the installation directory, or press ENTER to select /usr/local/Wolfram/Mathematica/10.2:
   > /usr/local/packages6/apps/binapps/mathematica/10.2

   Now installing...

   [*********************************************************************************************************************************************************************************************************]

   Type the directory path in which the Wolfram Mathematica script(s) will be created, or press ENTER to select /usr/local/bin:
   > /usr/local/packages6/apps/binapps/mathematica/10.2/scripts

   Create directory (y/n)?
   > y


   WARNING: No Avahi Daemon was detected so some Kernel Discovery features will not be available. You can install Avahi Daemon using your distribution's package management system.

   For Red Hat based distributions, try running (as root):

   yum install avahi

   Installation complete.

Remove the ``playerpass`` file ::

   rm /usr/local/packages6/apps/binapps/mathematica/10.2/Configuration/Licensing/playerpass

Install the University network ``mathpass`` file at ``/usr/local/packages6/apps/binapps/mathematica/10.2/Configuration/Licensing``

Modulefiles
-----------
* The :download:`11.0.1 module file </iceberg/software/modulefiles/apps/binapps/mathematica/11.0.1>`.
* The :download:`10.3.1 module file </iceberg/software/modulefiles/apps/binapps/mathematica/10.3.1>`.
* The :download:`10.2 module file  </iceberg/software/modulefiles/apps/binapps/mathematica/10.2>`.
