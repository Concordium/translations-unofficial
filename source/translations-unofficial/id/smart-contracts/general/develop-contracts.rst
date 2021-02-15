.. Should answer:
    - Why write a smart contract using rust?
    - What are the pieces needed to write a smart contract in rust?
        - State
            - Serialized
            - Schema
        - Init
        - Receive
    - What sort of testing is possible
    - Best practices?
        - Ensure 0 amount
        - Don't panic
        - Avoid heavy calculations

.. _writing-smart-contracts-id:

====================================
Mengembangkan smart contract di Rust
====================================

Pada blockchain concordium, smart contract diterapkan sebagai modul Wasm, tetapi
Wasm dirancang terutama sebagai target kompilasi dan tidak nyaman untuk
ditulis dengan tangan.
Sebaliknya, kami dapat menulis smart contract kami dalam bahasa pemrograman Rust_,
yang memiliki dukungan yang baik untuk mengompilasi ke Wasm.

smart contract tidak harus ditulis dengan Rust.
Ini hanyalah SDK pertama yang kami sediakan.
Wasm yang ditulis secara manual, atau Wasm yang dikompilasi dari C, C++, AssemblyScript_, dan
lainnya, sama-sama valid pada rantai tersebut, asalkan mengikuti :ref:`Batasan
Wasm yang kami terapkan <wasm-limitations-id>`.

.. seealso::

   Untuk informasi lebih lanjut tentang fungsi yang dijelaskan di bawah, lihat concordium_std_
   API untuk menulis smart contracts blockcahin Concordium di bahasa pemrograman Rust.

.. seealso::

   Lihat :ref:`contract-module-id` untuk info lebih lanjut tentang modul smart contract.

Modul smart contract dikembangkan di Rust sebagai kotak library, yang kemudian
dikompilasi ke Wasm.
Untuk mendapatkan ekspor yang benar, atribut `crate-type` harus disetel ke
``["cdylib", "rlib"]`` di dalam file manifes:

.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...

Menulis smart contract menggunakan ``concordium_std``
===================================================

Direkomendasikan untuk menggunakan kotak ``concordium_std``, yang menyediakan
lebih banyak pengalaman Rust-like untuk mengembangkan modul smart contract dan memanggil
fungsi host.

Kotak memungkinkan menulis init dan fungsi penerima sebagai Rust sederhana
fungsi yang dianotasi dengan ``#[init(...)]`` dan ``#[receive(...)]``, berurutan.

Berikut adalah contoh smart contract yang menerapkan penghitungan:

.. code-block:: rust

   use concordium_std::*;

   type State = u32;

   #[init(contract = "counter")]
   fn counter_init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let state = 0;
       Ok(state)
   }

   #[receive(contract = "counter", name = "increment")]
   fn contract_receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       ensure!(ctx.sender().matches_account(&ctx.owner()); // Only the owner can increment
       *state += 1;
       Ok(A::accept())
   }

Ada beberapa hal yang perlu diperhatikan:

.. todo::

   - Tuliskan persyaratan dengan cara yang lebih mudah dibaca (misalnya, bagi paragraf menjadi sub-poin).
   - Persyaratan ini harus menjadi bagian dari spesifikasi yang tertulis di suatu tempat,
     yaitu, bukan hanya sebagai bagian dari contoh ini.

- Jenis fungsinya:

  * Sebuah fungsi init harus bertipe ``&impl HasInitContext -> InitResult<MyState>``
    Dimana ``MyState`` adalah tipe yang mengimplementasikan sifat ``Serialize``.
  * Fungsi terima harus mengambil sebuah tipe parameter ``A: HasActions``,
    sebuah ``&impl HasReceiveContext`` dan sebuah ``&mut MyState`` parameter, dan menghasilkan
    sebuah ``ReceiveResult<A>``.

- Anotasi ``#[init(contract = "counter")]`` menandai fungsi yang diterapkan
  sebagai fungsi init dari kontrak bernama ``counter``.
  Secara konkret, ini berarti bahwa di balik layar, makro ini menghasilkan fungsi
  yang diekspor dengan tanda tangan dan nama yang diperlukan ``init_counter``.

- ``#[receive(contract = "counter", name = "increment")]`` deserializes dan
  memasok status untuk dimanipulasi secara langsung.
  Di balik layar, anotasi ini juga menghasilkan fungsi yang diekspor dengan nama
  ``counter.increment`` yang memiliki tanda tangan yang diperlukan, dan melakukan
  semua boilerplate dari deserialisasi status ke dalam tipe ``State`` yang diperlukan.

.. note::

   Perhatikan bahwa deserialisasi bukannya tanpa biaya, dan dalam beberapa kasus,
   pengguna mungkin menginginkan kontrol yang lebih baik atas penggunaan fungsi host.
   Untuk kasus penggunaan seperti itu, anotasi mendukung opsi ``low_level``, yang memiliki
   lebih sedikit overhead, tetapi membutuhkan lebih banyak dari pengguna.

.. todo::

   - Menggambarkan low-level
   - Perkenalkan konsep fungsi host sebelum menggunakannya dalam catatan di atas


