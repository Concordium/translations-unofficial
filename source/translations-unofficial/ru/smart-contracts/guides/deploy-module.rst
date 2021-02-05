.. _deploy-module:

=====================================
Развертывание модуля смарт-контрактов
=====================================

Это руководство покажет вам, как развернуть модуль смарт-контракта *в сети* и
как его назвать.

Подготовка
===========

Убедитесь, что вы :ref:`running a node<run-a-node>` используете последнюю версию :ref:`Concordium software<downloads>` и
что у вас есть :ref:`smart-contract module<setup-tools>`, готовый к развертыванию.

Поскольку развертывание модуля смарт-контракта осуществляется в форме транзакции,
вам также потребуется настроить ``concordium-client`` с аккаунтом с
достаточным количеством GTU для оплаты транзакции.

.. note::

   Стоимость транзакции зависит от размера модуля смарт-контракта.
   ``concordium-client`` показывает стоимость и запрашивает подтверждение
   перед выполнением какой-либо транзакции.

Развертывание
=============

Для развертывания модуля смарт-контракта ``my_module.wasm``, используйте аккаунт
с именем account-name, выполнив следующую команду:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. note::

   Параметр --sender можно опустить, если используется учетная запись "по умолчанию".
   Для краткости мы сделаем это ниже.

В случае успеха вывод должен быть похож на:

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'.

Обратите внимание на ссылку на модуль, так как она используется при создании экземпляров
смарт-контрактов.

.. seealso::

   For a guide on how to initialize smart contracts from a deployed module see
   :ref:`initialize-contract`.

   For more information about module references, see :ref:`references-on-chain`.

.. _naming-a-module:

Именование модуля
=================

Модулю может быть присвоен локальный псевдоним или *имя*, что упрощает обращение
к нему.
Имя хранится только локально в ``concordium-client``, и не отображается в сети.

.. seealso::

   For an explanation of how and where the names and other local settings are
   stored, see :ref:`local-settings`.

Чтобы добавить имя во время развертывания, используется параметр ``--name``.
Здесь мы называем модуль ``my_deployed_module``:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

В случае успеха вывод должен быть похож на:

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

Модули также могут быть названы с помощью команды ``name``.
Для именования развернутого моделя через ссылку
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` как
``some_deployed_module``, выполните следующую команду:

.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

Результат должен быть похож на:

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module'.
