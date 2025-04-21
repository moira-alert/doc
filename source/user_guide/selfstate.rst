Self State Monitor
==================

Self State Monitor is a built-in mechanism designed to protect
end user from false ``NODATA`` notifications and notify administrator
and end user about issues in Moira and/or Graphite systems.


Why Self State Monitor
-----------------------

A situation is possible when Graphite Relay, Redis DB or Moira-Filter
service breaks down. This leads to the fact that Moira doesn't receive
any metrics from Graphite. In this case, Moira has no metrics on which
it could check state of the triggers. According to the Moira logic,
it should switch triggers to ``NODATA`` state and send alert messages to users.

To handle this situation properly, we recommend turning on the Self
State Monitor. In this case, Moira will **prevent itself from sending
alert messages to end users but will notify administrators of the existing
problem or end users who have subscribed to Self State alerts (see :ref:`system-subscriptions-description`)**.

.. warning::

  When Self State Monitor detects a problem, it disables notifications to end users by their triggers, but can send notifications about the problem via system-subscriptions. When the problem resolved, Self State Monitor notifies admins about it and end users via system-subscriptions and turn back sending notifications automatically.

  Please, read this manual before using Self State Monitor in production.

.. seealso::

  For a better understanding, look at the architecture of the :ref:`Moira microservices <microservices-architecture>`.


.. _when-monitor-helps:

When Self State Monitor Helps
-----------------------------

Self state monitor checks these situations:

1. If there is no connection between Moira and Redis for longer
   than ``redis_disconect_delay``.
2. If Moira-Filter receive no metrics for longer than
   ``last_metric_received_delay``.
3. If Moira-Checker checks no triggers for longer than ``last_check_delay``.

.. seealso::

  All the above configuration parametres can be found in the :ref:`Moira-Notifier section <notifier-configuration>`
  on configuration page.


How Self State Monitor Works
----------------------------

When you turn Self State Monitor on, it works this way:

* Self State Monitor checks :ref:`Moira state <when-monitor-helps>`
  every 10 seconds.

* Something breaks down. It can be Graphite-Relay, connection
  to Redis DB or crashed Moira-Filter docker container.

* Self State send alarm message to administrator with issue discription and switch own state to ``WARN``

  *Here is an example of message*:

    .. image:: ../_static/helth-check-email.png
     :alt: email alarm message

* Self State Monitor turns Moira-Notifier service off,
  switching it in ``ERROR`` state.

  .. note::

    When Moira-Notifier switches to ``ERROR`` state, it mutes all messages to end users and only alerts administrators about Moira health issues.
    You need to fix existing problems and then manually switch Moira-Notifier back to ``OK`` using :ref:`API <notifier-state-api>`.

  *When Moira-Notifier not in* ``OK`` *state, Moira will show you an error in Web UI*:

    .. image:: ../_static/helth-check-webui.png
      :alt: WEB UI error notification

* Self State Monitor waits for ``user_notifications_interval`` and switches own state from ``WARN`` to ``ERROR`` if problem persists.
  Then users will be notified about the problem via their system-subscriptions.

* If problem disappears, the Self State Monitor switches own state to ``OK`` and sends notifications according to these rules.

  - Self State Monitor state mutates from ``WARN`` to ``OK`` then notification about Moira normalize are sent only to admins.
  - Self State Monitor state mutates from ``ERROR`` to ``OK`` then notification sends to both admins and users via system-subscriptions.

.. note::
   For a better understanding, look at pictures below
   
If Self State detects a problem, then it possible 2 ways to next actions:

* If problem persists a much more than ``user_notifications_interval``
  .. image:: ../_static/selfstate_full_cycle_WARN_to_ERROR.png
   :alt: Self State sends notifications to admins and users

* If problem persists a less than ``user_notifications_interval``
  .. image:: ../_static/selfstate_full_cycle_WARN_to_OK.png
   :alt: Self State sends notifications to admins only

-----

.. _notifier-state-api:

Turn Moira Notifier On and Off
------------------------------

You can reveal current Moira-Notifier state or change it
on a hidden ``/notifications`` page.

.. image:: ../_static/notifier-toggle.png
 :alt: Notifier toggle

.. warning::

  Please, note this toggle changes Moira-Notifier state, not user notifications preferences.

  When you disable notifications with this toggle, Moira-Notifier stops sending messages to all users.
