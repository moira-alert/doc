Changelog
=========

.. _govaluate: https://github.com/Knetic/govaluate
.. _carbonapi: https://github.com/go-graphite/carbonapi/blob/master/COMPATIBILITY.md#functions
.. |supported Graphite functions| replace:: supported Graphite functions
.. _supported Graphite functions: https://github.com/go-graphite/carbonapi/blob/master/COMPATIBILITY.md#functions


2.0
---

Version 2.0 is fully rewritten in Go instead of Python. This implies lower CPU load in Checker and API microservices, but also changes the list of |supported
Graphite functions|_.

We also introduce new UI based on React. It is not backwards-compatible with old API, but new API supports both old and new UI.


Breaking Changes
^^^^^^^^^^^^^^^^

- New structure of :doc:`installation/configuration` files.
- New Advanced mode expression format. Moira 2.0 supports govaluate_ expressions instead of Python expressions. Use ``moira-cli -convert-expressions`` to convert.
- API methods URLs do not have trailing slashes anymore.
- API ``/notification`` method returns valid JSON list instead of plain text.
- ``ttl`` parameter in API calls is always a number instead of string.
- API ``PUT`` methods strictly separate create and update operations.
- There is no ``tag maintenance`` entity anymore.
- Error messages return valid JSON instead of plain text.
- Support for Graphite functions changed. See carbonapi_ compatibility list for details.


Other Improvements
^^^^^^^^^^^^^^^^^^

- Internal Graphite metric names changed.
- Numerous bugs fixed. Some new were created :)
