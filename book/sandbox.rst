.. index::
    single: sandbox

Getting started with the Sandbox and Mongo ODM
==============================================

The easy way to start with Leaphly is probably the sandbox, with MongoODM

.. code-block:: bash

    php composer.phar -sdev create-project leaphly/leaphly-sandbox leaphly-sandbox/
    cd leaphly-sandbox
    app/console doctrine:mongodb:fixtures:load
    app/console fos:js:dump
    app/console assets:install web --symlink
    app/console assetic:dump
    app/console server:run &

Getting started with the Sandbox and ORM
========================================

If you prefer doctrine ORM and mysql, the sandbox provides also Entity:

.. code-block:: bash

    php composer.phar -sdev create-project leaphly/leaphly-sandbox leaphly-sandbox/
    cd leaphly-sandbox
    app/console doctrine:fixtures:load --env=orm
    app/console fos:js:dump --env=orm
    app/console assets:install web --symlink --env=orm
    app/console assetic:dump --env=orm
    app/console server:run --env=orm &

YEAHHH!
-------

Congratulations! The leaphly project is waiting you at `http://cart.local:8000`.