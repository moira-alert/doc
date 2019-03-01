UI
==

.. _RetailUI: https://github.com/skbkontur/retail-ui
.. _React: https://reactjs.org
.. _Storybook: https://storybook.js.org
.. _ESLint: https://eslint.org/
.. _Flow: https://flow.org/

UI is a static web application built with RetailUI_ based on React_.

Install dependencies.

.. code-block:: bash

   yarn build

Starts dev server on port 9000. You'll have to run ``yarn fakeapi`` in separate terminal to provide mock API data. Mock API server starts on port 9002.

.. code-block:: bash

   yarn start --env.API=local

Starts dev server with proxy to your API service. Make sure you setup local Moira API service and add it URL to webpack.config.js in devServer.proxy block.

.. code-block:: bash

   yarn storybook

Starts Storybook_ on port 9001.

.. code-block:: bash

   yarn lint

ESLint_ check. *Recommended to run before commit*.

.. code-block:: bash

   yarn flow

Starts Flow_ server for checking types. You can also run ``yarn flow.status`` for status, yarn ``flow.check`` for errors report, ``yarn flow.coverage.html`` to export html report with cute UI.