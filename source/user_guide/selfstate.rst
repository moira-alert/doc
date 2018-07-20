Self State Monitor
==================

Self State Monitor is a built-in mechanism designed to protect
end user from false ``NODATA`` notifications and notify administrator
about issues in Moira and/or Graphite systems.

Why Self State Monitor
-----------------------

A situation is possible when Graphite Relay, Redis DB or Moira-Filter service breaks down.
This leads to the fact that Moira doesn't receive any metrics from Graphite.
In this case, Moira has no metrics on which it could check state of the triggers.
According to the Moira logic, it should switch triggers to ``NODATA`` state
and send alert messages to users.

To handle this situation properly, we recommend turning on the Self State Monitor.
In this case, Moira will **prevent itself from sending alert messages to end users
but notify administrators of the existing problem**.

.. warning::

  When Self State Monitor detects a problem, it disables any notifications to end users
  and does not turn it back on without manual intervention.

  Please. read this manual before using Self State Monitor in production.

.. seealso::

  For a better understanding, look at the architecture of the :ref:`Moira microservices <microservices-architecture>`.

.. _when-monitor-helps:

When Self State Monitor helps
-----------------------------------

Self state monitor checks these situations:

1. If there is no connection between Moira and Redis for longer than ``redis_disconect_delay``.
2. If Moira-Filter receive no metrics for longer than ``last_metric_received_delay``.
3. If Moira-Checker checks no triggers for longer than ``last_check_delay``.

.. seealso::

  All the above configuration parametres can be found in the :ref:`Moira-Notifier section <notifier-configuration>`
  on configuration page.

How Self State Monitor works
---------------------------------------

When you turn Self State Monitor on, it works this way:

* Self State Monitor checks :ref:`Moira state <when-monitor-helps>` every 10 seconds.

* Something breaks down. It can be Graphite-Relay, connection to Redis DB or crashed Moira-Filter docker container.

* Self State send alarm message to administrator with issue discription.

  *Here is an example of message*:

    .. image:: ../_static/helth-check-email.png
     :alt: email alarm message

* Self State Monitor turns Moira-Notifier service off, switching it in ``ERROR`` state.

  *When Moira-Notifier not in* ``OK`` *state, Moira will show you an error in Web UI*:

    .. image:: ../_static/helth-check-webui.png
      :alt: WEB UI error notification

-----

.. _notifier-state-api:

Turn Moira-Notifier ON and OFF using API
-----------------------------------------------

You can get current Moira-Notifier state or update it using API.
At the moment there are only two methods available.

1. Get current Moira-Notifier state:

  .. code-block:: html

      GET /api/health/notifier HTTP/1.1
      Host: MOIRA-URL:8081

  Returns JSON with Notifier state and a message of error. Message is omitted if state is ``OK``.

    .. code-block:: JSON

       {
           "state": "ERROR",
           "message": "Something unexpected happened with Moira, so we temporarily turned off the notification mailing. We are already working on the problem and will fix it in the near future."
       }

2. Update Moira-Notifier state:

  .. code-block:: html

      PUT /api/health/notifier HTTP/1.1
      Host: MOIRA-URL:8081
      Content-Type: application/json

      {
          "state": "OK"
      }

  Allowed state values: ``<OK|ERROR>``.
