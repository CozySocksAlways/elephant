.. _install:

============
Installation
============

Elephant is published on `PyPI <https://pypi.org/project/elephant/>`_. You can
install it in one of two ways:

* **Project-based** (recommended for new users): a tool such as
  `uv <https://docs.astral.sh/uv/>`_ manages a project folder for you. The
  folder holds a manifest listing your dependencies and a lock file
  recording their exact versions, so your analysis can be reproduced later
  or on another machine.
* **Environment-based** (the traditional workflow): you create a virtual
  environment yourself, activate it, and install Elephant into it with
  ``pip`` or ``conda``. Use this to add Elephant to an environment you already
  have, for example a Jupyter kernel or an environment on a cluster.


.. _prerequisites:

*****************
Getting Elephant
*****************

Elephant requires `Python <https://www.python.org/>`_ 3.10 or newer; it is
tested with Python 3.10 to 3.14. uv installs a suitable Python for you.

.. tabs::


    .. tab:: Project-based (uv)

        1. `Install uv <https://docs.astral.sh/uv/getting-started/installation/>`_.

        2. Create a project and add Elephant to it:

           .. code-block:: sh

              uv init my-analysis
              cd my-analysis
              uv add elephant

        3. Run Python or your scripts in the project environment:

           .. code-block:: sh

              uv run python
              uv run my_script.py

        To use the advanced features of Elephant, add it with extras
        instead: ``uv add "elephant[extras]"``.


    .. tab:: Environment-based

        .. tabs::


            .. tab:: pip + venv

                1. Create a virtual environment in the folder ``.venv``:

                   .. code-block:: sh

                      python -m venv .venv

                2. Activate the environment:

                   .. code-block:: sh

                      # Linux / macOS
                      source .venv/bin/activate

                      # Windows (PowerShell or cmd)
                      .venv\Scripts\activate

                3. Install Elephant:

                   .. code-block:: sh

                      pip install elephant

                With uv installed, ``uv venv`` and ``uv pip install elephant``
                do the same.

                See :ref:`install_elephant` for extras, upgrades and the
                development version.


            .. tab:: conda

                1. Create your conda environment (e.g., ``elephant``):

                   .. code-block:: sh

                      conda create --name elephant python=3.13

                2. Activate your environment:

                   .. code-block:: sh

                      conda activate elephant

                3. Install Elephant from PyPI (it is not on conda-forge):

                   .. code-block:: sh

                      pip install elephant

                See :ref:`install_elephant` for extras, upgrades and the
                development version.

.. _install_elephant:

**********************
Installation options
**********************

The commands below use ``pip`` in an activated environment (see
:ref:`prerequisites`). In a uv project, use ``uv add`` instead of
``pip install``.

.. tabs::


    .. tab:: Stable release version

        Install the latest release from `PyPI <https://pypi.org/project/elephant/>`_:

        .. code-block:: sh

           pip install elephant

        To use the advanced features of Elephant, install it with the
        ``extras`` optional dependencies. The quotes are needed in some shells
        (e.g., zsh):

        .. code-block:: sh

           pip install "elephant[extras]"

        To upgrade to a newer release, use the ``--upgrade`` flag:

        .. code-block:: sh

           pip install --upgrade elephant

        The same with uv:

        .. code-block:: sh

           # uv project / uv-managed environment
           uv add "elephant[extras]"            # or: uv pip install "elephant[extras]"
           uv lock --upgrade-package elephant   # upgrade, then: uv sync

        The following optional dependency groups are available and can be
        combined, e.g. ``"elephant[extras,tutorials]"``:

        * ``extras``: scikit-learn, statsmodels and jinja2, needed by some
          advanced functions (e.g., ASSET with CUDA)
        * ``tutorials``: packages used in the tutorial notebooks (matplotlib,
          viziphant, ...)
        * ``cuda`` and ``opencl``: GPU acceleration, see
          :ref:`cuda_opencl_support`
        * ``tests`` and ``docs``: for running the test suite and building the
          documentation


    .. tab:: Development version

        The development version is installed from a clone of the
        `GitHub repository <https://github.com/NeuralEnsemble/elephant>`_ and
        needs `Git <https://git-scm.com/>`_.

        1. Clone the repository:

           .. code-block:: sh

              git clone https://github.com/NeuralEnsemble/elephant.git
              cd elephant

        2. Install the local version in editable mode, so that changes to the
           source code take effect without reinstalling:

        .. tabs::

            .. tab:: uv

                .. code-block:: sh

                    uv sync --extra extras
                    uv run python

                ``uv sync`` creates the environment in ``.venv`` and a
                ``uv.lock`` file; do not commit the lock file.

            .. tab:: pip + venv

                In an activated virtual environment:

                .. code-block:: sh

                    pip install -e ".[extras]"

            .. tab:: conda

                ``requirements/environment.yml`` also installs ``mpi4py`` and
                the ``extras`` and ``tutorials`` dependencies:

                .. code-block:: sh

                    conda remove -n elephant --all  # remove the previous environment
                    conda env create -f requirements/environment.yml
                    conda activate elephant
                    pip install -e .

