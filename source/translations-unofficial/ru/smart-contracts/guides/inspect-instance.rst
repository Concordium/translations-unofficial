.. _inspect-instance:

=================================
Проверка экземпляра смарт-контракта
=================================

Это руководство покажет вам, как проверить экземпляр смарт-контракта.
Проверка экземпляра покажет вам его имя, владельца, ссылку на модуль, баланс,
состояние и функции приема:

Подготовка
===========

Убедитесь, что вы :ref:`running a node<run-a-node>` с использованием последней версии :ref:`Concordium software<downloads>` и что у вас есть
экземпляр смарт-контракта в сети для проверки.

.. seealso::
   For how to deploy a smart contract module see :ref:`deploy-module` and for
   how to create an instance :ref:`initialize-contract`.

Проверка
==========

Чтобы проверить или показать информацию об экземпляре смарт-контракта с
индексом адреса ``0``, выполните следующую команду:

.. code-block:: console

   $concordium-client contract show 0

Результат должен быть похож на:

.. code-block:: console

   Contract:        my_contract
   Owner:           '4Lh8CPhbL2XEn55RMjKii2XCXngdAC7wRLL2CNjq33EG9TiWxj' (default)
   ModuleReference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'
   Balance:         0.000000 GTU
   State:
       {
           "first_field": 0,
           "second_field": 42
       }
   Functions:
    - receive_one
    - receive_two

.. seealso::

   For more information about contract instance addresses, see
   :ref:`references-on-chain`.

Уровень детализации проверки зависит от того, имеет ли команда ``show``
доступ к :ref:`contract schema <contract-schema>`.
Если схема встроена, она будет использоваться неявно.
В противном случае схему можно предоставить с помощью ``--schema /path/to/schema.bin``
параметра.

.. note::

   Файл схемы, предоставленный с помощью параметра ``--schema``, будет иметь приоритет
   над встроенной схемой.

.. seealso::

   :ref:`Read more about why and how to use smart contract schemas <contract-schema>`.
