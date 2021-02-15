.. _unit-test-contract-id:

============================
Unit menguji kontrak di Rust
============================

Panduan ini akan menunjukkan kepada Anda cara menulis pengujian unit untuk kontrak pintar yang ditulis dalam
Rust.
Untuk menguji modul Wasm smart contract, lihat :ref:`simulasi lokal dari fungsi kontrak<local-simulate-id`.

Smart contract di Rust ditulis sebagai library dan kita dapat mengujinya seperti library
dengan menganotasi fungsi dengan atribut ``#[test]``.

.. code-block:: rust

    // contract code
    ...

    #[cfg(test)]
    mod test {

        #[test]
        fn some_test() { ... }

        #[test]
        fn another_test() { ... }
    }

Menjalankan tes bisa dilakukan dengan menggunakan ``cargo``:

.. code-block:: console

   $cargo test

Secara default, perintah ini mengompilasi kontrak dan menguji kode perangkat
untuk target lokal Anda (kemungkinan besar ``x86_64``), dan menjalankannya.
Jenis pengujian ini dapat berguna dalam pengembangan awal dan untuk menguji
ketepatan fungsional.
Untuk pengujian komprehensif, penting untuk melibatkan platform target, yaitu,
`Wasm32`.
Ada sejumlah perbedaan halus antar platform, yang dapat mengubah perilaku kontrak.
Satu perbedaan adalah mengenai ukuran pointer, di mana `Wasm32` menggunakan empat
byte sebagai lawan delapan, yang umum untuk sebagian besar platform.

Menulis tes unit
==================

unit test biasanya mengikuti struktur tiga bagian di mana Anda: menyiapkan beberapa
status, menjalankan beberapa unit kode, dan membuat pernyataan tentang keadaan dan hasil
dari kode tersebut.

Jika fungsi kontrak ditulis menggunakan ``#[init(..)]`` atau
``#[receive(..)]``, kita dapat menguji fungsi ini secara langsung di unit test.

.. code-block:: rust

   use concordium_std::*;

   #[init(contract = "my_contract", payable, enable_logger)]
   fn contract_init(
      ctx: &impl HasInitContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
   ) -> InitResult<State> { ... }

   #[receive(contract = "my_contract", name = "my_receive", payable, enable_logger)]
   fn contract_receive<A: HasActions>(
      ctx: &impl HasReceiveContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
      state: &mut State,
   ) -> ReceiveResult<A> { ... }

Rintisan pengujian untuk argumen fungsi dapat ditemukan di submodul dari
``concordium-std`` yang disebut ``test_infrastructure``.

.. seealso::

   Untuk informasi dan contoh lebih lanjut, lihat dokumentasi peti
   concordium-std.

.. todo::

   Tunjukkan lebih banyak tentang bagaimana menulis tes unit

Menjalankan tes di Wasm
========================

Mengompilasi pengujian ke kode perangkat native sudah cukup untuk sebagian besar kasus,
tetapi juga memungkinkan untuk mengompilasi pengujian ke Wasm dan menjalankannya
menggunakan interpreter yang tepat, yang digunakan oleh node.
Hal ini membuat lingkungan pengujian lebih dekat dengan lingkungan on-chain dan dalam beberapa kasus
dapat menangkap lebih banyak bug.

Alat pengembangan ``cargo-concordium`` menyertakan penguji untuk Wasm, yang
menggunakan Wasm-interpreter yang sama dengan yang dikirim dalam Concordium node.

.. seealso::

   Untuk pentunjuk penginstalan ``cargo-concordium``, lihat :ref:`setup-tools-id`.

Pengujian unit harus dianotasi dengan ``#[concordium_test]`` daripada
``#[test]``, dan kami menggunakan ``#[concordium_cfg_test]`` daripada ``#[cfg(test)]``:

.. code-block:: rust

   // contract code
   ...

   #[concordium_cfg_test]
   mod test {

       #[concordium_test]
       fn some_test() { ... }

       #[concordium_test]
       fn another_test() { ... }
   }

Makro ``#[concordium_test]`` menyiapkan pengujian kami untuk dijalankan di Wasm, ketika
``concordium-std`` dikompilasi dengan fitur ``wasm-test``, dan sebaliknya
kembali berperilaku seperti ``#[test]``, artinya masih mungkin untuk menjalankan pengujian
unit yang menargetkan kode native menggunakan ``cargo test``.


Demikian pula makro ``#[concordium_cfg_test]`` menyertakan modul kami saat membangun
``concordium-std`` dengan ``wasm-test`` jika tidak berperilaku seperti ``#[test]``,
memungkinkan kami untuk mengontrol kapan harus menyertakan tes kedalamnya.

Pengujian sekarang dapat dibuat dan dijalankan menggunakan:

.. code-block:: console

   $cargo concordium test

Perintah ini menyusun pengujian untuk Wasm dengan fitur ``wasm-test`` yang diaktifkan
untuk ``concordium-std`` dan menggunakan penguji dari ``cargo-concordium``.

.. warning::

   Pesan kesalahan dari ``panic!``, Dan oleh karena itu, variasi yang berbeda
   dari ``assert!``, *Tidak* ditampilkan saat mengompilasi ke Wasm.

   Alih-alih,menggunakan varian ``fail!`` Dan ``claim!`` Untuk melakukan pernyataan saat
   pengujian, karena laporan ini mengembalikan pesan kesalahan ke penguji *sebelum*
   gagal dalam pengujian.
   Keduanya adalah bagian dari ``concordium-std``.

.. todo::

   Gunakan tautan concordium-std: docs.rs/concordium-std saat peti diterbitkan.
