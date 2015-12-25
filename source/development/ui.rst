UI application
==============

.. _TypeScript: http://www.typescriptlang.org
.. _AngularJS: https://angularjs.org
.. _webpack: https://webpack.github.io

UI is a static web application built with TypeScript_, AngularJS_ and webpack_.

Install dependencies.

.. code-block:: bash

   npm install

Run webpack dev server at http://localhost:8080.

.. code-block:: bash

   npm start

.. note:: UI doesn't work without API microservice

Run tests.

.. code-block:: bash

   karma start

By default, test output is in Teamcity-compatible format. Edit ``karma.conf.js`` to remove Teamcity
log output and code coverage reports.

.. code-block:: js

   //webpackConfig.module['postLoaders'] = [{
   //  test: /\.ts$/,
   //  exclude: /tests/,
   //  loader: 'istanbul-instrumenter'
   //}];
   ...
   //reporters: ['progress', 'coverage', 'teamcity'],
   reporters: ['progress'],
