.. Should answer:
    - Why write a smart contract using rust?
    - What are the pieces needed to write a smart contract in rust?
        - State
            - Serialized
            - Schema
        - Init
        - Receive
    - What sort of testing is possible
    - Best practices?
        - Ensure 0 amount
        - Don't panic
        - Avoid heavy calculations

.. _writing-smart-contracts:

==================================
Разработка смарт-контрактов в Rust
==================================

В concordium блокчейне смарт-контракты развернуты как модули Wasm, но Wasm
спроектирован в первую очередь как цель компиляции и его неудобно писать вручную.
Вместо этого мы можем написать наши смарт-контракты на языке программирования Rust_,
который хорошо поддерживает компиляцию в Wasm.

Смарт-контракты не обязательно писать на Rust.
Это просто первый SDK, который мы предоставляем.
Написанный вручную Wasm или Wasm, скомпилированный из C, C ++, AssemblyScript_,
и других, одинаково действителен в сети, пока он соответствует :ref:`Wasm
limitations we impose <wasm-limitations>`.

.. seealso::

   For more information on the functions described below, see the concordium_std_
   API for writing smart contracts on the Concordium blockchain in Rust.

.. seealso::

   See :ref:`contract-module` for more information about smart contract modules.

Модуль смарт-контракта разрабатывается в Rust как библиотека, которая затем
компилируется в Wasm. Чтобы получить правильный экспорт, атрибут `crate-type`
должны быть установлен в ``["cdylib", "rlib"]`` в файле манифест:

.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...

Написание смарт-контракта с использованием ``concordium_std``
=================================================

Рекомендуется использовать ``concordium_std``, который обеспечивает
более похожий на Rust интерфейс для разработки модулей смарт-контрактов
и вызова функций хоста.

Это позволяет писать функции init и receive как простые функции Rust,
помеченные символами ``#[init(...)]`` и ``#[receive(...)]``, соответственно.

Вот пример смарт-контракта, реализующего счетчик:

.. code-block:: rust

   use concordium_std::*;

   type State = u32;

   #[init(contract = "counter")]
   fn counter_init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let state = 0;
       Ok(state)
   }

   #[receive(contract = "counter", name = "increment")]
   fn contract_receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       ensure!(ctx.sender().matches_account(&ctx.owner()); // Only the owner can increment
       *state += 1;
       Ok(A::accept())
   }

Следует отметить несколько моментов:

.. todo::

   - Write up the requirements in an easier to read way (e.g., split up paragraphs into sub-bullets).
   - These requirements should be part of a specification that is written up somewhere,
     i.e., not just as part of this example.

- Тип функций:

  * init функция должна иметь тип ``&impl HasInitContext -> InitResult<MyState>``
    где ``MyState`` - это тип, реализующий ``Serialize`` признак.
  * receive функция должна принимать ``A: HasActions`` тип параметра,
    ``&impl HasReceiveContext`` и ``&mut MyState`` параметр, и возвращать
    ``ReceiveResult<A>``.

- Аннотация ``#[init(contract = "counter")]`` отмечает функцию, к которой она
  применяется, как функцию инициализации указанного контракта ``counter``.
  Конкретно это означает, что за кулисами этот макрос генерирует экспортируемую 
  функцию с необходимой подписью и именем ``init_counter``.

- ``#[receive(contract = "counter", name = "increment")]`` десериализует и предоставляет
  состояние, которым можно управлять напрямую.
  За кулисами эта аннотация также генерирует экспортируемую функцию с именем
  ``counter.increment``, имеющим требуемую подпись, и выполняет все стандартные 
  действия по десериализации состояния в требуемый тип ``State``.

.. note::

   Обратите внимание, что десериализация не обходится без затрат, и в некоторых
   случаях пользователю может потребоваться более детальный контроль
   над использованием функций хоста.
   Для таких случаев использования аннотации поддерживают ``low_level`` вариант,
   который требует меньше накладных расходов, но требует большего от пользователя.

.. todo::

   - Describe low-level
   - Introduce the concept of host functions before using them in the note above


Сериализуемое состояние и параметры
---------------------------------

.. todo:: Clarify what it means that the state is exposed similarly to ``File``;
   preferably, without referring to ``File``.

