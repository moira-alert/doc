Throttling
==========

Throttling is a distinctive and controversial feature of Moira.
If you are experiencing a delay or any other strange behavior
of notifications, chances are, it is because of throttling.

To understand throttling, imagine two triggers:

1. Send notification if CPU load on any of your servers is more than 75%.
2. Send notification if there is a fire in your server room.

It is a busy day, your servers are overloaded, and you are receiving
a ton of notifications about CPU load. Probably, you already have several
dozens of notifications in your inbox. You will likely delete all of them
at once, and you probably won't notice that one of these hundreds of letters
was about a fire in your server room.

So, the problem is: one misconfigured trigger spoils everything by spamming
your inbox with irrelevant notifications. Moira provides a protection
mechanism called throttling. Simple rules:

1. If a trigger sends more than 10 notifications per 1 hour,
   limit this trigger to 1 message per 30 minutes.
2. If a trigger sends more than 20 notifications per 3 hours,
   limit this trigger to 1 message per 1 hour.

It works like this:

- First notification is delivered immediately.
- Second notification is delivered immediately.
- ...
- Tenth notification is delivered immediately, and you get a warning:
  "Please, fix your system or tune this trigger to generate less events."
- Next notifications are delayed so that you receive one message
  per 30 minutes/1 hour. Nothing is lost, you just receive one message
  with a pack of events. Every message contains a warning: "Please, fix
  your system or tune this trigger to generate less events."

*Moira will enable and disable throttling automatically based
on frequency of events.*


Disabling Throttling
--------------------

There are four ways to disable throttling for a specific trigger:

1. Obey the warning message. That is, fix your system to generate less events.
   Or change trigger thresholds. Or use Graphite functions like
   ``movingAverage`` to remove spikes from your metric graph. This is the best
   method to deal with throttling.
2. Enable :doc:`maintenance` mode for some of your metrics. This will
   temporarily disable checking of a metric and give you time to fix
   the system:

.. image:: ../_static/maintenance.png
   :alt: maintenance mode

3. Manually reset throttling for your trigger. This basically means that
   you've fixed the system and would like to resume operation normally.
   It won't help if your trigger is still spamming notifications:

.. image:: ../_static/reset_throttling.png
   :alt: reset throttling

4. Entirely disable throttling for a subscription. *This is not recommended*,
   unless you really know what you are doing:

.. image:: ../_static/throttling.png
   :alt: disable throttling
   :width: 400
