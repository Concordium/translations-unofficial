.. highlight:: toml

.. _setup-contract-id:

===================================
Menyiapkan proyek smart contract
===================================

Smart contract Rust ditulis sebagai peti library Rust biasa.
library tersebut kemudian dikompilasi ke Wasm menggunakan target Rust
``wasm32-unknown-unknown`` dan, karena ini hanya library Rust, kita dapat menggunakan
Cargo_ untuk manajemen dependency.

Untuk menyiapkan proyek smart contract baru, pertama-tama buat direktori proyek. Dalam
direktori proyek jalankan perintah berikut ini di terminal:

.. code-block:: console

   $cargo init --lib

Ini akan menyiapkan proyek library Rust default dengan membuat beberapa file dan
direktori.
Direktori Anda sekarang harus berisi file ``Cargo.toml`` dan ``src``
direktori dan beberapa file tersembunyi.

Untuk bisa membangun Wasm kita perlu memberi tahu kargo yang benar ``crate-type``.
Ini dilakukan dengan menambahkan yang berikut ini di file ``Cargo.toml``::

   [lib]
   crate-type = ["cdylib", "rlib"]

Menambahkan library standar smart contract
==========================================

Langkah selanjutnya adalah menambahkan ``concordium-std`` sebagai dependensi.
Ini adalah library untuk Rust yang berisi makro prosedural dan fungsi untuk
menulis kontrak pintar yang kecil dan efisien.

library ditambahkan dengan membuka ``Cargo.toml`` dan menambahkan baris
``concordium-std = "*"`` (preferably, replace the `*` with the latest version of `concordium-std`_) in
the ``[dependencies]`` section(sebaiknya, ganti `*` dengan versi terbaru dari `concordium-std`_) di
bagian ``[dependencies]``::

   [dependencies]
   concordium-std = "0.4"

Dokumentasi peti dapat ditemukan di docs.rs_.

.. note::

   Jika Anda ingin menggunakan versi modifikasi dari peti ini, Anda harus mengklon
   repositori dengan ``concordium-std`` dan memiliki titik dependency
   di direktori, dengan menambahkan baris berikut ke dalam ``Cargo.toml``::

      [dependencies]
      concordium-std = { path = "./path/to/concordium-std" }

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rustup: https://rustup.rs/
.. _repository: https://gitlab.com/Concordium/concordium-std
.. _docs.rs: https://docs.rs/crate/concordium-std/
.. _`concordium-std`: https://docs.rs/crate/concordium-std/

Hanya itu saja! Anda sekarang siap untuk mengembangkan smart contract Anda sendiri.
