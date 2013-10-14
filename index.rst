Leaphly Documentation
=========================

This documentation is currently in development and far from complete. See `Documentation planning`_
for an overview of the work left to do. Want to help? Thank you, all help greatly appreciated!
The source of the `documentation is hosted on github`_.

Mission Statement
-----------------

    The Leaphly project makes it easier for developers to add cart functionality to php application made with Symfony2
    or to those application that could consume REST API.
    This software provides the tools and guidelines for building decoupled,
    high-quality and long-life e-commerce applications.


Why another Ecommerce?
----------------------

Actually we consider this project to be only a little part of an e-commerce architecture, the **cart**.

The e-commerce must dress in a perfect way the company, things get complicated easily
when the same company may sell several products with totally different properties and logic.

We are providing tool to build a **custom Cart**. There are clearly many e-commerce solutions available already,
but they tend to provides monolithic packages tailored towards end users.
Many carry a certain amount of legacy baggage which make them less than **ideal for developing highly
custom applications**, in contrast to what is possible with `Symfony2`_.

The same principle for the object oriented programming, could also be extended to services,
using Service Oriented Architecure, **build application out of smaller ones**.
That's how an high quality e-commerce should be implemented, **User**, **Product**, **Cart**, **Checkout process**, **Shipment** are all part of the same application,
but each part should provide and work with single responsability principle.

Users could be handled by a good CRM, the products by a good CART, and the checkout process by another application maybe front-end,
the report and the orders by the Business Intelligence, the Carts by this project as bundle or as Cart Service via Restful interface.

One Cart 2 different usages
---------------------------

You may install the cart as a simple bundle for use by service container, or use it via through RESTful API.


What is our target audience?
----------------------------

There are basically four main target audiences:

#. Developers who have built an existing custom application with Symfony2 and need a fast
   way to add support for cart. 

#. Developers that need to build a highly customized cart
   solution that no out-of-the-box cart ecommerce properly provide through customization alone.

#. Developers che durante la creazione del loro ecommerce pensano che una struttura SOA, sia perfetta per loro

#. Developers che hanno già un ecommerce e vorrebbero sostituire solo la gestione del carrello e del prodotto.

Be it sophisticated Cart features like multi-product, rest provider, api for cart finite state, ???
 etc. or just a single product like a mug shop.


Big Picture
------------


.. toctree::
    :maxdepth: 1

    bigpicture/flow
    bigpicture/strategy
    bigpicture/transition
    bigpicture/roles
    bigpicture/familyproduct
    bigpicture/price

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

How to configure your e-commerce

.. toctree::
    :maxdepth: 1

    reference/index

Cookbook
--------

Special solutions for specific use cases that go beyond standard usage.

Want to know more about the CART and how each part can be configured? There's a tutorial for each one.

Contributing
------------

We do need help!

- suggestion and feedback
- contributors

Dreaming
--------

- Leaphly Integrated with php-cr
- Leaphly Integrated with ORO-crm
- Leaphly works on different frameworks (eg. laravel)
- All the php e-commerces platform would share the same interfaces
- Melting of Leaphly with Vespolina and Sylius.


.. _`decoupled CMS`: http://decoupledcms.org
.. _`Symfony2`: http://symfony.com
.. _`about`: http://leaphly.org.com/about
.. _`Documentation planning`: https://github.com/symfony-cart/symfony-cart/wiki/Documentation-Planning
.. _`Symfony Content Management Framework`: http://leaphly.org.com
.. _`documentation is hosted on github`: https://github.com/symfony-cart/symfony-cart-docs
