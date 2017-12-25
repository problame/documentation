REST API
========

Authentication
--------------

The Miniflux API use HTTP Basic authentication.
The credentials are the username/password of your account.

Clients
-------

There are 2 official API clients, one written in Go and another written in Python.

Golang Client
~~~~~~~~~~~~~

- Repository: `<https://github.com/miniflux/miniflux-go>`_
- Reference: `<https://godoc.org/github.com/miniflux/miniflux-go>`_

Installation:

.. code:: bash

    go get -u github.com/miniflux/miniflux-go


Usage Example:

.. code:: go

    package main

    import (
        "fmt"

        "github.com/miniflux/miniflux-go"
    )

    func main() {
        client := miniflux.NewClient("https://miniflux.example.org", "admin", "secret")

        // Fetch all feeds.
        feeds, err := client.Feeds()
        if err != nil {
            fmt.Println(err)
            return
        }
        fmt.Println(feeds)
    }

Python Client
~~~~~~~~~~~~~

Repository: `<https://github.com/miniflux/miniflux-python>`_

Installation:

.. code:: bash

    pip install miniflux

Usage example:

.. code:: python

    import miniflux

    client = miniflux.Client("https://miniflux.example.org", "my_username", "my_secret_password")

    # Get all feeds
    feeds = client.get_feeds()

    # Refresh a feed
    client.refresh_feed(123)

    # Discover subscriptions from a website
    subscriptions = client.discover("https://example.org")

List of :code:`Client` methods:

- :code:`discover(website_url)`
- :code:`get_feeds()`
- :code:`get_feed(feed_id)`
- :code:`get_feed_icon(feed_id)`
- :code:`create_feed(feed_url, category_id)`
- :code:`update_feed(feed_id, title=None, feed_url=None, site_url=None, scraper_rules=None, rewrite_rules=None, crawler=None, category_id=None)`
- :code:`refresh_feed(feed_id)`
- :code:`delete_feed(feed_id)`
- :code:`get_feed_entry(feed_id, entry_id)`
- :code:`get_feed_entries(feed_id, status=None, offset=None, limit=None, order=None, direction=None)`
- :code:`get_entry(entry_id)`
- :code:`get_entries(status=None, offset=None, limit=None, order=None, direction=None)`
- :code:`update_entries(entry_ids, status)`
- :code:`toggle_bookmark(entry_id)`
- :code:`get_categories()`
- :code:`create_category(title)`
- :code:`update_category(category_id, title)`
- :code:`delete_category(category_id)`
- :code:`get_users()`
- :code:`get_user(user_id)`
- :code:`create_user(username, password, is_admin)`
- :code:`update_user(user_id, username=None, password=None, theme=None, language=None, timezone=None, entry_direction=None)`
- :code:`delete_user(user_id)`

API Reference
-------------

Status Codes
~~~~~~~~~~~~

- :code:`200`: Everything is OK
- :code:`201`: Resource created/modified
- :code:`204`: Resource removed/modified
- :code:`400`: Bad request
- :code:`401`: Unauthorized (bad username/password)
- :code:`403`: Forbidden (access not allowed)
- :code:`500`: Internal server error

Error Response
~~~~~~~~~~~~~~

.. code:: json

    {
        "error_message": "Some error"
    }

Discover Subscriptions
~~~~~~~~~~~~~~~~~~~~~~

Request:

.. code::

    POST /v1/discover
    Content-Type: application/json

    {
        "url": "http://example.org"
    }

Response:

.. code:: json

    [
        {
            "url": "http://example.org/feed.atom",
            "title": "Atom Feed",
            "type": "atom"
        },
        {
            "url": "http://example.org/feed.rss",
            "title": "RSS Feed",
            "type": "rss"
        }
    ]

Get Feeds
~~~~~~~~~

Request:

.. code::

    GET /v1/feeds

Response:

.. code:: json

    [
        {
            "id": 42,
            "user_id": 123,
            "title": "Example Feed",
            "site_url": "http://example.org",
            "feed_url": "http://example.org/feed.atom",
            "rewrite_rules": "",
            "scraper_rules": "",
            "crawler": false,
            "checked_at": "2017-12-22T21:06:03.133839-05:00",
            "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
            "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
            "parsing_error_count": 0,
            "parsing_error_message": "",
            "category": {
                "id": 793,
                "user_id": 123,
                "title": "Some category"
            },
            "icon": {
                "feed_id": 42,
                "icon_id": 84
            }
        }
    ]

Notes:

- :code:`icon` is :code:`null` when the feed doesn't have any favicon.

Get Feed
~~~~~~~~

Request:

.. code::

    GET /v1/feeds/42

Response:

.. code:: json

    {
        "id": 42,
        "user_id": 123,
        "title": "Example Feed",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "rewrite_rules": "",
        "scraper_rules": "",
        "crawler": false,
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_count": 0,
        "parsing_error_message": "",
        "category": {
            "id": 793,
            "user_id": 123,
            "title": "Some category"
        },
        "icon": {
            "feed_id": 42,
            "icon_id": 84
        }
    }

