Notifications
=============

When trigger changes his state a new notification event generated.

If this event matched by any subscription a new notification will be scheduled for specific contact.

If notification can not be delivered, it will be sended again after one minute.

This will be continued for configured ``resending_timeout`` until notification would be successfully sent.

In some cases notification will never be delivered for e.g. wrong contact type or value.

You can delete those notifications from hidden page /#/notifications/. That means you need type it in browser address bar.