.. _initialize-contract:

========================================
Инициализация экземпляра смарт-контракта
========================================

Это руководство покажет вам, как инициализировать смарт-контракт из развернутого
модуля смарт-контракта с параметрами в формате JSON или двоичном формате.
Кроме того, он покажет, как именовать экземпляр.

Подготовка
===========

Убедитесь, что вы :ref:`running a node<run-a-node>`, используя последнюю версию :ref:`Concordium software<downloads>` и что у вас есть
смарт-контракт :ref:`deployed <deploy-module>` в каком-то модуле сети.

Поскольку инициализация смарт-контракта - это транзакция, вы также должны убедиться,
что у ``concordium-client`` есть аккаунт с достаточным количеством GTU для оплаты
транзакции.

.. note::

   Стоимость этой транзакции зависит от размера параметров, отправляемых в
   init-функцию, и сложности самой функции.

Инициализация
==============

Для инициализации экземпляра смарт-контракта ``my_contract`` без параметров
из развернутого модуля со ссылкой
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` позволяя
использовать до 1000 NRG, выполните
следующую команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

В случае успеха вывод должен быть похож на:

.. todo::

   Update address format to ``<i, s>`` from ``{"index": i, "subindex": s}``
   (throughout the documentation).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Это сообщение означает, что был создан новый экземпляр контракта в сети
с указанным адресом.

.. seealso::

   To get a deeper understanding of contract initialization, see
   :ref:`contract-instances-init-on-chain`.

   For more information about module references and instance addresses,
   see :ref:`references-on-chain`.

   Using module references directly can be inconvenient; to name them, see
   :ref:`naming-a-module`.

.. _init-passing-parameter-json:

Передача параметров в JSON формате
----------------------------------

Параметр в JSON формате может быть передан, если указана :ref:`smart contract schema
<contract-schema>` либо в виде файла, либо встроена в модуль.
Схема используется для сериализации JSON в двоичный файл.

.. seealso::

   :ref:`Read more about why and how to use smart contract schemas <contract-schema>`.

   :ref:`Parameters can be also passed in binary format <init-passing-parameter-bin>`.

Для инициализации экземпляра смарт-контракта ``my_parameter_contract`` из
из модуля со ссылкой
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` с
файлом параметров ``my_parameter.json`` в JSON формате, выполните следующую команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

В случае успеха вывод должен быть похож на:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

В противном случае отображается ошибка с описанием проблемы.
Общие ошибки описаны в следующем разделе.

.. note::

   Если параметр, предоставленный в формате JSON, не соответствует типу,
   указанному в схеме, отобразится сообщение об ошибке. Например:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Если данный модуль не содержит встроенной схемы, его можно предоставить
   с помощью параметра``--schema /path/to/schema.bin``.

.. note::

   GTU также может быть передан экземпляру контракта во время инициализации
   с помощью параметра ``--amount AMOUNT``.


.. _init-passing-parameter-bin:

Передача параметров в двоичном формате
--------------------------------------

При передаче параметров в двоичном формате :ref:`contract schema
<contract-schema>` не требуется.

Для инициализации экземпляра смарт-контракта ``my_parameter_contract`` из
из модуля со ссылкой
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` с
файлом параметров ``my_parameter.bin`` в двоичном формате, выполните следующую команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


В случае успеха вывод должен быть похож на:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. seealso::

   For information on how to work with parameters in smart contracts, see
   :ref:`working-with-parameters`.

.. _naming-an-instance:

Именование экземпляра контракта
===============================

Экземпляру контракта можно присвоить локальный псевдоним или *имя*, что
упростит обращение к нему.
Имя хранится только локально в ``concordium-client`` и не отображается в сети.

.. seealso::

   For an explanation of how and where the names and other local settings are
   stored, see :ref:`local-settings`.

Чтобы добавить имя во время инициализации, используется параметр ``--name``.

Здесь мы инициализируем контракт ``my_contract`` из развернутого модуля
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` и называем
его ``my_named_contract``:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


В случае успеха вывод должен быть похож на:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

Экземпляры контрактов также могут быть названы с помощью команды ``name``.
Чтобы назвать экземпляр с индексом адреса ``0`` как ``my_named_contract``,
выполните следующую команду:

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

В случае успеха вывод должен быть похож на:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. seealso::

   For more information about contract instance addresses, see
   :ref:`references-on-chain`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
