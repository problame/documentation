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

Let's Encrypt Integration
-------------------------

You could use Let's Encrypt to handle the SSL certificate automatically and activate HTTP/2.0.

.. code:: bash

    export CERT_DOMAIN=my.domain.tld
    miniflux

- Your server must be reachable publicly on port 443
- In this mode, :code:`LISTEN_ADDR` is automatically set to :code:`:https`
- A cache directory is required, by default :code:`/tmp/cert_cache` is used, it could be overrided by using the variable :code:`CERT_CACHE`
- In this mode, the Miniflux process must run with a privileged user like root because the listen port is lower than 1024,
  if you are not comfortable with that, do not use this feature, instead use a reverse proxy like Nginx.

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
