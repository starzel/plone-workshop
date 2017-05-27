========
Features
========

Some of the many features of Plone
==================================

Main features:

- Content in a object-tree
- Traversal (portal/folder/document)
- Object-Database: ZODB
- REST-API
- Python-api
- User-, Groups and Permission-Management
- Search engine
- Multilingual


Demo

- Working with content
- Users, Roles and Workflows
- Configuring Plone



Extend Plone With Add-On Packages
=================================

* There are more than 2,000 add-ons for Plone. We will cover only a handful today.
* Using them saves a lot of time
* The success of a project often depends on finding the right add-on
* Their use, usefulness, quality and complexity varies a lot


Some notable add-ons
---------------------

`eea.facetednavigation <https://pypi.python.org/pypi/eea.facetednavigation/>`_
  Create faceted navigation and searches through the web.

`plone.app.mosaic <https://github.com/plone/plone.app.mosaic>`_
  Layout solution to easily create complex layouts through the web.

`collective.geo <http://collectivegeo.readthedocs.io/en/latest/>`_
  Flexible bundle of add-ons to geo-reference content and display in maps

`collective.mailchimp <https://pypi.python.org/pypi/collective.mailchimp>`_
  Allows visitors to subscribe to mailchimp newsletters

`bda.plone.shop <https://github.com/bluedynamics/bda.plone.shop>`_
  Webshop Solution for Plone.

`collective.lineage <https://pypi.python.org/pypi/collective.lineage>`_
  Microsites for Plone - makes subfolders appear to be autonomous Plone sites


.. _add-ons-find-label:

How to find add-ons
-------------------

* https://plone.org/download/add-ons
* https://pypi.python.org/pypi - use the search form!
* https://github.com/collective >1200 repos
* https://github.com/plone >260 repos
* https://community.plone.org - ask the community
* google (e.g. `Plone+Slider <http://lmgtfy.com/?q=plone+slider>`_)
* ask in irc and on the mailing list

.. seealso::

   * A talk on finding and managing add-ons: https://www.youtube.com/watch?v=Sc6NkqaSjqw



plone.dojo
----------

``plone.dojo`` is the addon you created and it is available for installation. You can find the code for it in the ``src/plone/dojo``-directory of the buildout.

We have to install it to enable its features.

* Go to http://localhost:8080/Plone/prefs_install_products_form
* Install ``plone.dojo 0.1``

.. note::

    More about the features of Plone: https://training.plone.org/5/mastering_plone/features.html

    More about add-ons: https://training.plone.org/5/mastering_plone/addons.html
