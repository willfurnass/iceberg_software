ImageJ
======

.. sidebar:: ImageJ

   :Version: 1.50g
   :URL: http://imagej.nih.gov/ij/

ImageJ is a public domain, Java-based image processing program developed at the National Institutes of Health.

Interactive Usage
-----------------

After connecting to Iceberg (see :ref:`ssh`),  :ref:`start an interactive session <sched_interactive>` then
load a specific version of ImageJ using: ::

   module load apps/binapps/imagej/1.50g

This module command also modifies your session's environment to provide Java 1.8 and Java 3D 1.5.2.
An environment variable called ``IMAGEJ_DIR`` is created by the module command that contains the path to the requested version of ImageJ.

You can start ImageJ, with default settings, with the command ::

   imagej

This starts ImageJ with 512MB of Java heap space.
To request more,
you need to run the Java ``.jar`` file directly
and use the ``-Xmx`` Java flag.
For example, to request 1 GB: ::

   java -Xmx1G -Dplugins.dir=$IMAGEJ_DIR/plugins/ -jar $IMAGEJ_DIR/ij.jar

Documentation
-------------
The `official ImageJ documentation <http://imagej.nih.gov/ij/download.html>`__.

Installation notes
------------------

Install version 1.49 using the :download:`install_imagej.sh </iceberg/software/install_scripts/apps/imagej/1.50g/install_imagej.sh>` script.

Run the GUI and update to the latest version by clicking on *Help* then *Update ImageJ*.

Rename the ``run`` script to ``imagej`` since ``run`` is too generic.

Modify the ``imagej`` script so that it reads: ::

   java -Xmx512m -jar -Dplugins.dir=/usr/local/packages6/apps/binapps/imagej/1.50g/plugins/ /usr/local/packages6/apps/binapps/imagej/1.50g/ij.jar

ImageJ's 3D plugin requires Java 3D which needs to be included in the JRE used by ImageJ.
I attempted a module-controlled version of Java3D but the plugin refused to recognise it. 
See `this Issue <https://github.com/rcgsheffield/sheffield_hpc/issues/279>`__ for details.

The plug-in will install Java3D within the JRE for you if you launch it and click *Plugins > 3D > 3D Viewer*.

Modulefile
----------
The module file is on the system at ``/usr/local/modulefiles/apps/binapps/imagej/1.50g``.

Its contents are: ::

   #%Module1.0#####################################################################
   ##
   ## ImageJ 1.50g modulefile
   ##

   ## Module file logging
   source /usr/local/etc/module_logging.tcl
   ##

   module load apps/java/1.8u71

   proc ModulesHelp { } {
         puts stderr "Makes ImageJ 1.50g available"
   }


   set version 1.50g
   set IMAGEJ_DIR /usr/local/packages6/apps/binapps/imagej/$version

   module-whatis   "Makes ImageJ 1.50g available"

   prepend-path PATH $IMAGEJ_DIR
   prepend-path CLASSPATH $IMAGEJ_DIR/
   prepend-path IMAGEJ_DIR $IMAGEJ_DIR
