Frontend
========

Карточка товара
---------------

Динамические данные на основе ТП
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ТП - торговые предложения.

Выбор торговых предложений осуществляет Vuejs приложение описанное в ``card-choose-offer.js``

::

   <div class="product-card--content" id="js-card-price-app" data-default-offer='<?=$model->getBasketObjectJson()?>' data-target-button=".js-cart-basket-button">

Работа с ценой и кнопкой в корзину

Работа с параметрами

Работа с изображениями


.. danger:: Приложения зарегистрированные в шаблоне должные иметь свойство ``inline-template``!