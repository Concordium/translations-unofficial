.. _list of types implementing the SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schema-id:

=======================
Membuat skema kontrak
=======================

Panduan ini akan menunjukkan cara membuat skema smart contract, cara mengekspornya
ke dalam file, dan/atau menyematkan skema ke modul smart contarct, semuanya menggunakan
``cargo-concordium``.

Persiapan
===========

Pertama, pastikan anda telah menginstal ``cargo-concordium`` dan jika belum, petunjuk ini
:ref:`setup-tools-id` akan membantu anda menginstalnya.

Kita juga membutuhkan Rust source code dari smart contract yang ingin anda buat
skemanya.

Siapkan kontrak untuk skema
===============================

Untuk membuat skema kontrak, pertama-tama kita harus menyiapkan smart
contract untuk membuat skema.

Kita dapat memilih bagian mana dari smart contract yang akan disertakan dalam skema.
Opsinya adalah menyertakan skema untuk status kontrak, dan/atau untuk setiap
parameter init dan fungsi terima.

Setiap jenis yang ingin kita sertakan dalam skema harus mengimplementasikan sifat ``SchemaType``.
Ini sudah dilakukan untuk semua tipe dasar dan beberapa tipe lainnya (lihat `daftar tipe yang menerapkan SchemaType`_).
Untuk sebagian besar kasus lainnya, ini juga dapat dicapai secara otomatis, menggunakan
``#[derive(SchemaType)]``::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

Mengimplementasikan sifat ``SchemaType`` secara manual hanya membutuhkan satu
fungsi, yang merupakan getter untuk ``schema::Type``, yang pada dasarnya menjelaskan
bagaimana tipe ini direpresentasikan sebagai byte dan bagaimana merepresentasikannya.

.. todo::

   Buat contoh yang menunjukkan cara mengimplementasikan ``SchemaType`` secara manual
   dan tautkan dari sini.

Menyertakan status kontrak
---------------------------

Untuk membuat dan menyertakan skema untuk status kontrak, kita menganotasi tipe
dengan makro ``#[contract_state(contract = ...)]`` ::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

Atau bahkan lebih sederhana lagi jika kondisi kontrak termasuk dalam tipe yang sudah diimplementasikan ``SchemaType``, misalnya, u32::

   #[contract_state(contract = "my_contract")]
   type State = u32;

Menyertakan parameter fungsi
-----------------------------

Untuk menghasilkan dan menyertakan skema parameter untuk init dan
Fungsi terima, kami mengatur opsional ``parameter`` atribut untuk
``#[init(..)]``- dan ``#[receive(..)]``-makro::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }

Membangun skema
===================

Sekarang, kita siap untuk membangun skema yang sebenarnya menggunakan ``cargo-concordium``, dan kita
memiliki opsi untuk menyematkan skema dan/atau menulis skema ke dalam file.

.. seealso::

   Untuk lebih lanjut tentang apa yang harus dipilih, lihat
   :ref:`disini<contract-schema-which-to-choose-id>`.

Menyematkan skema
--------------------

Untuk menyematkan skema ke modul smart contract, kami menambahkan
``--schema-embed`` ke perintah build

.. code-block:: console

   $cargo concordium build --schema-embed

Jika berhasil, output perintah akan memberi tahu Anda ukuran total
skema dalam byte.

Menampilkan skema ke file
--------------------------

Untuk menampilkan skema menjadi file, kita dapat menggunakan ``--schema-out=FILE``
di mana ``FILE`` adalah jalur file yang akan dibuat:

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
