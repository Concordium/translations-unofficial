.. _setup-tools:

=====================================
Установка инструментов для разработки
=====================================

Прежде чем мы сможем начать разработку смарт-контрактов, нам нужно настроить
среду.

Rust и Cargo
============

Во-первых, `install rustup`_, который установит Rust_ и Cargo_ на вашу
машину.
Затем используйте ``rustup`` для установки Wasm, которая используется для компиляции:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium это инструмент для разработки смарт-контрактов для блокчейна
Concordium.
Он может быть использован для :ref:`compiling<compile-module>` и
:ref:`testing<unit-test-contract>` смарт-контрактов, и включает такие функции,
как :ref:`building contract schemas<build-schema>`.

.. todo::

   Add links for testing and schemas.

Cargo Concordium поставляется как часть пакета :ref:`Concordium software<downloads>`.
Инструмент должен быть помещен в ваш PATH.

Для описания того, как использовать Cargo Concordium, выполните:

.. code-block:: console

   $cargo concordium --help

Программное обеспечение Concordium
==================================

Инструмент для развертывания и взаимодействия со смарт-контрактами
:ref:`concordium-client<concordium_client>`. Он распространяется как
часть пакета :ref:`Concordium software<downloads>`.

.. note::

   Чтобы развернуть модули смарт-контрактов и взаимодействовать с сетью,
   убедитесь, что вы :ref:`running a node<run-a-node>`.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _install rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
