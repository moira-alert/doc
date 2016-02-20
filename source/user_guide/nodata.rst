Dealing with NODATA
===================

If you have a simple trigger (like the one described in :doc:`/user_guide/simple`), you probably know what happens when
a metric has a very high value or a very low value. Free disk space is too low? You get a notification.

But what if your metric has *no* value? Literally, what if Moira is not receiving any data for your metric? How can you
know, whether you have enough disk space left or not? In this case, a trigger setting defines the behavior:

.. image:: ../_static/nodata.png
   :alt: NODATA settings
   :scale: 100%

When Moira hasn't been receiving data for more than 600 seconds, it will set a special NODATA state for this metric.
You can set any other state or change time delay here. For example, if you have an error metric, and no data means no
errors, you should set this to OK.

You will receive notifications when your metric goes in and out of NODATA state, just like any other state.
