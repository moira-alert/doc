Advanced Mode Trigger
=====================

Sometimes a simple trigger (:doc:`/user_guide/simple`) doesn't provide enough flexibility for your task.

For example, you may want to receive a notification when 5% of user requests take up more than a second to process, but
only if there are more than 100 requests per minute. Usually, you will have two separate metrics for this:

1. ``Nginx.requests.process_time.p95`` - 95th percentile of request processing time in milliseconds
2. ``Nginx.requests.count`` - request count per minute

Maybe you can construct a monstrous Graphite expression to reflect this combination, but Moira's Advanced Mode is better:

.. image:: ../_static/advanced.png
   :alt: advanced trigger

You can use any Python expression with predefined constants here:

- ``t1``, ``t2``, ... are values from your targets
- ``OK``, ``WARN``, ``ERROR``, ``NODATA`` are states that must be the result of evaluation
- ``PREV_STATE`` is equal to previously set state, and allows you to prevent frequent state changes

.. note:: Only T1 target can resolve into multiple metrics in Advanced Mode. T2, T3, ... must resolve to single metrics.
          Moira will calculate expression separately for every metric in T1.

.. note:: Expression evaluation is intended to be as safe as possible. You can't use any Python functions here.

Any incorrect expressions or bad syntax will result in EXCEPTION trigger state.
