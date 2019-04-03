Webhooks and Custom Scripts
===========================

Moira has two special kinds of senders: webhooks and scripts.

Script runs an executable on the same machine that runs notifier instances.
Webhook makes POST requests to a specified URL.

Scripts and webhooks are a flexible way to add integrations with
services that are not supported in Moira natively. You may even want
to add several different scripts for users to choose from. Next section
describes how to implement this.


Scripts
-------

Web UI users are not allowed to specify which executable to run.
It is unsafe to do so, and even if they could, there would be no way
for them to put this executable on servers (or in containers) that
run notifier instances.

Instead, executable path and arguments are specified in notifier
configuration file (see :doc:`/installation/configuration`).

You can specify several sections with different scripts:

.. code-block:: yaml

   - type: script
     name: jira
     exec: /usr/bin/post_to_jira --project=${contact_value}
   - type: script
     name: irc
     exec: /opt/myscripts/irc_adapter ${contact_value} ${trigger_id}

Then, in web UI configuration:

.. code-block:: json

   {
       "contacts": [
           {
               "type": "jira",
               "label": "Create JIRA issue",
               "placeholder": "Project name"
           },
           {
               "type": "irc",
               "label": "Post to IRC channel",
               "placeholder": "Channel name"
           }
       ],
       ...
   }


Webhooks
--------

Web UI users can specify a URL while creating webhook type contact.
On each event, Moira will make a POST request to this URL with the
following payload.


========= =========== =====================================
Attribute Type        Description
========= =========== =====================================
trigger   Trigger     Trigger data
events    Event Array List of events
contact   Contact     Contact data
plot      String      Base64 string containing trigger plot
throttled Bool        True if notifications are throttled
========= =========== =====================================


Fields description
------------------

Trigger
~~~~~~~

=========== ============ ====================
Attribute   Type         Description
=========== ============ ====================
id          String       Trigger ID
name        String       Trigger name
description String       Trigger description
tags        String Array List of trigger tags
=========== ============ ====================


Event
~~~~~

============= ======= =====================
Attribute     Type    Description
============= ======= =====================
metric        String  Metric name
value         Float64 Metric value
timestamp     Int64   Event timestamp
trigger_event Bool    Event type
state         String  Current metric state
old_state     String  Previous metric state
============= ======= =====================


Contact
~~~~~~~

========= ====== ==============
Attribute Type   Description
========= ====== ==============
type      String Contact type
value     String Contact value
id        String Contact ID
user      String Contact Author
========= ====== ==============


Example
-------

.. code-block:: json

   {
       "trigger": {
           "id": "triggerID",
           "name": "triggerName",
           "description": "triggerDescription",
           "tags": [
               "triggerTag1",
               "triggerTag2"
           ]
       },
       "events": [
           {
               "metric": "metricName1",
               "value": 0,
               "timestamp": 499165200,
               "trigger_event": false,
               "state": "OK",
               "old_state": "ERROR"
           },
           {
               "metric": "triggerName",
               "value": 0,
               "timestamp": 1445412480,
               "trigger_event": true,
               "state": "OK",
               "old_state": "ERROR"
           },
           {
               "metric": "metricName2",
               "value": 0,
               "timestamp": -446145720,
               "trigger_event": false,
               "state": "OK",
               "old_state": "ERROR"
           }
       ],
       "contact": {
           "type": "webhookContactName",
           "value": "https://localhost/webhooks/moira",
           "id": "9728adae-1487-4e5b-80f6-8496f59b223e",
           "user": "author"
       },
       "plot": "",
       "throttled": false
   }
