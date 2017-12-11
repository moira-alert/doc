Docker
======

.. |Docker Hub| replace:: Docker Hub
.. _Docker Hub: https://hub.docker.com/u/moira/

You can quickly test a local Moira installation using Docker containers from |Docker Hub|_.

.. code-block:: bash

  docker pull moira/web2
  docker pull moira/filter
  docker pull moira/checker
  docker pull moira/notifier
  docker pull moira/api

Containers are preconfigured to serve Web UI at ``localhost:8080`` and accept graphite metrics at ``localhost:2003``.
