Routing
=======

(Only for REST functionality) Define the routing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You could define the routing for all you need.
The basic rules are:

-  Each role has a dedicated controller (as a Service), either for Cart and Cart Items
-  Prevent routing conflicts using the prefix key


The controllers naming convention are : leaphly_cart.cart.{{**role_name**}}.controller

ex: with 3 roles [limited, full, javascript] your controllers are

-  leaphly_cart.cart.limited.controller
-  leaphly_cart.cart.full.controller
-  leaphly_cart.cart.javascript.controller
-  leaphly_cart.cart_item.limited.controller
-  leaphly_cart.cart_item.full.controller
-  leaphly_cart.cart_item.javascript.controller


Import the routing files you will have ready made pages for things
such as logging in, creating carts via REST Api.

In YAML:

.. code-block:: yaml

    # app/config/routing.yml
    leaphly_cart_routing:
        type: rest
        resource: "@AcmeCartBundle/Resources/config/routing.xml"

Or if you prefer XML:

.. code-block:: xml

    <!-- app/config/routing.xml -->
    <import resource="@AcmeCartBundle/Resources/config/routing.xml"/>

Now define the role specific endpoints

In YAML:

.. code-block:: yaml

    # AcmeCartBundle/Resources/config/routing.yml
    leaphly_cart_limited_routing:
        type: rest
        resource: leaphly_cart.cart.limited.controller

    leaphly_cart_item_limited_routing:
        type: rest
        resource: leaphly_cart.cart_item.limited.controller

    leaphly_cart_full_routing:
        type: rest
        resource: leaphly_cart.cart.full.controller

    leaphly_cart_full_routing:
        type: rest
        resource: leaphly_cart.cart_item.full.controller

Or if you prefer XML:

.. code-block:: xml

    <!-- app/config/routing.xml -->
    <import id="leaphly_cart_limited_routing" type="rest" resource="leaphly_cart.cart.limited.controller" name-prefix="api_1_" />
    <import id="leaphly_cart_full_routing" type="rest" resource="leaphly_cart.cart.full.controller" name-prefix="api_1_" />

    <import id="leaphly_cart_item_limited_routing" type="rest" resource="leaphly_cart.cart_item.limited.controller" name-prefix="api_1_" />
    <import id="leaphly_cart_item_full_routing" type="rest" resource="leaphly_cart.cart_item.full.controller" name-prefix="api_1_" />

Cart Transition routing
=======================

If you want expose the REST Api for cart state's transitions add the following routing entry to the routing configuration:

In YAML:

.. code-block:: yaml

    leaphly_cart_transition:
        type: rest
        parent: leaphly_cart_full_routing
        resource: Leaphly\CartBundle\Controller\CartTransitionsController
        name-prefix: "api_1_"

Or if you prefer XML:

.. code-block:: xml

    <import id="cartTransitions" type="rest" resource="Leaphly\CartBundle\Controller\CartTransitionsController" name-prefix="api_1_" parent="leaphly_cart_full_routing" />


