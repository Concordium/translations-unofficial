.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rust-analyzer: https://github.com/rust-analyzer/rust-analyzer

.. _compile-module-ru:

==========================================
Компилирование Rust модуля смарт-контракта
==========================================

Это руководство покажет вам, как скомпилировать модуль смарт-контракта,
написанный на Rust, в модуль Wasm.

Подготовка
===========

Убедитесь, что установлены Rust и Cargo, а также ``wasm32-unknown-unknown``,
вместе с ``cargo-concordium`` и исходным кодом Rust для модуля смарт-контракта,
который вы хотите скомпилировать.

.. seealso::

   For instructions on how to install the developer tools see
   :ref:`setup-tools-ru`.

Компиляция в Wasm
=================

Чтобы помочь в создании модулей смарт-контрактов и воспользоваться преимуществами
таких функций, как :ref:`схема смарт-контракта <contract-schema-ru>`, мы рекомендуем
использовать инструмент ``cargo-concordium`` для создания смарт-контрактов Rust_.

Для того чтобы собрать смарт-контракт, запустите:

.. code-block:: console

   $cargo concordium build

Это использует Cargo_ для сборки, но запускает дальнейшую оптимизацию результата.

.. seealso::

   For building the schema for a smart contract module, some :ref:`further
   preparation is required <build-schema-ru>`.

.. note::

   Также можно скомпилировать с помощью Cargo_ напрямую, запустив:

   .. code-block:: console

      $cargo build --target=wasm32-unknown-unknown [--release]

   Обратите внимание, что даже с ``--release`` созданный модуль Wasm включает
   в себя отладочную информацию.

Удаление информации о хосте из сборки
=====================================

Скомпилированный модуль Wasm может содержать информацию от хост-машины, создающей
двоичный файл; такую информацию, как абсолютный путь к каталогу ``.cargo``.

Для большинства людей это не секретная информация, но вы должны знать об этом
моменте.

В Linux можно проверить, запустив:

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: Решение

Идеальным решением было бы полностью удалить этот путь, но это, к сожалению,
не тривиальная задача в целом.

Эту проблему можно обойти, используя флаг ``--remap-path-prefix``
при компиляции контракта.
В Unix-подобных системах флаг может быть передан непосредственно
``cargo concordium`` с помощью переменной окружения ``RUSTFLAGS``:

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

Это заменит домашний путь пользователя пустой строкой. Другие пути могли
быть отображено аналогичным образом. Обычно используется ``--remap-path-prefix=from=to``
будет отображать ``from`` к ``to`` в начале любого встроенного пути.

Флаг также может быть постоянно установлен в файле ``.cargo/config``,
в разделе build:

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

где `<user>` должен быть заменен пользователем, создающим модуль wasm.

Предостережения
---------------

Приведенное выше, скорее всего, не устранит проблему, если компонент ``rust-src``
установлен для Rust набора инструментов. Этот компонент требуется некоторым
инструментам Rust, таким как rust-анализатор.

.. seealso::

   An issue reporting the problem with ``--remap-path-prefix`` and ``rust-src``
   https://github.com/rust-lang/rust/issues/73167
