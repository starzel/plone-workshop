Browser Views
=============

Edit ``browser/dojo.py``.

Use the method ``dojo`` to return something else. In the next example you will again access the ``context`` in which the view is called.

In a method of the Browser view the talk wich was ``context`` in the template is now accessed as ``self.content``.

.. code-block:: python

    def dojo(self):
        """Add code here.
        """
        context = self.context
        title = context.title
        portal_type = context.portal_type
        url = context.absolute_url()
        return "This is the {0} '{1}' at {2}".format(portal_type, title, url)

The template ``dojo.pt`` still needs the tal-statement:

.. code-block:: xml

    <p tal:content="python: view.dojo()">
        Hello World!
    </p>

Open http://localhost:8080/Plone/dexterity-for-the-win/dojo and it will show you information on that talk.

.. note::

    Changes in python-files are picked up by restarting Plone or using ``plone.reload``: http://localhost:8080/@@reload



plone.api
---------

Covers 20% of the tasks any Plone developer does 80% of the time.

Check if plone.api first if you are not shure how to do omething.

The api is divided in five sections. Here are examples for each:

* `Content`: `Create content <http://docs.plone.org/external/plone.api/docs/content.html#create-content>`_
* `Portal`: `Send E-Mail <http://docs.plone.org/external/plone.api/docs/portal.html#send-e-mail>`_
* `Groups`: `Grant roles to group <http://docs.plone.org/external/plone.api/docs/group.html#grant-roles-to-group>`_
* `Users`: `Get user roles <http://docs.plone.org/external/plone.api/docs/user.html#get-user-roles>`_
* `Environment`: `Switch roles inside a block <http://docs.plone.org/external/plone.api/docs/env.html#switch-roles-inside-a-block>`_


Exercise 1
----------

Create 5 talks each time the new method ``create_talks`` is called.

* Use the docs at http://docs.plone.org/external/plone.api/docs/content.html#create-content to find out how to do that.
* Use ``logger.info("something")`` to print log-messages to the console.

..  admonition:: Solution
    :class: toggle

    .. code-block:: python

        # -*- coding: UTF-8 -*-
        from Products.Five.browser import BrowserView
        from plone import api

        import logging
        logger = logging.getLogger(__name__)


        class DojoView(BrowserView):
            """A browser view.
            """

            def create_talks(self, amount=5):
                """Create talks
                """
                portal = api.portal.get()

                for i in range(amount):
                    obj = api.content.create(
                        type='talk',
                        title='A talk',
                        container=portal)
                    logger.info("Created talk {0}".format(obj.absolute_url()))


Browser Views
-------------

* can be called by accessing it as a url in the browser. Append ``/dojo`` to any url and the view will be called.
* can be associated with a template (like ``dojo.pt``) to return some html.
* can be reused in your code using ``plone.api.content.get_view(context, '<name of the view>')``. This allows you to reuse code and methods. The method that returns.

The modified method ``dojo`` that returned information on the current object can be reused any time like this:

.. code-block:: python

    from Products.Five.browser import BrowserView
    from plone import api

    class SomeOtherView(BrowserView):

        def __call__(self):
            portal = api.portal.get()
            some_talk = portal['dexterity-for-the-win']
            dojo_view = plone.api.content.get_view(some_talk, 'dojo')
            typeinfo = dojo_view.dojo()

``typoinfo`` will now be "This is the talk 'Dexterity for the win' at http://localhost:8080/Plone/dexterity-for-the-win"

In this case the method ``__call__`` is used to execute some logic. You do not need a template do to so. Do not use the method ``__init__`` for that! You would still need to register the view in configure.zcml:

.. code-block:: xml

    <browser:page
        name="some_view"
        for="*"
        class=".dojo.SomeOtherView"
        permission="zope2.View"
        />


Exercise 2
----------

Modify the method ``create_talks`` to allow the user to pass the number of talks to be created as a query-string.

* Query-strings are stored on the request. You can access the request with ``self.request``.
* Query-strings are by default on as strings.

