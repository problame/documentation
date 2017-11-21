Configuration
=============

Miniflux doesn't use any config file, **only environment variables**.

- :code:`WORKER_POOL_SIZE`: Number of background processes (default=5)
- :code:`POLLING_FREQUENCY`: Refresh interval in minutes for feeds (default=60)
- :code:`BATCH_SIZE`: Number of feeds to send to the queue for each interval (default=10)
- :code:`DATABASE_URL`: Connection URL to connect to Postgresql (default=postgres://postgres:postgres@localhost/miniflux2?sslmode=disable)
- :code:`DATABASE_MAX_CONNS`: Number of concurrent database connections (default=20)
- :code:`LISTEN_ADDR`: HTTP server address (default=127.0.0.1:8080)
