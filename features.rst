Features
========

Reader
------

- Feed formats supported: Atom, RSS 1.0/2.0 and JSON
- OPML import/export
- Supports enclosures/attachments (Podcasts, videos, music and images)
- Play videos from YouTube channels directly inside Miniflux
- Group subscriptions by categories
- Fetch website icons (favicons)
- External content is sanitized before being displayed

Privacy
-------

- Remove pixel trackers
- Fetch original links when the feed is coming from FeedBurner
- Open external links with the attributes :code:`rel="noopener noreferrer" referrerpolicy="no-referrer`
- Image proxy to avoid mixed content warnings with HTTPS

Content Manipulation
--------------------

- Fetch original web page when only a summary is available
- Improve article contents (could add image title as caption for comics)

User Interface
--------------

- Stylesheet optimized for readability
- Responsive design (works on desktop, tablet and mobile devices)
- Doesn't require to download any application from the App/Play Store
- Keyboard shortcuts
- Themes (black and white)

Technical stuff
---------------

- self-hosted
- Written in Go (Golang)
- Use Postgres as database
- There are no dependencies, only a single static binary
- Feeds are updated in the background by an internal scheduler
