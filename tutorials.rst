Installation Tutorials
======================

Here are a couple of tutorials to help you to install Miniflux.

Installing Miniflux on your own server
--------------------------------------

This example is for **Ubuntu 16.04**.
Let's say you want to install Miniflux on a VPS.

1. Install Postgresql: ``apt install postgresql``
2. Prepare the database:

.. code:: bash

    # Switch to the postgres user
    $ su - postgres

    # Create a database user for Miniflux
    $ createuser -P miniflux
    Enter password for new role: ******
    Enter it again: ******

    # Create a database for miniflux that belongs to our user
    $ createdb -O miniflux miniflux

    # Create the extension hstore as superuser
    $ psql miniflux -c 'create extension hstore'
    CREATE EXTENSION

3. Install Miniflux:

.. code:: bash

    # Download the latest Debian package from the release page
    # In this example, this is the version 2.0.5
    $ wget https://github.com/miniflux/miniflux/releases/download/2.0.5/miniflux_2.0.5_amd64.deb

    # Install the package
    $ dpkg -i miniflux_2.0.5_amd64.deb

    # Run the SQL migrations
    $ export DATABASE_URL=postgres://miniflux:secret@localhost/miniflux?sslmode=disable
    $ miniflux -migrate
    Current schema version: 0
    Latest schema version: 16
    Migrating to version: 1
    Migrating to version: 2
    Migrating to version: 3
    [...]

    # Create the first user
    $ miniflux -create-admin
    Enter Username: superman
    Enter Password: ******

    # Update the config file /etc/miniflux.conf
    # Add/Edit this lines:
    # DATABASE_URL=postgres://miniflux:secret@localhost/miniflux?sslmode=disable
    # LISTEN_ADDR=0.0.0.0:80
    $ vim /etc/miniflux.conf

    # Authorize Miniflux to listen on port 80
    $ setcap cap_net_bind_service=+ep /usr/bin/miniflux

    # Restart the process to take the new config values into consideration
    systemctl restart miniflux

    # Check the logs to make sure the process is running properly
    $ journalctl -u miniflux
    [INFO] Starting Miniflux...
    [INFO] [Worker] #0 started
    [INFO] [Worker] #1 started
    [INFO] [Worker] #2 started
    [INFO] [Worker] #3 started
    [INFO] Listening on "0.0.0.0:80" without TLS

4. Now, you can access to your Miniflux instance via ``http://your-server/``

Running Miniflux with Docker Compose
------------------------------------

You could use Docker to try quickly Miniflux on your local machine:

Create a ``docker-compose.yml`` file into a folder called ``miniflux`` for example.

.. code::

    version: '3'
    services:
      miniflux:
        image: miniflux/miniflux:2.0.6
        ports:
          - "80:8080"
        depends_on:
          - db
        environment:
          - DATABASE_URL=postgres://miniflux:secret@db/miniflux?sslmode=disable
          - RUN_MIGRATIONS=1
          - CREATE_ADMIN=1
          - ADMIN_USERNAME=admin
          - ADMIN_PASSWORD=test123
      db:
        image: postgres:10.1
        environment:
          - POSTGRES_USER=miniflux
          - POSTGRES_PASSWORD=secret

Then run ``docker-compose up`` and go to ``http://localhost/``.

After the first user has been created, you should remove the variables ``CREATE_ADMIN``, ``ADMIN_USERNAME`` and ``ADMIN_PASSWORD``.

Deploying Miniflux on Heroku
----------------------------

Since the version 2.0.6, you can deploy Miniflux on `Heroku <https://www.heroku.com/>`_ in few seconds.

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

Deploying Miniflux on AlwaysData
--------------------------------

`AlwaysData <https://www.alwaysdata.com/>`_ is a French shared hosting provider.
You can install Miniflux in few minutes on their platform.

1. Open an account
2. Via the admin panel, create a Postgresql database and define a user/password
3. Create a website, choose "User Program", use a custom shell-script, for example ``~/start.sh``

.. image:: _static/alwaysdata_1.png

4. Enable the SSH access and open a session `ssh account@ssh-account.alwaysdata.net`
5. Install Miniflux:

.. code:: bash

    wget https://github.com/miniflux/miniflux/releases/download/2.0.5/miniflux-linux-amd64
    mv miniflux-linux-amd64 miniflux
    chmod +x miniflux

6. Create a shell script to start miniflux, let's call it ``start.sh``:

.. code:: bash

    #!/bin/sh

    export LISTEN_ADDR=$ALWAYSDATA_HTTPD_IP:$ALWAYSDATA_HTTPD_PORT
    export DATABASE_URL="host=postgresql-xxxxx.alwaysdata.net dbname=xxxx user=xxxx password=xxx sslmode=disable"

    ~/miniflux

7. Make the script executable: ``chmod +x start.sh``
8. Run the db migrations and a create the first user:

.. code:: bash

    export DATABASE_URL=".... replace me...."
    ./miniflux -migrate
    ./miniflux -create-admin

9. Go to ``https://your-account.alwaysdata.net``

Via the admin panel, in Advanced > Processes, you can even see the Miniflux process running:

.. image:: _static/alwaysdata_2.png
