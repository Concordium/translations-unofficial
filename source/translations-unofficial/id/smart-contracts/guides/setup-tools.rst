.. _setup-tools-id:

===============================
Instal alat untuk pengembangan
===============================

Sebelum kita dapat mulai mengembangkan smart contract, kita perlu menyiapkan
lingkungan.

Rust dan Cargo
==============

Pertama, `instal rustup`_, yang akan menginstal keduanya, Rust_ and Cargo_ di perangkat
anda.
Kemudian gunakan ``rustup`` untuk menginstal target Wasm, yang digunakan untuk penyusunan:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium adalah alat untuk mengembangkan kontrak pintar untuk blockchain
Concordium.
ini dapat digunakan untuk :ref:`penyusunan<compile-module-id>` dan
:ref:`testing<unit-test-contract-id>` smart contract, dan mengaktifkan fitur seperti
:ref:`membangun skema kontrak<build-schema-id>`.

.. todo::

   Tambahkan tautan untuk pengujian dan skema.

Cargo Concordium didistribusikan sebgai bagian dari paket :ref:`Concordium software<downloads>`.
Alat tersebut harus ditempatkan di PATH Anda.

Untuk penjelasan tentang bagaimana menggunakan Cargo Concordium jalankan:

.. code-block:: console

   $cargo concordium --help

Concordium software
===================

Alat untuk mnerapkan dan berinteraksi dengan smart contract ialah
:ref:`concordium-client<concordium_client>`. didistribusikan sebagai bagian dari
paket :ref:`Concordium software<downloads>`.

.. note::

   Untuk menerapkan modul smart contract dan berinteraksi dengan rantai, pastikan
   bahwa anda :ref:`menjalankan node<run-a-node>`.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _instal rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
