Configuration
=============

Miniflux doesn't use any config file, **only environment variables**.

- :code:`WORKER_POOL_SIZE`: Number of background processes (default=5)
- :code:`POLLING_FREQUENCY`: Refresh interval in minutes for feeds (default=60)
- :code:`BATCH_SIZE`: Number of feeds to send to the queue for each interval (default=10)
- :code:`DATABASE_URL`: Connection URL to connect to Postgresql (default=postgres://postgres:postgres@localhost/miniflux2?sslmode=disable)
- :code:`DATABASE_MAX_CONNS`: Number of concurrent database connections (default=20)
- :code:`LISTEN_ADDR`: HTTP server address (default=127.0.0.1:8080)
- :code:`BASE_URL`: Base URL (default=http://localhost/)
- :code:`CERT_FILE`: SSL certificate (default="")
- :code:`KEY_FILE`: SSL private key (default="")
- :code:`CERT_DOMAIN`: Use Let's Encrypt to configure automatically a certificate for this domain (default="")
- :code:`CERT_CACHE`: Let's Encrypt cache directory (default="/tmp/cert_cache")

Let's Encrypt Integration
-------------------------

You could use Let's Encrypt to handle the SSL certificate automatically and benefits of HTTP/2.0.

.. code:: bash

    export CERT_DOMAIN=my.domain.tld
    miniflux

- Your server must be reachable on Internet on port 443
- In this mode, :code:`LISTEN_ADDR` is automatically set to :code:`:https`
- A cache directory is required, by default :code:`/tmp/cert_cache` is used, it could be overrided by using the variable :code:`CERT_CACHE`

Manual HTTPS Configuration
--------------------------

This configuration allows you to use the HTTP/2.0 protocol.

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

Then you can access to your server by using HTTPS.
