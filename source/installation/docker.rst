Docker Containers
=================

.. _Docker: https://github.com/warmfusion/moira-docker
.. _documentation: https://github.com/warmfusion/moira-docker

.. warning:: This is a third-party installation container provided by community. Use at your own risk.

You can quickly test a local moira installationg using Docker_ containers provided by Toby Jackson.

.. code-block:: bash

  git clone https://github.com/warmfusion/moira-docker.git
  cd moira-docker
  docker-compose up


.. tip:: If using Docker-Machine you can navigate to the Moira interface on http://$(docker-machine ip dev):8080/


This should setup a small cluster of Docker containers ready to accept data and handle notifications, with the interface on ``http://localhost:8080`` and accepts graphite metrics on ``localhost:2003``

See Tobys documentation_ for more information on submitting data to your new containers, and the :userguide:`/user_guide/` for everything else.

