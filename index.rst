==========================
 Packaging Python in 2022
==========================

.. rst-class:: right

.. image:: _static/python-platypus.png
   :alt: cartoon platypus

Packaging
=========

.. rst-class:: build

- Packaging: turning a project into a distribution
- Project: collection of files managing python libraries and or executables and metadata about how to distribute them
   - usually a single python package
- Distribution: a file that an end-user can install to get a package at a specific version
   - often downloaded from the internet

:ref:`https://packaging.python.org/en/latest/glossary/`

.. include:: ./trim.rst

.. note::

   * packaging is not commutative
      * installed distributions cannot always be transformed back into a dist
   * "package" is the only term python interpreter has an optionion about
   * "packaging" not defined by either pypa or python, but pypa probably would call it "releasing a project"
   * attrs: single library
   * pip: single executable
   * black: both a library and several executables

Packager
========

.. rst-class:: build

- Transform your package into a project
- Build a distribution from your project
- Publish this distribution
- Download this project

.. .. rst-class:: ribbon

.. .. image:: _static/swirl.svg

.. note::

   * you are the packager and I assume these are your goals leaving here today. Although you may have already started some
   * because you want others to download and use your distribution

.. include:: ./trim.rst

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

.. include:: ./trim.rst

.. note::

   * the exact layout to be a package, and importable, is up to the langugae, and out of scope for this talk
   * these files, in this structure is what you want to end up on an end-user's machine
   * the rest of this talk will be about how to do that

The Project
===========

::

   europython
   ├── pyproject.toml
   └── europython
       ├── __init__.py
       ├── keynotes.py
       └── sessions
          ├── __init__.py
          ├── posters.py
          ├── talks.py
          └── tutorials.py

.. include:: ./trim.rst

.. note::

   * it doesn't actually matter where the package lives within the project. For this example it will stay at the root

pyproject.toml
==============

- Single location to describe all project metadata
   - including how to build a distribution from this project
- Not in itself a build tool
- Human and machine readable configuration

:ref:`https://toml.io`

.. include:: ./trim.rst

.. note::

   * pyproject it tool agnostic
   * toml format is independent of python
   * all build tools can share pyproject.toml

Build System Metadata
=====================

- Specify all packages required to bootstrap the build of your project
   - there will be at least one: the project's build backend
- Specify the location of entrypoints within a build backend
   - all backends support a common interface

.. include:: ./trim.rst

.. note::

   * choice of backend is up to the project author
   * a project can only ever have one backend (at a time)
   * series of public functions and their arguments defined by PEPs

Build Backends:
===============

- setuptools
- flit
- maturin
- enscons
- poetry

.. include:: ./trim.rst

pyproject.toml
==============

::

   [build-system]
   requires = []
   build-backend = ""

.. include:: ./trim.rst

.. note::

   * not every pyproject.toml must have a build-system. But all projects *should* have a pyproject.toml and *should* specify the build-backend even if nothing else

.. nextslide::

::

   [build-system]
   requires = ["setuptools>=61"]
   build-backend = "setuptools.build_meta"

.. include:: ./trim.rst

Project Metadata:
=================

- Specify details about your project
   - pypi.org fields
   - solving information
   - documentation

.. include:: ./trim.rst

pyproject.toml
==============

::

   [build-system]
   requires = ["setuptools>=58", "wheel"]
   build-backend = "setuptools.build_meta"
   
   [project]
   name = "europython"
   version = "2022.1"
   description = "by the community and for the community"
   authors = [{"Jeremiah Paige", email = "jeremiah@example.com"}]
   dependencies = ["EPS"]

   [project.urls]
   schedule = "https://ep2022.europython.eu/schedule/2022-07-15"
   FAQ = "https://ep2022.europython.eu/faq"
   source = "https://github.com/EuroPython/website"

:ref:`https://packaging.python.org/en/latest/specifications/declaring-project-metadata/`

.. include:: ./trim.rst

.. note::

   * just some of the many project entries

