.. .. index::
    single: Installation; Getting Started

Installing the Leaphly Cart in your symfony application
========================================================

Prerequisites
-------------

This version of the bundle requires Symfony 2.1+.

Install LeaphlyCartBundle
--------------------------

From zero to cart in five moves!

1. Download LeaphlyCartBundle using composer
2. Enable the Bundle
3. Create your ``Cart`` and ``Item`` classes
4. Configure the LeaphlyCartBundle
5. Update your database schema

Step 1: Download Leaphly Cart Bundle using composer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add LeaphlyCartBundle in your composer.json:

.. code-block:: js

    {
        "require": {
            "leaphly/cart-bundle": "@dev",
            "pugx/godfather": "@dev",
            "yohang/finite": "@dev",
            "jms/serializer-bundle": "@dev"
        }
    }

Now tell composer to download the bundle by running the command:

.. code-block:: bash

    $ php composer.phar update leaphly/cart-bundle

.. More simply, run
..
  .. code-block:: bash
..
    $ php composer.phar require Leaphly/cart-bundle

Step 2: Enable the bundle
~~~~~~~~~~~~~~~~~~~~~~~~~

Enable the bundle in the kernel:

.. code-block:: php

    <?php
    // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            // ...
            new Leaphly\CartBundle\LeaphlyCartBundle(),
            // ...
        );
    }

Step 3: Create your Cart and Item classes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The goal of this bundle is to persist some `Cart` class to a database (MySql,
MongoDB). Your first job, then, is to create the `Cart` class
for your application. This class can look and act however you want: add any
properties or methods you find useful. This is *your* `Cart` class.

The bundle provides base classes which are already mapped for most fields
to make it easier to create your entity. Extend the base `Cart` class from
the ``Model`` folder is therefore enough to have a functioning cart,
it will inherit the doctrine's default metadata, so if you need to add fields
you need to say them how to be persisted too.

**Warning:**

    When you extend from the mapped superclass provided by the bundle,
    don't redefine the mapping for the other fields as it is provided by
    the bundle.

Your ``Cart`` class can live inside any bundle in your application. For
example, if you work at "Acme" company, then you might create a bundle
called ``AcmeCartBundle`` and place your ``Cart`` class in it.

In the following sections, you'll see examples of how your ``Cart``
class should look, depending on how you're storing your carts (Doctrine
ORM, MongoDB ODM).

**Note:**

    The doc uses a bundle named ``AcmeCartBundle``. If you want to use
    the same name, you need to register it in your kernel. But you can
    of course place your cart class in the bundle you want.

**Warning:**

    If you override the **construct()** method in your Cart class, be sure
    to call **parent::\ construct()**, as the base Cart class depends on
    this to initialize some fields.

Doctrine ORM
____________

Doctrine ORM Cart
^^^^^^^^^^^^^^^^^

If you're persisting your carts via the Doctrine ORM, then your ``Cart``
class should live in the ``Entity`` namespace of your bundle and look
like this to start:

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/CartBundle/Entity/Cart.php

Doctrine ORM ITEMS
^^^^^^^^^^^^^^^^^^

In Doctrine ORM you need an Item Entity. But in a real world you need the same number of items type
depending on the number of the product you want to sell.

If you have multiple products you need to create multiple items entity so you may need the doctrine inheritance.

You could find an example of class here:

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/CartBundle/Entity/Item.php

and then the different items:

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/Product/ConferenceBundle/Entity/TicketItem.php

and

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/Product/TshirtBundle/Entity/TshirtItem.php

Be careful to compile the doctrine inheritance proprely, the type JOINED or SINGLE depends of your domain.
For Item Class follow this flow:

-  Extends the abstract ```Leaphly\CartBundle\Model\Item``` class

-  Define your domain-specific items with ORM\Inheritance directive

.. code-block:: php

     /**
     *
     * Acme\CartBundle\Entity
     *
     * @ORM\Table(name="cart_item")
     * @ORM\Entity()
     * @ORM\InheritanceType("JOINED")
     * @ORM\DiscriminatorColumn(name="discr", type="string")
     * @ORM\DiscriminatorMap({
     *      "ticket"  = "Acme\Product\ConferenceBundle\Entity\TicketItem",
     *      "tShirt"  = "Acme\Product\TshirtBundle\Entity\TshirtItem"
     * })
     *
     * @ORM\HasLifecycleCallbacks()
     */
    abstract class BaseItem extends BaseItem
    {
        ...
    }

