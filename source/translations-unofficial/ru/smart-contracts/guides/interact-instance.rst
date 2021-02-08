.. _interact-instance-ru:

============================================
Взаимодействие с экземпляром смарт-контракта
============================================

Это руководство покажет вам, как взаимодействовать с экземпляром смарт-контракта,
что означает запуск receive-функциии, которая, возможно, обновляет состояние
экземпляра.

Подготовка
===========

Убедитесь, что вы :ref:`запустили ноду <run-a-node>`, используя последнюю версию :ref:`программного обеспечения Concordium <downloads>` и
что у вас есть экземпляр смарт-контракта в сети для проверки.

.. seealso::

   Чтобы узнать, как развернуть модуль смарт-контракта, см. :ref:`deploy-module-ru` и
   как создать экземпляр :ref:`initialize-contract-ru`.

Поскольку взаимодействие со смарт-контрактом является транзакцией, вы также должны
убедиться, что у ``concordium-client`` установлен аккаунт с достаточным количеством GTU для
оплаты транзакций.

.. note::

   Стоимость этой транзакции зависит от размера параметров, отправляемых в
   функцию приема, и сложности самой функции.

Взаимодействие
==============

Чтобы обновить экземпляр с индексом адреса ``0`` с помощью
receive-функции ``my_receive`` без параметров, позволяя использовать до 1000 NRG,
выполните следующую команду:

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

В случае успеха результат должен быть похож на:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

Передача параметров в JSON формате
----------------------------------

Параметр в JSON формате может быть передан, если указана :ref:`схема смарт-контракта
<contract-schema-ru>`, которая поставляется либо в виде файла, либо встроена в модуль.
Схема используется для сериализации JSON в двоичный файл.

.. seealso::

   :ref:`Узнайте больше о том, почему и как использовать схемы смарт-контрактов
   <contract-schema-ru>`.

Чтобы обновить экземпляр с индексом адреса ``0`` с помощью
receive-функции ``my_parameter_receive`` с файлом параметров ``my_parameter.json`` в JSON
формате, выполните следующую команду:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json

В случае успеха результат должен быть похож на:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

В противном случае отображается ошибка с описанием проблемы.
Общие ошибки описаны в следующем разделе.

.. seealso::

   Для получения дополнительной информации об адресах экземпляра контракта см.
   :ref:`references-on-chain`.

.. note::

   Если параметр, предоставленный в JSON формате, не соответствует типу,
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

   Если данный модуль не содержит встроенной схемы, это может быть предоставлено
   с помощью параметра ``--schema /path/to/schema.bin``.

.. note::

   GTU также можно перенести в контракт во время обновлений с помощью
   параметра ``--amount AMOUNT``.

Передача параметров в двоичном формате
--------------------------------------

При передаче параметров в двоичном формате
:ref:`схема смарт-контракта <contract-schema-ru>` не требуется.

Чтобы обновить экземпляр с индексом адреса ``0`` с помощью
receive-функции ``my_parameter_receive`` с файлом параметров ``my_parameter.bin`` в двоичном
формате, выполните следующую команду:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

В случае успеха результат должен быть похож на:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. seealso::

   Для получения информации о том, как работать с параметрами в смарт-контрактах, см.
   :ref:`working-with-parameters`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
