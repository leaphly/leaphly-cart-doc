Roles Configuration
===================

1. Why Cart Roles?

   -  Because users could be a ( guest \|\| consumer \|\| administrator)
      and you need different exposed fields for each of these
   -  Diffent REST endpoints, Ex. your javascript client-side application
      haven't the same scope than a PHP application

The Roles Configuration
~~~~~~~~~~~~~~~~~~~~~~~

Each roles **REQUIRE**
1. a cart form factory that implements ``Leaphly\CartBundle\Form\Factory\FactoryInterface``
2. a (godfather) strategy instance

.. code-block:: yaml

    leaphly_cart:
        roles:
            api:
                form: leaphly_cart.cart.admin.form.factory  # Mandatory
                strategy: godfather.api                     # Optional, Default is godfather.{role_name}

And now you have services used for the PHP application and REST controllers
for client side (javascript) application. The service and controller's
instances follows a naming convention:

Controllers
===========

-  Cart: ``leaphly_cart.cart.controller.{$roleName}``
-  Item: ``leaphly_cart.cart_item.controller.{$roleName}``

Services
========

-  Cart: ``leaphly_cart.cart.{$roleName}.service``
-  Item: ``leaphly_cart.cart_item.{$roleName}.service``


Full Configuration
==================

.. code-block:: yaml

    leaphly_cart:
        db_driver: orm # other valid values are 'orm', 'couchdb' and 'propel'
        cart_class: Acme\CartBundle\Entity\Cart
        item_class: Acme\CartBundle\Entity\Item
        service:
            product_family_provider: acme_cart.product_family_provider
        roles:
            full:
                form: leaphly_cart.cart.admin.form.factory
                strategy: godfather.full
            limited:
                form: leaphly_cart.cart.admin.form.factory
                strategy: godfather.limited
