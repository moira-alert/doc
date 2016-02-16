Configuration
=============

All Moira microservices can use a single configuration file in YAML format.

By default, microservices will look for ``/etc/moira/config.yml``, but you can change this location
in systemd configuration files (pass your path as a command-line parameter) and in
``moira-alert/worker/moira/config.py``.

.. literalinclude:: ../../config.example.yml
   :language: yaml
