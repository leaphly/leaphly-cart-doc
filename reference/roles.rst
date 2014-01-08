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

.. code-block:: yaml

    leaphly_cart:
        roles:
            api:
                form: leaphly_cart.cart.admin.form.factory  # Mandatory

And now you have services used for the PHP application and REST controllers
for client side (javascript) application. The service and controller's
instances follows a naming convention:

Controllers
===========

-  Cart: ``leaphly_cart.cart.controller.{$roleName}``
-  Item: ``leaphly_cart.cart_item.controller.{$roleName}``

Handlers
========

-  Cart: ``leaphly_cart.cart.{$roleName}.service``
-  Item: ``leaphly_cart.cart_item.{$roleName}.service``

