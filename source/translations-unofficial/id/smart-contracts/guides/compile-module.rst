.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rust-analyzer: https://github.com/rust-analyzer/rust-analyzer

.. _compile-module-id:

====================================
Menyusun modul smart contract Rust
====================================

Panduan ini akan menunjukkan kepada Anda cara menyusun modul smart contract yang
ditulis dalam Rust ke modul Wasm.

Persiapan
===========

Pastikan untuk menginstal Rust,Cargo dan target ``wasm32-unknown-unknown``,
bersama dengan ``cargo-concordium`` dan kode sumber rust untuk module
smart contract module, yang ingin anda susun.

.. seealso::

   Untuk instruksi tentang cara memasang alat pengembang, lihat
   :ref:`setup-tools-id`.

Menyusun ke Wasm
=================

Untuk membantu membangun modul smart contract dan memanfaatkan fitur
seperti :ref:`skema kontrak <contract-schema-id>`, kami merekomdasikan
untuk menggunakan alat ``cargo-concordium`` untuk membangun Rust_ smart contracts.

untuk menmbangun sebuah smart contract, jalankan:

.. code-block:: console

   $cargo concordium build

Ini meggunakan Cargo_ untuk membangun, tetapi menjalankan pengoptimalan lebih lanjut pada hasil.

.. seealso::

   Untuk membuat skema untuk modul kontrak pintar, beberapa :ref: `diperlukan
   persiapan lebih lanjut <build-schema-id>`.

.. note::

   Juga dimungkinkan untuk mengkompilasi menggunakan Cargo_ secara langsung dengan menjalankan:

   .. code-block:: console

      $cargo build --target=wasm32-unknown-unknown [--release]

   Perhatikan bahwa bahkan dengan set ``--release``, modul Wasm yang dihasilkan menyertakan
   informasi debug.

Menghapus informasi host dari build
====================================

Modul Wasm yang dikompilasi dapat berisi informasi dari mesin host yang
membangun biner; informasi seperti jalur absolut dari direktori ``.cargo``.

Bagi kebanyakan orang ini bukan informasi sensitif, tetapi penting
untuk menyadarinya.

Di Linux, jalur dapat diperiksa dengan menjalankan:

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: The solution

Solusi yang ideal adalah menghapus jalur ini seluruhnya, tetapi sayangnya
itu bukan tugas yang sepele pada umumnya.

Ada kemungkinan untuk mengatasi masalah ini dengan menggunakan flag ``--remap-path-prefix``
saat menyusun kontrak.
Pada sistem Unix-like flag dapat diteruskan langsung ke pemanggilan ``cargo concordium``
menggunakan variabel lingkungan ``RUSTFLAGS``:

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

Yang akan menggantikan jalur beranda pengguna dengan string kosong. Jalur lain
dapat dipetakan dengan cara yang sama. Secara umum menggunakan ``--remap-path-prefix=from=to``
akan memetakan ``from`` to ``to`` di awal setiap jalur yang disematkan.

flag juga dapat di set secara permanen di dalam file ``.cargo/config`` di kotak anda
dibawah bagian build:

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

di mana `<user>` harus diganti dengan pengguna yang membuat modul wasm.

Peringatan
----------

Hal di atas kemungkinan besar tidak akan memperbaiki masalah jika komponen
``rust-src`` dipasang untuk toolchain Rust. Komponen ini dibutuhkan oleh beberapa
alat Rust seperti alat rust-analyzer.

.. seealso::

   Masalah dengan ``--remap-path-prefix`` dan ``rust-src`` dapat dilihat di
   https://github.com/rust-lang/rust/issues/73167