Every specific item class will extends your abstract BaseItem and this is the place
where put all your domain stuff.

Doctrine MongoDB ODM
____________________

MongoDB Cart
^^^^^^^^^^^^

If you're persisting your carts via the Doctrine MongoDB ODM, then your
``Cart`` class should live in the ``Document`` namespace of your bundle.

You could find an example of class here:

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/CartBundle/Model/Cart.php

If you have multiple products you need to create multiple items entity so you may need the doctrine inheritance.

With Mongo ODM you don't need to create the central 'Item' class, below an example on how to create the different items:

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/Product/ConferenceBundle/Document/TicketItem.php

and

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/Product/TshirtBundle/Document/TshirtItem.php


Step 4: Configure the LeaphlyCartBundle
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The next step is to configure the bundle to work with the specific needs of your
application.

Add the following configuration to your ``config.yml`` file according to
which type of datastore you are using.

.. code-block:: yaml

    # app/config/config.yml
    leaphly_cart:
        db_driver: orm # or odm, required
        cart_class: Acme\CartBundle\Entity\Cart #required
        roles:
            full:
                form: leaphly_cart.cart.admin.form # required

As you can see, you will need the following information:

-  The type of driver you are using (``orm``, ``mongodb``).
-  The fully qualified class name (FQCN) of the ``Cart`` class you created in Step 3.
-  The access roles:
   each role need a form (as a service) that maps only the authorized field.
   Example: the full role will map all Cart fields but the limited role map all field
   except the price and state properties.
   Via Service container you could use the handler via  `leaphly_cart.cart.full.handler`.

**Note:**

    LeaphlyCartBundle uses a compiler pass to register mappings for the
    base Cart and Item model classes with the object manager that you
    configured it to use. (Unless specified explicitly, this is the
    default manager of your doctrine configuration.)

**Note:**
    LeaphlyCartBundle uses a compiler pass to register controllers and handlers, so
    if you want to know which services has been creating in the black box just run
    `app/console container:debug | grep leaphly`


Step 5: (Only for REST functionality) Import LeaphlyCartBundle routing files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that you have activated and configured the bundle, all that is left
to do is import the LeaphlyCartBundle routing files.

You could expose different roles with different REST endpoints so for each
role you want expose, you should define a routing entry and point it to the relative controller.
The LeaphlyCartBundle will create a dedicated-role controllers (as a service) with a
naming convention.

If you define a role called full, the controllers will be defined:

- `leaphly_cart.cart.full.controller`
- `leaphly_cart.cart_item.full.controller`

add to the project the route `app/config/routing.yml`

.. code-block:: yaml

    leaphly_cart:
        type: rest
        resource: "@AcmeCartBundle/Resources/config/rest.xml"
        prefix:   /api/v1/

then create the `rest.xml` in your cart bundle "@AcmeCartBundle/Resources/config/rest.xml".

.. code-block:: xml

    <import id="carts_full" type="rest" resource="leaphly_cart.cart.full.controller" name-prefix="api_1_full_" prefix="/full" />
    <import id="cartItems_full" type="rest" resource="leaphly_cart.cart_item.full.controller" name-prefix="api_1_full_" parent="carts_full" prefix="/full" />

If you want to enable transition, and the finite state machine to the cart, you should add also this route:

.. code-block:: xml

    <import id="cartTransitions" type="rest" resource="Leaphly\CartBundle\Controller\CartTransitionsController" name-prefix="api_1_" parent="carts" />

Step 6: Update your database schema
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that the bundle is configured, the last thing you need to do is
update your database schema because you have added new entities.

For ORM run the following command.

.. code-block:: bash

    $ php app/console doctrine:schema:update --force

For MongoDB carts you can run the following command to create the
indexes.

.. code-block:: bash

    $ php app/console doctrine:mongodb:schema:create --index

Next Steps
~~~~~~~~~~

Now that you have completed the basic installation and configuration of
the LeaphlyCartBundle, you are ready to learn about more advanced
features and usages of the bundle.

