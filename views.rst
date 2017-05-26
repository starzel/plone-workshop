Browser Views
=============

Edit ``browser/dojo.py``.

Use the method ``dojo`` to return something else. In the next example you will again access the ``context`` in which the view is called.

In a method of the Browser view the talk wich was ``context`` in the template is now accessed as ``self.content``.

.. code-block:: python

    def dojo(self):
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

    Changes in python-files are picked up by restarting Plone or using the addon ``plone.reload``: http://localhost:8080/@@reload
    To use ``plone.reload`` add it to :file:`buildout.cfg` in the part **instance**, re-run buildout and restart Plone.



plone.api
---------

Covers 20% of the tasks any Plone developer does 80% of the time.

Check if plone.api first if you are not shure how to do omething.

The api is divided in five sections. Here are examples for each:

* `Content`: `Create content <https://docs.plone.org/develop/plone.api/docs/content.html#create-content>`_
* `Portal`: `Send E-Mail <https://docs.plone.org/develop/plone.api/docs/portal.html#send-e-mail>`_
* `Groups`: `Grant roles to group <https://docs.plone.org/develop/plone.api/docs/group.html#grant-roles-to-group>`_
* `Users`: `Get get current user <https://docs.plone.org/develop/plone.api/docs/user.html#get-currently-logged-in-user>`_



Browser Views
-------------

* can be called by accessing it as a url in the browser. Append ``/dojo`` to any url and the view will be called.
* can be associated with a template (like ``dojo.pt``) to return some html.
* can be reused in your code using ``plone.api.content.get_view('<name of the view>', context, request)``. This allows you to reuse code and methods.

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

``typeinfo`` will now be "This is the talk 'Dexterity for the win' at http://localhost:8080/Plone/dexterity-for-the-win"

In this case the method ``__call__`` is used to execute some logic. You do not need a template do to so. Do not use the method ``__init__`` for that!

You would still need to register the view in configure.zcml:

.. code-block:: xml

    <browser:page
        name="some_view"
        for="*"
        class=".dojo.SomeOtherView"
        permission="zope2.View"
        />


Exercise 1
----------

* Create a new view callable as /@@demo_content in a new file :file:`demo.py`
* The view should create 5 talks each time it is called
* Use the docs at https://docs.plone.org/develop/plone.api/docs/content.html#create-content to find out how to do that.
* Only managers should be able to use the view (the permission is called **cmf.ManagePortal**)
* Reload the frontpage after calling the view
* Display a message about the results (https://docs.plone.org/develop/plone.api/docs/portal.html#show-notification-message)
* For extra credits use requests and http://www.icndb.com/api/ to populate the talks with funny jokes.

..  admonition:: Solution
    :class: toggle

    Add this to :file:`configure.zcml`:

    .. code-block:: xml

      <browser:page
          name="demo_content"
          for="*"
          class=".demo.DemoContent"
          permission="cmf.ManagePortal"
          />


    .. code-block:: python

        # -*- coding: utf-8 -*-
        from Products.Five import BrowserView
        from plone import api
        from plone.protect.interfaces import IDisableCSRFProtection
        from zope.interface import alsoProvides

        import json
        import logging
        import requests

        logger = logging.getLogger(__name__)


        class DemoContent(BrowserView):

            def __call__(self):
                self.create_talks()
                self.request.response.redirect(self.context.absolute_url())

            def create_talks(self, amount=5):
                """Create some talks"""

                alsoProvides(self.request, IDisableCSRFProtection)
                portal = api.portal.get()
                plone_view = api.content.get_view('plone', self.context, self.request)
                jokes = self.random_jokes(amount)
                for data in jokes:
                    joke = data['joke']
                    obj = api.content.create(
                        container=portal,
                        type='talk',
                        title=plone_view.cropText(joke, length=20),
                        description=joke,
                        type_of_talk='Talk',
                    )
                    logger.info("Created talk {0}".format(obj.absolute_url()))
                api.portal.show_message(
                    u'Created {0} talks!'.format(amount), self.request)

            def random_jokes(self, amount):
                jokes = requests.get(
                    'http://api.icndb.com/jokes/random/{0}'.format(amount))
                return json.loads(jokes.text)['value']


* How should such a method usually be called?
* Why does it need ``alsoProvides(self.request, IDisableCSRFProtection)``?



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
                brains = api.content.find(
                    portal_type='talk',
                    review_state='private',
                )
                for brain in brains:
                    obj = brain.getObject()
                    api.content.transition(obj, to_state='published')
                    results.append(obj.absolute_url())
                    logger.info('Published talk {0}'.format(obj.absolute_url()))

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


Read more: https://training.plone.org/5/mastering_plone/api.html
