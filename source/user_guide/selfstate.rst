Self State Monitor
==================

Self state monitor is a built-in protection against false ``NODATA`` notifications.
It is useful if the communication between the datacenters or the Graphite-relay service breaks down.

How it works
-------------

Moira tries to protect users from false ``NODATA`` notifications.
If something unexpected happens with Redis or Moira-services, self state monitor will:

* switch Moira-Notifier to ``ERROR`` state;

  .. note:: Moira-Notifier will never switch back to ``OK`` state without administrator intervention.
   For more information see :ref:`notifier-state-api`.

* send alarm message to contacts from :ref:`moira_selfstate\\contacts <notifier-configuration>`;

  .. image:: ../_static/helth-check-email.png
     :alt: email alarm message

* show error status in Web UI.

  .. image:: ../_static/helth-check-webui.png
    :alt: WEB UI error notification


When it works
-------------

Self state monitor checks these metrics:

1. If there is no connection to Redis for longer than :ref:`moira_selfstate\\redis_disconect_delay <notifier-configuration>`;
2. If Moira-Filter receive no metrics for longer than :ref:`moira_selfstate\\last_metric_received_delay <notifier-configuration>`;
3. If Moira-Checker checks no triggers for longer than :ref:`moira_selfstate\\last_check_delay <notifier-configuration>`.

.. _notifier-state-api:

Manipulate Notifier state using API
-----------------------------------

At the moment, there are only two API methods.

1. Get current Moira-Notifier state:

  .. code-block:: html

      GET /api/health/notifier HTTP/1.1
      Host: MOIRA-URL:8081

  Returns JSON with Notifier state and a message of error (if state is not ``OK``).

2. Update Moira-Notifier state:

  .. code-block:: html

      PUT /api/health/notifier HTTP/1.1
      Host: MOIRA-URL:8081
      Content-Type: application/json

      {
          "state": "OK"
      }

  Allowed state values: ``<OK|ERROR>``.
