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
    Latest schema version: 1
    Migrating to version: 1

Create Admin User
-----------------

.. code:: bash

    miniflux -create-admin
    Enter Username: root
    Enter Password:

Flush all Sessions
------------------

Flushing all sessions disconnect all users.

.. code:: bash

    miniflux -flush-sessions
    Flushing all sessions (disconnect users)
