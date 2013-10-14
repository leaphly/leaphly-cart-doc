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
        public function createItem(CartInterface $cart, array $parameters)
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

Now you need only the `godgather's <https://github.com/PUGX/godfather>`__ tag that register the handler for strategy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: xml

            <service id="conference.item.handler" class="%conference.item_handler.class%">
                <argument>%conference.item.class%</argument>
                <argument type="service" id="form.factory"/>
                <tag name="godfather.strategy" instance='api' context_name="item_handler" context_key="ConferenceProduct" />
            </service>

