============
Introduction
============

The ``conda`` command is the primary interface for managing Anaconda installations. It can query and search the Anaconda package index and current Anaconda installation, create new Anaconda environments, and install and update packages into existing Anaconda environments.

------------------
conda Overview
------------------

.. _package:
.. index::
    pair: terminology; package

.. _environment:
.. index::
    pair: terminology; environment

``conda`` is an application for finding and installing Anaconda software packages. An Anaconda **package** is a binary tarball containing system-level libraries, python modules, executable programs, or other components. ``conda`` keeps track of dependencies between packages and platform specifics, making it simple to create working environments from different sets of packages. An Anaconda **environment** is a filesystem directory that contains a specific collection of Anaconda packages. As a concrete example, you might want to have one environment that provides NumPy 1.7, and another environment that provides NumPy 1.6 for legacy testing. ``conda`` makes this kind of mixing and matching easy.  To begin using an environment, simply set your PATH variable to point to its `bin` directory.

.. _channel:
.. index::
    pair: terminology; channel

.. _locally_available:
.. index::
    pair: terminology; locally available

.. _activated:
.. index::
    pair: terminology; activated

.. _deactivated:
.. index::
    pair: terminology; deactivated

Anaconda packages are downloaded from remote **channels**, which are simply URLs to directories containing Anaconda packages.
The ``conda`` command starts with a default set of channels to search, but users may exert control over this list; for example, if they wish to maintain a private or internal channel (see Configuration_ for details).
Continuum provides the following standard channels:
 * ``http://repo.continuum.io/pkgs/dev`` - Experimental or developmental versions of packages
 * ``http://repo.continuum.io/pkgs/gpl`` - GPL licensed packages
 * ``http://repo.continuum.io/pkgs/free`` - non GPL open source packages
To view all available packages, you can use ``conda search --all``.  See the :ref:`search command examples <search_example>` for more information.

Once an Anaconda package has been downloaded, it is said to be **locally available**.
A locally available package that has been linked into an Anaconda environment is said to be **activated**.
Conversely, unlinking a package from an environment causes it to be **deactivated**.


.. _location:
.. index::
    pair: terminology; location

.. _known:
.. index::
    pair: terminology; known

Since Anaconda environments are simply directories, they may be created anywhere. However, Anaconda has a notion of **locations** which are also simply directories that are known to ``conda``, and contain environments within. Anaconda environments created in such locations are said to be **known**, and can be displayed for easy reference. Anaconda has a default system location, but additional locations may be specified (see `Directory Structure`_ and Configuration_, respectively, for more details).


--------------------------
Package Naming Conventions
--------------------------

Names and versions of software packages do not follow any prescribed rules.  However, in order to facilitate communication and documentation, Anaconda employs the following naming conventions with respect to packages:

.. _package_name:
.. index::
    pair: terminology; package name
    seealso: name; package name

**package name**
    The name of a package, without any reference to a particular version. Anaconda package names are normalized, and may contain only lowercase alpha characters, numeric digits, underscores, or hyphens. In usage documentation, these will be referred to by ``package_name``.

.. _package_version:
.. index::
    pair: terminology; package version
    seealso: name; package version

**package version**
    A version number or string, often similar to *X.Y* or *X.Y.Z*, but may take other forms as well.

.. _build_string:
.. index::
    pair: terminology; build string
    seealso: name; build string

**build string**
    An arbitrary string that identifies a particular build of a package for Anaconda. It may contain suggestive mnemonics but these are subject to change and should not be relied upon or attempted to be parsed for any specific information.

.. _canonical_name:
.. index::
    pair: terminology; canonical name
    seealso: name; canonical name

**canonical name**
    The canonical name consists of the package name, version, and build string joined together by hyphens: *name*-*version*-*buildstring*. In usage documentation, these will be referred to by ``canonical_name``.

.. _filename:
.. index::
    pair: terminology; filename

**file name**
    Anaconda package filenames are canonical names, plus the suffix *.tar.bz2*.


These components are illustrated in the following figure:

.. figure::  images/conda_names.png
   :align:   center

   Different parts of Anaconda package names.

.. _package_spec:
.. index::
    pair: terminology; package specification
    seealso: package spec; package specification

Additionally, a **package specification** is a package name, together with a package version (which may be partial or absent), joined by "=". Here are some examples:

* *python=2.7.3*
* *python=2.7*
* *python*

In usage documentation, these will be referred to by ``package_spec``.

.. _meta_package:

-------------
Meta-Packages
-------------
``conda`` also provides the notion of **meta-packages**. A meta-package is an Anaconda package that contains a list of explicit
packages to install without any further dependency checking. When installing a meta-package, its listed packages override and will replace any existing package versions that may already be installed in an Anaconda environment. When creating, updating, or installing into environments, only one meta-package may be specified, and no additional packages may be specified.

.. _directory_structure:

-------------------
Directory Structure
-------------------

The Anaconda installation has the following directory structure:

*ROOT_DIR*
    The directory that Anaconda was installed into; for example, */opt/anaconda* or *C:\\Anaconda*

    */pkgs*
        Also referred to as *PKGS_DIR*. This directory contains exploded packages, ready to be activated in Anaconda environments. Each package resides in a subdirectory corresponding to its canonical name.

    */envs*
        The system location for additional Anaconda environments to be created.

    |   */bin*
    |   */include*
    |   */lib*
    |   */share*
    |       These subdirectories comprise the default Anaconda environment.

Other Anaconda environments contain the same subdirectories as the default environment, and may be located anywhere on the same filesystem as *PKGS_DIR*.

.. _config:

-------------
Configuration
-------------
There is very little user configuration that ``conda`` requires; however, ``conda`` will read minimal configuration from a *$HOME/.condarc* file, if it is present. The *.condarc* file follows simple `YAML syntax`_.

Here is an example:

.. code-block:: bash

    # This is the default conda runtime configuration

    # channel locations. These override conda defaults, i.e., conda will
    # search *only* the channels listed here, in the order given.
    channels:
      - http://repo.continuum.io/pkgs/dev
      - http://repo.continuum.io/pkgs/gpl
      - http://repo.continuum.io/pkgs/free

    # environment locations. These locations are in *addition* to the system
    # location at $ROOT_DIR/envs.
    locations:
       - ~/envs


----------------------------------------------
Creating Python 3.3 or Python 2.6 environments
----------------------------------------------

Anaconda supports Python 2.6, 2.7 & 3.3.  The default is Python 2.7.

To get started, you need to create an environment using the :ref:`conda create <create_example>`
command.

.. code-block:: bash

    $ conda create -n py33 python=3.3 anaconda

Here, 'py33' is the name of the environment to create, and "anaconda" is the
meta-package that includes all of the actual Python packages comprising
the Anaconda distribution.  When creating a new environment and installing
the Anaconda meta-package, the numpy and python versions can be specified,
e.g. "numpy=1.7" or "python=3.3".

.. code-block:: bash

    $ conda create -n py26 python=2.6 anaconda

After the environment creation process completes, adjust your PATH variable
to point to this directory.  On Linux/MacOSX systems, this can be easily
done using:

.. code-block:: bash

    $ source activate <env name>

    # This command assumes ~/anaconda/bin/activate is the first 'activate' on your current PATH

This will modify your Bash PS1 to include the name of the environment.

.. code-block:: bash

   $ source activate myenv
   (myenv)$

On Windows systems, you should change or set the PATH manually.

Now you're ready to begin using the Python located in your created
environment.

If you would like to deactivate this environment and revert your PATH to its previous state, use:

.. code-block:: bash

    $ source deactivate


---------------------------------
Update Anaconda to latest version
---------------------------------

To update to the latest version of Anaconda, you must first ensure you have the latest version of conda:

.. code-block:: bash

    $ conda update conda

    # Now you are ready to update Anaconda

    $ conda update

Look here for additional :ref:`update examples <update_example>`.


.. _YAML syntax: http://en.wikipedia.org/wiki/YAML