Notes:

- :code:`icon` is :code:`null` when the feed doesn't have any favicon.

Get Feed Icon
~~~~~~~~~~~~~

Request:

.. code::

    GET /v1/feeds/42/icon

Response:

.. code:: json

    {
        "id": 262,
        "data": "image/png;base64,iVBORw0KGgoAAA....",
        "mime_type": "image/png"
    }

Notes:

- If the feed doesn't have any favicon, a 404 is returned.

Create Feed
~~~~~~~~~~~

Request:

.. code::

    POST /v1/feeds
    Content-Type: application/json

    {
        "feed_url": "http://example.org/feed.atom",
        "category_id": 22
    }

Response:

.. code:: json

    {
        "feed_id": 262,
    }

Update Feed
~~~~~~~~~~~

Request:

.. code::

    PUT /v1/feeds/42
    Content-Type: application/json

    {
        "title": "New Feed Title",
        "category": {
            "id": 22
        }
    }

Response:

.. code:: json

    {
        "id": 42,
        "user_id": 123,
        "title": "New Feed Title",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "rewrite_rules": "",
        "scraper_rules": "",
        "crawler": false,
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_count": 0,
        "parsing_error_message": "",
        "category": {
            "id": 22,
            "user_id": 123,
            "title": "Another category"
        },
        "icon": {
            "feed_id": 42,
            "icon_id": 84
        }
    }

Refresh Feed
~~~~~~~~~~~~

Request:

.. code::

    PUT /v1/feeds/42/refresh

Notes:

- Returns :code:`204` status code for success.
- This API call is synchronous and can takes hundred of milliseconds.

Remove Feed
~~~~~~~~~~~

Request:

.. code::

    DELETE /v1/feeds/42

Get Feed Entry
~~~~~~~~~~~~~~

Request:

.. code::

    GET /v1/feeds/42/entries/888

Response:

.. code:: json

    {
        "id": 888,
        "user_id": 123,
        "feed_id": 42,
        "title": "Entry Title",
        "url": "http://example.org/article.html",
        "author": "Foobar",
        "content": "<p>HTML contents</p>",
        "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
        "published_at": "2016-12-12T16:15:19Z",
        "status": "read",
        "starred": false,
        "feed": {
            "id": 42,
            "user_id": 123,
            "title": "New Feed Title",
            "site_url": "http://example.org",
            "feed_url": "http://example.org/feed.atom",
            "rewrite_rules": "",
            "scraper_rules": "",
            "crawler": false,
            "checked_at": "2017-12-22T21:06:03.133839-05:00",
            "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
            "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
            "parsing_error_count": 0,
            "parsing_error_message": "",
            "category": {
                "id": 22,
                "user_id": 123,
                "title": "Another category"
            },
            "icon": {
                "feed_id": 42,
                "icon_id": 84
            }
        }
    }

Get Entry
~~~~~~~~~

Request:

.. code::

    GET /v1/entries/888

Response:

.. code:: json

    {
        "id": 888,
        "user_id": 123,
        "feed_id": 42,
        "title": "Entry Title",
        "url": "http://example.org/article.html",
        "author": "Foobar",
        "content": "<p>HTML contents</p>",
        "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
        "published_at": "2016-12-12T16:15:19Z",
        "status": "read",
        "starred": false,
        "feed": {
            "id": 42,
            "user_id": 123,
            "title": "New Feed Title",
            "site_url": "http://example.org",
            "feed_url": "http://example.org/feed.atom",
            "rewrite_rules": "",
            "scraper_rules": "",
            "crawler": false,
            "checked_at": "2017-12-22T21:06:03.133839-05:00",
            "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
            "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
            "parsing_error_count": 0,
            "parsing_error_message": "",
            "category": {
                "id": 22,
                "user_id": 123,
                "title": "Another category"
            },
            "icon": {
                "feed_id": 42,
                "icon_id": 84
            }
        }
    }

Get Feed Entries
~~~~~~~~~~~~~~~~

Request:

.. code::

    GET /v1/feeds/42/entries?limit=1&order=id&direction=asc

Available filters:

- :code:`status`: Entry status (read, unread or removed)
- :code:`offset`
- :code:`limit`
- :code:`order`: "id", "status", "published_at", "category_title", "category_id"
- :code:`direction`: "asc" or "desc"

Response:

