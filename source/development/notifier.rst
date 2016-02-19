Notifier
========

.. _Go: https://golang.org
.. _Ginkgo: https://onsi.github.io/ginkgo/

This microservice is written in Go_. To run tests, first get all dependencies.

.. code-block:: bash

   cd notifier/notifier
   go get

Then, run Ginkgo_ tests.

.. code-block:: bash

   cd notifier/tests
   ginkgo

Writing Your Own Notification Sender
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First, look at built-in senders:

- notifier/slack
- notifier/pushover
- notifier/mail

All of them implement interface ``Sender`` from ``interfaces.go``. Please, note that scheduling and
throttling require that senders support packing several events into one message.

You should include your new sender in ``configureSenders`` method of ``notifier/main.go`` with
appropriate type.

Senders have access to their settings in common config, which is passed to the ``Init`` method.
