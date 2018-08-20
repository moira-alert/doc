Advanced Mode Trigger
=====================

.. _govaluate: https://github.com/Knetic/govaluate/blob/master/MANUAL.md
.. _redis: https://redis.io/
.. _graphite: https://github.com/go-graphite/carbonapi

Sometimes a simple trigger (:doc:`/user_guide/simple`) doesn't provide enough flexibility for your task.

For example, you may want to receive a notification when 5% of user requests take up more than a second to process, but
only if there are more than 100 requests per minute. Usually, you will have two separate metrics for this:

1. ``Nginx.requests.process_time.p95`` - 95th percentile of request processing time in milliseconds
2. ``Nginx.requests.count`` - request count per minute

Maybe you can construct a monstrous Graphite expression to reflect this combination, but Moira's Advanced Mode is better:

.. image:: ../_static/advanced.png
   :alt: advanced trigger

You can use any govaluate_ expression with predefined constants here:

- ``t1``, ``t2``, ... are values from your targets
- ``OK``, ``WARN``, ``ERROR``, ``NODATA`` are states that must be the result of evaluation
- ``PREV_STATE`` is equal to previously set state, and allows you to prevent frequent state changes

.. note:: Only T1 target can resolve into multiple metrics in Advanced Mode. T2, T3, ... must resolve to single metrics.
          Moira will calculate expression separately for every metric in T1.

Any incorrect expressions or bad syntax will result in EXCEPTION trigger state.

Data source
------------

If :ref:`remote-triggers-checker` is enabled, you can choose between following Data Sources:

- Redis_ — Moira database. By default Redis stores data for only several hours. It covers most of user cases when you need real-time alerting.
- Graphite_ — remote Graphite-like HTTP API. It should be used only when you need to get metrics for a large period.

  .. warning:: Please, use this Data Source with caution. It may cause extra load on Graphite HTTP API.

  .. important::

    Please, keep in mind that functions in Remote and Local triggers can work differently.
    To avoid this, make sure you use Carbonapi with the same revision as in Moira. Latest Carbonapi listed in :doc:`../changelog`.
