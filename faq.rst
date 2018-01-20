Frequently Asked Questions
==========================

Feature X was available in Miniflux 1?
--------------------------------------

Miniflux 2 doesn't try to reimplement all features of Miniflux 1.
The minimalist approach is pushed a little bit further.

If you really miss something, **you must contribute to the project**, but remember, **you have to keep the minimalist philosophy of Miniflux**.

Why are you not developing my feature request?
----------------------------------------------

- Developing a software takes a lot of time.
- This is a free and open source project, no one owes you anything.
- If you miss something, contribute to the project.
- Don't expect anyone to work for free.
- As mentioned above, the number of features is voluntarily limited. Nobody likes bloatware.
- Improving existing features is more important than adding new ones.

How to create themes for Miniflux 2?
------------------------------------

As of now, Miniflux 2 doesn't have any mechanism to load external stylesheets to avoid dependencies.
Themes are embedded into the binary.

If you would like to submit a new official theme you must send a pull-request.
But do not forget that **you will have to maintain your theme over the time**, otherwise, your theme will be removed from the code base.

Why there is no plugin system?
------------------------------

- Because this software has a minimalist approach.
- Because implementing a plugin system increase the complexity of the software.
- Because people do not maintain their plugins over the time.

What is "Save this article"?
----------------------------

"Save" sends the feed entry to third-party services like Pinboard or Instapaper if configured.

How are items removed from the database?
----------------------------------------

When a subscription is refreshed, entries marked as "removed" and not visible anymore in the XML feed are removed from the database.

What "Flush History" does?
--------------------------

"Flush History" changes the status of entries from "read" to "removed" (except for bookmarks).
Entries with the status "removed" are not visible in the user interface.
