Architecture
============

.. _target: http://graphite.readthedocs.org/en/1.0/url-api.html#target

.. raw:: html

    <style> .moira-state { padding: 5px; padding-left: 13px; padding-right: 13px; border-radius: 1pc; display: inline-block; color: white} </style>

Terminology
-----------

Trigger
^^^^^^^

Check description, consists of the following parts:

- name
- one or more targets
- warn, error value limits or expression to calculate state
- one or more tags
- ttl limit, after wich trigger will turn into NODATA state
- check schedule

State
^^^^^

Each metric can be only in one state at some time moment:

.. raw:: html

   <span style="background-color: #00bfa5" class="moira-state">OK</span>
   
   <span style="background-color: #ffc107" class="moira-state">WARN</span>
   
   <span style="background-color: #ff5722" class="moira-state">ERROR</span>
   
   <span style="background-color: #9e9e9e" class="moira-state">NODATA</span>

Target
^^^^^^

Graphite target_, contains metric patterns and functions

For example:

.. code-block:: text 
   
   averageSeries(server.web*.load)

Pattern
^^^^^^^

Raw graphite metric or it mask according multiple graphite metrics

For example, this is pattern from previous example:

.. code-block:: text

   server.web*.load
   
Last check
^^^^^^^^^^

Information about last trigger checking for each target:

- checked value
- timestamp
- state

Trigger event
^^^^^^^^^^^^^

Created if any of trigger metrics state changed

Contains:

- trigger id
- metric name (given by target)
- new state
- previous state
- timestamp

Tags
^^^^

Strings markers for grouping triggers and selection them in subscriptions.

Subscription
^^^^^^^^^^^^

Contains:
 
- set of tags
- contacts to send notifications
- schedule

Dataflow
--------

Save and filter incoming metrics
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. image:: ../_static/dfd-cache.svg
   :alt: cache
   :width: 50%

When user adds a new trigger, metrics patterns are parsed from trigger targets and saved in redis moira-pattern-list.

Cache refreshes this list every one second.

Matched pattern metrics are saved in redis moira-metric:<metricname> set.

Redis pub/sub used to inform checker-master, that incoming metric should be checked as soon as possible.

Checker-master read triggers by pattern from redis moira-pattern-triggers:<pattern> list and adds triggers to check set
moira-triggers-tocheck.

In case of no data for metrics, all triggers add to check once per nodata_check_interval setting.

Check triggers
^^^^^^^^^^^^^^

.. image:: ../_static/dfd-checker.svg
   :alt: checker
   :width: 50%
   
Checker-worker constantly reads redis moira-triggers-tocheck set and calculates trigger targets values.

Target can contain one or multiple metrics, so results are written as per metrics, which name resolved by target.

Redis moira-metric-last-check:<trigger_id> contain last check json with metrics states.

In case some metric has changed its state, new event written to moira-trigger-events list if value timestamp is allowed by trigger schedule.

Process trigger events
^^^^^^^^^^^^^^^^^^^^^^

.. image:: ../_static/dfd-notifier-events.svg
   :alt: checker
   :width: 30%

Part of notifier constantly pull new events from moira-trigger-events and schedule notifications according to subscription schedule and throttling rules.

Trigger set of tags must include subscription set of tags to process subscription for trigger.

Subscription schedule delays notifications of occurred event to beginning of next allowed time interval. This distinct trigger schedule, when trigger event not generated.

Throttling rules will delay notifications: 

- for 30 min in case events generated more than 10 count during last hour
- for 1 hour in case events generated more than 20 count  during last 3 hours

Scheduled notifications are written to moira-notifier-notifications set

Process notifications
^^^^^^^^^^^^^^^^^^^^^^

.. image:: ../_static/dfd-notifier-notifications.svg
   :alt: checker
   :width: 50%

Part of notifier constantly pull schedled noficications from moira-notifier-notifications set.

It calls sender for certain contact type and writes notification back to redis in case of send error.