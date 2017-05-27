Plone Workshop
==============

*by Philip Bauer*

Held at the PyConWEB in Munich, 27.05.2017

Installation
------------

Please run these commands to install Plone on your Laptop. Use Python 2.7:

.. code-block:: bash

    $ virtualenv plone-workshop
    $ cd plone-workshop
    $ source bin/activate
    $ pip install git+https://github.com/plone/bobtemplates.plone.git
    $ ./bin/mrbob -O plone.dojo bobtemplates:plone_addon

Answer the six questions with the default except for the Plone version. Here use the latest: 5.0.7

.. code-block:: text

    --> Plone version [5.0.6]: 5.0.7

.. code-block:: bash

    $ cd plone.dojo
    $ ../bin/python bootstrap-buildout.py
    $ ./bin/buildout

.. note::

    If you get errors about setuptools-versions like ``VersionConflict: ... Requirement.parse('zc.buildout==2.5.3'))`` simply restart the command ``./bin/buildout`` (up to three times...).

.. warning::

    ``./bin/buildout`` will take a *very looooooong time* since it installs 265 dependencies - yes, Plone is pretty big.


Start Plone on http://localhost:8080

..  code-block:: bash

    $ ./bin/instance fg



Plone
-----

* Plone is an open source Content Management System (CMS) built in Python.
* Plone has a multitude of powerful features, is easily accessible to editors but also fun for programmers.


The Workshop
------------

* The point of this Workshop is to show how easy it is to your the first steps as a Plone-programmer.
* In the first part we'll use some of the core-features and model our data
* In the second part we'll and display that data and custom features with python and addons



Contents:

..  toctree::
    :maxdepth: 0
    :numbered: 0

    installation
    demo
    content
    mosaic
    templates
    views



About me
--------

* Web-Developer from Munich
* I run the company Starzel.de since 2000
* I do Python and Plone since 2005
* Github: https://github.com/pbauer
* Twitter: https://twitter.com/StarzelDe
* bauer@starzel.de
* www.starzel.de
