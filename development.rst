Development
===========

Requirements
------------

- Git
- Go >= 1.9

Checkout the source code
------------------------

.. code:: bash

    cd $GOPATH/src
    mkdir -p github.com/miniflux
    cd github.com/miniflux
    git clone https://github.com/miniflux/miniflux2.git

Build a binary of the application
---------------------------------

.. code:: bash

    # All binaries
    make build

    # Only Linux
    make build-linux

    # Only Mac OS
    make build-darwin

Run the software for development
--------------------------------

.. code:: bash

    make run

Regenerate embedded files
-------------------------

To avoid any dependencies, all assets (Javascript, CSS, images, translations) are automatically included in the source code.

.. code:: bash

    go generate
