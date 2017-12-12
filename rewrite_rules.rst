Rewrite Rules
=============

To improve the reading experience, it's possible to alter the content of feed items.

For example, if you are reading a popular comic website like XKCD, it's nice to have to image title (alt attribute) added under the image.
Especially on mobile devices when there is no hover event.

List of Rules
-------------

- :code:`add_image_title`: Get the first image and add the title under the image.
- :code:`add_youtube_video`: Insert Youtube video inside the article (automatic for Youtube.com)

Adding Rewrite Rules
--------------------

Miniflux includes a set of default rules for some websites, but you could define your own rules.

On the feed edit page, enter your custom rules in the field "Rewrite Rules" like this:

.. code::

    rule1,rule2

Separate each rule by a comma.
As of now, only :code:`add_image_title` is available, but later other rules could be added.
