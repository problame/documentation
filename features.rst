Features
========

Reader
------

- Feed formats supported: Atom, RSS 1.0/2.0 and JSON
- OPML import/export
- Support multiple enclosures/attachments (Podcasts, videos, music, and images)
- Play videos from YouTube channels directly inside Miniflux
- Categories
- Bookmarks
- Fetch website icons (favicons)
- Save articles to third-party services
- Available in Chinese, Dutch, English, French, German, and Polish

Privacy
-------

- Remove pixel trackers
- Fetch original links when the feed is coming from FeedBurner
- Open external links with the attributes :code:`rel="noopener noreferrer" referrerpolicy="no-referrer"`
- Image proxy to avoid mixed content warnings with HTTPS
- Play Youtube videos by using the domain `youtube-nocookie.com`
- Block any external Javascript code to avoid tracking

Content Manipulation
--------------------

- Fetch original article and returns relevant contents (Readability)
- Custom scraper rules based on CSS selectors
- Custom rewriting rules
    - Append image title for comics
    - Add Youtube video

User Interface
--------------

- Stylesheet optimized for readability
- Responsive design (works on desktop, tablet, and mobile devices)
- No fancy user interface
- Doesn't require to download an application from the App/Play Store
- You could add Miniflux to the home screen
- Keyboard shortcuts
- Touch events on mobile devices
- Themes (black and white)

Integrations
------------

- Send articles to Pinboard, Instapaper, Wallabag, or Nunux Keeper
- Bookmarklet to subscribe to a website directly from any browsers
- Use existing mobile applications to read your feeds by using the Fever API
- REST API with clients written in Go and Python

Authentication
--------------

- Username/password
- Google (OAuth2)

Technical stuff
---------------

- Self-hosted
- Written in Go (Golang)
- Use only Postgres as database
- Single static binary (no more dependencies hell)
- Automatic HTTPS configuration with Let's Encrypt
- Use your own SSL certificate
- Supports HTTP/2.0 if TLS is configured
- Feeds are updated in the background by an internal scheduler
- External content is sanitized before being displayed
- Use content security policy that allows only application Javascript and block inline code and styles
- Works only in modern browsers
