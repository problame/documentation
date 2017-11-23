Upgrading to a New Version
==========================

1. Disconnect all users by flushing all sessions: :code:`miniflux -flush-sessions`
2. Stop the process
3. Backup your database
4. Check that your backup is working for real
5. Run database migrations: :code:`miniflux -migrate`
6. Start the process
