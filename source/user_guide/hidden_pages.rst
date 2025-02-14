Hidden Pages
============

Some rarely used features of Moira are hidden on pages that are not
linked from anywhere. This is a deliberate design decision to reduce
visual clutter in the main UI.

You need to type an address of a hidden page manually, like this: ``http://moira.example.com/hidden_page``.

.. _notifications-hidden-page:

Notifications
-------------

If Moira encounters an error while sending a notification, it will try
again every minute for the next 24 hours. After that period, the notification
is considered lost. You can configure this via ``resending_timeout`` parameter
in notifier yaml config.

In some cases notifications will never be delivered, for example if a user
specifies invalid contact.

If you need to interrupt this behavior, you can manually delete failing
notifications at ``/notifications``.


Tags
----

Deleting a tag is a rare and dangerous operation, but you still can
do it at ``/tags``.

Tags list shows how many triggers and subscriptions use a tag.
You can't delete a tag if there is at least one trigger that uses it.
You **can** delete a tag that is used in a subscription.


Patterns
--------

You can see a list of all Graphite patterns with links to corresponding
triggers and list of all matching metrics at ``/patterns``.


Contacts
--------

Sometimes you need to find (and/or modify, delete) contact registered in Moira,
which not belongs to you or your team.
For this purpose ``/contacts`` page maybe useful.
If authorization is enabled this page is available only for admins.


Teams
--------

We recommend to use teams to store contacts and subscriptions
because after creating inside the team they are visible to all team members.
With ``/teams/all`` you can find all the teams exist in Moira.
If authorization is enabled this page is available only for admins.


Noisiness
--------

As soon as you've set up your triggers, contact and subscriptions you may want to know
what triggers generate more events than overs.
With ``/noisiness`` you can find how many events each trigger has generated.