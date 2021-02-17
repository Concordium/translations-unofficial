.. _setup-tools:

======================================
Установка инструментов для разработки
======================================

До того, как мы приступим к разработке смарт-контрактов, нам потребуется настроить среду.

Rust and Cargo
==============

Сначала, `install rustup`_, который установит Rust_ и Cargo_ на Вашей машине. Затем используйте ``rustup`` для установки цели Wasm, которая используется для компиляции:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium – это утилита для разработки смарт-контрактов для блокчейна Concordium. Она может быть использована для :ref:`компиляции<compile-module>` и
:ref:`тестирования<unit-test-contract>` смарт-контрактов, и активирует дополнительные возможности, такие как
:ref:`сборка схемы контракта<build-schema>`.

.. todo::

   Добавьте ссылки для тестирования и схемы.

Cargo Concordium распространяется как часть пакета :ref:`Concordium software<downloads>`.
Эта утилита должна быть прописана в PATH.

Для получения инструкций по использованию Cargo Concordium выполните команду:

.. code-block:: console

   $cargo concordium --help

Concordium software
===================

Утилита :ref:`concordium-client<concordium_client>` предназначена для развёртывания и взаимодействия со смарт-контрактами.
Она распространяется как часть пакета :ref:`Concordium software<downloads>` package.

.. note::

   Для развёртывания модулей смарт-контракта и взаимодействия с цепочкой, убедитесь что :ref:`запущена нода<run-a-node>`.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _install rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