Status dan parameter yang dapat diserialkan
-------------------------------------------

.. todo:: Memperjelas apa artinya status diekspos dengan cara yang mirip dengan ``File``;
   sebaiknya, tanpa merujuk ke ``File``.

On-chain, status instance direpresentasikan sebagai array byte dan diekspos
dalam antarmuka yang mirip dengan antarmuka ``File`` dari pustaka standar Rust.

Hal ini dapat dilakukan dengan menggunakan ``Serialize`` sifat yang berisi fungsi
(de-)serialisasi.

Kotak ``concordium_std`` menyertakan sifat ini dan penerapannya untuk
kebanyakan tipe di pustaka standar Rust.
Ini juga mencakup makro untuk mendapatkan sifat untuk struct dan enum yang ditentukan
pengguna.

.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

Hal yang sama diperlukan untuk parameter init dan fungsi terima.

.. note::

   Sebenarnya kita hanya perlu melakukan deserialisasi byte ke tipe parameter kita,
   tetapi akan lebih mudah untuk dapat membuat serialisasi tipe saat menulis pengujian unit.

.. _working-with-parameters-id:

Bekerja dengan parameter
------------------------

Parameter untuk fungsi init dan terima adalah, sama seperti status
instance, deserialisasikan sebagai byte arrays.
Meskipun array byte dapat digunakan secara langsung, mereka juga dapat dideserialisasi menjadi
data terstruktur.

Cara termudah untuk melakukan deserialisasi parameter adalah melalui fungsi `get()`_
dari sifat `Get`_.

Sebagai contoh, lihat kontrak berikut di mana parameter
``ReceiveParameter`` di-deserialisasi pada baris yang disorot:

.. code-block:: rust
   :emphasize-lines: 24

   use concordium_std::*;

   type State = u32;

   #[derive(Serialize)]
   struct ReceiveParameter{
       should_add: bool,
       value: u32,
   }

   #[init(contract = "parameter_example")]
   fn init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let initial_state = 0;
       Ok(initial_state)
   }

   #[receive(contract = "parameter_example", name = "receive")]
   fn receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let parameter: ReceiveParameter = ctx.parameter_cursor().get()?;
       if parameter.should_add {
           *state += parameter.value;
       }
       Ok(A::accept())
   }

Fungsi terima di atas tidak efisien dalam hal deserialisasi
``value`` bahkan ketika tidak diperlukan, yaitu, ketika ``should_add`` adalah ``false``.

Untuk mendapatkan lebih banyak kendali, dan dalam hal ini, lebih efisien, kitda dapat 
deserialisasi parameter menggunakan sifat `Read`_:

.. code-block:: rust
   :emphasize-lines: 7, 10

   #[receive(contract = "parameter_example", name = "receive_optimized")]
   fn receive_optimized<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let mut cursor = ctx.parameter_cursor();
       let should_add: bool = cursor.read_u8()? != 0;
       if should_add {
           // Only decode the value if it is needed.
           let value: u32 = cursor.read_u32()?;
           *state += value;
       }
       Ok(A::accept())
   }

Perhatikan bahwa ``value`` hanya dideserialisasi jika ``should_add`` adalah
``true``.
Meskipun peningkatan efisiensi minimal dalam contoh ini, namun dapat berdampak
besar untuk contoh yang lebih kompleks.


Membangun modul smart contract dengan ``cargo-concordium``
==========================================================

Kompilator Rust memiliki dukungan yang baik untuk mengkompilasi ke Wasm menggunakan
target ``wasm32-unknown-unknown``.
Namun, bahkan ketika mengompilasi dengan ``--release`` build yang dihasilkan menyertakan
menyertakan bagian besar informasi debug di bagian kustom, yang tidak berguna untuk
smart contracts on-chain.

Untuk mengoptimalkan build dan memungkinkan fitur baru seperti skema embedding, kami
merekomendasi menggunakan ``cargo-concordium`` untuk membuat smart contracts.

.. seealso::

   Untuk instruksi tentang cara membangun menggunakan ``cargo-concordium`` lihat
   :ref:`compile-module`.


Menguji smart contracts
=======================

Tes unit dengan rintisan
------------------------

Mensimulasikan panggilan kontrak
--------------------------------

Praktik terbaik
===============

Jangan panik
------------

.. todo::

   Gunakan trap sebagai gantinya.

Hindari membuat black holes
---------------------------

Smart contract tidak diperlukan untuk menggunakan jumlah GTU yang dikirim ke sana, dan secara
default smart contract tidak menentukan perilaku apa pun untuk mengosongkan saldo
dari sebuah instance, jika seseorang mengirim beberapa GTU.
GTU ini akan selamanya *hilang*, dan tidak ada cara untuk memulihkannya.

Oleh karena itu, praktik yang baik untuk menerapkan smart contract yang tidak berhubungan dengan GTU,
untuk memastikan jumlah GTU yang dikirim adalah selalu nol dan menolak perminataan yang tidak
memenihi ketentuan ini.

Pindahkan kalkulasi berat ke off-chain
--------------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Get: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Read: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/
