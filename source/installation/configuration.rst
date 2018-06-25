Configuration
=============

By default, microservices will look for ``/etc/moira/<servicename>.yml``, but you can change this location
by passing your path as a command-line parameter ``--config``.

On this page you can find examples of configuration files for Moira microservices.


Filter
------

.. _storage-schemas.conf: http://graphite.readthedocs.io/en/latest/config-carbon.html#storage-schemas-conf

.. literalinclude:: ../../filter.example.yml
   :language: yaml

storage-schemas.conf_ is graphite carbon configuration file that should match similarly-named file in your Graphite installation.


Checker
-------

.. literalinclude:: ../../checker.example.yml
   :language: yaml


Notifier
--------

.. literalinclude:: ../../notifier.example.yml
   :language: yaml

By default mail sender will use 'Fancy' template:

.. image:: ../_static/fancy-email-template.png
   :alt: Fancy email template
   :width: 400

API
---

.. literalinclude:: ../../api.example.yml
   :language: yaml


UI
--

.. literalinclude:: ../../ui.example.json
   :language: json
