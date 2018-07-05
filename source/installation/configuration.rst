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


API
---

.. literalinclude:: ../../api.example.yml
   :language: yaml


WEB UI
------

.. literalinclude:: ../../web.example.json
   :language: json

- `type` — contact type: pushover, slack, mail, script, telegram, twilio sms, twilio voice;
- `validation` — regular expression for user contact;
- `title` — hint shown in input field;
- `help` — help text in Markdown_ markup .

.. image:: ../_static/web-ui-example.png
   :alt: WEB UI example
   :width: 400

.. _Markdown: https://daringfireball.net/projects/markdown/syntax
