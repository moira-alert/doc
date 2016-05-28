Configuration
=============

.. _storage-schemas.conf: http://graphite.readthedocs.io/en/latest/config-carbon.html

All Moira microservices can use a single configuration file in YAML format.

By default, microservices will look for ``/etc/moira/config.yml``, but you can change this location
in systemd configuration files (pass your path as a command-line parameter) and in
``moira-alert/worker/moira/config.py``

.. literalinclude:: ../../config.example.yml
   :language: yaml

storage-schemas.conf_ is graphite carbon configuration file.

Example:

.. code-block:: text

   [default_1min_for_1day]
   pattern = .*
   retentions = 1m:1d