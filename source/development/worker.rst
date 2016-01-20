API and Checker microservices
=============================

.. _Twisted: http://twistedmatrix.com

Both microservices are built with Python Twisted_ framework and include a part of Graphite sources
for Graphite function evaluation.

.. note:: Run ``redis-server`` before you run these microservices.

Install .

.. code-block:: bash

   python setup.py install

Run API.

.. code-block:: bash

   moira-api

Run Checker.

.. code-block:: bash

   moira-checker

Run tests.

.. code-block:: bash

   cd moira-alert/worker/tests
   trial functional
