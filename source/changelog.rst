Changelog
=========

.. _govaluate: https://github.com/Knetic/govaluate
.. _carbonapi: https://github.com/go-graphite/carbonapi/blob/ccac7217894801a5a6ceb8602a70ea0d79e975cf/cmd/carbonapi/COMPATIBILITY.md#functions
.. |supported Graphite functions| replace:: supported Graphite functions
.. _supported Graphite functions: https://github.com/go-graphite/carbonapi/blob/ccac7217894801a5a6ceb8602a70ea0d79e975cf/cmd/carbonapi/COMPATIBILITY.md#functions

2.5.0
-----

- Feature usage of icons in Slack notifications `moira-alert/moira#180 <https://github.com/moira-alert/moira/issues/180>`_. See more: :ref:`slack-icons`.
- Improve plotting conditions to render non-empty timeseries only `moira-alert/moira#197 <https://github.com/moira-alert/moira/issues/197>`_.
- Fix advanced schedule in subscriptions `moira-alert/moira#162 <https://github.com/moira-alert/moira/pull/162>`_.
- Remove deprecated method ``PUT trigger/{{triggerId}}/maintenance``, use ``PUT trigger/{{triggerId}}/setMaintenance`` instead `moira-alert/moira#161 <https://github.com/moira-alert/moira/pull/161>`_.
- Upgrade NPM dependencies for security reasons `moira-alert/moira#194 <https://github.com/moira-alert/moira/issues/194>`_.
- Get rid of deprecated 2.2 related conversion code. Fixed a bug that turned old pseudo-tags ``ERROR`` ``DEGRADATION`` ``HIGH DEGRADATION`` to subscription settings checkboxes `moira-alert/moira#184 <https://github.com/moira-alert/moira/issues/184>`_.
- Fix multibyte splitting bug in graph titles `moira-alert/moira#179 <https://github.com/moira-alert/moira/issues/179>`_.
- Limit supported browser by Chrome, Firefox, Safari (all mobile and desktop) and Edge (only desktop). Only last 2 major versions `moira-alert/web2.0#210 <https://github.com/moira-alert/web2.0/pull/210>`_.

2.4.0
-----

- Timeseries graphs in notifications `moira-alert/moira#148 <https://github.com/moira-alert/moira/pull/148>`_. See more :ref:`subscriptions-plotting`.
- Add api method ``GET trigger/{{triggerId}}/render`` to imlement timeseries plotting in api `moira-alert/moira#137 <https://github.com/moira-alert/moira/pull/137>`_.
- Add maintenance for a whole trigger. Add new api method ``PUT trigger/{{triggerId}}/setMaintenance``. ``PUT trigger/{{triggerId}}/maintenance`` is deprecated now `moira-alert/moira#138 <https://github.com/moira-alert/moira/pull/138>`_, `moira-alert/web2.0#199 <https://github.com/moira-alert/web2.0/pull/199>`_.
- Add extra maintenance intervals: 14 and 30 days `moira-alert/web2.0#198 <https://github.com/moira-alert/web2.0/pull/198>`_.
- Add option to mute notifications about new metrics in the trigger `moira-alert/moira#120 <https://github.com/moira-alert/moira/pull/120>`_. See more: :doc:`/user_guide/nodata`.
- Allow user to remove all ``NODATA`` metrics from trigger `moira-alert/moira#124 <https://github.com/moira-alert/moira/pull/124>`_.
- Check Lazy triggers (triggers without any subscriptions) less frequently `moira-alert/moira#131 <https://github.com/moira-alert/moira/pull/131>`_. See more :ref:`lazy-triggers-checker`.
- Run single NODATA checker worker at single moment `moira-alert/moira#129 <https://github.com/moira-alert/moira/pull/129>`_.
- Avoid throttling of remote-triggers when trigger switches to ``EXCEPTION`` and back to ``OK`` `moira-alert/moira#121 <https://github.com/moira-alert/moira/pull/121>`_.
- Consider the status of the trigger when rendering the trigger status indicator `moira-alert/web2.0#195 <https://github.com/moira-alert/web2.0/pull/195>`_.
- Replace useless trigger export button with "Duplicate" `moira-alert/web2.0#189 <https://github.com/moira-alert/web2.0/pull/189>`_.
- Add Moira-Notifier toggle on :doc:`/user_guide/hidden_pages` `moira-alert/web2.0#191 <https://github.com/moira-alert/web2.0/pull/191>`_.
  **Please, read** :doc:`/user_guide/selfstate` **first**.
