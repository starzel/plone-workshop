Templates
=========

A simple view with a template
-----------------------------

Browser views are the swiss army knife of Plone Programmer.

Open ``http://localhost:8080/Plone/dojo``.

``dojo`` is a browser view provided by the package ``plone.dojo``.

It is registered in ``browser/configure.zcml``

.. code-block:: xml

    <browser:page
        name="dojo"
        for="*"
        class=".dojo.DojoView"
        template="dojo.pt"
        permission="zope2.View"
        />

This refers to a python-class ``DojoView`` that is found in ``browser/dojo.py``

.. code-block:: python

    # -*- coding: UTF-8 -*-
    from Products.Five.browser import BrowserView
    from plone import api

    import logging
    logger = logging.getLogger(__name__)


    class DojoView(BrowserView):
        """A browser view.
        """

        def dojo(self):
            """Add code here.
            """

            return "Hello Plone!"

And to a template ``dojo.pt`` that is ``browser/dojo.pt``.

.. code-block:: html

    <h1>
        ${python: view.dojo()}
    </h1>

This template uses tal to modify the rendered html.

.. note::

    Learn all about tal and page templates at https://training.plone.org/5/mastering_plone/zpt.html

    You should prefer python-statements in templates over path-statements because python makes it clear that ``dojo`` is a method of ``view`` instead of a subobject, a attribute or a callable. As a path-statement the above would look like this: ``tal:content="view/dojo"``.


Exercise 1
----------

Change the template ``dojo.pt`` to also display the title of the current object:

..  admonition:: Solution
    :class: toggle

    .. code-block:: html

        <h1>${python: context.title}</h1>


Exercise 2
----------

Create a talk "Dexterity for the win!" and add information to all fiels, especially the speaker and the email-adress.

Now call the view ``dojo`` on that talk by opening http://localhost:8080/Plone/dexterity-for-the-win/dojo in the browser.

For that talk, modify the template ``dojo.pt``:

* Render a mail-link to the speaker.
* Display the speaker instead of the raw email-adress.
* If there is no speaker-name display the adress.
* To modify attributes of html-tags my adding your statements into the attributes directly like ``title="${python: context.type_of_talk.capitalize()}"``.

..  admonition:: Solution
    :class: toggle

    .. code-block:: html

        <a href="${python: 'mailto:{0}'.format(context.email)}">
           ${python: context.speaker if context.speaker else context.email}
        </a>

.. note::

    Alternatively you can also use ``tal:attributes="<attr> <value>"`` to modify attributes.


Exercise 3
----------

Wrap the code in ``dojo.pt`` in the following code:

.. code-block:: xml

    <html metal:use-macro="context/main_template/macros/master">
    <metal:main fill-slot="main">

        <!-- Leave your code here! -->

    </metal:main>
    </html>

Replace the ``main` in ``fill-slot="main"`` with ``content-core`` and see what changes.
