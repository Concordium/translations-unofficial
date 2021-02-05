.. _local-simulate:

======================================
Локальне моделювання функцій контракту
======================================

Це керівництво про те, як локально імітувати виклик деякої init-функції або receive-функції з модуля смарт-контракту Wasm в заданому контексті і стані.
Це моделювання корисно для перевірки смарт-контракту та його результатів в конкретних сценаріях.

.. seealso::

   For a guide on automated unit tests, see :ref:`unit-test-contract`.

Підготовка
==========

Переконайтеся, що у вас встановлений ``cargo-concordium``, якщо немає, дотримуйтесь керівництву :ref:`setup-tools`.
Вам також знадобиться модуль смарт-контракту в Wasm для моделювання.

.. todo::

   Write the rest, when the schema stuff is in place.

Моделювання створення об'єкту
=============================

Щоб змоделювати створення екземпляра смарт-контракту за допомогою ``cargo-concordium``, виконайте наступну команду:

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` (використовується з параметром ``--context``) - це файл, який містить контекстну інформацію, таку як поточний стан ланцюжка, відправник транзакції і обліковий запис, що викликала цю функцію.
Прикладом такого контексту може бути:

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

   For a reference of the context see :ref:`simulate-context`.


Моделювання оновлень
====================

Щоб змоделювати оновлення примірника смарт-контракту контракту за допомогою ``cargo-concordium``, виконайте:

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin

``receive-context.json`` (використовується з параметром  ``--context``) - це файл, який містить контекстну інформацію, таку як поточний стан ланцюжка, відправник транзакції, обліковий запис, що викликала цю функцію, і обліковий запис або адреса, відправили поточне повідомлення. Прикладом такого контексту може бути:

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

   For a reference of the context see :ref:`simulate-context`.
