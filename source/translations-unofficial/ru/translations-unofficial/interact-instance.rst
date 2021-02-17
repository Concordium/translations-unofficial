.. _interact-instance:

=============================================
Взаимодействие с экземпляром смарт-контракта
=============================================

Это руководство покажет, как взаимодействовать с экземпляром смарт-контракта, что означает вызов функции приёма, которая, возможно, обновляет состояние экземпляра.

Подготовка
===========

Убедитесь, что :ref:`нода запущена <run-a-node>` используя последнюю версию :ref:`Concordium software<downloads>`
и что имеется экземпляр on-chain смарт-контракта для проверки.

.. seealso::
   О том, как развернуть модуль смарт-контракта, смотрите :ref:`deploy-module` и о том, как создать экземпляр, смотрите :ref:`initialize-contract` .

Поскольку взаимодействия с контрактом являются транзакциями, необходимо убедиться, что установлен ``concordium-client`` с аккаунтом, имеющим достаточное количество GTU для оплаты транзакций.

.. note::

   Стоимость этой транзакции зависит от размера параметров, посылаемых в функцию приёма и сложности самой функции.

Взаимодействие
===============

Для обновления экземпляра с адресным индексом ``0`` uиспользуя функцию приёма без параметров ``my_receive`` с допустимым количеством energy до 1000, выполните следующую команду:

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

В случае успеха результат будет выглядеть примерно так:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

Передача параметров в формате JSON
------------------------------------

Параметры в формате JSON могут быть переданы, если поддерживается  :ref:`smart contract schema
<contract-schema>` , как в случае записи в файл, так и в случае встраивания в модуль. Схема используется для сериализации JSON в двоичный формат.

.. seealso::

   :ref:`Узнайте больше о том, зачем и как используются схемы контрактов
   <contract-schema>`.

Для обновления экземпляра с адресным индексом ``0`` используя функцию приёма
``my_parameter_receive`` с файлом параметров ``my_parameter.json`` в формате JSON, выполните следующую команду:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json

В случае успеха результат будет выглядеть примерно так:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

В противном случае, отображается сообщение об ошибке с описанием возникшей проблемы. Распространённые ошибки описаны в следующем разделе.

.. seealso::

   Для получения более подробной информации об адресах экземпляров контракта,
   смотрите
   :ref:`references-on-chain`.

.. note::

   Если параметр, указанный в формате JSON, не соответствует заданному в схеме типу, будет выведено сообщение об ошибке. Например:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Если заданный модуль не содержит встроенную схему, это можно решить с помощью параметра ``--schema /path/to/schema.bin``.

.. note::

   GTU также можно передать контракту в процессе обновления с помощью параметра
   ``--amount AMOUNT``.

Передача параметров в двоичном формате
---------------------------------------

При передаче параметров в двоичном формате :ref:`contract schema <contract-schema>` не требуется.

Для обновления экземпляра с адресным индексом ``0`` используя функцию приёма
``my_parameter_receive`` с файлом параметров ``my_parameter.bin`` в двоичном формате, выполните следующую команду:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

В случае успеха результат будет выглядеть примерно так:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. seealso::

   Для получения инструкций о том, как работать с параметрами в смарт-контракте,
   смотрите
   :ref:`working-with-parameters`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
