.. _list of types implementing the SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schema-ru:

==========================
Построение схемы контракта
==========================

Это руководство покажет вам, как построить схему смарт-контракта, как
экспортировать ее в файл и/или встроить схему в модуль смарт-контракта, используя
``cargo-concordium``.

Подготовка
==========

Во-первых, убедитесь, что у вас установлен ``cargo-concordium``, а если нет,
то руководство :ref:`установки <setup-tools-ru>` вам поможет.

Нам также понадобится исходный код смарт-контракта на Rust, для которого вы хотите
построить схему.

Настройка контракта для схемы
=============================

Чтобы построить схему контракта, мы сначала должны подготовить наш смарт-контракт
для построения схемы.

Мы можем выбрать, какие части нашего смарт-контракта включить в схему.
Эти параметры должны включать схему для состояния контракта и/или для каждого
из параметров функций init и receive.

Каждый тип, который мы хотим включить в схему, должен реализовывать свойство ``SchemaType``.
Это уже сделано для всех базовых типов и некоторых других типов (см. `list of types implementing the SchemaType`_).
В большинстве других случаев это также можно сделать автоматически, используя
``#[derive(SchemaType)]``::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

Реализация свойства ``SchemaType`` вручную требует указания только одной функции,
которая является геттером для ``schema::Type``, которая по существу описывает,
как этот тип представлен в байтах и как его необходимо представлять.

.. todo::

   Create an example showing how to manually implement ``SchemaType`` and link
   to it from here.

Включение состояния контракта
-----------------------------

Чтобы сгенерировать и включить схему для состояния контракта, мы аннотируем тип
с помощью макроса ``#[contract_state(contract = ...)]``::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

Или еще проще, если состояние контракта имеет тип, который уже реализует ``SchemaType``, например u32::

   #[contract_state(contract = "my_contract")]
   type State = u32;

Включение параметров функции
-----------------------------

Чтобы сгенерировать и включить схему параметров для функций init и receive,
мы устанавливаем необязательный ``параметр`` атрибута для
``#[init(..)]``- и ``#[receive(..)]``-макросов::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }

Построение схемы
================

Теперь мы готовы построить актуальную схему с помощью ``cargo-concordium``, и
у нас есть варианты встроить схему и/или записать схему в файл.

.. seealso::

   For more on which to choose see
   :ref:`here<contract-schema-which-to-choose-ru>`.

Встраивание схемы
-----------------

Чтобы встроить схему в модуль смарт-контракта, мы добавляем
``--schema-embed`` к команде сборки

.. code-block:: console

   $cargo concordium build --schema-embed

В случае успеха вывод команды сообщит вам общий размер схемы в байтах.

Вывод схемы в файл
------------------

Чтобы вывести схему в файл, мы можем использовать ``--schema-out=FILE``
где ``FILE`` - это путь к создаваемому файлу:

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