.. code:: json

    {
        "total": 10,
        "entries": [
            {
                "id": 888,
                "user_id": 123,
                "feed_id": 42,
                "title": "Entry Title",
                "url": "http://example.org/article.html",
                "author": "Foobar",
                "content": "<p>HTML contents</p>",
                "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
                "published_at": "2016-12-12T16:15:19Z",
                "status": "read",
                "starred": false,
                "feed": {
                    "id": 42,
                    "user_id": 123,
                    "title": "New Feed Title",
                    "site_url": "http://example.org",
                    "feed_url": "http://example.org/feed.atom",
                    "rewrite_rules": "",
                    "scraper_rules": "",
                    "crawler": false,
                    "checked_at": "2017-12-22T21:06:03.133839-05:00",
                    "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
                    "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
                    "parsing_error_count": 0,
                    "parsing_error_message": "",
                    "category": {
                        "id": 22,
                        "user_id": 123,
                        "title": "Another category"
                    },
                    "icon": {
                        "feed_id": 42,
                        "icon_id": 84
                    }
                }
            }
        ]

Get Entries
~~~~~~~~~~~

Request:

.. code::

    GET /v1/entries?status=unread&direction=desc

Available filters:

- :code:`status`: Entry status (read, unread or removed)
- :code:`offset`
- :code:`limit`
- :code:`order`: "id", "status", "published_at", "category_title", "category_id"
- :code:`direction`: "asc" or "desc"

Response:

.. code:: json

    {
        "total": 10,
        "entries": [
            {
                "id": 888,
                "user_id": 123,
                "feed_id": 42,
                "title": "Entry Title",
                "url": "http://example.org/article.html",
                "author": "Foobar",
                "content": "<p>HTML contents</p>",
                "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
                "published_at": "2016-12-12T16:15:19Z",
                "status": "unread",
                "starred": false,
                "feed": {
                    "id": 42,
                    "user_id": 123,
                    "title": "New Feed Title",
                    "site_url": "http://example.org",
                    "feed_url": "http://example.org/feed.atom",
                    "rewrite_rules": "",
                    "scraper_rules": "",
                    "crawler": false,
                    "checked_at": "2017-12-22T21:06:03.133839-05:00",
                    "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
                    "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
                    "parsing_error_count": 0,
                    "parsing_error_message": "",
                    "category": {
                        "id": 22,
                        "user_id": 123,
                        "title": "Another category"
                    },
                    "icon": {
                        "feed_id": 42,
                        "icon_id": 84
                    }
                }
            }
        ]

Update Entries
~~~~~~~~~~~~~~

Request:

.. code::

    PUT /v1/entries
    Content-Type: application/json

    {
        "entry_ids": [1234, 4567],
        "status": "read"
    }

Notes:

- Returns :code:`204` status code for success.

Toggle Entry Bookmark
~~~~~~~~~~~~~~~~~~~~~

Request:

.. code::

    PUT /v1/entries/1234/bookmark

Notes:

- Returns :code:`204` status code for success.

Get Categories
~~~~~~~~~~~~~~

Request:

.. code::

    GET /v1/categories

Response:

.. code:: json

    [
        {"title": "All", "user_id": 267, "id": 792},
        {"title": "Engineering Blogs", "user_id": 267, "id": 793}
    ]

Create Category
~~~~~~~~~~~~~~~

Request:

.. code::

    POST /v1/categories
    Content-Type: application/json

    {
        "title": "My category"
    }

Response:

.. code:: json

    {
        "id": 802,
        "user_id": 267,
        "title": "My category"
    }

Update Category
~~~~~~~~~~~~~~~

Request:

.. code::

    PUT /v1/categories/802
    Content-Type: application/json

    {
        "title": "My new title"
    }

Response:

.. code:: json

    {
        "id": 802,
        "user_id": 267,
        "title": "My new title"
    }

Delete Category
~~~~~~~~~~~~~~~

Request:

.. code::

    DELETE /v1/categories/802

Create User
~~~~~~~~~~~

Request:

.. code::

    POST /v1/users
    Content-Type: application/json

    {
        "username": "bob",
        "password": "test123",
        "is_admin": false
    }

Response:

.. code:: json

    {
        "id": 270,
        "username": "bob",
        "language": "en_US",
        "timezone": "UTC",
        "theme": "default",
        "entry_sorting_direction": "asc"
    }

Notes:

- You must be administrator to create users.

Update User
~~~~~~~~~~~

Request:

.. code::

    PUT /v1/users/270
    Content-Type: application/json

    {
        "username": "joe"
    }

Available fields:

- :code:`username`
- :code:`password`
- :code:`is_admin` (boolean)
- :code:`theme`
- :code:`language`
- :code:`timezone`

Response:

.. code:: json

    {
        "id": 270,
        "username": "joe",
        "language": "en_US",
        "timezone": "UTC",
        "theme": "default",
        "entry_sorting_direction": "asc"
    }

Notes:

- You must be administrator to update users.

Get User
~~~~~~~~

Request:

.. code::

    GET /v1/users/270

Response:

.. code:: json

    {
        "id": 270,
        "username": "bob",
        "language": "en_US",
        "timezone": "UTC",
        "theme": "default",
        "entry_sorting_direction": "asc"
    }

Notes:

- You must be administrator to fetch users.

Delete User
~~~~~~~~~~~

Request:

.. code::

    DELETE /v1/users/270

Notes:

- You must be administrator to delete users.
