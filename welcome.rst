Welcome to the Plone Workshop!
==============================

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
