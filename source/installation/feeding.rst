Feeding Metrics to Moira
========================

.. _carbon-c-relay: https://github.com/grobian/carbon-c-relay

Moira needs to keep its own local copy of your metric data to improve performance and reduce load
on your existing graphite cluster. This means data needs to be duplicated from your existing stream
and sent to your existing cluster and to your moira installation.

Unfortunatly, the Carbon-Relay with Graphite does not support duplication of data to multiple
backends, and so you need to use a more feature rich carbon relay such as carbon-c-relay_.

The following is a basic example configuration which defines two clusters and sends all metrics
to both at once. One cluster is Moira installation, and the other uses consistent hashing across
a three node cluster of Carbon servers.

.. code-block:: text

   cluster moira
   forward
       moira-host:2003
   ;

   cluster graphite
    carbon_ch
        127.0.0.1:2006=a
        127.0.0.1:2007=b
        127.0.0.1:2008=c
    ;

   match *
       send to 
           moira
           graphite
   ;
.. note:: replace **moira-host** with your host name
