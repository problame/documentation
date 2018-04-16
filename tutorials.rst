Installation Tutorials
======================

Deploying Miniflux on Heroku
----------------------------

Since the version 2.0.6, you can deploy Miniflux on Heroku in few seconds.

1. Clone the repository on your machine: ``git clone https://github.com/miniflux/miniflux.git``
2. Create a new Heroku application: ``heroku apps:create``
3. Add the Postgresql addon: ``heroku addons:create heroku-postgresql:hobby-dev``
4. Add environment variables to setup the application:

.. code::

    # This parameter will create all tables in the database.
    heroku config:set RUN_MIGRATIONS=1

    # The following parameters will create the first user.
    heroku config:set CREATE_ADMIN=1
    heroku config:set ADMIN_USERNAME=admin
    heroku config:set ADMIN_PASSWORD=test123

5. Deploy the application on Heroku: ``git push heroku master``
6. After the application is installed successfully, you don't need these variables anymore:

.. code::

    heroku config:unset CREATE_ADMIN
    heroku config:unset ADMIN_USERNAME
    heroku config:unset ADMIN_PASSWORD

- To watch the logs, use ``heroku logs``.
- You can also run a one-off container to run the commands manually: ``heroku run bash``.
  The Miniflux binary will be located into the folder ``bin``.
- To update Miniflux, pull the new version from the repository and push to Heroku again.
