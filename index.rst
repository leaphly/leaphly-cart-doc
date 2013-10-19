Leaphly Documentation
=========================

This documentation is currently in development and far from complete.
Want to help? Thank you, all help greatly appreciated!
The source of the `documentation is hosted on github`_.

Mission Statement
-----------------

    The Leaphly project makes it easier for developers to add cart functionality to the Symfony2 applications
    or to those applications that could consume REST API.
    This software provides the tools and guidelines for building decoupled, high-quality
    and long-life e-commerce applications.

Why another Ecommerce?
----------------------

Actually we consider this project to be only a little part of an e-commerce architecture,
we are providing tool to build a **custom shopping-cart**.

There are clearly many e-commerce solutions available already,
but they tend to provide monolithic packages, some are shipped by many coupled bundles,
others handle multi-products diversity with EAV. Many carry a certain amount of legacy
baggage which make them less than ideal for developing highly custom applications,
in contrast to what is possible with `Symfony2`_.

One Cart 2 different usages
---------------------------

You may install the cart as a simple bundle using the service container, or install into a symfony project and then
using it via through RESTful API.

What is our target audience?
----------------------------

There are basically few main target audiences:

#. Developers who have built an existing custom application with Symfony2 and need a fast
   way to add support for cart. 

#. Developers that during the creation of their ecommerce think that a structure SOA, is perfect for them.

#. Developers who already have an ecommerce and would like to replace only the management of the shopping cart.

Be it sophisticated Cart features like multi-product, rest provider, api for cart finite state,
or just a single product like a mug shop.

Book
----

This is the Leaphly bible. It's the reference for any user of the Cart, who
will typically want to keep this close at hand.

.. toctree::
    :maxdepth: 1

    book/index
    book/installation
    book/sandbox


Configuration
-------------

How to configure your leaphly

.. toctree::
    :maxdepth: 1

    reference/index

Cookbook
--------
.. toctree::
    :maxdepth: 1

    cookbook/index

Special solutions for specific use cases that go beyond standard usage.

Want to know more about the CART and how each part can be configured? There's a tutorial for each one.

Contributing
------------

We do need help!

- suggestion and feedback
- contributors

Dreaming
--------

- Leaphly integration with php-cr
- Leaphly adapter for with ORO-crm
- Leaphly as library for different frameworks (eg. laravel...)
- All the php e-commerces platform would share the same interfaces
- Leaphly adapter for Leaphly with Vespolina and Sylius.

.. _`Symfony2`: http://symfony.com
.. _`about`: http://leaphly.org.com
.. _`Leaphly Cart`: http://leaphly.org.com
.. _`documentation is hosted on github`: https://github.com/leaphly/leaphly-cart-doc
