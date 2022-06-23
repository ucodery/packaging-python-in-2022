==========================
 Packaging Python in 2022
==========================

Packaging
=========

.. rst-class:: build

- Packaging: turning a project into a distribution
- Project: collection of files managing one or more python libraries and or executables and metadata about how to distribute it
   - usually a single python package
- Distribution: a file that an end-user can install to get a package at a specific version
   - often downloaded from the internet
   - does not necessarily commute with packaging

https://packaging.python.org/en/latest/glossary/

.. note::

   * attrs: single library
   * pip: single executable
   * black: both a library and several executables
   * "package" is the only term python interpreter has an optionion about
   * "packaging" not defined by either pypa or python, but pypa probably would call it "releasing a project"

Packager
========

- Transform your package into a project
- Build a distribution from your project
- upload this distribution

.. note::

   * you are the packager and I assume these are your goals leaving here today. Although you may have already started some

The Package
===========

::

   europython
   ├── __init__.py
   ├── keynotes.py
   └── sessions
      ├── __init__.py
      ├── posters.py
      ├── talks.py
      └── tutorials.py

.. nextslide::

::

   📦 europython

.. note::

   * the exact layout to be a package, and importable, is up to the langugae, and out of scope for this talk
   * these files, in this structure is what you want to end up on an end-user's machine
   * the rest of this talk will be about how to do that

The Project
===========

::

   europython
   ├── 📦 europython
   └── pyproject.toml

.. note::

   * it doesn't actually matter where the package lives within the project. For this example it will stay at the root

The pyproject.toml File
=======================

- This is where it all starts.

build-system metadata
=====================

- specify all packages required to bootstrap the build of your project
- specify the exact command to kick off a build of your project

Build Backends
==============

Backends:

- setuptools
- flit
- maturin
- enscons
- poetry

project metadata
================

- specify details about your project
   - pypi.org fields
   - solving information
   - documentation

The pyproject.toml File
=======================

::

   [build-system]
   requires = []
   build-backend = ""

- if you're going to have a pyproject.toml it better at least have build-system
- if you have build-system you must specify the build-backend
- if you specify a build-backend then you almost certainly have at least one requirement

.. note::

   * not every pyproject.toml must have a build-system. But all projects *should* have a pyproject.toml and *should* specify the build-backend even if nothing else
   * bulid-system.requires can be empty, but really how is that ever going to work?

.. nextslide::

::

   [build-system]
   requires = ["setuptools>=58", "wheel"]
   build-backend = "setuptools.build_meta"

.. nextslide::

::

   [build-system]
   requires = ["setuptools>=58", "wheel"]
   build-backend = "setuptools.build_meta"
   
   [project]
   name = "europython"
   version = 1.0.1

The Project
===========

::

   europython
   ├── 📦 europython
   └── pyproject.toml

.. note::

   * We now have a project, that contains a package
   * but we have not packaged the project yet
   * none of our metadata does anything - its all static (as we want it)
   * we must actively make a distribution

The Distribution
================

- single file representing a snapshot of the project
   - wheels - ``.whl``
   - sdists - ``.tar.gz``
- packaging can involve creating more than one distribution at a time 
   - one sdist and one or more wheels per version release

.. note::

   * wheels are binary (precompiled). wheels are zipfiles
   * sdists are source (all compilation happens on user's machine). sdists are (usually) tarballs

Build Frontends
===============

- this is what places the package somewhere python can import it
- not specified in the project metadata

.. note::

  * choice of frontend is up to the end user

.. nextslide::

- pip
- flit
- build
- poetry

.. note::

   * some but not all backends are also frontends
   * some frontends are not backends

Distribution
============

- upload to pypi.org

.. nextslide::

- twine
- flit
- poetry

Package Managers
================

This is probably how you interact with any given package

.. nextslide::

- pip
- pdm
- poetry

Packaging
=========

- Transform your package into a project

.. nextslide::

- Transform your package into a project
  - include a ``pyproject.toml``
  - select a build backend [setuptools]

.. nextslide::

- Transform your package into a project
  - include a ``pyproject.toml``
  - select a build backend [setuptools]
- Build a distribution from your project

.. nextslide::

- Transform your package into a project
  - include a ``pyproject.toml``
  - select a build backend [setuptools]
- Build a distribution from your project
  - create files that snapshot your project at a version
  - [pip]

.. nextslide::

- Transform your package into a project
  - include a ``pyproject.toml``
  - select a build backend [setuptools]
- Build a distribution from your project
  - create files that snapshot your project at a version
  - [pip]
- upload this distribution

.. nextslide::

- Transform your package into a project
  - include a ``pyproject.toml``
  - select a build backend [setuptools]
- Build a distribution from your project
  - snapshot your project at a version [pip]
  - create sdist and wheels
- upload this distribution
  - upload to a repository like pypi.org [twine]
