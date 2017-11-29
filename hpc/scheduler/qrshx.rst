.. _qrshx:

qrshx
=====
``qrshx`` is a scheduler command that requests an interactive session on a worker node. The resulting session will support graphical applications. You will usually run this command from a login node.

Examples
--------
Request an interactive X-Windows session that provides the default amount of memory resources and launch the ``gedit`` text editor: ::

    qrshx
    gedit

Request an interactive X-Windows session that provides 10 Gigabytes of real memory and launch the latest version of MATLAB: ::

    qrshx -l rmem=10G
    module load apps/matlab
    matlab

Request an interactive X-Windows session that provides 10 Gigabytes of real memory and 4 CPU cores: ::

    qrshx -l rmem=10G -pe openmp 4

Sysadmin notes
--------------
``qrshx`` not a standard SGE command; it was specific to the University of Sheffield's clusters.

On ShARC ``qrshx`` resides at ``/usr/local/scripts/qrshx`` and has `this content <https://gist.github.com/willfurnass/10277756070c4f374e6149a281324841>`_.

On Iceberg it resides at ``/usr/local/bin/qrshx`` and is: ::

    #!/bin/sh
    exec qrsh -v DISPLAY -pty y "$@" bash
