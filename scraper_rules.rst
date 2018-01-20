Scraper Rules
=============

When an article contains only an extract of the content, you could fetch the original web page and apply a set of rules to get relevant contents.

Miniflux uses CSS selectors for custom rules.
These custom rules can be saved in the feed properties (Select a feed and click on edit).

Examples
--------

- :code:`div#articleBody`: Fetch a `div` element with the ID `articleBody`
- :code:`div.content`: Fetch all `div` elements with the class `content`
- :code:`article, div.article`: Use a comma to define multiple rules

Miniflux includes a list of predefined rules for popular websites.
You could contribute to the project to keep them up to date.

Under the hood, Miniflux uses the library `Goquery <https://github.com/PuerkitoBio/goquery>`_.
