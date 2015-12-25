Security
========

Typically, internal DevOps tools like Graphite are deployed in intranet without any external access,
so you can skip authentication and leave everything accessible to everyone. But powerful Moira
features, like separate subscriptions for tags, work best when you have some way to tell apart users.

Moira doesn't provide any authentication mechanism. It is hard to find one that fits all users.
Instead, Moira accepts ``X-WebAuth-User`` header with some user id, like login name. You are free to
set up any reverse proxy and configure it to provide this header.

If you don't, Moira will assume that user id is "anonymous".

.. warning:: Even if you do provide authentication header, please note that most parts of Moira are
             read and write accessible to every user, and there is no meaningful way of authorization
             in Moira. This is by design, because Moira is an internal DevOps tool. Separating users
             is a convenience, not protection feature.
