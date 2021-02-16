.. _deploy-module:

===================================
Розгортання модуля смарт-контрактів
===================================

Це керівництво покаже вам, як розгорнути модуль смарт-контракту *on-chain* і як його назвати.

Підготовка
==========

Переконайтеся, що ви :ref:`running a node<run-a-node>` використовуєте останню версію :ref:`Concordium software<downloads>` і що у вас є :ref:`smart-contract module<setup-tools>`, готовий до розгортання.

Оскільки розгортання модуля смарт-контракту здійснюється у формі транзакції, вам також буде потрібно налаштувати ``concordium-client`` з аккаунтом з достатньою кількістю GTU для оплати транзакції.

.. note::

   Вартість транзакції залежить від розміру модуля смарт-контракту. ``concordium-client`` показує вартість і запитує підтвердження перед виконанням будь-якої транзакції.

Розгортання
===========

Для розгортання модуля смарт-контракту ``my_module.wasm``, використовуйте акаунт з ім'ям account-name, виконавши наступну команду:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. note::

   Параметр --sender можна опустити, якщо використовується обліковий запис "за замовчуванням". Для стислості ми зробимо це нижче.

У разі успіху висновок повинен бути схожий на:

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'.

Зверніть увагу на посилання на модуль, так як вона використовується при створенні екземплярів смарт-контрактів.

.. seealso::

   For a guide on how to initialize smart contracts from a deployed module see
   :ref:`initialize-contract`.

   For more information about module references, see :ref:`references-on-chain`.

.. _naming-a-module:

Іменування модуля
=================

Модулю може бути присвоєно локальний псевдонім або *ім'я*, що спрощує звернення до нього.
Ім'я зберігається тільки локально в ``concordium-client``, і не відображається в мережі.

.. seealso::

   For an explanation of how and where the names and other local settings are
   stored, see :ref:`local-settings`.

Щоб додати ім'я під час розгортання, використовується параметр ``--name``.
Тут ми називаємо модуль ``my_deployed_module``:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

У разі успіху висновок повинен бути схожий на:

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

Модулі також можуть бути названі за допомогою команди ``name``.
Для іменування розгорнутого Моделя через посилання ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` як ``some_deployed_module``, виконайте наступну команду:

.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

Результат повинен бути схожий на:

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module'.
