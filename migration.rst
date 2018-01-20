Migration from Miniflux 1.2.x
=============================

Miniflux 2.x is not backward compatible with Miniflux 1.x.

Differences between Miniflux 1.2 and Miniflux 2.0
-------------------------------------------------

- Miniflux 2 supports multiple attachments.
- Miniflux 2 uses categories instead of groups, only one category can be assigned to a feed.
- Miniflux 2 have fewer settings.
- Miniflux 2 doesn't have public RSS feed or cronjobs.
- Miniflux 2 doesn't use API tokens anymore, for the Fever API, choose your own password and for the REST API use your account password.
- Miniflux 2 stores favicons into the database instead of using the local filesystem.
- Miniflux 2 themes are embedded into the application.
- Miniflux 2 doesn't support RTL languages.
- Miniflux 2 supports only Postgresql.
- Miniflux 2 is written in Go (Golang) instead of PHP.

OPML Import
-----------

If you don't care about your previous data, export your feeds from Miniflux 1.x in OPML and import them into Miniflux 2.

Migration Script
----------------

There is a migration script in `Miniflux Legacy repository <https://github.com/miniflux/miniflux-legacy>`_: :code:`scripts/migrate-v2.php`.

- This script requires direct access to the old and the new database.
- The first group linked to a feed will become the category associated with the imported feed.
- Only bookmarked items are migrated.
- Since entries are not identified in the same way in Miniflux 2, you may have duplicated entries when refreshing your imported feeds.

Step 1
~~~~~~

Make sure you are using the latest version of Miniflux 1.2.x.

Step 2
~~~~~~

Install Miniflux 2 without creating any users. Create only the database schema (just run the migrations).

Step 3
~~~~~~

Go into the Miniflux 1.2.x directory and run the script:

.. code:: bash

    php scripts/migrate-v2.php --dsn="pgsql:host=localhost;dbname=miniflux2;user=postgres;password=postgres"

    Destination is "pgsql:host=localhost;dbname=miniflux2;user=postgres;password=postgres"
    * 2 user(s) to migrate
    * Migrating user: #254 => #284
    * Migrating integrations
    * Migrating categories
    * Migrating feeds
    * Migrating entries
    [...]

The script takes the `PDO DSN <http://php.net/manual/en/ref.pdo-pgsql.connection.php#refsect1-ref.pdo-pgsql.connection-examples>`_ of Miniflux 2 database as argument.
Adjust the parameters to your own environment.