- Show contact type icon on :doc:`/user_guide/hidden_pages` `moira-alert/web2.0#196 <https://github.com/moira-alert/web2.0/pull/196>`_.
- Show TTL and TTLState in Advanced mode `moira-alert/web2.0#197 <https://github.com/moira-alert/web2.0/pull/197>`_.
- Throw an exception if first target is no longer valid `moira-alert/moira#122 <https://github.com/moira-alert/moira/pull/122>`_.
- Refactor cli. Remove old converters, whiсh were written before moira 2.2 `moira-alert/moira#139 <https://github.com/moira-alert/moira/pull/139>`_.
- Update golang to version 1.11.2 `moira-alert/moira#147 <https://github.com/moira-alert/moira/pull/147>`_.
- Flush trigger events when removing the trigger `moira-alert/moira#116 <https://github.com/moira-alert/moira/pull/116>`_.
- Remove redundant Graphite-metrics that counted the time of check of each single trigger `moira-alert/moira#117 <https://github.com/moira-alert/moira/pull/117>`_.
- Add api method ``GET trigger/search`` to implement full-text trigger search in api, ``GET trigger/page`` is deprecated now `moira-alert/moira#125 <https://github.com/moira-alert/moira/pull/125>`_.
- Fix Redis leakages: some data was not removed properly from Redis storage `moira-alert/moira#129 <https://github.com/moira-alert/moira/pull/129>`_.
- Fix bug in trigger schedule due to which triggers were considered suppressed between 23:59:00 and 00:00:59 `moira-alert/moira#127 <https://github.com/moira-alert/moira/pull/127>`_.
- Fix bug in trigger when specific schedule time didn't work if start time was bigger than end time `moira-alert/moira#119 <https://github.com/moira-alert/moira/pull/119>`_.
- Fix bug in ``Create and test`` button when add new subscription `moira-alert/web2.0#194 <https://github.com/moira-alert/web2.0/pull/194>`_.
- Fix bug that increases updated last checks count when user create or update trigger from api (or web) `moira-alert/moira#146 <https://github.com/moira-alert/moira/pull/146>`_.
- Fix bug which allowed to use other people's contacts your in subscriptions `moira-alert/moira#145 <https://github.com/moira-alert/moira/pull/145>`_.
- Fix bug that allowed to create and use an empty tag in subscriptions and triggers `moira-alert/moira#144 <https://github.com/moira-alert/moira/pull/144>`_.
- Fix bug when senders didn't resolve ``EXCEPTION`` state `moira-alert/moira#156 <https://github.com/moira-alert/moira/pull/156>`_.
- Update `Moira Client 2.4 <https://github.com/moira-alert/python-moira-client/releases/tag/2.4>`_.
- Update `Moira Trigger Role 2.4 <https://galaxy.ansible.com/moira-alert/moira-trigger-role>`_.

.. important:: **Redis DB conversion is required.**

  Moira 2.4 has some structure changes in Redis DB. 
  It will work fluently out of the box, but lazy triggers will still be checked every time on new metrics.

  You can upgrade from moira 2.2 or 2.3 using corresponding flag in ``--from-version`` variable.

    .. code-block:: bash

      moira-cli --config=/etc/moira/cli.yml --update --from-version=2.2/2.3

  If you would like to downgrade back to Moira 2.2 or 2.3, you should run CLI-converter.

    .. code-block:: bash

      moira-cli --config=/etc/moira/cli.yml --downgrade --to-version=2.2/2.3

  Both cases imply usage of Moira-Cli v.2.4, you can find it on `Release Page <https://github.com/moira-alert/moira/releases>`_.

2.3.1
-----

- Fix ``last_remote_check_delay`` option in :ref:`Notifier configuration <notifier-configuration>` `moira-alert/moira#114 <https://github.com/moira-alert/moira/pull/114>`_.

2.3
---

