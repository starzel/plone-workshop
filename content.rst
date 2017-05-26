Content
=======

Dexterity allows admininstrators to create content-types through the web.


Creating a new type
-------------------

* Go to http://localhost:8080/Plone/@@dexterity-types
* Add a new content type "Talk" and some fields for it:

  * Add Field "Type of talk", type "Choice". Add options: Talk, Keynote, Training
  * Add Field "Details", type "Rich Text" with a maximal length of 2000
  * Add Field "Audience", type "Multiple Choice". Add options: Beginner, Advanced, Professionals

* Test the content type
* Return to the control panel http://localhost:8080/Plone/@@dexterity-types
* Extend the new type

  * "Speaker", type: "Text line"
  * "Email", type: "Email"
  * "Image", type: "Image", not required
  * "Speaker Biography", type: "Rich Text"

* Test again





Behaviors
---------

In the Dexerity controlpanel select **talk**, select the tab **behaviors** and enable the behavior **Event Basic** (http://localhost:8080/Plone/dexterity-types/talk/@@behaviors).



.. seealso::

    Read more: https://training.plone.org/5/mastering_plone/dexterity.html
