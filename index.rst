Leaphly Documentation
=========================

This documentation is currently in development and far from complete.
Want to help? Thank you, all help greatly appreciated!
The source of the `documentation is hosted on github`_.

Mission Statement
-----------------

    The Leaphly project makes it easier for developers to add shopping cart functionality to the php and also to
    Symfony2 applications.
    This software provides the tools and guidelines for building decoupled, high-quality
    and long-life e-commerce applications.

Why another E-commerce tool?
----------------------------

Actually we consider this project to be only a little part of an e-commerce architecture,
we are providing tool to build a **custom shopping-cart**.

There are clearly many e-commerce solutions available already,
but they tend to provide monolithic packages, some are shipped by many coupled bundles,
others handle multi-products diversity with EAV. Many carry a certain amount of legacy
baggage which make them less than ideal for developing highly custom applications,
in contrast to what is possible with `Symfony2`_.

One Cart, different usages
---------------------------

You may install the cart as a simple bundle and then using the service container,
or install into a new flat-php project and then using it via through REST API.

The container API are consistent with the REST API:

In a bundle you could use via dependency injection the api:

    $cart = $this->container->get('leaphly.cart')->getCart(1);

    $cart = $this->container->get('leaphly.cart')->postCart($parameters);

    $this->container->get('leaphly.cart')->putCart($cart);

    $this->container->get('leaphly.cart')->patchCart($cart);

    $this->container->get('leaphly.cart')->deleteCart($cart);

with the REST api:

    [GET]    /carts/{id}

    [POST]   /carts

    [PUT]    /carts/{id}

    [PATCH]  /carts/{id}

    [DELETE] /carts/{id}

What is our target audience?
----------------------------

There are basically few main target audiences:

#. Developers who have built an existing custom application with Symfony2 and need a fast
   way to add support for cart. 

#. Developers that during the creation of their e-commerce think that a structure SOA, is perfect for them.

#. Developers who already have an ecommerce and would like to replace only the management of the shopping cart.

Be it sophisticated Cart features like multi-product, rest provider, api for cart finite state,
or just a single product like a mug shop.

What is not our target audience?
--------------------------------

For all that want a simple full feature all-in ecommerce that could cover all the domain problems,
we really believe in Object Oriented Desing and we are against to the model into the EAV.

Getting started
---------------

Install the Sandbox fully working example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is the Leaphly bible. It's the reference for any user of the Cart, who
will typically want to keep this close at hand.

.. toctree::
    :maxdepth: 1

    book/sandbox

Install as component
^^^^^^^^^^^^^^^^^^^^

This is the Leaphly bible. It's the reference for any user of the Cart, who
will typically want to keep this close at hand.

.. toctree::
    :maxdepth: 1

    book/installation-symfony-bundle
    book/installation-library

Configuration
-------------

How to configure your leaphly application

.. toctree::
    :maxdepth: 1

    reference/roles
    reference/multiproduct
    reference/routing


Cookbook
--------
.. toctree::
    :maxdepth: 1

    cookbook/index

Special solutions for specific use cases that go beyond standard usage.

Want to know more about the Cart and how each part can be configured? There's a tutorial for each one.

Contributing
------------

We do need help!

- suggestion and feedback
- contributors

Dreaming
--------

- Leaphly integration with PHP-CR
- Leaphly adapter for with ORO-CRM
- Leaphly adapter for Leaphly with Vespolina and Sylius.

.. _`Symfony2`: http://symfony.com
.. _`about`: http://leaphly.org.com
.. _`Leaphly Cart`: http://leaphly.org.com
.. _`documentation is hosted on github`: https://github.com/leaphly/leaphly-cart-doc
