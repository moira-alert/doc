RPM and DEB Packages
====================

.. _Web: https://github.com/moira-alert/web/releases
.. _Notifier: https://github.com/moira-alert/notifier/releases
.. _Worker: https://github.com/moira-alert/worker/releases
.. _Cache: https://github.com/moira-alert/cache/releases

All stable versions of Moira components are tagged on GitHub. For every tag, we automatically build RPM and DEB
packages. You can download these packages on each repository `releases` page:

1. Web_
2. Notifier_
3. Worker_ (RPM and DEB packages are not ready yet, but you can install with `pip`)

  .. code-block:: bash

    wget https://github.com/moira-alert/worker/releases/download/v1.1.14/moira_worker-1.1.14.tar.gz
    sudo pip install moira_worker-1.1.14.tar.gz

4. Cache_

We don't maintain strict version dependencies between packages (sorry), but everything will be OK if you install/upgrade
all of the packages at once.
