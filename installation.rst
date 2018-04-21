Installation
============

You can download pre-compiled Miniflux binaries and packages on the releases page: `<https://github.com/miniflux/miniflux/releases>`_.

Manual Installation
-------------------

1. Copy the binary somewhere
2. Make the file executable: :code:`chmod +x miniflux`
3. Define the environment variable :code:`DATABASE_URL` if necessary
4. ``CREATE EXTENSION hstore`` in the database or specify a user with ``SUPERUSER`` privileges. (:ref:`Details <migrations-superuser>`)
5. Run the SQL migrations: :code:`miniflux -migrate`
6. Create an admin user: :code:`miniflux -create-admin`
7. Start the application: :code:`miniflux`

You should configure a process manager like systemd or supervisord to supervise the Miniflux daemon.
The Debian or RPM packages are doing that for you.

Debian Package Installation
---------------------------

You must have Debian >= 8 or Ubuntu >= 16.04.
When using the Debian package, the Miniflux daemon is supervised by systemd.

1. Install Debian package: :code:`dpkg -i miniflux_2.0.0_amd64.deb`
2. Check process status: :code:`systemctl status miniflux`
3. Define the environment variable :code:`DATABASE_URL` if necessary
4. Run the SQL migrations: :code:`miniflux -migrate`
5. Create an admin user: :code:`miniflux -create-admin`

Systemd reads the `environment variables <configuration.html>`_ from the file :code:`/etc/miniflux.conf`.
You must restart the service to take the new values into consideration.

The files to build the Debian packages are available here: `<https://github.com/miniflux/package-deb>`_.

RPM Package Installation
------------------------

You must have Fedora or Centos/Redhat >= 7.
When you use the RPM package, the Miniflux daemon is supervised by systemd.

1. Install Miniflux RPM: :code:`rpm -ivh miniflux-2.0.0-1.0.x86_64.rpm`
2. Define the environment variable :code:`DATABASE_URL` if necessary
3. Run the SQL migrations: :code:`miniflux -migrate`
4. Create an admin user: :code:`miniflux -create-admin`
5. Enable the systemd service: :code:`systemctl enable miniflux`
6. Start the process with systemd: :code:`systemctl start miniflux`
7. Check process status: :code:`systemctl status miniflux`

Systemd reads the `environment variables <configuration.html>`_ from the file :code:`/etc/miniflux.conf`.
You must restart the service to take the new values into consideration.

The files to build the RPM packages are available here: `<https://github.com/miniflux/package-rpm>`_.

Docker Usage
------------

Pull the image and run the container: :code:`docker run -d -p 80:8080 miniflux/miniflux:version` (Replace version).
You will probably need to pass some environment variables like the :code:`DATABASE_URL`.

You could also use Docker Compose. Here an example of :code:`docker-compose.yml` file:

.. code:: yaml

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
      db:
        image: postgres:10.1
        environment:
          - POSTGRES_USER=miniflux
          - POSTGRES_PASSWORD=secret


Remember that you still need to run the database migrations and create the first user:

.. code:: bash

    # Run database migrations
    docker exec -ti <container-name> /usr/local/bin/miniflux -migrate

    # Create the first user
    docker exec -ti <container-name> /usr/local/bin/miniflux -create-admin

The Dockerfile is available here: `<https://github.com/miniflux/docker>`_.

Another way of doing the same thing, is to populate the variables ``RUN_MIGRATIONS``, ``CREATE_ADMIN``, ``ADMIN_USERNAME`` and ``ADMIN_PASSWORD``.
