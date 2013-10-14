.. index::
    single: sandbox

Using Sandbox
=============

cd /var/www

git create-project -sdev ....

cd cartSandbox

app/console doctrine:mongodb:fixtures:load --env=mongo

app/console fos:js:dump --env=mongo

app/console assets:install web --symlink --env=mongo

app/console assetic:dump --env=mongo

create virtual host

sudo -- sh -c "echo 127.0.0.1 cart.local  >> /etc/hosts"

<VirtualHost *:80>
     DocumentRoot /var/www/cartSandbox/web/
     ServerName cart.local
     <Directory  /var/www/cartSandbox/web/>
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

Congratulations! You are now able to create a simple web site using the
Leaphly cart. From here, each chapter will tell you a bit more about the CART
and more about the things behind the SimpleCMSBundle. In the end, you'll be
able to create more advanced blog systems and other CMS websites.
