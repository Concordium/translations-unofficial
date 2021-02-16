.. _list of types implementing the SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schema:

========================
Побудова схеми контракту
========================

Це керівництво покаже вам, як побудувати схему смарт-контракту, як експортувати її в файл і(або) вбудувати схему в модуль смарт-контракту, використовуючи ``cargo-concordium``.

Підготовка
==========

По-перше, переконайтеся, що у вас встановлений ``cargo-concordium`` а якщо ні, то :ref:`setup-tools` вам допоможе.

Нам також знадобиться вихідний код смарт-контракту на Rust, для якого ви хочете побудувати схему.

Налаштування контракту для схеми
================================

Чтобы построить схему контракта, мы сначала должны подготовить наш смарт-контракт для построения схемы.

Ми можемо вибрати, які частини нашого смарт-контракту включити в схему.
Ці параметри повинні включати схему для стану контракту і(або) для кожного з параметрів функцій init і receive.

Кожен тип, який ми хочемо включити в схему, повинен реалізовувати властивість ``SchemaType``.
Це вже зроблено для всіх базових типів і деяких інших типів (див. `list of types implementing the SchemaType`_).
У більшості інших випадків це також можна зробити автоматично, використовуючи ``#[derive(SchemaType)]``::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

Реалізація властивості ``SchemaType`` вручну вимагає вказівки тільки однієї функції, яка є геттером для ``schema::Type``, яка по суті описує, як цей тип представлений в байтах і як його необхідно представляти.

.. todo::

   Create an example showing how to manually implement ``SchemaType`` and link
   to it from here.

Включення стану контракту
-------------------------

Щоб згенерувати і включити схему для стану контракту, ми аннотіруем тип за допомогою макросу ``#[contract_state(contract = ...)]``::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

Або ще простіше, якщо стан контракту має тип, який вже реалізує ``SchemaType``, наприклад u32::

   #[contract_state(contract = "my_contract")]
   type State = u32;

Включення параметрів функції
----------------------------

Щоб згенерувати і включити схему параметрів для функцій init і receive, ми встановлюємо необов'язковий ``параметр`` атрибута для ``#[init(..)]`` та ``#[receive(..)]`` макросів::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }

Побудова схеми
==============

Тепер ми готові побудувати актуальну схему за допомогою ``cargo-concordium``, і у нас є варіанти вбудувати схему і(або) записати схему в файл.

.. seealso::

   For more on which to choose see
   :ref:`here<contract-schema-which-to-choose>`.

Вбудовувані схеми
-----------------

Щоб вбудувати схему в модуль смарт-контракту, ми додаємо ``--schema-embed`` до команди збірки

.. code-block:: console

   $cargo concordium build --schema-embed

У разі успіху висновок команди повідомить вам загальний розмір схеми в байтах.

Висновок схеми в файл
---------------------

Щоб вивести схему в файл, ми можемо використовувати ``--schema-out=FILE`` де ``FILE`` - це шлях до створюваного файлу:

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
