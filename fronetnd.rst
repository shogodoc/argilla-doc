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


Выбор ТП

::

    <offers-list-radio :offers-list='<?=json_encode($offers)?>' inline-template>
      <div>
        <label class="item js-radio"
               v-bind:class="getClass(offer)"
               v-bind:data-size="offer.text"
               v-for="offer in offersList"
               @click="click(offer)">{{offer.text}}</label>
      </div>
    </offers-list-radio>

::
    <offers-list-select :offers-list='<?=json_encode($offers)?>' inline-template>
      <div>
        <select class="no-default-selectric" v-model="selectedId">
          <option v-for="offer in offersList" v-bind:value="offer.id" v-html="offer.text"></option>
        </select>

        id value: {{selectedId}}
      </div>
    </offers-list-select>


Работа с ценой и кнопкой в корзину

::

    <price-component :price="this.offer.price" :old-price="this.offer.oldPrice" inline-template>
      <div class="product-card__price-group">
        <div class="product-card__old-price" v-if="oldPrice">
          <span v-html="oldPriceFormatted"></span>
          <div class="product-card__discount" v-html="economyPercentFormatted"></div>
        </div>
        <div class="product-card__price" v-html="priceFormatted"><?php echo PriceHelper::price($model->getPrice(), ' <span class="currency"><span>₽</span></span>');?></div>
      </div>
    </price-component>

Работа с параметрами

::

  <offers-param inline-template>
    <div class="card-page__main-top-grid">
      <?php
      $size = $model->parameter('size');
      ?>
      <?php if( $size->isSet() ) {?>
        <div class="card-page__param card-page__param--weight" v-if="param.size">
          <span class="card-page__param-label" v-html="param.size.name+':'"><?=$size->name?>:</span> <span v-html="param.size.value"><?=$size->value?></span>
        </div>
      <?php }?>
    </div>
  </offers-param>


Работа с изображениями

::

  <offers-image inline-template>
    <div class="product-card--main-image js-gallery">
      <?php if ($image = $model->getImage()) { ?>
        <a href="<?php echo $image?>" class="js-gallery-item">
          <img v-if="getMainImage()" v-bind:src="getMainImage()" class="animate-image" />
          <img v-else src="<?php echo $image->big?>" alt="<?php echo $model->getHeader()?>" class="animate-image" />
        </a>
      <?php } ?>
    </div>
  </offers-image>


.. danger:: Приложения зарегистрированные в шаблоне должные иметь свойство ``inline-template``!