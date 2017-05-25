========
Features
========

Some of the many features of Plone
==================================

- Users and Roles
- Working with content
- Workflow
-


Addons
======

- > 2000 addons for Plone
- Their use, usefulness, quality and complexity varies a lot
- Finding the right addon can be hard
  - Ask in the User-group
  - The top 100 are soon on the new plone.com and plone.org


plone.dojo
----------

``plone.dojo`` is the addon you created and it is available for installation. You can find the code for it in the ``src/plone/dojo``-directory of the buildout.

We have to install it to enable its features.

* Go to http://localhost:8080/Plone/prefs_install_products_form
* Install ``plone.dojo 0.1``

The code is in ``<buidout-directory>/src/plone.dojo/src/plone/dojo/``. All paths from now on are relative to this. The weird directory-structure is needed to make a package testable by itself.

.. note::

    More about the features of Plone: http://plone-training.readthedocs.org/en/latest/features.html

    More about add-ons: http://plone-training.readthedocs.org/en/latest/addons.html
