Configuration
=============

Miniflux doesn't use any config file, **only environment variables**.

- :code:`DEBUG`: Toggle debug output (default is off)
- :code:`WORKER_POOL_SIZE`: Number of background processes (default=5)
- :code:`POLLING_FREQUENCY`: Refresh interval in minutes for feeds (default=60)
- :code:`BATCH_SIZE`: Number of feeds to send to the queue for each interval (default=10)
- :code:`DATABASE_URL`: Connection URL to connect to Postgresql (default=postgres://postgres:postgres@localhost/miniflux2?sslmode=disable)
- :code:`DATABASE_MAX_CONNS`: Number of concurrent database connections (default=20)
- :code:`LISTEN_ADDR`: HTTP server address (default=127.0.0.1:8080)
- :code:`BASE_URL`: Base URL (default=http://localhost/)
- :code:`SESSION_CLEANUP_FREQUENCY`: Session garbage collector frequency (Default is 24 hours)
- :code:`HTTPS`: Force cookies to use secure flag (default is empty, try to detect automatically if HTTPS is used)
- :code:`CERT_FILE`: SSL certificate (default="")
- :code:`KEY_FILE`: SSL private key (default="")
- :code:`CERT_DOMAIN`: Use Let's Encrypt to configure automatically a certificate for this domain (default="")
- :code:`CERT_CACHE`: Let's Encrypt cache directory (default="/tmp/cert_cache")
- :code:`OAUTH2_PROVIDER`: OAuth2 provider to use, at this time only "google" is supported (default="")
- :code:`OAUTH2_CLIENT_ID`: OAuth2 client ID (default="")
- :code:`OAUTH2_CLIENT_SECRET`: OAuth2 client secret (default="")
- :code:`OAUTH2_REDIRECT_URL`: OAuth2 redirect URL (default="")
- :code:`OAUTH2_USER_CREATION`: Set to 1 to authorize user creation (default=0)
- :code:`DISABLE_HSTS`: Disable HTTP Strict Transport Security header (enabled by default if HTTPS)
- :code:`RUN_MIGRATIONS`: Run database migrations, set to value to ``1`` (default="")
- :code:`CREATE_ADMIN`: Create admin user automatically, set value to ``1`` (default="")
- :code:`ADMIN_USERNAME`: Admin user login, used only if ``CREATE_ADMIN`` is enabled (default="")
- :code:`ADMIN_PASSWORD`: Admin user password, used only if ``CREATE_ADMIN`` is enabled (default="")

Database Connection Parameters
------------------------------

Miniflux use the `Golang library pq <https://github.com/lib/pq>`_ to communicate with Postgres.
Connection parameters are available `on this page <https://godoc.org/github.com/lib/pq#hdr-Connection_String_Parameters>`_.

The default value for :code:`DATABASE_URL` is :code:`postgres://postgres:postgres@localhost/miniflux2?sslmode=disable`.
You don't necessary need to use the URL format.

If you would like to connect via a Unix socket, you could do:

.. code:: bash

    export DATABASE_URL="user=postgres password=postgres dbname=miniflux2 sslmode=disable host=/path/to/socket/folder"
    ./miniflux

.. warning:: Password that contains special characters like ``^`` might be rejected since Miniflux 2.0.3.
             Golang v1.10 is `now validating the password <https://go-review.googlesource.com/c/go/+/87038>`_ and will return this error: ``net/url: invalid userinfo``.
             To avoid this issue, do not use the URL format for ``DATABASE_URL`` or make sure the password is URL encoded.

Running Miniflux on port 443 or 80
----------------------------------

Ports less than 1024 are reserved for privileged users.
If you have installed Miniflux with the RPM or Debian package, systemd run the process as the `miniflux` user.

To give Miniflux the ability to bind to privileged ports as a non-root user, add the capability `CAP_NET_BIND_SERVICE` to the binary:

.. code:: bash

    setcap cap_net_bind_service=+ep /usr/bin/miniflux

Check that the capability is added:

.. code:: bash

    getcap /usr/bin/miniflux
    /usr/bin/miniflux = cap_net_bind_service+ep

Here, we assume you installed the Miniflux binary into /usr/bin.

Let's Encrypt Integration
-------------------------

You could use Let's Encrypt to handle the SSL certificate automatically and activate HTTP/2.0.

.. code:: bash

    export CERT_DOMAIN=my.domain.tld
    miniflux

- Your server must be reachable publicly on port 443 and port 80 (http-01 challenge)
- In this mode, :code:`LISTEN_ADDR` is automatically set to :code:`:https`
- A cache directory is required, by default :code:`/tmp/cert_cache` is used, it could be overrided by using the variable :code:`CERT_CACHE`

.. note:: Miniflux supports http-01 challenge since the version 2.0.2

Manual HTTPS Configuration
--------------------------

Here an example to generate your self-signed certificate:

.. code:: bash

    # Generate the private key:
    openssl genrsa -out server.key 2048
    openssl ecparam -genkey -name secp384r1 -out server.key

    # Generate the certificate:
    openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650

Start the server like this:

.. code:: bash

    # Configure the environment variables:
    export CERT_FILE=/path/to/server.crt
    export KEY_FILE=/path/to/server.key
    export LISTEN_ADDR=":https"

    # Start the server:
    miniflux

Then you can access to your server by using an encrypted connection with the HTTP/2.0 protocol.

OAuth2 Authentication
---------------------

OAuth2 allows you to sign in with an external provider.
At this time, only Google is supported.

Google
~~~~~~

1. Create a new project in Google Console
2. Create a new OAuth2 client
3. Set an authorized redirect URL: :code:`https://my.domain.tld/oauth2/google/callback`
4. Define the OAuth2 environment variables and start the process

.. code:: bash

    export OAUTH2_PROVIDER=google
    export OAUTH2_CLIENT_ID=replace_me
    export OAUTH2_CLIENT_SECRET=replace_me
    export OAUTH2_REDIRECT_URL=https://my.domain.tld/oauth2/google/callback

    miniflux

Now from the settings page, you can link your existing user to your Google account.

If you would like to authorize anyone to create user account, you must set :code:`OAUTH2_USER_CREATION=1`.
Since Google do not have the concept of username, the email address is used as username.

Reverse-Proxy Configuration with Subfolder
------------------------------------------

Since the version 2.0.2, you can host your Miniflux instance under a subfolder.

You must define the environment variable :code:`BASE_URL` for Miniflux, for example:

.. code:: bash

    export BASE_URL=http://example.org/rss/

You can use the reverse-proxy software of your choice, here an example with Nginx:

.. code:: bash

    location /rss/ {
        proxy_pass http://127.0.0.1:8080/rss/;
        proxy_set_header Host $host;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

This example assume that you are running the Miniflux daemon on `127.0.0.1:8080`.

Now you can access to your Miniflux instance at `http://example.org/rss/`.
In this configuration, cookies are using the path `/rss`.
