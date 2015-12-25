API and Checker microservices
=============================

.. _Twisted: http://twistedmatrix.com

Both microservices are built with Python Twisted_ framework and include a part of Graphite sources
for Graphite function evaluation.

.. note:: Run ``redis-server`` before you run these microservices.

Install dependencies.

.. code-block:: bash

   pip install -r requirements.txt

Run API.

.. code-block:: bash

   cd worker/bin/api
   twistd -y server.py -n

Run Checker.

.. code-block:: bash

   cd worker/bin
   twistd -y checker/server.py -n

Run tests.

.. code-block:: bash

   cd worker/tests
   trial functional
