.. _theano_sharc:

Theano
======

.. sidebar:: Theano

   :URL: http://deeplearning.net/software/theano/index.html

Theano is a Python library that allows you to define, optimize, and evaluate mathematical expressions involving multi-dimensional arrays efficiently. Theano is most commonly used to perform Deep Learning and has excellent GPU support and integration through PyCUDA. The following steps can be used to setup and configure Theano on your own profile.

About Theano on ShARC
---------------------

**A GPU-enabled worker node must be requested in order to use the GPU version of this software. See** :ref:`GPUComputing_sharc` **for more information.**

Theano is available on ShARC as both Singularity images and by local installation.

As Theano and all its dependencies are written in Python. A Singularity image is available for instant usage and it can also be installed locally in your home directory. For local installation, the use of Anaconda (:ref:`sharc-python-conda`) is recommended as it is able to create a virtual environment in your home directory, allowing the installation of new Python packages without admin permission.

This software and documentation is maintained by the `RSES group <https://rse.shef.ac.uk/>`_ and `GPUComputing@Sheffield <http://gpucomputing.shef.ac.uk/>`_. For feature requests or if you encounter any problems, please raise an issue on the `GPU Computing repository <https://github.com/RSE-Sheffield/GPUComputing/issues>`_.

Local Installation
------------------

First, :ref:`start an interactive session <sched_interactive>`.
To use GPUs, see :ref:`GPUInteractive_sharc`.

Load the relevant modules with the following command: ::

	module load libs/cudnn/5.1/binary-cuda-8.0.44
	module load apps/python/conda

Create a conda environment to load relevant modules on your local user account: ::

	conda create -n theano python=3.5
	source activate theano

Install the other Python module dependencies which are required using conda: ::

	conda install -y numpy scipy nose
	pip install pydot-ng
	pip install parameterized
	conda install -y theano pygpu



For optimal Theano performance, enable the CUDA memory manager CNMeM. To do this, create the .theanorc file in your HOME directory and set the fraction of GPU memory reserved by Theano. The exact amount of energy may have to be hand-picked: if Theano asks for more memory that is currently available on the GPU, an error will be thrown during import of theano module. Create or edit the ``.theanorc`` file with nano: ::

	nano ~/.theanorc

Add the following lines and, if necessary, change the 0.8 number to whatever works for you ::

	[lib]
	cnmem=0.8

Run python and verify that Theano is working correctly ::

	python -c "import theano;theano.test()"

Every Session Afterwards and in Your Job Scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The previous instuctions installs Theano and its dependencies inside your home directory but every time you use a new session or within your job scripts, the modules must be loaded and conda must be activated again. Use the following command to activate the Conda environment with Theano installed: ::

	module load libs/cudnn/5.1/binary-cuda-8.0.44
	module load apps/python/conda
	source activate theano

Theano Singularity Images
-------------------------

.. note::
 Theano singularity images support is now discontinued as the use of conda virtual environments is deemed to be more customisable and simpler to use. Existing images will still be available but to use a newer version of Theano, please follow instructions above to install Theano to your home directory.

Singularity images are self-contained virtual machines similar to Docker. For more information on Singularity and how to use the images, see :ref:`singularity_sharc`.

A symlinked file is provided that always point to the latest image: ::

	/usr/local/packages/singularity/images/theano/gpu.img

To get a bash terminal in to an image for example, use the command: ::

	singularity exec --nv /usr/local/packages/singularity/images/theano/gpu.img /bin/bash

The ``exec`` command can also be used to call any command/script inside the image e.g. ::

	singularity exec --nv /usr/local/packages/singularity/images/theano/gpu.img python your_theano_script.py

**The** ``--nv`` **flag enables the use of GPUs within the image and can be removed if the software you're using does not use the GPU.**

You may get a warning similar to ``groups: cannot find name for group ID ...``, this can be ignored and will not have an affect on running the image.

The paths ``/fastdata``, ``/data``, ``/home``, ``/scratch``, ``/shared`` are automatically mounted to your ShARC filestore directories. For GPU-enabled images the ``/nvlib`` and ``/nvbin`` is mounted to the correct Nvidia driver version for the node that you're using.

Theano is installed as part of Anaconda and can be found inside the image at: ::

	/usr/local/anaconda3-4.2.0/lib/python3.5/site-packages/theano

**To submit jobs that uses a Singularity image, see** :ref:`use_image_batch_singularity_sharc` **for more detail.**

Image Index
^^^^^^^^^^^

Paths to the actual images and definition files are provided below for downloading and building of custom images.

* Shortcut to Latest Image
	* ``/usr/local/packages/singularity/images/theano/gpu.img``
* GPU Images
	* Latest: 0.9.0-GPU-Ubuntu16.04-CUDA8-cudNN5.0-Anaconda3.4.2.0
		* Path: ``/usr/local/packages/singularity/images/theano/0.9.0-GPU-Ubuntu16.04-CUDA8-cudNN5.0-Anaconda3.4.2.0.img``
		* Def file: :download:`/sharc/software/apps/singularity/theano.def </sharc/software/apps/singularity/theano.def>`
