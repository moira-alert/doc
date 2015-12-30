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

   cd moira-alert/worker
   twistd -y moira/api/server.py -n

Run Checker.

.. code-block:: bash

   cd moira-alert/worker
   twistd -y moira/checker/server.py -n

Run tests.

.. code-block:: bash

   cd moira-alert/worker/tests
   trial functional
