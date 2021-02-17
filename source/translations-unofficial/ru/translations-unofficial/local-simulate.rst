.. _local-simulate:

=======================================
Локальная симуляция функций контракта
=======================================

Это руководство о том, как локально симулировать обращение к init-функции или к функции приёма из модуля смарт-контракта Wasm в заданном контексте и состоянии. Эта симуляция полезна для проверки смарт-контрактов и результатов выполнения определённых сценариев.

.. seealso::

   Для получения инструкций по автоматизированным модульным тестам, смотрите :ref:`unit-test-contract`.

Подготовка
===========

Убедитесь, что ``cargo-concordium`` установлен, если нет – следуйте руководству
:ref:`setup-tools`.
Вам также потребуется модуль смарт-контракта в Wasm, который нужно симулировать.

.. todo::

  Напишите, что ещё потребуется после размещения схемы контракта.

Симулирование реализации
========================

Чтобы симулировать реализацию экземпляра смарт-контракта с помощью
``cargo-concordium``, выполните следующую команду:

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` (используется с параметром ``--context``) это файл, который содержит контекстную информацию, такую как текущее состояние в цепочке, отправитель транзакции, и какой аккаунт вызвал эту функцию. Пример этого контекста может выглядеть так:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "initOrigin": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
   }

.. seealso::

   Для получения справки по контексту смотрите :ref:`simulate-context`.


Симулирование обновлений
=========================

Чтобы симулировать обновление для контракта экземпляра смарт-контракта с помощью
``cargo-concordium``, выполните следующую команду:

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin

``receive-context.json`` (используется с параметром ``--context`` ) – это файл, содержащий контекстную информацию, такую как текущее состояние в цепочке, отправитель транзакции, какой аккаунт вызвал эту функцию, и какой аккаунт или адрес отправил текущее сообщение. Пример этого контекста может выглядеть так:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "invoker": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "selfAddress": {"index": 0, "subindex": 0},
       "selfBalance": "0",
       "sender": {
           "type": "account",
           "address": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
       },
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
       "owner": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
   }

.. seealso::

   Для получения справки по контексту смотрите :ref:`simulate-context`.
