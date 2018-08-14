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

Add required tags into subscription to receive notifications from triggers with these tags.

Matching rules are:

- If subscription has only one tag, you will receive notifications from any trigger with this tag.

  - Create Triggger1 with tags: ``["DevOps", "Moira-duty"]``
  - Create Triggger2 with tags: ``["DevOps"]``
  - Create Subscription1 with tags: ``["DevOps"]``
    
    By using Subscription1 you will receive events for both Triggger1 and Triggger2 

- If subscription has multiple tags, you will receive notifications only from triggers which include all this tags.

  - Create Subscription2 with tags: ``["DevOps", "Moira-duty"]``
  
    By using Subscription2 you will receive events only for Trigger1

Ignore specific states transitions
----------------------------------

You also can reduce number of notifications ignoring unnecessary event. For this purpose use following check boxes:

- Send notifications when triggers degraded only

  Only following states transitions will require notifications:

  - ``OK`` |rarr| ``WARN``
  - ``OK`` |rarr| ``ERROR``
  - ``OK`` |rarr| ``NODATA``
  - ``WARN`` |rarr| ``ERROR``
  - ``WARN`` |rarr| ``NODATA``
  - ``ERROR`` |rarr| ``NODATA``

- Do not send WARN notifications

  Following states transitions will be ignored:

  - ``OK`` |rarr| ``WARN``
  - ``WARN`` |rarr| ``OK``


Create and Test
---------------

You can just save your subscription, but if you want to be 100% sure it works, you should immediately test it.
Dummy notification message will arrive shortly.
