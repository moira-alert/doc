.. _auth_basic_module: http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html

Security
========

Typically, internal DevOps tools like Graphite are deployed in intranet
without any external access, so you can skip authentication and leave
everything accessible to everyone. But powerful Moira features, like
separate subscriptions for tags, work best when you have some way
to tell apart users.

Moira doesn't provide any authentication mechanism. It is hard to find
one that fits all situations. Instead, Moira accepts ``X-WebAuth-User``
header with some user id, like login name. You are free to set up any
reverse proxy and configure it to provide this header.

If you don't, Moira will assume that user id is "anonymous".

.. warning:: Even if you do provide authentication header, please note that most parts of Moira are
             read and write accessible to every user, and there is no meaningful way of authorization
             in Moira. This is by design, because Moira is an internal DevOps tool. Separating users
             is a convenience, not protection feature.


Example of Nginx Configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Assuming that Moira UI static files are in ``/var/www/moira-web``
and API is running on port 8081

.. code-block:: text

   server {
     auth_basic "Moira";
     auth_basic_user_file /etc/nginx/htpasswd;

     listen 0.0.0.0:80 default_server;

     location / {
       root /var/www/moira;
       index index.html;
       try_files $uri $uri/ /index.html;
     }

     location /api/ {
       proxy_pass http://127.0.0.1:8081;
       proxy_set_header X-WebAuth-User $remote_user;
     }
   }

Look at auth_basic_module_ if you need more details of Nginx
basic authentication.


Webhooks and Custom Scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^

When configuring :doc:`/installation/webhooks_scripts`, note that
``${contact_value}`` is substituted with user input value from web UI.
It means that a malicious user can potentially run anything or make
arbitrary web requests on a server that runs Moira notifier.
Always make sure you can trust your users if you use ``${contact_value}``
templated parameter in your scripts or webhooks.
