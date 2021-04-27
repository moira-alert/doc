Backend
=======

.. _Go: https://golang.org
.. _GoConvey: http://goconvey.co

Project Setup
-------------
Backend microservices are written in Go_ (with module support, minimal Go lang required is 1.11), this is how you can get started writing your own:

1. Create a fork of the project from GitHub
2. Create a base directory to check out the project into e.g.
    .. code-block:: bash

        /Development/go/moira/
3. Set your GOPATH to that directory and change into it
    .. code-block:: bash

        export GOPATH=/Development/go/moira/
        cd $GOPATH
4. Get GoConvey_ for tests
    .. code-block:: bash

        go get github.com/smartystreets/goconvey
5. Export your ``GOPATH``'s ``bin`` directory
    .. code-block:: bash

        export PATH=$PATH:$GOPATH/bin
6. Checkout your Moira clone into the root of the ``GOPATH``
    .. code-block:: bash

        git clone https://github.com/<your username>/moira $GOPATH/src/github.com/moira-alert/moira

7. Go into ``$GOPATH/src/github.com/moira-alert/moira``

8. Launch GoConvey_
    .. code-block:: bash

        goconvey

You are now ready to start hacking. Goconvey_ will keep running in the background, scanning for new test classes to execute. If you don't want to run the entire suite, start Goconvey_ from the root of directory you will be developing your code from.

Alternatively you can run a quick test on the terminal with

.. code-block:: bash

    go test -v


Writing Your Own Notification Sender
------------------------------------

First, look at built-in senders:

- senders/slack
- senders/pushover
- senders/mail

All of them implement interface ``Sender`` from ``interfaces.go``.
Please, note that scheduling and throttling require senders to support
packing several events into one message.

You should include your new sender in ``RegisterSenders``
method of ``notifier/registrator.go`` with appropriate type.

Senders have access to their settings in common config,
which is passed to the ``Init`` method.
