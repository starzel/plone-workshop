============
Installation
============

Use Python 2.7, a virtualenv and the scaffolding-generator `mr.bob` (similar to `cookiecutter`).

.. code-block:: bash

    $ virtualenv plone-workshop
    $ cd plone-workshop
    $ source bin/activate
    $ pip install git+https://github.com/plone/bobtemplates.plone.git
    $ ./bin/mrbob -O plone.dojo bobtemplates:plone_addon

You need to answer some questions. Answer with the default for all but Plone version. Here enter the latest version: 5.0.7

.. code-block:: text

    --> What kind of package would you like to create? Choose between 'Basic', 'Dexterity'. [Basic]:

    --> Author's name [Philip Bauer]:

    --> Author's email [bauer@starzel.de]:

    --> Author's GitHub username:

    --> Package description [An add-on for Plone]:

    --> Plone version [5.0.6]: 5.0.7


This creates the scaffolding for configuration and code for a plone-site. It is actually a full-grown python module (look at the file `setup.py`) and can be used as a real plone-addon. In the workshop we'll use it a a playground that configures Plone and allows us to add custom features.

The next steps install the dependencies and configures your Plone-instance.

.. code-block:: bash

    $ cd plone.dojo
    $ ../bin/python bootstrap-buildout.py
    $ ./bin/buildout

The **main** command is ``./bin/buildout``. It will take a very looooooong time since it installs 265 dependencies - yes, Plone pretty big. If you work with Plone regularly it is much faster because you use a buildout-cache (similar to a local pypi-server).

Now Plone is ready to run.


Running Plone
=============

Start Plone in *debug-mode*:

..  code-block:: bash

    $ ./bin/instance fg

Now when you open your browser on http://localhost:8080 you can see your site. You can stop Plone with ``ctrl+c``.

Run Plone in *production-mode*:

..  code-block:: bash

    $ ./bin/instance start

Stop Plone:

..  code-block:: bash

    $ ./bin/instance stop