The Project
===========

::

   europython
   ├── pyproject.toml
   └── europython
       ├── __init__.py
       ├── keynotes.py
       └── sessions
          ├── __init__.py
          ├── posters.py
          ├── talks.py
          └── tutorials.py

.. include:: ./trim.rst

.. note::

   * We now have a project, that contains a package
   * but we have not packaged the project yet
   * none of our metadata does anything - its all static (as we want it)
   * we must actively make a distribution

Build Frontends
===============

- Calls the backend specified in pyproject.toml
- Handles user interaction
- Not specified in the project metadata

.. include:: ./trim.rst

.. note::

  * choice of frontend is up to the end user
  * pyproject.toml is not callable. So the end user must have some way of executing our chosen backend
  * frontend also responsible for isolating the backend and providing requirements

.. nextslide::

- build
- flit
- maturin
- enscons
- poetry

.. include:: ./trim.rst

.. note::

   * some but not all backends are also frontends
   * some frontends are not backends

.. nextslide::

::

   pyproject-build

::

   europython
   ├── dist
   │   ├── europython-2022.1-py2-py3-none-any.whl
   │   └── europython-2022.1.tar.gz
   ├── pyproject.toml
   └── europython
       ├── __init__.py
       ├── keynotes.py
       └── sessions
          ├── __init__.py
          ├── posters.py
          ├── talks.py
          └── tutorials.py

.. include:: ./trim.rst

.. note::

   * this is the command that actually calls the ``build`` package

Distribution
================

- Single file representing a snapshot of the project
   - wheel (``.whl``)
   - sdist (``.tar.gz``)
- Packaging can involve creating more than one distribution
   - one sdist and one or more wheels per version release

.. include:: ./trim.rst

.. note::

   * wheels are binary (precompiled). wheels are zipfiles
   * sdists are source (all compilation happens on user's machine). sdists are (usually) tarballs

Publish
=======

- Share distribution files with end-users
- Upload to pypi.org

.. include:: ./trim.rst

.. nextslide::

- twine
- flit
- maturin
- poetry

.. include:: ./trim.rst

.. nextslide::

::

   twine upload --repository-url=https://test.pypi.org/legacy/ dist/*
   twine upload dist/*

.. include:: ./trim.rst

.. note::

   * not necessary to do the first upload to test.pypi, but best practice
   * remember, once it is uploaded to the real pypi.org you CANNOT change it

Package Managers
================

- Install distributions into a python environment
- Download distributions
- Solve for dependencies
- May have to dispatch to build backends during installation

.. include:: ./trim.rst

.. note::

   * nothing so far has actually put our package on the PYTHONPATH
   * do not always download, can install from local cache
   * also determine the best distribution based on a project name and version range
   * must build when installing from (local) source or sdist

.. nextslide::

- pip
- pipx
- pdm
- poetry

.. include:: ./trim.rst

.. nextslide::

::

   pip install 'europython>=2022'

.. include:: ./trim.rst

Packaging
=========

.. rst-class:: build

- Transform your package into a project
   - include a ``pyproject.toml``
   - select a build backend [``setuptools``]
   - record project metadata
- Build distributions from your project
   - snapshot your project at a version
   - create sdist and wheel [``build``]

.. include:: ./trim.rst

.. nextslide::

.. rst-class:: build

- Publish these distributions
   - upload to a repository like pypi.org [``twine``]
- Install a package
   - download and unpack a distribution [``pip``]
   - solves based on version ranges and current platform
   - also fulfills all runtime requirements of the distribution

.. include:: ./trim.rst

.. note::

   * bonus package lifecycle steps not part of "packaging"
   * notice we upload a distribution but download a package

.. include:: ./trim.rst

==========================
 Questions?
==========================

:ref:`https://github.com/ucodery/packaging-python-in-2022`

.. include:: ./foot.rst

.. rst-class:: right

.. image:: _static/python-platypus.png
   :alt: cartoon platypus
