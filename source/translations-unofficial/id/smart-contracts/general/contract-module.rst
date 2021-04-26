.. _contract-module-id:

======================
Modul smart contract
======================

smart contract diterapkan pada rantai dalam *modul smart contract*.

.. note::

   Sebuah Modul smart contract sering disebut hanya sebagai *modul*.

Sebuah modul dapat berisi satu atau lebih smart contract, yang memungkinkan kode untuk dibagikan
di antara kontrak dan secara opsional dapat berisi :ref:`skema kontrak
<contract-schema-id>`.

.. graphviz::
   :align: center
   :caption: A smart contract module containing two smart contracts.

   digraph G {
       subgraph cluster_0 {
           node [fillcolor=white, shape=note]
           label = "Module";
           "Crowdfunding";
           "Escrow";
       }
   }

Modul harus mandiri, dan hanya memiliki daftar impor terbatas
yang memungkinkan interaksi dengan rantai.
Ini disediakan oleh lingkungan host dan tersedia untuk kontrak
pintar dengan mengimpor modul bernama ``concordium``.

.. seealso::

   Lihat :ref:`host-functions` untuk referensi lengkap.

Bahasa On-chain
=================

Di blockchain Concordium, bahasa smart contract adalah bagian dari `Web
Assembly`_ (disingkat Wasm) yang dirancang untuk menjadi target kompilasi portabel
dan dijalankan di lingkungan sandboxed. Ini berguna karena smart
contract akan dijalankan oleh baker di jaringan yang tidak selalu mempercayai
kodenya.

Wasm adalah bahasa tingkat rendah dan tidak praktis untuk penulisan dengan tangan. Sebaliknya
seseorang dapat menulis smart contract dalam bahasa tingkat tinggi yang kemudian
dikompliasi ke Wasm.

.. _wasm-limitations-id:

Batasan
-----------

.. todo::

   Tambahkan batasan lain, seperti bagian awal ...

Lingkungan blockchain sangatlah khusus, dalam arti bahwa setiap node harus dapat
melaksanakan kontrak dengan cara yang persis sama, menggunakan sumber daya
yang sama persis. Jika tidak, node akan gagal mencapai konsensus
di status rantai. Untuk alasan ini smart contract perlu ditulis dalam subset Wasm
yang terbatas.

Angka floating point
^^^^^^^^^^^^^^^^^^^^^^

Meskipun Wasm memiliki dukungan untuk angka floating point, smart contract
tidak diperbolehkan untuk menggunakannya. Alasan untuk hal ini adalah bahwa bilangan
floating-point Wasm dapat memiliki nilai ``NaN`` ("bukan bilangan") khusus yang
perilakunya dapat mengakibatkan nondeterminisme.

Pembatasan berlaku secara statis, yang berarti bahwa smart contract tidak boleh
berisi tipe floating point, juga tidak bisa berisi instruksi yang melibatkan nilai
floating point.


Penerapan
==========

Menerapkan modul ke rantai berarti mengirimkan bytecode modul sebagai
transaksi ke jaringan Concordium. Jika *valid* transaksi ini akan dimasukkan
dalam satu blok. Transaksi ini, seperti setiap transaksi lainnya, memiliki
biaya terkait. Biaya didasarkan pada ukuran bytecode dan dibebankan
untuk memeriksa validitas modul dan penyimpanan on-chain.

Penerapan itu sendiri tidak dijalankan
smart contract. Untuk mengeksekusi, pengguna harus terlebih dahulu membuat *instance* kontrak.

.. seealso::

   Lihat :ref:`contract-instances-id` untuk informasi lebih lanjut.

.. _smart-contracts-on-chain-id:

.. _smart-contracts-on-the-chain-id:

.. _contract-on-chain-id:

.. _contract-on-the-chain-id:

smart contract di rantai
===========================

smart contract pada rantai adalah kumpulan fungsi yang diekspor dari modul yang
diterapkan. Mekanisme konkret yang digunakan untuk ini adalah bagian ekspor
`Web Assembly`_. Smart contract harus mengekspor satu fungsi untuk menginisialisasi instans baru
dan dapat mengekspor nol atau lebih fungsi untuk memperbarui instance

Karena modul smart contract dapat mengekspor fungsi untuk beberapa smart contract
yang berbeda, kami mengaitkan fungsi menggunakan skema penamaan:

- ``init_<contract-name>``: Fungsi untuk menginisialisasi smart contract harus
   mulai dengan `` init_`` diikuti dengan nama smart contract. Kontrak
   hanya boleh terdiri dari karakter alfanumerik atau tanda baca ASCII, dan tidak
   diizinkan untuk mengandung simbol ``.``.

- ``<contract-name>.<receive-function-name>``: Fungsi untuk berinteraksi dengan sebuah
   smart contract diawali dengan nama kontrak, diikuti dengan ``.`` dan sebuah
   nama untuk fungsinya. Sama seperti untuk fungsi init, nama kontrak tidak diperbolehkan
   untuk memuat simbol ``.``.

.. note::

   Jika Anda mengembangkan smart contract menggunakan Rust dan ``concordium-std``,
   prosedural makro ``#[init(...)]`` dan ``#[receive(...)]`` aturlah
   skema penamaan yang benar.

.. _Web Assembly: https://webassembly.org/
