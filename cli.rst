Command Line Usage
==================

Show Version
------------

.. code:: bash

    miniflux -version
    2.0.0

Show Build Information
----------------------

.. code:: bash

    miniflux -info
    Version: 2.0.0
    Build Date: 2017-11-20T22:45:00
    Go Version: go1.9

Enable Debug Mode
-----------------

.. code:: bash

    miniflux -debug
    [2018-02-05T02:38:32] [INFO] Debug mode enabled
    [2018-02-05T02:38:32] [INFO] Starting Miniflux...

Run Database Migrations
-----------------------

.. code:: bash

    miniflux -migrate
    Current schema version: 0
    Latest schema version: 12
    Migrating to version: 1
    Migrating to version: 2
    Migrating to version: 3
    Migrating to version: 4
    Migrating to version: 5
    Migrating to version: 6
    Migrating to version: 7
    Migrating to version: 8
    Migrating to version: 9
    Migrating to version: 10
    Migrating to version: 11
    Migrating to version: 12

When you run the migrations, make sure that all Miniflux processes are stopped.

SQL statements like :code:`create extension if not exists hstore` requires :code:`SUPERUSER` privileges.
Several solutions are available:

1) Give :code:`SUPERUSER` privileges to miniflux user only during the schema migration:

.. code:: sql

    ALTER USER miniflux WITH SUPERUSER;
    -- Run the migrations (miniflux -migrate)
    ALTER USER miniflux WITH NOSUPERUSER;

2) You could create the hstore extension as superuser before to run the migrations.

Create Admin User
-----------------

.. code:: bash

    miniflux -create-admin
    Enter Username: root
    Enter Password:

Reset User Password
-------------------

.. code:: bash

    miniflux -reset-password
    Enter Username: myusername
    Enter Password: ****

Flush all Sessions
------------------

Flushing all sessions disconnect all users.

.. code:: bash

    miniflux -flush-sessions
    Flushing all sessions (disconnect users)
