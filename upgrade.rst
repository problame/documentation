Upgrading to a New Version
==========================

Procedure
---------

1. Disconnect all users by flushing all sessions: :code:`miniflux -flush-sessions`
2. Stop the process
3. Backup your database
4. Check that your backup is really working
5. Run database migrations: :code:`miniflux -migrate`
6. Start the process

Debian Package
--------------

Follow instructions mentioned above and run: :code:`dpkg -i miniflux_2.x.x_amd64.deb`.

RPM Package
-----------

Follow instructions mentioned above and run: :code:`rpm -Uvh miniflux-2.x.x-1.0.x86_64.rpm`.

Docker Image
------------

- Pull the new image with the new tag: :code:`docker pull miniflux/miniflux:2.x.x`
- Stop and remove the old container: :code:`docker stop <container_name> && docker rm <container_name>`
- Start a new container with the latest tag: :code:`docker run -d -p 80:8080 miniflux/miniflux:2.x.x`

If you use Docker Compose, define the new tag in the YAML file and restart the container.
