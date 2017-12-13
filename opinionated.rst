Opinionated?
============

Miniflux is a minimalist software.
The purpose of this application is to read feeds.
*Nothing else*.

Focus on Simplicity
-------------------

- Having a gazillion of features makes the software hard to maintain, hard to troubleshoot and increase the number of bugs.
- This software doesn't try to satisfy the needs of everyone.

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
- ORM abstracts some interesting features provided your database.
- Managing schema migrations with Sqlite is painful.
- Postgresql is powerful, rock solid and battle tested.
- Postgresql is a great independent open source software.

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

Using the web UI on your smartphone is not so bad.

- Developing a native mobile application takes a lot of time.
- Big corporations control everything that's happening on their respective store, it's a closed ecosystem.
- The web is the universal platform and also your app store.
- Miniflux uses a responsive stylesheet and works pretty well on a smartphone.
