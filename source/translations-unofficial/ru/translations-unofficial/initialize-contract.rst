=========================================
Инициализация экземпляра смарт-контракта
=========================================

Это руководство покажет, как инициализировать смарт-контракт из развёрнутого модуля смарт-контракта с параметрами в формате JSON или в двоичном формате. В дополнение, будет показано, как именовать экземпляр.

Подготовка
===========

Убедитесь, что :ref:`нода запущена<run-a-node>` используя последнюю версию :ref:`Concordium software<downloads>`
и что имеется :ref:`развернутый <deploy-module>` смарт-контракт в каком-либо on-chain модуле.

Поскольку инициализация смарт-контракта является транзакцией, убедитесь также, что установлен ``concordium-client``
с аккаунтом, имеющим достаточное количество GTU для оплаты транзакции.

.. note::

   Стоимость этой транзакции зависит от размера параметров, посылаемых в init-функцию и сложности самой функции.

Инициализация
==============

Для инициализации экземпляра смарт-контракта ``my_contract``
без параметров из развёрнутого модуля по ссылке
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` с допустимым количеством NRG до 1000, выполните следующую команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

В случае успеха результат будет выглядеть примерно так:

.. todo::

   Измените формат адреса на ``<i, s>`` с ``{"index": i, "subindex": s}``
   (во всей документации).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Появление этого сообщения означает, что новый экземпляр on-chain контракта создан по указанному адресу.

.. seealso::

   Для более глубокого понимания инициализации контракта, смотрите
   :ref:`contract-instances-init-on-chain`.

   Для получения более подробной информации по ссылкам на модули и адресам
   экземпляров смотрите :ref:`references-on-chain`.

   Использование ссылок на модули может быть неудобным. О том, как их именовать,
   смотрите :ref:`naming-a-module`.

Передача параметров в формате JSON
------------------------------------

Параметры в формате JSON могут быть переданы, если поддерживается  :ref:`smart contract schema
<contract-schema>` как в случае записи в файл, так и в случае встраивания в модуль. Схема используется для сериализации JSON в двоичный формат.

.. seealso::

   :ref:`Узнайте больше о том, зачем и как используются схемы смарт-контрактов <contract-schema>`.

   :ref:`Параметры можно передавать в двоичном формате <init-passing-parameter-bin>`.

Для инициализации экземпляра контракта ``my_parameter_contract`` из модуля по ссылке
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` с файлом параметров
``my_parameter.json`` в формате JSON, выполните следующую команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

В случае успеха результат будет выглядеть примерно так:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

В противном случае, отображается сообщение об ошибке с описанием возникшей проблемы. Распространённые ошибки описаны в следующем разделе.

.. note::

   Если параметр, указанный в формате JSON, не соответствуют заданному в схеме типу, будет выведено сообщение об ошибке. Например:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Если заданный модуль не содержит встроенную схему, это можно решить с помощью параметра ``--schema /path/to/schema.bin`` parameter.

.. note::

   GTU также можно передать экземпляру контракта в процессе инициализации с помощью параметра ``--amount AMOUNT`` parameter.


Передача параметров в двоичном формате
---------------------------------------

При передаче параметров в двоичном формате :ref:`contract schema
<contract-schema>` не требуется.

Для инициализации экземпляра контракта ``my_parameter_contract`` из модуля по ссылке
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` с файлом параметров
``my_parameter.bin`` в двоичном формате, выполните следующую команду:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


В случае успеха результат будет выглядеть примерно так:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. seealso::

   Для получения инструкций о том, как работать с параметрами в смарт-контракте,
   смотрите :ref:`working-with-parameters`.

Именование экземпляра контракта
================================

Экземпляру контракта можно присвоить локальный псевдоним, или *имя*, которое делает обращение к нему проще. Имя хранится только локально утилитой
``concordium-client``, и невидимо on-chain.

.. seealso::

   Для объяснения, где и как сохраняются имена и другие локальные параметры,
   смотрите :ref:`local-settings`.

Для добавления имени в процессе инициализации используется параметр ``--name``.
Инициализируем контракт ``my_contract`` из развёрнутого модуля по ссылке
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` и назовём его ``my_named_contract``:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


В случае успеха результат будет выглядеть примерно так:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

Имя экземпляра контракта может быть также задано с помощью команды ``name``.
Чтобы присвоить экземпляру с адресным индексом ``0`` имя ``my_named_contract``, выполните следующую команду:

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

В случае успеха результат будет выглядеть примерно так:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. seealso::

   Для получения более подробной информации об адресах экземпляров контракта,
   смотрите :ref:`references-on-chain`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
