Модули
======

Конфигурация модулей
~~~~~~~~~~~~~~~~~~~~

Для модулей существует 2 варианта конфигурации. Первый это конфигурация по умолчанию, располагается по пути ``protected/modules/[название модуля]/default_config.json``. Второй переопределяет конфигурацию по умолчанию, путем объединения. Располагается ``protected/config/[название модуля]_module.json``, для модуля review это будет файл ``protected/config/review_module.json``. Конфигурационные файлы имеют формат json и должны быть валидны.
Пример конфигурационного файла: ::

 {
   "fields": {
     "author_email": true,
     "author_name": {
       "show": true,
       "label": "Имя автора"
     },
     "text_positive": false,
     "text_negative": false,
     "rating": true,
     "like": false,
     "dislike": false,
     "url": true
   }
 }

Поле ``"fields"`` отвечает за отключение полей в backend. Содержит название атрибутов моделей как ключа например ``author_email`` и может принимать значение ``true`` или ``false``. Так же можно задавать ``label`` для этого нужно записать конфигурацию как в примере::

     "author_name": {
       "show": true,
       "label": "Имя автора"
     },

``show`` отвечает за видимость поля в grid и в форме.

.. note:: Если поле ``"fields"`` не задано в конфигурационном файле, то оно включено по умолчанию!

Миграции
~~~~~~~~

Таблицы модулей нужно исключить из схемы БД ``.sql/schema.sql``, их нужно в нести в миграции.
Миграции для модулей должны лежать в директории модуля ``protected/modules/названиеМодуля/migrations``, например для модуля *отзывов* директория миграций следующая: ``protected/modules/reviews/migrations``.
Миграции модуля должна содержать реализацию метода ``down()`` в котором будет уничтожиться таблица модуля с предварительной проверкой существования таблицы, а метод ``up()`` должен в начале вызвать метод ``down()``. Это позволит всегда успешно выполнять миграцию, даже если таблица предварительно была создана в напрямую в базе.
Рассмотрим пример миграции для модуля отзывов::

    public function up()
    {
      $this->down();
      $this->execute("CREATE TABLE `{{review}}` (
        `id` int(11) NOT NULL AUTO_INCREMENT,
        `author_name` varchar(255) NOT NULL,
        `date_time` datetime NOT NULL,
        `text` mediumtext NOT NULL,
        `model` varchar(255) NOT NULL,
        `model_id` varchar(255) NOT NULL,
        `url` varchar(255) NOT NULL,
        `properties_json` mediumtext NOT NULL COMMENT 'JSON',
        `visible` tinyint(1) NOT NULL,
        `author_email` varchar(255) NOT NULL,
        `text_positive` text NOT NULL,
        `text_negative` text NOT NULL,
        `rating` int(1) DEFAULT NULL,
        `like` int(11) DEFAULT NULL,
        `dislike` int(11) DEFAULT NULL,
        PRIMARY KEY (`id`),
        KEY `model` (`model`,`model_id`),
        KEY `model_2` (`model`,`model_id`,`visible`),
        KEY `visible` (`visible`)
      ) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;
      ");
    }

    public function down()
    {
      $this->execute("DROP TABLE IF EXISTS `{{review}}`");
      return true;
    }

Для выполнения миграции модуля существует консольная команда::

  bin/yiic migratemodule названиеМодуля

Эта команда только перезаписывать путь к папке миграций, и ее синтаксис аналогичен команде ``bin/yiic migrate``.

.. note:: Если необходимо внести изменения в схему таблиц модулей, то для применения изменений нужно сначала откатить миграции модулей, а потом накатить миграции по новой. Например::

    bin/yiic migratemodule review down 1
    bin/yiic migratemodule review up

