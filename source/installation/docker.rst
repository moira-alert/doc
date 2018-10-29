Docker
======

.. |Docker Hub| replace:: Docker Hub
.. _Docker Hub: https://hub.docker.com/u/moira/

You can quickly test a local Moira installation using Docker containers from |Docker Hub|_ and docker-compose file in documentation repository.

.. code-block:: bash

  git clone https://github.com/moira-alert/docker-compose.git
  cd docker-compose
  docker-compose pull
  docker-compose up

Containers are preconfigured to serve Web UI at ``localhost:8080`` and accept graphite metrics at ``localhost:2003``.
