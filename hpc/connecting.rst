.. _connecting:

Connecting to a cluster using SSH
=================================

The most versatile way to **run commands and submit jobs** on one of the clusters is to 
use a mechanism called `SSH <https://en.wikipedia.org/wiki/Secure_Shell>`__, 
which is a common way of remotely logging in to computers 
running the Linux operating system.  



To connect to another machine using SSH you need to 
have a SSH *client* program installed on your machine.  
macOS and Linux come with a command-line (text-only) SSH client pre-installed.  
On Windows there are various graphical SSH clients you can use, 
including *MobaXTerm*.

.. note::

    You can connect to **Iceberg** using SSH (or the related protocols SCP and SFTP) from 
    anywhere (on campus and off campus with/without a `VPN connection <https://www.sheffield.ac.uk/cics/vpn>`_)
    but if you are using **ShARC** or **Bessemer** and you are **off-campus** then 
    you need to `set up a VPN connection first <https://www.sheffield.ac.uk/cics/vpn>`_ 
    (or establish one SSH connection to Iceberg then a second from Iceberg to ShARC or Bessemer).

SSH client software on Windows
------------------------------

Download and install the *Installer edition* of `mobaxterm <https://mobaxterm.mobatek.net/download-home-edition.html>`_.

After starting MobaXterm you should see something like this:

.. image:: /images/mobaxterm-welcome.png
   :width: 50%
   :align: center

Click *Start local terminal* and if you see something like the following then please continue to :ref:`ssh`.

.. image:: /images/mobaxterm-terminal.png
   :width: 50%
   :align: center

Running commands from a terminal (from the command-line) may initially be
unfamiliar to Windows users but this is the recommended approach for
running commands on ShARC and Iceberg as 
it is the idiomatic way of interfacing with the Linux clusters.

SSH client software on Mac OS/X and Linux
-----------------------------------------

Linux and macOS (OS X) both typically come with a command-line SSH client pre-installed.

If you are using macOS and want to be able to run graphical applications on the clusters then 
you need to install the latest version of the `XQuartz <https://www.xquartz.org/>`_ *X Windows server*.

Open a terminal (e.g. *Gnome Terminal* on Linux or *Terminal* on macOS) and then go to :ref:`ssh`.

.. _ssh:

Establishing a SSH connection
-----------------------------

Once you have a terminal open run the following command to 
log in to a cluster: ::

    ssh -X $USER@$SGE_CLUSTER_NAME.shef.ac.uk

Here you need to:

* replace ``$USER`` with your CiCS username (e.g. ``te1st``)
* replace ``$SGE_CLUSTER_NAME`` with ``sharc``, ``bessemer`` or ``iceberg``.

.. note::

    **macOS users**: if this fails then:

    * Check that your `XQuartz <https://www.xquartz.org/>`_ is up to date then try again *or*
    * Try again with ``-Y`` instead of ``-X``

This should give you a prompt resembling the one below: ::

    [te1st@sharc-login1 ~]$

At this prompt type: ::

    qsh

Like this: ::

    [te1st@sharc-login2 ~]$ qsh
    Your job 135355 ("INTERACTIVE") has been submitted
    waiting for interactive job to be scheduled ....
    Your interactive job 135355 has been successfully scheduled.

Which will pop up another terminal window, which supports graphical applications.

.. note::

    When you login to a cluster you reach one of two login nodes. 
    You **should not** run applications on the login nodes.
    Running ``qsh`` gives you an interactive terminal 
    on one of the many worker nodes in the cluster.

    If you only need terminal-based (command-line only) applications 
    you can run the ``qrsh`` command,
    which will give you a shell on a worker node, 
    but without graphical application (X server) support.


.. raw:: html

   <p>
    This video shows the connection process using MobaXterm, and then connection
    and running MATLAB from a <code>qsh</code> terminal.
   </p>

   <video style="margin-left: auto; margin-right:auto; display: block;" width=70% controls>
       <source src="https://rcg.group.shef.ac.uk/tutorial_videos/mobaxterm-login-matlab-demo.webm" type="video/webm" />
       <source src="https://rcg.group.shef.ac.uk/tutorial_videos/mobaxterm-login-matlab-demo.mp4" type="video/mp4" />
   </video>

What Next?
----------

Now you have connected to a cluster, 
you can look at how to submit jobs with :ref:`submit-queue` or 
look at the software installed on 
:ref:`ShARC <sharc-software>`, :ref:`Iceberg <iceberg-software>`
and :ref:`Bessemer <bessemer-software>`.
