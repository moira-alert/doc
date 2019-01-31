Google Summer of Code
=====================

Here is the ideas page for Google Summer of Code 2019.

We encourage interested students to contact mentors to discuss these ideas or propose
new ones. The Moira team would appreciate your contributions.

About Moira
-----------

Moira is a realtime alerting system based on Graphite data.

TODO: Add more about Moira.

Moira's source code is licensed under MIT.

Mentors
-------

* Alexander Sushko (`sashasushko <https://github.com/sashasushko>`_), web UI developer
* Alexey Kirpichnikov (`beevee <https://github.com/beevee>`_), core contributor
* Arkady Borovsky (`borovskyav <https://github.com/borovskyav>`_), core developer
* Timur Kamaev (`kamaev <https://github.com/kamaev>`_), core developer

Ideas
-----

Support for additional delivery channels
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Explanation.**
Moira supports a number of delivery channels such as email, Slack, Telegram, etc. to inform users that a certain trigger was activated (see :ref:`subscriptions`).
The aim of this project is to provide support for a number of additional delivery channels.
To do so, one should talk to community and research possible channels to be added, contribute corresponding `senders <https://github.com/moira-alert/moira/tree/master/senders>`_, and tune the web UI to allow users to create subscriptions using new channels.

**Code reference.**
See `email sender <https://github.com/moira-alert/moira/blob/master/senders/mail/mail.go>`_ source code or `Pushover sender <https://github.com/moira-alert/moira/blob/master/senders/pushover/pushover.go>`_ source code.

**Required skills.**
Go skills to add senders, a bit of JavaScript and React to tune the web UI.

**Expected outcome.**
Some qualitative or quantitative data on channel popularity is collected.
Several delivery channels are added to Moira and released.

**Mentors.**
Alexey Kirpichnokov (alexkir@kontur.ru),
Alexander Sushko (sushko@kontur.ru).