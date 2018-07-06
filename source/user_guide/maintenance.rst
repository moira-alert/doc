Maintenance
===========

Maintenance is a proper way to mute alerting on specific metrics. It can be useful during planned work.
E.g., you are going to move server from one data center to another and don't want Moira to disturb you.

.. image:: ../_static/maintenance.png
   :alt: maintenance mode

Examples
-------------------------------------

When you switch a metric into maintenance, Moira will mute all state changes during that period.
You will receive notification, if the state before and after maintenance turn out to be different.

Example 1. Alert will not be sent
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* metric ``awesomeMetric1`` is in ``OK`` state;
* Rick_ switches metric into maintenance for an hour;
* within the hour metric changes its state several times:

  - ``OK`` → ``WARN``,
  - ``WARN`` → ``ERROR``,
  - ``ERROR`` → ``OK``;

* after one-hour maintenance ends, metric is in ``OK`` state;
* Moira checks if metric state changed during maintenance:

  - ``awesomeMetric1`` state before maintenance: ``OK``;
  - ``awesomeMetric1`` state after maintenance ``OK``;
* nothing to notify about: the state remained the same as it was before the maintenance period.

Example 2. Alert will be sent
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* metric ``awesomeMetric2`` is in ``OK`` state;
* Rick_ switches metric into maintenance for an hour;
* within the hour metric changes its state several times:

  - ``OK`` → ``WARN``,
  - ``WARN`` → ``ERROR``,
  - ``ERROR`` → ``OK``,
  - ``OK`` → ``ERROR``;

* after one-hour maintenance ends, metric is in ``ERROR`` state;
* Moira checks if metric state changed during maintenance:

  - ``awesomeMetric2`` state before maintenance: ``OK``;
  - ``awesomeMetric2`` state after maintenance ``ERROR``;

* Moira sends message to user: the state has changed from that which was before the maintenance period.

.. _Rick: https://www.youtube.com/watch?v=dQw4w9WgXcQ
