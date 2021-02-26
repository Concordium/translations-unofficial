.. _setup-tools-uk:

===================================
Установка інструментів для розробки
===================================

Перш ніж ми зможемо почати розробку смарт-контрактів, нам потрібно налаштувати середовище.

Rust і Cargo
============

По-перше, `install rustup`_, який встановить Rust_ і Cargo_ на вашу машину.
Потім використовуйте ``rustup`` для установки Wasm, яка використовується для компіляції:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium це інструмент для розробки смарт-контрактів для блокчейна Concordium.
Він може бути використаний для :ref:`компіляції <compile-module-uk>` і :ref:`тестування <unit-test-contract-uk>` смарт-контрактів, і включає такі функції, як :ref:`побудова схем смарт-контрактів <build-schema-uk>`.

.. todo::

   Додайте посилання на тестування та схеми.

Cargo Concordium поставляється як частина пакета :ref:`програмне забезпечення Concordium <downloads>`.
Інструмент повинен бути доданий в PATH.

Для опису того, як використовувати Cargo Concordium, виконайте:

.. code-block:: console

   $cargo concordium --help

Програмне забезпечення Concordium
=================================

Інструмент для розгортання та взаємодії зі смарт-контрактами :ref:`concordium-client <concordium_client>`.
Він поширюється як частина пакету :ref:`програмне забезпечення Concordium <downloads>`.

.. note::

   Щоб розгорнути модулі смарт-контрактів і взаємодіяти з мережею, переконайтеся, що ви :ref:`запустили ноду <run-a-node-uk>`.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _install rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