- Add API methods: ``DELETE /notification/all`` and ``DELETE /event/all`` `moira-alert/moira#73 <https://github.com/moira-alert/moira/pull/73>`_.
- Add notifier config option: DateTime format for email sender `moira-alert/moira#74 <https://github.com/moira-alert/moira/pull/74>`_.
- Add Graphite-API support for remote triggers `moira-alert/moira#75 <https://github.com/moira-alert/moira/pull/75>`_. See more: :ref:`remote-triggers-checker`. Thanks to `@errx <https://github.com/errx>`_.
- Fix newlines in trigger description body for web and email sender `moira-alert/moira#76 <https://github.com/moira-alert/moira/pull/76>`_.
- Add option to enable runtime metrics in Graphite-section of configuration `moira-alert/moira#79 <https://github.com/moira-alert/moira/pull/79>`_.
- Add new fancy email template 🎂 `moira-alert/moira#82 <https://github.com/moira-alert/moira/pull/82>`_.
- Change default trigger state to TTLState option instead of NODATA `moira-alert/moira#83 <https://github.com/moira-alert/moira/pull/83>`_.
- Refactor maintenance logic `moira-alert/moira#87 <https://github.com/moira-alert/moira/pull/87>`_. See more: :doc:`/user_guide/maintenance`.
- Add basic false NODATA protection `moira-alert/moira#90 <https://github.com/moira-alert/moira/pull/90>`_. See more: :doc:`/user_guide/selfstate`.
- Prohibit removal of contact with assigned subscriptions found `moira-alert/moira#91 <https://github.com/moira-alert/moira/pull/91>`_.
- Make trigger exception messages more descriptive `moira-alert/moira#92 <https://github.com/moira-alert/moira/pull/92>`_.
- Make filter cache capacity configurable `moira-alert/moira#93 <https://github.com/moira-alert/moira/pull/93>`_. See more :ref:`Filter Configuration <filter-configuration>`.
- Fix incorrect behavior in which the trigger did not return from the ``EXCEPTION`` state `moira-alert/moira#94 <https://github.com/moira-alert/moira/pull/94>`_.
- Remove deprecated pseudo-tags, use checkboxes instead `moira-alert/moira#95 <https://github.com/moira-alert/moira/pull/95>`_. See more: :ref:`subscription-states-transitions`.
- Allow to use single-valued thresholds (ex. only ``WARN`` or only ``ERROR``) `moira-alert/moira#96 <https://github.com/moira-alert/moira/pull/96>`_.
- Reduce the useless CPU usage in Moira-Filter `moira-alert/moira#98 <https://github.com/moira-alert/moira/pull/98>`_. Thanks to `@errx <https://github.com/errx>`_.
- Add concurrent matching workers in Moira-Filter `moira-alert/moira#99 <https://github.com/moira-alert/moira/pull/99>`_. Thanks to `@errx <https://github.com/errx>`_.
- Update Carbonapi to 1.0.0-rc.0 `moira-alert/moira#101 <https://github.com/moira-alert/moira/pull/101>`_.
- Improve checker performance `moira-alert/moira#103 <https://github.com/moira-alert/moira/pull/103>`_.
- Add Markdown support in contact edit modal view `moira-alert/web2.0#138 <https://github.com/moira-alert/web2.0/pull/138>`_.
- Fix default timezone in trigger `moira-alert/web2.0#173 <https://github.com/moira-alert/web2.0/pull/173>`_.
- Add ability to type negative numbers in simple trigger edit mode  `moira-alert/web2.0#169 <https://github.com/moira-alert/web2.0/pull/169>`_.
- Fix trailing whitespaces in tag search bar `moira-alert/web2.0#139 <https://github.com/moira-alert/web2.0/pull/139>`_.
- Update `Moira Client 2.3.4 <https://github.com/moira-alert/python-moira-client/releases/tag/2.3.4>`_.
- Update `Moira Trigger Role 2.3 <https://galaxy.ansible.com/moira-alert/moira-trigger-role>`_.

.. important:: **Redis DB conversion is desirable.**

  Moira 2.3 has some structure changes in Redis DB. 
  It will work fluently out of the box, but we recommend you to run converter once Moira is updated.

  .. code-block:: bash

    moira-cli -update --config=/etc/moira/cli.yml

  .. code-block:: YAML
      :name: cli.yml
      :caption: /etc/moira/cli.yml

      redis:
        host: localhost
        port: "6379"
        dbid: 0
      log_file: stdout
      log_level: debug

  If you would like to downgrade back to Moira 2.2, you should run CLI-converter.

  .. code-block:: bash

    moira-cli -downgrade --config=/etc/moira/cli.yml

  Both cases imply usage of Moira-Cli v.2.3, you can find it on `Release Page <https://github.com/moira-alert/moira/releases>`_.

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
