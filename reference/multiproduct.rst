Domain-Specific logic (Multi-Product)
=====================================

The Item Object
~~~~~~~~~~~~~~~

When a customer chooses to buy a product needs to add additional
information such as the size of the shirt, the color, or the date of the
conference's ticket, so every product has its own properties and
domain-specific logic for taxes, discounts etc...

-  **ConferenceItem**

.. code:: php

    Class ConferenceItem extends BaseItem {
        protected $date;
        protected $seats;
        protected $discount;
        ...

-  **TshirtItem**

.. code:: php

    Class TshirtItem extends BaseItem {
         protected $color;
         protected $size;
         protected $deliveryMethod;
         ...

In this example you need to define an ItemHandler for each Item family
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: php

    class ItemHandler implements ItemHandlerInterface
    {
        public function postItem(CartInterface $cart, array $parameters)
        {
            if($form->isValid()){
                /**
                 * Put here your Domain logic
                 */
                return $form->getData();
            }

            throw new InvalidFormException('Invalid submitted form', $form->getErrors());
        }
    }


You could see a working example at:

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/Product/ConferenceBundle/Handler/ItemHandlerFullAccess.php


Now you need only the `godfather's <https://github.com/PUGX/godfather>`__ tag that registers the handler for the strategy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create the service and add the godfather tag in your `/Resource/config/mongodb.xml`

.. code:: xml

            <service id="conference.item.handler" class="%conference.item_handler.class%">
                <argument>%conference.item.class%</argument>
                <argument type="service" id="form.factory"/>
                <tag name="godfather.strategy" instance='full' context_name="item_handler" context_key="ConferenceProduct" />
            </service>

You could see a working example at:

https://github.com/leaphly/leaphly-sandbox/blob/master/src/Acme/Product/ConferenceBundle/Resources/config/mongodb.xml

