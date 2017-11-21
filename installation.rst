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
