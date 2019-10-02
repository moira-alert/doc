Backend
=======

.. _Go: https://golang.org
.. _GoConvey: http://goconvey.co

Backend microservices are written in Go_.

Run GoConvey_ tests.

.. code-block:: bash

   go get github.com/smartystreets/goconvey
   goconvey


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
