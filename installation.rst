Installation
============

Manual Installation
-------------------

1. Copy the binary somewhere
2. Make the file executable: :code:`chmod +x miniflux`
3. Define the environment variable :code:`DATABASE_URL` if necessary
4. Run the SQL migrations: :code:`miniflux -migrate`
5. Create an admin user: :code:`miniflux -create-admin`
6. Start the application: :code:`miniflux`

RPM Package Installation
------------------------

If you use a Fedora or Centos/Redhat > 7:

1. Install Miniflux RPM: :code:`rpm -ivh miniflux-2.0.0-1.0.x86_64.rpm`
2. Run the SQL migrations: :code:`miniflux -migrate`
3. Create an admin user: :code:`miniflux -create-admin`
4. Start the process with systemd: :code:`systemctl start miniflux`
5. Check process status: :code:`systemctl status miniflux`

The `environment variables <configuration.html>`_ for Miniflux can be found in the file :code:`/etc/miniflux.conf`.
You must restart the service to take the new values into consideration.

Docker Usage
------------

Pull the image and run the container: :code:`docker run -d -p 80:8080 miniflux/miniflux:version` (Replace version).
You will probably need to pass some environment variables like the :code:`DATABASE_URL`.

You could also use Docker Compose. Here an example of :code:`docker-compose.yml` file:

.. code:: yaml

    version: '2'
    services:
      miniflux:
        image: miniflux/miniflux:replace-me
        ports:
          - "80:8080"
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
