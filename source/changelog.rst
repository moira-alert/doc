Changelog
=========

.. _govaluate: https://github.com/Knetic/govaluate
.. _carbonapi: https://github.com/go-graphite/carbonapi/blob/a5304b99d57da999a8561f6dee7745ef88164d2c/COMPATIBILITY.md#functions
.. |supported Graphite functions| replace:: supported Graphite functions
.. _supported Graphite functions: https://github.com/go-graphite/carbonapi/blob/a5304b99d57da999a8561f6dee7745ef88164d2c/COMPATIBILITY.md#functions


2.2
---

- Add Redis Sentinel support.
- Increase new metric event processing speed by adding a cache on metric patterns.
- Update carbonapi (new functions: map, reduce, delay; updated: asPercent).
- Optimize reading metrics while checking trigger (removed unnecessary Redis transaction).
- Add domain autoresolving for self-metrics sending to Graphite.
- Fix concurrent read/write from expression cache.
- Re-enable Markdown in Slack sender.
- Optimize internal metric collection.
- Replace pseudotags with ordinary checkboxes in Web UI (but not on backend yet).
- Fix bug that allowed to create pseudotags (ERROR, etc.) as ordinary tags.
- Add metrics for each trigger handling time.
- Translate pagination.
- Make sorting by status the default option on trigger page.
- Hide tag list on trigger edit page.
- Sort tags alphabetically everywhere.
- Highlight metric row on mouse hover.
- Automatically add tags from search bar when creating new trigger.
- Add metric name to "Trigger has same timeseries names" error message.
- Update event names in case trigger name had changed.
- Fix bug in triggers with multiple targets. Metrics from targets T2, T3, ... were not deleted properly.
- Fix old-style configuration files in platform-specific packages.
- Fix bug that prevented non-integer timestamps from processing.
- Fix logo image background.
- Fix sorting on -s and 0s.
- Fix UI glitch while setting maintenance time.
- Fix retention scheme parsing for some rare cases with comments.


2.1
---

- Throw an exception if any target except the first one resolves in more than one metric.
- Fix Moira version detection in CI builds.
- Add user login information to API request logs.
- Fix long interval between creating a new trigger and getting data into that trigger.


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
