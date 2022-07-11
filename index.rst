==========================
 Packaging Python in 2022
==========================

.. figure:: _static/python-platypus.png
   :alt: cartoon platypus
   :align: right

Packaging
=========

.. rst-class:: build

- Packaging: turning a project into a distribution
- Project: collection of files managing python libraries and or executables and metadata about how to distribute them
   - usually a single python package
- Distribution: a file that an end-user can install to get a package at a specific version
   - often downloaded from the internet
   - does not necessarily commute with packaging

.. rst-class:: resource

https://packaging.python.org/en/latest/glossary/

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

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
- Upload this distribution
- Download this project

.. .. rst-class:: ribbon

.. .. image:: _static/swirl.svg

.. rst-class:: nfirst

.. figure:: _static/EP22logosmall.svg
   :width: 96px

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * you are the packager and I assume these are your goals leaving here today. Although you may have already started some
   * because you want others to download and use your distribution

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

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

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

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * it doesn't actually matter where the package lives within the project. For this example it will stay at the root

The pyproject.toml File
=======================

- Single location to describe all project metadata
   - including how to build a distribution from this project
- Not in itself a build tool
- Human and machine readable configuration

.. rst-class:: resource

https://toml.io

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. nextslide::

build-system Metadata:

- Specify all packages required to bootstrap the build of your project
   - there will be at least one: the project's build backend
- Specify the exact command to kick off a build of your project

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. nextslide::

Build Backends:

- setuptools
- flit
- maturin
- enscons
- poetry

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :alt: octocat

   @ucodery

.. nextslide::

::

   [build-system]
   requires = []
   build-backend = ""

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :alt: octocat

   @ucodery

.. note::

   * not every pyproject.toml must have a build-system. But all projects *should* have a pyproject.toml and *should* specify the build-backend even if nothing else
   * bulid-system.requires can be empty, but really how is that ever going to work?

.. nextslide::

::

   [build-system]
   requires = ["setuptools>=61"]
   build-backend = "setuptools.build_meta"

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :alt: octocat

   @ucodery

.. nextslide::

Project Metadata:

- Specify details about your project
   - pypi.org fields
   - solving information
   - documentation

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :alt: octocat

   @ucodery

.. nextslide::

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

.. rst-class:: resource

https://packaging.python.org/en/latest/specifications/declaring-project-metadata/

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :alt: octocat

   @ucodery

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

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * We now have a project, that contains a package
   * but we have not packaged the project yet
   * none of our metadata does anything - its all static (as we want it)
   * we must actively make a distribution

Distribution
================

- Single file representing a snapshot of the project
   - wheel (``.whl``)
   - sdist (``.tar.gz``)
- Packaging can involve creating more than one distribution
   - one sdist and one or more wheels per version release

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * wheels are binary (precompiled). wheels are zipfiles
   * sdists are source (all compilation happens on user's machine). sdists are (usually) tarballs

Build Frontends
===============

- Calls the backend specified in pyproject.toml
- Handles user interaction
- Not specified in the project metadata

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

  * choice of frontend is up to the end user
  * pyproject.toml is not callable. So the end user must have some way of executing our chosen backend
  * frontend also responsible for isolating the backend and providing requirements

.. nextslide::

Build Frontends:

- build
- flit
- maturin
- enscons
- poetry

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * some but not all backends are also frontends
   * some frontends are not backends

.. nextslide::

::

   pyproject-build

::

   europython
   └── dist
       ├── europython-2022.1-py2-py3-none-any.whl
       └── europython-2022.1.tar.gz

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * this is the command that actually calls the ``build`` package

Distribution
============

- Share distribution files with end-users
- Upload to pypi.org

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. nextslide::

- twine
- flit
- maturin
- poetry

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. nextslide::

::

   twine upload --repository-url=https://test.pypi.org/legacy/ dist/*
   twine upload dist/*

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * not necessary to do the first upload to test.pypi, but best practice
   * remember, once it is uploaded to the real pypi.org you CANNOT change it

Package Managers
================

- Install distributions into a python environment
- Download distributions
- Solve for dependencies
- May have to dispatch to build backends during installation

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * nothing so far has actually put our package on the PYTHONPATH
   * do not always download, can install from local cache
   * also determine the best distribution based on a project name and version range
   * msut build when installing from (local) source or sdist

.. nextslide::

- pip
- pipx
- pdm
- poetry

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

Packaging
=========

.. rst-class:: build

- Transform your package into a project
   - include a ``pyproject.toml``
   - select a build backend [setuptools]
   - record project metadata
- Build distributions from your project
   - snapshot your project at a version
   - create sdist and wheels [build]

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. nextslide::

.. rst-class:: build

- Upload this distribution
   - upload to a repository like pypi.org [twine]
- Install a package
   - download and unpack a distribution [pip]
   - solves based on version ranges and current platform
   - also fulfills all runtime requirements of the distribution

.. rst-class:: second

.. figure:: _static/Twitter-Mark.png
   :width: 32px
   :alt: twitter

   @ucodery

.. figure:: _static/GitHub-Mark-32px.png
   :width: 32px
   :alt: octocat

   @ucodery

.. note::

   * bonus package lifecycle steps not part of "packaging"
   * notice we upload a distribution but download a package


==========================
 Questions?
==========================

.. figure:: _static/python-platypus.png
   :alt: cartoon platypus
   :align: right
