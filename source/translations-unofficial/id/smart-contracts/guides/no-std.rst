.. _no-std-id:

=============================
Bangun menggunakan ``no_std``
=============================

Panduan ini menunjukkan cara mengaktifkan ``no_std`` untuk smart contract rust Anda,
berpotensi mengurangi ukuran modul Wasm yang dihasilkan beberapa kilobyte.

Persiapan
===========

Menyuasun ``concordium-std`` tanpa membutuhkan fitur ``std`` menggunakan rust
nightly toolchain, yang dapat diinstal menggunakan ``rustup``:

.. code-block:: console

   $rustup toolchain install nightly

Mensetting modul untuk ``no_std``
====================================

library ``concordium-std`` memperlihatkan fitur ``std``, yang memungkinkan
penggunaan library standar rust.
Fitur ini diaktifkan secara default.

Untuk menonaktifkannya, seseorang hanya perlu menonaktifkan fitur default untuk
``concordium-std`` di dependensi modul Anda.

.. code-block:: rust

   [dependencies]
   concordium-std = { version: "=0.2", default-features = false }

Untuk dapat beralih antara dengan dan tanpa std, tambahkan juga ``std`` ke modul
anda sendiri, yang mengaktifkan fitur ``std`` dari ``concordium-std``:

.. code-block:: rust

   [features]
   std = ["concordium-std/std"]

Ini adalah pengaturan dari contoh smart contract, dimana ``std`` untuk setiap
modul smart contract diaktifkan secara default.

Membangun modul
===================

Untuk menggunakan nightly toolchain, tambahkan ``+nightly`` tepat setelah
``cargo``:

.. code-block:: console

   $cargo +nightly concordium build

Jika Anda ingin menonaktifkan fitur default dari modul smart contract anda sendiri,
Anda dapat memberikan argumen tambahan untuk ``cargo``:

.. code-block:: console

   $cargo +nightly concordium build -- --no-default-features