В цепочке состояние экземпляра представляется в виде массива байтов и отображается
в интерфейсе, аналогичном интерфейсу ``File`` стандартной библиотеки Rust.

Это можно сделать с помощью ``Serialize`` трейта, который содержит функции
(де-)сериализации.

В комплект ``concordium_std`` включен этот трейта, а также реализации для
большинства типов стандартной библиотеки Rust.
Он также включает макросы для получения признака для определяемых пользователем
структур и перечислений.

.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

То же самое необходимо для параметров для init и receive функций.

.. note::

   Строго говоря, нам нужно только десериализовать байты в наш тип параметра,
   но удобно иметь возможность сериализовать типы при написании модульных тестов.

.. _working-with-parameters:

Работа с параметрами
-----------------------

Параметры функций инициализации и приема, как и состояние экземпляра, представлены
в виде байтовых массивов. Хотя байтовые массивы можно использовать напрямую,
их также можно десериализовать в структурированные данные.

Самый простой способ десериализации параметра через использовании функции `get()`_ 
свойства `Get`_.

В качестве примера посмотрите на следующий контракт, в котором параметр
``ReceiveParameter`` десериализуется в выделенной строке:

.. code-block:: rust

   use concordium_std::*;

   type State = u32;

   #[derive(Serialize)]
   struct ReceiveParameter{
       should_add: bool,
       value: u32,
   }

   #[init(contract = "parameter_example")]
   fn init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let initial_state = 0;
       Ok(initial_state)
   }

   #[receive(contract = "parameter_example", name = "receive")]
   fn receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let parameter: ReceiveParameter = ctx.parameter_cursor().get()?;
       if parameter.should_add {
           *state += parameter.value;
       }
       Ok(A::accept())
   }

Вышеупомянутая receive функция неэффективна в том смысле, что она десериализует
``value``, даже когда это не нужно, то есть когда ``should_add`` это ``false``

Чтобы получить больший контроль и, в данном случае, большую эффективность,
мы можем десериализовать параметр с помощью свойства `Read`_:

.. code-block:: rust

   #[receive(contract = "parameter_example", name = "receive_optimized")]
   fn receive_optimized<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let mut cursor = ctx.parameter_cursor();
       let should_add: bool = cursor.read_u8()? != 0;
       if should_add {
           // Only decode the value if it is needed.
           let value: u32 = cursor.read_u32()?;
           *state += value;
       }
       Ok(A::accept())
   }

Обратите внимание, что ``value`` десериализуется только в том случае, если
``should_add`` это ``true``.
Хотя в этом примере выигрыш в эффективности минимален, он может оказать
существенное влияние на более сложные примеры.


Создание модуля смарт-контрактов с ``cargo-concordium``
==========================================================

Компилятор Rust хорошо поддерживает компиляцию в Wasm с использованием
``wasm32-unknown-unknown``.
Однако даже при компиляции с ``--release`` результирующая сборка включает
большие разделы отладочной информации, которые бесполезны для смарт-контрактов
в сети. 

Чтобы оптимизировать сборку и учесть новые функции, такие как встраивание схем,
мы рекомендуем использовать ``cargo-concordium`` для создания смарт-контрактов.

.. seealso::

   For instructions on how to build using ``cargo-concordium`` see
   :ref:`compile-module`.


Тестирование смарт-контрактов
=======================

Unit тесты с заглушками
---------------------

Моделирование вызова контракта
-----------------------

Лучшие практики
==============

Без паники
-----------

.. todo::

   Use trap instead.

Избегайте появления черных дыр
--------------------------

Смарт-контракт не обязан использовать количество отправленных ему GTU, и
по умолчанию смарт-контракт не определяет никакого поведения для опустошения
баланса экземпляра, если кто-то должен был отправить ему какое-то GTU.
Эти ГТУ были бы тогда навсегда потеряны, и не было бы никакого способа восстановить их.

Поэтому хорошей практикой для смарт-контрактов, которые не имеют дела с GTU,
является обеспечение того, чтобы отправленная сумма GTU была равна нулю,
и отклонение любых вызовов, которые не являются таковыми.

Перемещение тяжелых вычислений вне сети
---------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Get: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Read: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/
