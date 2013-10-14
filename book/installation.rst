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
4. Create your ``Product`` Classes

.. 5. Configure the LeaphlyCartBundle

5. Update your database schema

Step 1: Download LeaphlyCartBundle using composer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add LeaphlyCartBundle in your composer.json:

.. code-block:: js

    {
        "require": {
            "leaphly/cart-bundle": "~1.0@dev"
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
ORM, MongoDB ODM, or CouchDB ODM).

**Note:**

    The doc uses a bundle named ``AcmeCartBundle``. If you want to use
    the same name, you need to register it in your kernel. But you can
    of course place your cart class in the bundle you want.

**Warning:**

    If you override the **construct()** method in your Cart class, be sure
    to call **parent::\ construct()**, as the base Cart class depends on
    this to initialize some fields.

a) Doctrine ORM Cart class
^^^^^^^^^^^^^^^^^^^^^^^^^^

If you're persisting your carts via the Doctrine ORM, then your ``Cart``
class should live in the ``Entity`` namespace of your bundle and look
like this to start:

Annotations
'''''''''''

.. code-block:: php

    <?php
    // src/Acme/CartBundle/Entity/Cart.php

    namespace Acme\CartBundle\Entity;

    use Leaphly\CartBundle\Model\Cart as BaseCart;
    use Doctrine\ORM\Mapping as ORM;

    /**
     * @ORM\Entity
     * @ORM\Table(name="leaphly_cart")
     */
    class Cart extends BaseCart
    {
        /**
         * @ORM\Column(type="string")
         */
        protected $promocode;

        public function __construct()
        {
            parent::__construct();
            // your own logic
        }
    }

**Note:**

    ``Cart`` is a reserved keyword in SQL so you cannot use it as table
    name.

yaml
''''

If you use yml to configure Doctrine you must add two files. The Entity
and the orm.yml:

.. code-block:: php

    <?php
    // src/Acme/CartBundle/Entity/Cart.php

    namespace Acme\CartBundle\Entity;

    use Leaphly\CartBundle\Model\Cart as BaseCart;

    /**
     * Cart
     */
    class Cart extends BaseCart
    {
        /**
         * @ORM\Column(type="string", length="50")
         */
        protected $promocode;

        public function __construct()
        {
            parent::__construct();
            // your own logic
        }
    }

.. code-block:: yaml

    # src/Acme/CartBundle/Resources/config/doctrine/Cart.orm.yml
    Acme\CartBundle\Entity\Cart:
        type:  entity
        table: leaphly_cart
        fields:
            promocode:
                type: string
                length: 50

b) MongoDB Cart and Item classes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you're persisting your carts via the Doctrine MongoDB ODM, then your
``Cart`` class should live in the ``Document`` namespace of your bundle
and look like this to start:

.. code-block:: php

    <?php
    // src/Acme/CartBundle/Document/Cart.php

    namespace Acme\CartBundle\Document;

    use Leaphly\CartBundle\Model\Cart as BaseCart;
    use Doctrine\ODM\MongoDB\Mapping\Annotations as MongoDB;

    /**
     * @MongoDB\Document
     */
    class Cart extends BaseCart
    {
        /**
         * @MongoDB\Column(type="string")
         */
        protected $promocode;

        public function __construct()
        {
            parent::__construct();
            // your own logic
        }
    }

Step 4: Configure the LeaphlyCartBundle
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The next step is to configure the bundle to work with the specific needs of your
application.

Add the following configuration to your ``config.yml`` file according to
which type of datastore you are using.

.. code-block:: yaml

    # app/config/config.yml
    leaphly_cart:
        db_driver: orm # other valid values are 'mongodb'
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

Or if you prefer XML:

.. code-block:: xml

    <!-- app/config/config.xml -->

    <!-- other valid 'db-driver' values are 'mongodb' and 'couchdb' -->
    <leaphly_cart:config db-driver="orm" cart-class="Acme\CartBundle\Entity\Cart" item-class="Acme\CartBundle\Entity\Item">
        <service product-family-provider="acme_cart.product_family_provider" />
        <roles>
            <role name="full" form="leaphly_cart.cart.admin.form.factory" strategy="godfather.full" />
            <role name="limited" form="leaphly_cart.cart.admin.form.factory" strategy="godfather.limited" />
        </ roles>
    </ leaphly_cart:config>

As you can see, you will need the following information:

-  The type of datastore you are using (``orm``, ``mongodb``).
-  The fully qualified class name (FQCN) of the ``Cart`` class which you
   created in Step 3b.
-  The product family provider service name (for more information on the
   family provider can go in the section devoted to it, for now we can only
   say that the service need to understand how to manipulate different types of products)
-  The security roles: When you sign in to the cart by leaphly's APIs you will have the
   opportunity to do so with different levels of authorization, so you can be sure that
   functional changes such as, for example, the extension of the expiration time, or set
   the cart as "paid". To do this you just need to provide the form class and the
   corresponding strategy; you will deepen this issue here.

**Note:**

    LeaphlyCartBundle uses a compiler pass to register mappings for the
    base Cart and Item model classes with the object manager that you
    configured it to use. (Unless specified explicitly, this is the
    default manager of your doctrine configuration.)

..
 Step 5: Import LeaphlyCartBundle routing files
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Now that you have activated and configured the bundle, all that is left
.. to do is import the LeaphlyCartBundle routing files.

.. By importing the routing files you will have ready made pages for things
.. such as logging in, creating carts, etc.

.. In YAML:
..
    .. code-block:: yaml
..
    # app/config/routing.yml
    leaphly_cart_routing:
        resource: "@LeaphlyCartBundle/Resources/config/routing.xml"
..
.. Or if you prefer XML:
..
 .. code-block:: xml
..
    <!-- app/config/routing.xml -->
    <import resource="@LeaphlyCartBundle/Resources/config/routing.xml"/>

Step 5: Update your database schema
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

