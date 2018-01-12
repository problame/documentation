Opinionated?
============

Miniflux is a minimalist software.
The purpose of this application is to read feeds.
*Nothing else*.

Focus on Simplicity
-------------------

- Having a gazillion of features makes the software hard to maintain, hard to troubleshoot and increase the number of bugs.
- This software doesn't try to satisfy the needs of everyone.

Why the user interface is ugly?
-------------------------------

- The Miniflux layout is optimized to scan entries quickly.
- The design of Miniflux is inspired by `Hacker News <https://news.ycombinator.com/>`_, `Lobsters <https://lobste.rs/>`_ and `Pinboard <https://pinboard.in/>`_.
- To be honest, the main developer of Miniflux is not a UI/UX guy.

Why are you not developing my feature request?
----------------------------------------------

- Developing a software takes a lot of time. Don't expect anyone to work for free.
- As mentioned above, the number of features is volountary limited. Nobody likes bloatware.
- Improving existing features is more important than adding new ones.

Why choose Golang as programming language?
------------------------------------------

Go is probably the best choice for self-hosted software:

- Go is a simple programming language.
- Running code concurrently is part of the language.
- It's faster than a scripting language like PHP or Python.
- The final application is a binary compiled statically without any dependency.
- You just need to drop the executable on your server to deploy the application.
- You don't have to worry about what version of PHP/Python is installed on your machine.
- Packaging the software using RPM/Debian/Docker is straightforward.

Why Postgresql?
---------------

Miniflux is compatible only with Postgres.

- Supporting multiple databases increases the complexity of the software.
- Testing the software with all major versions of Mysql, MariaDB, Sqlite, Postgres is a lot of work.
- ORM abstracts some interesting features provided your database.
- Managing schema migrations with Sqlite is painful.
- Postgresql is powerful, rock solid and battle tested.
- Postgresql is a great independent open source software.
- Miniflux uses HSTORE/JSONB data types and handle user timezones with Postgres.

Why no Javascript framework?
----------------------------

Miniflux uses Javascript only where it's necessary.

- Rendering templates server side is so simple and fast enough for that kind of application.
- Using Javascript frameworks increase complexity.
- The Javascript ecosystem is moving all the time, sticking to the standard is probably more sustainable.

Why only ECMAScript 6?
----------------------

Miniflux uses ES6 and the Fetch API.

- All modern browsers support ES6 nowadays.
- Only Internet Explorer 11 doesn't support ES6, but who cares?
- Using a Javascript transpiler introduce another set of useless dependencies.

Why there is no mobile application?
-----------------------------------

Using the web UI on your smartphone is not so bad. The stylesheet is responsive and you can even swipe entries.

- Developing a native mobile application takes a lot of work.
- You must know pretty well iOS/Android specific SDKs, languages and frameworks, which is not the case of every body.
- You must develop your application twice, one for Android and another one for iOS, unless you use some kind of hackish toolkit like React Native or similar.
- You have to pay a fee to publish your app on the store even if your app doesn't make any money.
- Big corporations control everything that's happening on their respective store, it's a closed ecosystem.
- The web is the universal platform and could be also your app store.
