Manual Installation
===================

.. _golang: https://golang.org/doc/install
.. _redis: http://redis.io/download
.. _nginx: http://nginx.org/en/download.html
.. _web.json: https://github.com/moira-alert/moira/blob/master/pkg/api/web.json
.. _configuration: https://moira.readthedocs.io/en/latest/installation/configuration.html#api


.. tip:: To get Moira running quickly, try :doc:`/installation/docker` version

There are following components you need to install
before running Moira microservices:

1. golang_ version 1.9 or higher
2. redis_ database version 3.2 or higher
3. web server e.g. nginx_


Build Moira Microservices
-------------------------

.. code-block:: bash

    go get -u github.com/moira-alert/moira
    cd $GOPATH/src/github.com/moira-alert/moira
    make build

You will find binaries in ``$GOPATH/src/github.com/moira-alert/moira/build``.


Download Web UI Application
---------------------------

https://github.com/moira-alert/web2.0/releases/latest

Download and unpack ``.tar.gz`` file into Nginx static
files directory (e.g. ``/var/local/www/moira``).


Configure
---------

1. If you need to override default settings, place configuration
   files somewhere on your disk (e.g. ``/etc/moira/``). You can dive into
   :doc:`/installation/configuration` syntax on a separate page.

2. Place nginx configuration file to proper location
   (e.g. ``/etc/nginx/conf.d/moira.conf``):

.. code-block:: text

    server {
        listen 127.0.0.1:80;
        location / {
            root /var/local/www/moira;
            index index.html;
            try_files $uri $uri/ /index.html;
        }
        location /api/ {
            proxy_pass http://127.0.0.1:8081;
        }
    }

3. If you need to override UI settings, edit web.json_ file.
You can find its location in API configuration_.


Run
---

1. Run nginx and redis-server
2. Run microservices

.. code-block:: bash

    $GOPATH/src/github.com/moira-alert/moira/build/cache
    $GOPATH/src/github.com/moira-alert/moira/build/checker
    $GOPATH/src/github.com/moira-alert/moira/build/notifier
    $GOPATH/src/github.com/moira-alert/moira/build/api

Now you need to feed your metrics to Moira (see :doc:`/installation/feeding`)
on port 2003 and to create alerts in UI (see :doc:`/user_guide/index`).
