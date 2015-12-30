Installation
============

.. _carbon-c-relay: https://github.com/grobian/carbon-c-relay
.. _Node.js: https://nodejs.org/
.. _NPM: https://www.npmjs.com/

Moira is designed to be cross-platform and scalable. You can arrange parts of Moira any way that
best suits your load and your production environment. We don't believe that we can provide a single
installer that will satisfy any user's needs.

This page provides simple instructions for CentOS 7 to try and get a grip of Moira inner workings
and functionality. When you become familiar with Moira, hopefully, you will be able to devise a way
to install Moira on your production cluster, regardless of OS or number of nodes.

.. note:: We suggest that you run all Moira microservices under a special ``moira`` user account

Get Moira
^^^^^^^^^

Some parts of Moira are written in Go. We distribute Moira as a source code, so in order to
correctly build binaries you should obey Go rules of directory structure. Fortunately, Python is
not so directory-tree-dependent, so you can just check out all of Moira into your GOPATH
(``/opt/go`` in our example).

.. code-block:: bash

   yum install golang
   export GOPATH=/opt/go
   mkdir -p /opt/go/src/github.com/moira-alert
   cd /opt/go/src/github.com/moira-alert
   git clone https://github.com/moira-alert/web.git
   git clone https://github.com/moira-alert/cache.git
   git clone https://github.com/moira-alert/notifier.git
   git clone https://github.com/moira-alert/worker.git


Copy configuration file
^^^^^^^^^^^^^^^^^^^^^^^

Following the DRY principle, all Moira microservices can share the same configuration file.
You can use an example configuration from our repository. We suggest you put it into ``/etc/moira``
directory.

.. code-block:: bash

   cd moira
   cp conf.example.yml /etc/moira/config.yml

You can dive into :doc:`/configuration` syntax on a separate page.


Redis
^^^^^

All Moira microservices communicate through a single Redis database.

.. code-block:: bash

   yum install redis
   systemctl enable redis
   systemctl start redis


Microservices
^^^^^^^^^^^^^

Cache
-----

Build it from source.

.. code-block:: bash

   cd /opt/go/src/github.com/moira-alert/cache
   go get

.. note:: Check that executable file **/opt/go/bin/cache** is created

Create systemd service configuration at ``/etc/systemd/system/moira-cache.service``.

.. code-block:: text

   [Unit]
   Description=moira-cache - metric stream filtering and caching for Moira

   [Service]
   ExecStart=/opt/go/bin/cache --config=/etc/moira/config.yml
   WorkingDirectory=/opt/go/bin/
   User=moira
   Group=moira
   PIDFile=/var/run/moira-cache.pid
   PermissionsStartOnly=true
   ExecStartPre=-/usr/bin/touch /var/run/moira-cache.pid
   ExecStartPre=/usr/bin/chown -R moira:moira /var/run/moira-cache.pid
   Restart=on-failure
   ExecReload=/bin/kill -USR2 $MAINPID

   [Install]
   WantedBy=multi-user.target

.. note:: Cache supports zero-downtime restart, which is executed on SIGUSR2 (use ``reload`` command)

Create user and log directory.

.. code-block:: bash

   useradd moira
   mkdir -p /var/log/moira/cache
   chown -R moira:moira /var/log/moira

Run.

.. code-block:: bash

   systemctl daemon-reload
   systemctl enable moira-cache
   systemctl start moira-cache


Checker
-------

.. code-block:: bash

   yum install make gcc python-devel python-pip
   cd /opt/go/src/github.com/moira-alert/worker
   pip install -r requirements.txt

Create systemd service configuration at ``/etc/systemd/system/moira-checker.service``.

.. code-block:: text

   [Unit]
   Description=moira-checker - graphite metric checker service based on twisted python framework

   [Service]
   ExecStart=/usr/bin/twistd --nodaemon --python /opt/go/src/github.com/moira-alert/worker/moira/checker/server.py --pidfile=  --logger moira.logs.checker -r epoll
   WorkingDirectory=/opt/go/src/github.com/moira-alert/worker/
   User=moira
   Group=moira
   Restart=always

   [Install]
   WantedBy=multi-user.target

Run.

.. code-block:: bash

   systemctl daemon-reload
   systemctl enable moira-checker
   systemctl start moira-checker


API
---

Create systemd service configuration at ``/etc/systemd/system/moira-api.service``.

.. code-block:: text

   [Unit]
   Description=moira-api - REST-API service over http based on twisted python framework

   [Service]
   ExecStart=/usr/bin/twistd --nodaemon --python /opt/go/src/github.com/moira-alert/worker/moira/api/server.py --pidfile=  --logger moira.logs.api -r epoll
   WorkingDirectory=/opt/go/src/github.com/moira-alert/worker/
   User=moira
   Group=moira
   Restart=always

   [Install]
   WantedBy=multi-user.target

Run.

.. code-block:: bash

   systemctl daemon-reload
   systemctl enable moira-api
   systemctl start moira-api


Notifier
--------

Build it from source.

.. code-block:: bash

   cd /opt/go/src/github.com/moira-alert/notifier/notifier
   go get

.. note:: Check that executable file **/opt/go/bin/notifier** is created


Create systemd service configuration at ``/etc/systemd/system/moira-notifier.service``.

.. code-block:: text

   [Unit]
   Description=moira-notifier - event notifications for Moira

   [Service]
   ExecStart=/opt/go/bin/notifier --config=/etc/moira/notifier.yml
   WorkingDirectory=/opt/go/bin
   User=moira
   Group=moira
   Restart=always
   TimeoutStopSec=30s

   [Install]
   WantedBy=multi-user.target

Run.

.. code-block:: bash

   systemctl daemon-reload
   systemctl enable moira-notifier
   systemctl start moira-notifier


UI
--

User interface is a static web application. In order to build it, you'll need Node.js_ and NPM_.

.. code-block:: bash

   cd /opt/go/src/github.com/moira-alert/web
   npm run build
   mkdir /var/local/www/moira
   cp -r dist /var/local/www/moira/
   cp index.html /var/local/www/moira/
   cp config.example.json /var/local/www/moira/config.json

.. note:: Edit config.json and remove unused contact types.


Usually, you'll want to use Nginx to serve pages. Here is a config example.

.. code-block:: text

    server {
        listen 127.0.0.1:80;
        location / {
            root /var/local/www/moira;
            index index.html;
        }
        location /api/ {
            proxy_pass http://127.0.0.1:8081;
        }
    }


Send metrics to cache
---------------------

This is an important and non-obvious part. Default Graphite relay does not support duplicating
metric stream to several backends, which is how Moira works. Good news is that you probably already
use a different relay that supports duplication, because default relay is very slow.

Here is an example of carbon-c-relay_ configuration for Moira.

.. code-block:: text

   cluster moira
   forward
       moira-host:2003
   ;

   match *
       send to moira
   ;

.. note:: replace **moira-host** with your host name
