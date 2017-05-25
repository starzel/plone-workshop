Content
=======

Dexterity allows admininstrators to create content-types through the web.


Creating a new type
-------------------

* Go to http://localhost:8080/Plone/@@dexterity-types
* Add a new content type "Talk" and some fields for it:

  * Add Field "Type of talk", type "Choice". Add options: talk, keynote, training
  * Add Field "Details", type "Rich Text" with a maximal length of 2000
  * Add Field "Audience", type "Multiple Choice". Add options: beginner, advanced, pro

* Test the content type
* Return to the control panel http://localhost:8080/Plone/@@dexterity-types
* Extend the new type

  * "Speaker", type: "Text line"
  * "Email", type: "Text line"
  * "Image", type: "Image", not required
  * "Speaker Biography", type: "Rich Text"

* Test again

Read more: https://training.plone.org/5/mastering_plone/dexterity.html
