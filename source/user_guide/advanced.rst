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
- ``OK``, ``WARN``, ``ERROR`` are states that must be the result of evaluation

.. note:: Use only targets that resolve into one metric in Advanced Mode. There is no way for Moira to understand how to
          use multiple-metric targets in an expression.

.. note:: Expression evaluation is intended to be as safe as possible. You can't use any Python functions here.

Any incorrect expressions or bad syntax will result in EXCEPTION trigger state.