***********
MPI support
***********

Some Elephant modules (ASSET, SPADE, etc.) are parallelized to run with MPI.
In order to make use of MPI parallelization, you need to install the
``mpi4py`` package and an MPI library. On a computing cluster, use the MPI
library provided there (e.g., ``module load``) and install only ``mpi4py``;
otherwise install Open MPI alongside it:

.. tabs::

    .. tab:: uv

        .. code-block:: sh

            uv add mpi4py openmpi           # in a uv project
            uv pip install mpi4py openmpi   # in an environment

    .. tab:: pip

        .. code-block:: sh

            pip install mpi4py openmpi

    .. tab:: conda

        .. code-block:: sh

            conda install -c conda-forge mpi4py openmpi

The ``openmpi`` package from PyPI is available for Linux and macOS. On Windows,
or to use an MPI library that is already installed on your system, see the
`mpi4py installation guide
<https://mpi4py.readthedocs.io/en/stable/install.html>`_.

To run a python script that supports MPI parallelization, run in a terminal:

.. code-block:: sh

    mpiexec -n numprocs python -m mpi4py pyfile [arg] ...

In a uv project, prefix the command with ``uv run``.
For more information, refer to `mpi4py
<https://mpi4py.readthedocs.io/en/stable/mpi4py.run.html>`_ documentation.


.. _cuda_opencl_support:

***********************
CUDA and OpenCL support
***********************

:ref:`asset` module supports CUDA and OpenCL. These are experimental features.
You can have one, both, or none installed in your system.

.. tabs::

    .. tab:: CUDA

        To leverage CUDA acceleration on an NVIDIA GPU card, `CUDA toolkit
        <https://developer.nvidia.com/cuda-downloads>`_ must installed on
        your system. Then run the following command in a terminal:

        .. code-block:: sh

            pip install pycuda

        In case you experience issues installing PyCUDA, `this guide
        <https://medium.com/leadkaro/setting-up-pycuda-on-ubuntu-18-04-for-
        gpu-programming-with-python-830e03fc4b81>`_ offers a step-by-step
        installation manual.

        If PyCUDA is detected and installed, CUDA backend is used by default in
        Elephant ASSET module. To turn off CUDA support, set ``ELEPHANT_USE_CUDA``
        environment flag to ``0``.


    .. tab:: OpenCL

        If you have a laptop with a built-in Intel Graphics Card, you can still
        leverage significant performance optimization with OpenCL backend.
        The simplest way to install PyOpenCL is to run a conda command:

        .. code-block:: sh

            conda install -c conda-forge pyopencl intel-compute-runtime

        However, if you have root (sudo) privileges, it's recommended to install
        up-to-date `Intel Graphics Compute Runtime
        <https://github.com/intel/compute-runtime/releases>`_ system-wide and then
        install PyOpenCL as follows:

        .. code-block:: sh

            conda install -c conda-forge pyopencl ocl-icd-system

        Set ``ELEPHANT_USE_OPENCL`` environment flag to ``0`` to turn off
        PyOpenCL support.

        .. note::

            Make sure you've disabled GPU Hangcheck as described in the
            `Intel GPU developers documentation <https://www.intel.com/content/
            www/us/en/docs/oneapi/installation-guide-linux/2023-1/
            gpu-disable-hangcheck.html>`_. Do it with caution -
            using your graphics card to perform computations may make the system
            unresponsive until the compute program terminates.

.. _no-compile-spade:

****************************
Resolving compilation issues
****************************

Some modules in Elephant make use of C extensions to speed up computation.
However, those extensions need to be compiled before use. In some cases, this
causes problems. For example, the compiler on the current machine does not
fulfill the requirements for the extension, certain libraries are missing,
or no compiler is available at all.

In order to circumvent this problem, the following commands allow to avoid the
compilation for specific or for all C extensions.

.. tabs::

    .. tab:: general
        Use the following to install elephant without C extensions:

        .. code-block:: sh

            pip install elephant --install-option='--no-compile'

    .. tab:: spade
        To avoid compilation of the c++ extension ``fim.cpp`` used in :ref:`spade`, install the package with:

        .. code-block:: sh

            pip install elephant --install-option='--no-compile-spade'

        In this case the pure python implementation of :ref:`spade` is still available.

************
Dependencies
************

Elephant relies on two special packages, installed by default:

    * `quantities <http://pypi.python.org/pypi/quantities>`_ - support for physical quantities with units (mV, ms, etc.)
    * `neo <http://pypi.python.org/pypi/neo>`_ - electrophysiology data manipulations
