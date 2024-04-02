Webhooks and Custom Scripts
===========================

Moira has two special kinds of senders: webhooks and scripts.

Script runs an executable on the same machine that runs notifier instances.
Webhook makes POST requests to a specified URL. Scripts and webhooks are
a flexible way to add integrations with services that are not supported
in Moira natively.

You may want to add several different scripts for users to choose from.
Next section describes how to implement this.


Scripts
-------

You can specify executable path and arguments in the notifier
configuration file (see :doc:`/installation/configuration` for details).

Add a separate section for each script:

.. code-block:: yaml

   - sender_type: script
     contact_type: jira
     exec: /usr/bin/post_to_jira --project=${contact_value}
   - sender_type: script
     contact_type: irc
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


Templated Parameters
--------------------

You may have noted that we use templated parameters like
``${contact_value}`` in configuration examples. You can use these
parameters in script as well as webhook contacts. Notifier will
replace them with actual values extracted from event.

================ ================================================
Parameter        Value
================ ================================================
${contact_id}    Contact ID
${contact_value} Contact value, as specified by user via web UI
${contact_type}  Contact type, as specified in web UI config file
${trigger_id}    Trigger ID
================ ================================================


Webhooks
--------

On each event, Moira will make a POST request to the URL specified
in notifier configuration file with the following JSON payload 
if the request `body` in the config is empty.


========= =========== =====================================
Attribute Type        Description
========= =========== =====================================
trigger   Trigger     Trigger data
events    Event Array List of events
contact   Contact     Contact data
plot      String      Base64 string containing trigger plot
throttled Bool        True if notifications are throttled
========= =========== =====================================


Fields Description
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


HTTP Headers
------------

============ ================
Name         Value
============ ================
User-Agent   Moira
Content-Type application/json
...          ...
============ ================

... - You can set additional headers in the webhook config 
(see :doc:`/installation/configuration` for details).


Example of a request without explicitly specifying the request body in the config
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

You can also explicitly specify the request body using go-templates. 
Available fields: .Contact.Value and .Contact.Type

Example of a configuration with an explicit request body
-------

.. code-block:: yaml

   - sender_type: webhook
     contact_type: test_webhook
     url: http://localhost:8080/webhook
     body: { "name": "test-name", "value": {{ .Contact.Value | quote }} }
     headers:
        test-header: test-value

Example of a request with explicitly specifying the request body in the config 
-------

.. code-block:: json

   {
       "name": "test-name",
       "value": "test-contact-value"
   }
