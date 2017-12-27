Integrations
============

Fever API
---------

Miniflux implements the Fever API in addition to its own REST API.
The Fever API allows you to use existing mobile applications to read your feeds instead of the web user interface.

To activate the Fever API, go into the integration section and choose a username/password.

.. image:: _static/fever.png

The API endpoint is :code:`https://example.org/fever/`

Compatible Apps
~~~~~~~~~~~~~~~

- `Reeder <http://reederapp.com/>`_ (iOS/Mac OS)
- `Unread <https://www.goldenhillsoftware.com/unread/>`_ (iOS)

Notes
~~~~~

- Saving an entry will add a new bookmark and save the article
- Only the JSON format is supported
- Refreshing feeds is not possible
- Links, sparks and kindlings are not supported

Pinboard
--------

You could save articles to `Pinboard <https://pinboard.in/>`_.

.. image:: _static/pinboard.png

To activate this service, go to the integration section and enter your Pinboard API credentials.
You must use the API token, not your password.

Instapaper
----------

You could save articles to `Instapaper <https://www.instapaper.com/>`_.

.. image:: _static/instapaper.png

To activate this service, go to the integration section and enter your Instapaper credentials.

Wallabag
--------

`Wallabag <https://wallabag.org/>`_ is a self hostable application for saving web pages.

.. image:: _static/wallabag.png

- The API URL is the root URL of your instance, for example if you have the hosted version use: :code:`https://app.wallabag.it/`.
- To create a new API client, go to the section "API clients management" and choose "Create a new client".
