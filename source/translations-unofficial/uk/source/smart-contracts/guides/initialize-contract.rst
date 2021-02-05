.. _initialize-contract:

=====================================
Ініціалізація об'єкта смарт-контракту
=====================================

Це керівництво покаже вам, як форматувати смарт-контракт з розгорнутого модуля смарт-контракту з параметрами в форматі JSON або довічним форматі.
Крім того, він покаже, як назвати екземпляр.

Підготовка
==========

Переконайтеся, що ви :ref:`running a node<run-a-node>`, використовуючи останню версію :ref:`Concordium software<downloads>` і що у вас є смарт-контракт :ref:`deployed <deploy-module>` в якомусь модулі мережі.

Оскільки ініціалізація смарт-контракту - це транзакція, ви також повинні переконатися, що у ``concordium-client`` є аккаунт з достатньою кількістю GTU для оплати транзакції.

.. note::

   Вартість цієї транзакції залежить від розміру параметрів, що відправляються в init-функцію, і складності самої функції.

Ініціалізація
=============

Для ініціалізації примірника смарт-контракту ``my_contract`` без параметрів з розгорнутого модуля з посиланням ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` дозволяючи використовувати до 1000 NRG, виконайте наступну команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

У разі успіху результат повинен бути схожий на:

.. todo::

   Update address format to ``<i, s>`` from ``{"index": i, "subindex": s}``
   (throughout the documentation).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Це повідомлення означає, що був створений новий об'єкт контракту в мережі з вказаною адресою.

.. seealso::

   To get a deeper understanding of contract initialization, see
   :ref:`contract-instances-init-on-chain`.

   For more information about module references and instance addresses,
   see :ref:`references-on-chain`.

   Using module references directly can be inconvenient; to name them, see
   :ref:`naming-a-module`.

.. _init-passing-parameter-json:

Передача параметрів в JSON форматі
----------------------------------

Параметр в JSON форматі може бути переданий, якщо вказана :ref:`smart contract schema<contract-schema>` або у вигляді файлу, або вбудована в модуль.
Схема використовується для сериализации JSON в бінарний файл.

.. seealso::

   :ref:`Read more about why and how to use smart contract schemas <contract-schema>`.

   :ref:`Parameters can be also passed in binary format <init-passing-parameter-bin>`.

Для ініціалізації примірника смарт-контракту ``my_parameter_contract`` з з модуля з посиланням ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` з файлом параметрів ``my_parameter.json`` в JSON форматі, виконайте наступну команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

У разі успіху результат повинен бути схожий на:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

В іншому випадку відображається помилка з описом проблеми.
Загальні помилки описані в наступному розділі.

.. note::

   Якщо параметр, наданий у форматі JSON, не відповідає типу, зазначеному в схемі, відобразиться повідомлення про помилку.
   Наприклад:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Якщо даний модуль не містить вбудованої схеми, його можна надати за допомогою параметра ``--schema /path/to/schema.bin``.

.. note::

   GTU також може бути переданий примірнику контракту під час ініціалізації за допомогою параметра ``--amount AMOUNT``.

.. _init-passing-parameter-bin:

Передача параметрів в бінарному форматі
---------------------------------------

При передачі параметрів в бінарному форматі :ref:`contract schema<contract-schema>` не потребується.

Для ініціалізації примірника смарт-контракту ``my_parameter_contract`` з модуля з посиланням ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` з файлом параметрів ``my_parameter.bin`` в бінарному форматі, виконайте наступну команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


У разі успіху результат повинен бути схожий на:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. seealso::

   For information on how to work with parameters in smart contracts, see
   :ref:`working-with-parameters`.

.. _naming-an-instance:

Іменування об'єкту контракту
============================

Примірнику контракту можна привласнити локальний псевдонім або *ім'я*, що спростить звернення до нього. Ім'я зберігається тільки локально в ``concordium-client`` і не відображається в мережі.

.. seealso::

   For an explanation of how and where the names and other local settings are
   stored, see :ref:`local-settings`.

Щоб додати ім'я під час ініціалізації, використовується параметр ``--name``.

Тут ми инициализируем контракт ``my_contract`` з розгорнутого модуля ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` і називаємо його ``my_named_contract``:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


У разі успіху результат повинен бути схожий на:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

Об'єкти контрактів також можуть бути названі за допомогою команди ``name``. Щоб назвати екземпляр з індексом адреси ``0`` як ``my_named_contract``, виконайте наступну команду:

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

У разі успіху результат повинен бути схожий на:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. seealso::

   For more information about contract instance addresses, see
   :ref:`references-on-chain`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
