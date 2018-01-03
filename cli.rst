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
