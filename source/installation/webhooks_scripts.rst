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
---------------------------------------------------------------------------------

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
--------------------------------------------------------

.. code-block:: yaml

   - sender_type: webhook
     contact_type: test_webhook
     url: http://localhost:8080/webhook
     body: { "name": "test-name", "value": {{ .Contact.Value | quote }} }
     headers:
        test-header: test-value

Example of a request with explicitly specifying the request body in the config 
------------------------------------------------------------------------------

.. code-block:: json

   {
       "name": "test-name",
       "value": "test-contact-value"
   }

Delivery checks
---------------
As you may notice, Moira will do her best to send notification.
But often successful sending doesn't mean that notification was successfully delivered
(for example if delivering is a long lasting operation).

In order to solve the problem we add the delivery checks feature to webhook sender.

First please read the notifier :doc:`/installation/configuration` and pay attention to webhook sender.

The most important fields for performing delivery checks is ``url_template`` and ``check_template``.
These two fields are both `go templates <https://pkg.go.dev/text/template>`_ and support `sprig functions <https://masterminds.github.io/sprig/>`_.


How does it work?
~~~~~~~~~~~~~

When the notification is sent (HTTP POST request performed) Moira reads response status code and response body.
If response code is greater or equal 200 and less than 300, then for Moira it means that notification is sent ok, otherwise sent failed.

After successful notification sending ``url_template`` is filled with data to provide a valid url.
URL and other necessary information is stored in the database.

Separate goroutine reads such info from database (every ``check_timeout`` seconds) and perform delivery check request (HTTP GET request), with:

* URL got after filling ``url_template``
* user and password specified in ``delivery_check`` option of config
* headers specified in ``delivery_check`` option of config and headers from **HTTP Headers** above

If delivery check request succeeds, then the response body and some other fields (you can find details below) is used
to fill ``check_template``. The result of filling ``check_template`` must be one of valid strings
(which represent delivery states, more info could also be found below).
Based on calculated delivery state and count of already performed attempts Moira will do one of the following things:

* Mark delivery notification ok in metrics
* Mark delivery notification failed in metrics
* Mark that delivery checks is stopped in metrics
* Schedule one more delivery check

``url_template``
~~~~~~~~~~~~~~~~
Here is the list of data that available for use in ``url_template``

================== ============== ==============================================
Attribute          Type           Description
================== ============== ==============================================
.Contact.Type      string         Contact type
.Contact.Value     string         Contact value
.TriggerID         string         Trigger id
.SendAlertResponse map[string]any JSON response on POST request decoded into map
================== ============== ==============================================

The result of filling template must be a valid url.

For example, if we have:

* Contact.Type: slack
* Contact.Value: some_channel
* TriggerID: some-trigger-id
* And on send alert we have response:

.. code-block:: json

    {
        "some_value": 25,
        "another_value": "hello"
    }

And our ``url_template`` is the following:

.. code-block::

    "https://example.com/{{ .Contact.Type }}/{{ .Contact.Value }}/{{ .TriggerID }}/{{ .SendAlertResponse.another_value }}"

Our result URL will be:

.. code-block::

    "https://example.com/slack/some_channel/some-trigger-id/hello"

``check_template``
~~~~~~~~~~~~~~~~~~
Here is the list of data that available for use in ``check_template``

====================== ============== =============================================
Attribute              Type           Description
====================== ============== =============================================
.Contact.Type          string         Contact type
.Contact.Value         string         Contact value
.TriggerID             string         Trigger id
.DeliveryCheckResponse map[string]any JSON response on GET request decoded into map
====================== ============== =============================================

The result of filling the template must be one of the strings below:

============ ==============================================================================
String value Description
============ ==============================================================================
OK           Should be returned if notification was successfully delivered
FAILED       Should be returned if notification definitely was not delivered
PENDING      Should be returned if notification has not yet been delivered
EXCEPTION    Should be returned if error occurred while understanding the state of delivery
============ ==============================================================================

For example, if we have:

* Contact.Type: slack
* Contact.Value: some_channel
* TriggerID: some-trigger-id
* And on delivery check request we have response:

.. code-block:: json

    {
        "contact_value": "some_channel",
        "some_value": 25,
        "important_value": "ok"
    }

And our ``check_template`` is:

.. code-block::

    {{-if and
        (eq .DeliveryCheckResponse.contact_value .Contact.Value)
        (eq .DeliveryCheckResponse.important_value "ok")
    -}}
        OK
    {{- else -}}
        FAILED
    {{- end -}}

The result of filling the template will be ``OK``.
For Moira this means that notification was successfully delivered.

For the same ``check_template`` but following delivery check response body:

.. code-block:: json

    {
        "contact_value": "some_channel",
        "some_value": 25,
        "important_value": "not ok"
    }

The result of filling the template will be ``FAILED``.
For Moira this means that notification definitely was not delivered.

**Note** that if the result of filling ``check_template`` is one of ``PENDING``, ``EXCEPTION``,
Moira continues to perform delivery checks until ``max_attempts`` count will be performed.
