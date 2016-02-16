Feeding metrics to Moira
========================

.. _carbon-c-relay: https://github.com/grobian/carbon-c-relay

This is an important and non-obvious part. Default Graphite relay does not support duplicating
metric stream to several backends, which is how Moira works. Good news is that you probably already
use a different relay that supports duplication, because the default relay is very slow.

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
