.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rust-analyzer: https://github.com/rust-analyzer/rust-analyzer

.. _compile-module:

====================================
Компілювання Rust модуля смарт-контракту 
====================================

Це керівництво покаже вам, як скомпілювати модуль смарт-контракту, написаний на Rust, в модуль Wasm. 

Підготовка
===========

Переконайтеся, що встановлені Rust і Cargo, а також ``wasm32-unknown-unknown``, разом з c``cargo-concordium`` і вихідним кодом Rust для модуля смарт-контракту, який ви хочете скомпілювати. 

.. seealso::

   For instructions on how to install the developer tools see
   :ref:`setup-tools`.

Компіляція в Wasm 
=================

Щоб допомогти в створенні модулів смарт-контрактів і скористатися перевагами таких функцій, як :ref:`contract schemas <contract-schema>`, ми рекомендуємо використовувати інструмент ``cargo-concordium`` для створення смарт-контрактів Rust_.

Для того щоб зібрати смарт-контракт, запустіть: 

.. code-block:: console

   $cargo concordium build

Це використовує Cargo_ для збірки, але запускає подальшу оптимізацію результату. 

.. seealso::

   For building the schema for a smart contract module, some :ref:`further
   preparation is required <build-schema>`.

.. note::

   Також можна скомпілювати за допомогою Cargo_ безпосередньо, запустивши: 

   .. code-block:: console

      $cargo build --target=wasm32-unknown-unknown [--release]

   Зверніть увагу, що навіть з -``--release`` створений модуль Wasm включає в себе зневадження. 

Видалення інформації про вузол з збірки 
====================================

Скомпільований модуль Wasm може містити інформацію від хост-машини, що створює двійковий файл; таку інформацію, як абсолютний шлях до каталогу ``.cargo``.

Для більшості людей це не секретна інформація, але ви повинні знати про цей момент.

В Linux можна перевірити, запустивши: 

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: Рішення

Ідеальним рішенням було б повністю видалити цей шлях, але це, на жаль, не тривіальна задача в цілому.

Цю проблему можна обійти, використовуючи прапор ``--remap-path-prefix`` при компіляції контракту.
В Unix-подібних системах прапор може бути переданий безпосередньо ``cargo concordium`` за допомогою змінної оточення ``RUSTFLAGS``: 

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

Це замінить домашній шлях користувача символом нового рядка. Інші шляхи могли бути відображено аналогічним чином. Зазвичай використовується ``--remap-path-prefix=from=to`` відображатиме ``from`` до ``to`` на початку будь-якого вбудованого шляху.

Прапор також може бути постійно встановлений у файлі ``.cargo/config``, в розділі build: 

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

где `<user>` должен быть заменен пользователем, создающим модуль wasm.

Застереження
-------

Наведене вище, швидше за все, все ще виникають проблеми, якщо компонент ``rust-src`` встановлений для Rust набору інструментів. Цей компонент потрібно деяким інструментам Rust, таким як rust-analyzer_. 

.. seealso::

   An issue reporting the problem with ``--remap-path-prefix`` and ``rust-src``
   https://github.com/rust-lang/rust/issues/73167
