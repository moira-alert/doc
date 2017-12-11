.. include:: <isonum.txt>

Setting Up Your Subscriptions
=============================

By now you should have at least one trigger saved. If you don't, go back to the :doc:`/user_guide/simple` page.

First, add some delivery channels:

.. image:: ../_static/channels.png
   :alt: delivery channel settings

If your Moira installation is configured with separate user accounts, you will see only your channels
and subscriptions on this page. Otherwise, every user will see the same page with the same channels and subscriptions.

Consult :doc:`/installation/security` page for instructions on separating user accounts.

Once you have at least one channel, you can create a subscription. Press ``+ Add subscription`` button:

.. image:: ../_static/subscriptions.png
   :alt: subscription settings


Tags
----

You will receive notifications from triggers with these tags. Note that a trigger must contain *all*
of the subscription tags to match. For example, if you create a subscription for ``tag1``, ``tag2``,
you will receive notifications from trigger with ``tag1``, ``tag2``, ``tag3``, but not from trigger
with ``tag1`` only.

``WARN`` and ``ERROR`` are special pseudo-tags that match only events with corresponding level. For example, you may wish to subscribe your Pushover account only to ERROR-level notifications.

``DEGRADATION`` is a special pseudo-tag that match any event, where trigger's state degraded, e.g:

  - ``OK`` |rarr| ``WARN``
  - ``WARN`` |rarr| ``ERROR``
  - ``OK`` |rarr| ``NODATA``

In addition to ``DEGRADATION``, a special ``HIGH DEGRADATION`` pseudo-tag will be added for the following events:

  - ``OK`` |rarr| ``ERROR``
  - ``OK`` |rarr| ``NODATA``
  - ``WARN`` |rarr| ``NODATA``
  - ``ERROR`` |rarr| ``NODATA``


Create and Test
---------------

You can just save your subscription, but if you want to be 100% sure it works, you should immediately test it.
Dummy notification message will arrive shortly.
