Manual Installation
===================

.. _golang: https://golang.org/doc/install
.. _redis: http://redis.io/download
.. _python: https://www.python.org/downloads/
.. _nginx: http://nginx.org/en/download.html
.. _config.json: https://github.com/moira-alert/web/blob/master/config.json.example


.. tip: For a quick-start getting Moira working, why not try out a :docker:`/installation/docker` version

There are following components you need to install before running Moira microservices:

1. golang_ version 1.4 or higher
2. redis_ database version 2.8 or higher
3. python_ version 2.7
4. web server e.g. nginx_

Install Moira Microservices
---------------------------

.. code-block:: bash

   export GOPATH=<your gopath>
   go get github.com/moira-alert/cache
   go get github.com/moira-alert/notifier/notifier
   pip install git+https://github.com/moira-alert/worker.git


Download Web UI Application
---------------------------

https://github.com/moira-alert/web/releases/latest

Configure
---------

1. Place configuration file to the default location, ``/etc/moira/config.yml``

You can dive into :doc:`/installation/configuration` syntax on a separate page.

2. Place nginx configuration file to ``/etc/nginx/conf.d/moira.conf``

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

3. Place UI config.json_ file to ``/var/local/www/moira/config.json``

Run
---

1. Run nginx and redis-server
2. Run cache

.. code-block:: bash

   $GOPATH/bin/cache --config=/etc/moira/config.yml

3. Run notifier

.. code-block:: bash

   $GOPATH/bin/notifier --config=/etc/moira/config.yml

4. Run API

.. code-block:: bash

   moira-api

5. Run checker

.. code-block:: bash

   moira-checker

Now you need to feed your metrics to Moira (see :doc:`/installation/feeding`) on port 2003 and to create alerts in UI (see :doc:`/user_guide/index`).