..  admonition:: Solution
    :class: toggle

    .. code-block:: python

        # -*- coding: UTF-8 -*-
        from Products.Five.browser import BrowserView
        from plone import api

        import logging
        logger = logging.getLogger(__name__)


        class DojoView(BrowserView):
            """A browser view.
            """

            def create_talks(self):
                """Create talks
                """
                amount = int(self.request.get(amount, 5))
                portal = api.portal.get()
                for i in range(amount):
                    obj = api.content.create(
                        type='talk',
                        title='A talk',
                        container=portal)
                    logger.info("Created talk {0}".format(obj.absolute_url()))


    Query-strings are automatically passed as parameters to the ``__call__``. So you could also do:

    .. code-block:: python

        # -*- coding: UTF-8 -*-
        from Products.Five.browser import BrowserView
        from Products.Five.browser.pagetemplatefile import ViewPageTemplateFile
        from plone import api

        import logging
        logger = logging.getLogger(__name__)


        class DojoView(BrowserView):
            """A browser view.
            """

            template = ViewPageTemplateFile("dojo.pt")

            def __call__(self, amount=5):
                self.amount = int(amount)
                return self.template()

            def create_talks(self):
                """Create talks
                """
                portal = api.portal.get()
                for i in range(self.amount):
                    obj = api.content.create(
                        type='talk',
                        title='A talk',
                        container=portal)
                    logger.info("Created talk {0}".format(obj.absolute_url()))

    In this case you have to render the template by hand, because that is usually taken care of the inherited ``__call__``-method of your browser view.


portal-tools
------------

Some parts of Plone are very complex modules in themselves and have an api.

Here are a few examples:

portal_catalog
    ``unrestrictedSearchResults()`` returns search-results without checking if the current user has the permission to access the objects.

    ``uniqueValuesFor()`` returns all entries in a index

portal_setup
    ``runAllExportSteps()`` generates a tarball containing artifacts from all export steps.

portal_quickinstaller
    ``isProductInstalled()`` checks wether a product is installed.

Look in the ``interfaces.py`` in the respective package and read the docstrings.



Exercise 3
----------

Find all private talks in the page and publish them. Display a html-list of the published items.

* Use the tool ``portal_catalog`` to query for types.
* Use ``plone.api.content.transition`` to publish.
* There are some pittfalls ;-)

..  admonition:: Solution
    :class: toggle

    .. code-block:: python

        # ...

        class DojoView(BrowserView):

            # ...

            def publish_all_talks(self):
                """Publish all private talks
                """
                results = []
                portal_catalog = api.portal.get_tool('portal_catalog')
                brains = portal_catalog(
                    portal_type="talk",
                    review_state="private",
                )
                for brain in brains:
                    obj = brain.getObject()
                    api.content.transition(obj, to_state='published')
                    results.append(obj.absolute_url())
                    logger.info("Published talk {0}".format(obj.absolute_url()))

                return results

    Add this to the template ``dojo.pt``:

    .. code-block:: html

        <h2>Published talks:</h2>
        <ul tal:define="talks python:view.publish_all_talks()">
            <li tal:repeat="talk talks"
                tal:content="talk">
            </li>
            <li tal:condition="not: talks">
                No talks published
            </li>
        </ul>


Look at ``views.py`` to see a more advanced example.



Debugging
---------

tracebacks and the log
    The log (and the console when running in foreground) collect all log-messages Plone prints. When a exception occurs Plone thows a traceback. Most of the time the traceback is everything you need to find out what is going wrong. Also adding your own information to the log is very simple.

pdb
    The python debugger pdb is the single most important tool for us when programming. Just add ``import pdb; pdb.set_trace()`` in your code and debug away!

Products.PDBDebugMode
    A addon that has two killer-features.

    **Post-mortem debugging**: throws you in a pdb whenever a exception occurs. This way you can find out what is going wrong.

    **pdb-view**: simply adding ``/pdb`` to a url drops you in a pdb-session with the current context as ``self.context``. From there you can do just about anything.

plone.reload
    An addon that allows to reload code that you changed without restarting the site. It is also used by plone.app.debugtoolbar.


Read more: http://plone-training.readthedocs.org/en/latest/api.html
