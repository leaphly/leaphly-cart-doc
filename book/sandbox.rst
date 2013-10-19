.. index::
    single: sandbox

Getting started with the Sandbox and MongoODM
=============================================

The easy way to start with leaphly is probably the sandbox, with MongoODM

.. code-block:: bash

    cd /var/www
    php composer.phar -sdev create-project leaphly/leaphly-sandbox leaphly-sandbox/
    cd leaphly-sandbox
    app/console doctrine:mongodb:fixtures:load --env=mongo
    app/console fos:js:dump --env=mongo
    app/console assets:install web --symlink --env=mongo
    app/console assetic:dump --env=mongo

Then create  the virtual host

sudo -- sh -c "echo 127.0.0.1 cart.local  >> /etc/hosts"

.. code-block:: apache

    <VirtualHost *:80>
         DocumentRoot /var/www/leaphly-sandbox/web/
         ServerName cart.local
         <Directory  /var/www/leaphly-sandbox/web/>
               Options FollowSymLinks
               AllowOverride All
               Allow from all
         <IfModule mod_rewrite.c>
           RewriteEngine On
           RewriteCond %{REQUEST_FILENAME} -f
           RewriteRule .? - [L]
           RewriteRule .? %{ENV:BASE}/app_mongo.php [L]
        </IfModule>
        </Directory>
    </VirtualHost>


Summary
-------

Congratulations! The leaphly project is waiting you at `http://cart.local`.