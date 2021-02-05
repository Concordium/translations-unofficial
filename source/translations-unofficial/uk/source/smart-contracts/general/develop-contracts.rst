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
Розробка смарт-контрактів в Rust
==================================

У concordium блокчейне смарт-контракти розгорнуті як модулі Wasm, але Wasm спроектований в першу чергу як мета компіляції і його незручно писати вручну. 
Замість цього ми можемо написати наші смарт-контракти на мові програмування Rust_, який добре підтримує компіляцію в Wasm. 

Смарт-контракти не обов'язково писати на Rust. 
Це просто перший SDK, який ми надаємо. 
Написаний вручну Wasm або Wasm, скомпільований з C, C ++, AssemblyScript_, і інших, однаково дійсний мережі, поки він відповідає :ref:`Wasm limitations we impose <wasm-limitations>`.

.. seealso::

   For more information on the functions described below, see the concordium_std_
   API for writing smart contracts on the Concordium blockchain in Rust.

.. seealso::

   See :ref:`contract-module` for more information about smart contract modules.

Модуль смарт-контракту розробляється в Rust як бібліотека, яка потім компілюється в Wasm.
Щоб отримати правильний експорт, атрибут `crate-type` повинні бути встановлений в ``["cdylib", "rlib"]`` у файлі маніфест: 

.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...

Написання смарт-контракту з використанням ``concordium_std``
=================================================

Рекомендується використовувати ``concordium_std``, який забезпечує більш схожий на Rust інтерфейс для розробки модулів смарт-контрактів і виконання функцій хоста.

Це дозволяє писати функції init і receive як прості функції Rust, помічені символами ``#[init(...)]`` та ``#[receive(...)]``, відповідно.

Ось приклад смарт-контракту, що реалізує лічильник: 

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

Слід зазначити кілька моментів: 

.. todo::

   - Write up the requirements in an easier to read way (e.g., split up paragraphs into sub-bullets).
   - These requirements should be part of a specification that is written up somewhere,
     i.e., not just as part of this example.

- Тип функцій:
  * init функція повинна мати тип ``&impl HasInitContext -> InitResult<MyState>`` де ``MyState`` - це тип, який реалізує ``Serialize`` ознака. 
  * receive функція повинна приймати ``A: HasActions`` тип параметра, ``&impl HasReceiveContext`` і ``&mut MyState`` параметр, і повертати ``ReceiveResult<A>``. 

- Анотація ``#[init(contract = "counter")]`` зазначає функцію, до якої вона застосовується, як функцію ініціалізації зазначеного контракту ``counter``. 
  Конкретно це означає, що за лаштунками цей макрос генерує електроенергію, що експортується функцію з необхідною підписом і ім'ям ``init_counter``. 

- ``#[receive(contract = "counter", name = "increment")]`` десеріалізує і надає стан, яким можна керувати безпосередньо. 
  За лаштунками ця інструкція також генерує електроенергію, що експортується функцію з ім'ям ``counter.increment``, які мають необхідну підпис, і виконує всі стандартні дії по десеріалізациі стану в необхідний тип ``State``. 

.. note::

   Зверніть увагу, що десеріалізацію не обходиться без витрат, і в деяких випадках користувачеві може знадобитися більш детальний контроль над використанням функцій хоста. 
   Для таких випадків використання анотації підтримують ``low_level`` варіант, який вимагає менше накладних витрат, але вимагає більшого від користувача.

.. todo::

   - Describe low-level
   - Introduce the concept of host functions before using them in the note above


Серіалізовані стан і параметри 
---------------------------------

.. todo:: Clarify what it means that the state is exposed similarly to ``File``;
   preferably, without referring to ``File``.

У ланцюжку стан екземпляра представляється у вигляді масиву байтів і відображається в інтерфейсі, аналогічному інтерфейсу ``File`` стандартної бібліотеки Rust.

Це можна зробити за допомогою ``Serialize`` трейта, який містить функції (де-) сериализации.

У комплект ``concordium_std`` включений цей трейта, а також реалізації для більшості типів стандартної бібліотеки Rust. 
Він також включає макроси для отримання ознаки для визначених користувачем структур і перерахувань. 

.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

Те ж саме необхідно для параметрів для init і receive функцій.

.. note::

   Строго кажучи, нам потрібно тільки десеріалізовать байти в наш тип параметра, але зручно мати можливість серіалізовать типи при написанні модульних тестів. 

.. _working-with-parameters:

Робота з параметрами 
-----------------------

Параметри функцій ініціалізації і прийому, а також стан екземпляра, представлені у вигляді байтових масивів. 
Хоча байтові масиви можна використовувати безпосередньо, їх також можна десеріалізовать в структуровані дані.

Найпростіший спосіб десеріалізациі параметра через використання функції `get()`_ властивості `Get`_ .

Як приклад подивіться на наступний контракт, в якому параметр ``ReceiveParameter`` десеріалізуется в виділеному рядку: 

.. code-block:: rust
   :emphasize-lines: 24

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

Вищезазначена receive функція неефективна в тому сенсі, що вона десеріалізует ``value``, навіть коли це не потрібно, то є коли ``should_add`` це ``false`` 

Щоб отримати більший контроль і, в даному випадку, більшу ефективність, ми можемо десеріалізовать параметр за допомогою властивості `Read`_: 

.. code-block:: rust
   :emphasize-lines: 7, 10

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

Зверніть увагу, що ``value`` десеріалізуется тільки в тому випадку, якщо ``should_add`` це ``true``. 
Хоча в цьому прикладі виграш в ефективності мінімальний, він може зробити істотний вплив на більш складні приклади. 

Створення модуля смарт-контрактів з ``cargo-concordium``
==========================================================

Компілятор Rust добре підтримує компіляцію в Wasm з використанням ``wasm32-unknown-unknown``. 
Однак навіть при компіляції з ``--release`` результуюча збірка включає великі розділи налагоджувальної інформації, які не приносять користі для смарт-контрактів в мережі.

Щоб оптимізувати збірку і врахувати нові функції, такі як вбудовування схем, ми рекомендуємо використовувати ``cargo-concordium`` для створення смарт-контрактів. 

.. seealso::

   For instructions on how to build using ``cargo-concordium`` see
   :ref:`compile-module`.


Тестування смарт-контрактів 
=======================

Unit тести з заглушками
---------------------

Моделювання виклику контракту 
-----------------------

Найкращі практики
==============

Don't panic
-----------

.. todo::

   Use trap instead.

Уникайте появи чорних дір 
--------------------------

Смарт-контракт не зобов'язаний використовувати кількість відправлених йому GTU, і за замовчуванням смарт-контракт не визначає ніякого поведінки для спустошення балансу примірника, якщо хтось мав відправити йому якесь GTU.
Ці GTU були б тоді назавжди втрачені, і не було б ніякого способу відновити їх.

Тому гарною практикою для смарт-контрактів, які не мають справи з GTU, є забезпечення того, щоб відправлена сума GTU дорівнювала нулю, і відхилення будь-яких викликів, які не є такими. 

Переміщення важких обчислень поза мережею
---------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Get: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Read: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/
