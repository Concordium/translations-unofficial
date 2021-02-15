.. _contract-instances-id:

========================
Instances smart contract
========================

.. todo::

   - Memperjelas bagaimana instances terkait dengan smart contract terkait dengan modul
     (misalnya, sekarang dikatakan bahwa sebuah instance adalah status + modul tetapi
     gambar di bawah menunjukkan contoh sebagai kontrak + status).
   - Tentukan bagaimana tepatnya kita harus mendefinisikan modul smart contract (sebagai konsepnya sendiri
     atau sebagai modul Wasm), dan apakah kita harus membicarakannya atau tidak.
   - Putuskan apakah kita harus memiliki contoh kode konkret, dan apakah harus
     berada di Wasm atau Rust atau mungkin pseudocode.
   - Pertimbangkan untuk memiliki gambar yang menjelaskan hubungan antara modul dan instance.

**Instances smart contract** adalah modul smart contract bersama dengan
keadaan dan sejumlah token GTU tertentu.
Beberapa instances smart contract dapat dibuat dari modul yang sama.
Sebagai contoh, untuk sebuah kontrak :ref:`lelang <auction-id>`, mungkin terdapat beberapa instances, masing-masing
didedikasikan untuk penawaran item tertentu dan dengan pesertanya sendiri.

Instances smart contract dapat dibuat dari yang diterapkan :ref:`modul kontrak
pintar<contract-module-id>` melalui transaksi ``init`` yang memanggil fungsi
yang diminta dalam modul smart contract. Fungsi ini dapat mengambil
parameter.
Hasil akhirnya harus berupa status smart contract awal dari
instance.

.. note::

   Instances smart contract sering disebut sebagai *instance* saja.

.. graphviz::
   :align: center
   :caption: Example of smart contract module containing two smart contracts:
             Escrow and Crowdfunding. Each contract has two instances.

   digraph G {
       rankdir="BT"

       subgraph cluster_0 {
           label = "Module";
           labelloc=b;
           node [fillcolor=white, shape=note]
           "Crowdfunding";
           "Escrow";
       }

       subgraph cluster_1 {
           label = "Instances";
           style=dotted;
           node [shape=box, style=rounded]
           House;
           Car;
           Gadget;
           Boardgame;
       }

       House:n -> Escrow;
       Car:n -> Escrow;
       Gadget:n -> Crowdfunding;
       Boardgame:n -> Crowdfunding;
   }

Status dari sebuah insatnces smart contract
===========================================

Status instance smart contract terdiri dari dua bagian, status yang ditentukan pengguna
dan jumlah GTU yang dipegang kontrak, yaitu *saldonya*. saat
merujuk ke status yang kami maksud biasanya hanya status yang ditentukan pengguna. Alasan untuk
memperlakukan jumlah GTU secara terpisah adalah karena GTU hanya dapat dikirim
dan diterima sesuai dengan aturan jaringan, misalnya, kontrak tidak dapat membuat
atau menghancurkan token GTU.

.. _contract-instances-init-on-chain-id:

Instansiasi smart contract on-chain
=======================================

Setiap smart contract harus mengandung fungsi untuk membuat instances kontrak
pintar. Fungsi seperti itu disebut sebagai *fungsi init*.

Untuk membuat sebuah instance smart contract, sebuah akun mengirimkan transaksi khusus dengan
referensi ke modul smart contract yang diterapkan dan nama
fungsi init yang akan digunakan untuk instansiasi.

Transaksi juga dapat menyertakan sejumlah GTU, yang ditambahkan ke saldo
dari instance smart contract. Sebuah parameter untuk fungsi tersebut diberikan sebagai bagian
dari transaksi dalam bentuk array byte.

Untuk meringkas, transaksi tersebut meliputi:

- Referensi ke modul smart contract.
- Nama fungsi init.
- Parameter ke fungsi init.
- Jumlah GTU untuk instance.

Fungsi init dapat memberi sinyal bahwa ia tidak ingin membuat instance baru
dengan parameter tersebut. Jika fungsi init menerima parameter, itu mengatur
status awal instance dan saldonya. Instance diberi alamat
di rantai dan akun yang mengirim transaksi menjadi pemilik dari instance.
jika fungsi menolak, tidak ada instance yang dibuat dan hanya
transaksi untuk mencoba membuat instance yang terlihat on-chain.

.. seealso::

   Lihat :ref:`initialize-contract-id` panduan tentang cara menginisialisasi sebuah
   kontrak dalam uji coba.

Status instance
===============

Setiap instance smart contract memiliki statusnya sendiri yang direpresentasikan secara on-chain
sebagai array byte. Instance ini menggunakan fungsi yang disediakan oleh lingkungan
host untuk membaca, menulis dan mengubah ukuran status.

.. seealso::

   Lihat :ref:`host-functions-state` untuk referensi fungsi ini.

Status smart contract terbatas ukurannya. Saat ini batas status kontrak
pintar adalah 16KiB.

.. seealso::

   Lihat :ref:`resource-accounting` untuk info lebih lanjut tentang ini.

Berinteraksi dengan sebuah instance
===================================

smart contract dapat mengekspos nol atau lebih, fungsi untuk berinteraksi dengan
sebuah instans, yang disebut sebagai *fungsi terima*.

Sama seperti fungsi init, fungsi terima dipicu menggunakan
transaksi, yang berisi sejumlah GTU untuk kontrak dan argumen
ke fungsi dalam bentuk byte.

Singkatnya, transaksi untuk interaksi smart contract meliputi:

- Alamat ke instance smart contract.
- Nama fungsi terima.
- Parameter untuk fungsi terima.
- Jumlah GTU untuk instance.

.. _contract-instance-actions-id:

Pencatatan Peristiwa
====================

.. todo::

   Jelaskan peristiwa apa dan mengapa itu berguna.
   Susun ulang/klarifikasi "pantau peristiwa".

Peristiwa dapat dicatat selama pelaksanaan fungsi smart contract. Ini adalah
kasus untuk fungsi init dan terima. Log dirancang untuk penggunaan
di off-chain, sehingga seseorang di luar rantai dapat memantau kejadian dan
bereaksi terhadapnya. Log tidak dapat diakses oleh smart contract, atau aktor lain
di dalam rantai. kejadian dapat dicatat menggunakan fungsi yang disediakan oleh lingkungan
host.

.. seealso::

   Lihat :ref:`host-functions-log` untuk referensi fungsi ini.

Log kejadian ini disimpan oleh baker dan dimasukkan dalam ringkasan transaksi.

Mencatat suatu keajdian memiliki biaya terkait, serupa dengan biaya penulisan ke
status kontrak. Dalam kebanyakan kasus, masuk akal untuk mencatat beberapa byte untuk
mengurangi biaya.

.. _action-descriptions-id:

Deskripsi aksi
===================

Fungsi terima mengembalikan *deskripsi aksi* yang akan dieksekusi
oleh lingkungan host pada rantai.

aksi yang mungkin dihasilkan oleh kontrak adalah:

- **Terima** adalah aksi primitif yang selalu berhasil.
- **Transfer sederhana** GTU dari instance ke akun yang ditentukan.
- **Kirim**: memanggil fungsi terima dari instance smart contract yang ditentukan,
  dan secara opsional mentransfer beberapa GTU dari instance pengiriman ke instnace
  penerima.

Jika suatu aksi gagal dijalankan, fungsi terima dikembalikan, membiarkan
status dan saldo instance tidak berubah. Namun,

- transaksi yang memicu fungsi terima (tidak berhasil) masih ditambahkan ke rantai, dan
- biaya transaksi, termasuk biaya pelaksanaan aksi yang gagal,
  dibebankan dari akun pengirim.

Memproses beberapa deskripsi aksi
---------------------------------------

Anda dapat merangkai deskripsi aksi menggunakan kombinator **and**.
Urutan deskripsi aksi ``A`` **and** ``B``

1) Eksekusi ``A``.
2) Jika ``A`` berhasil, Eksekusi ``B``.
3) Jika ``B`` gagal seluruh urutan aksi gagal (dan hasil ``A`` dikembalikan).

Penanganan kesalahan
--------------------

Gunakan kombinator **or** untuk menjalankan aksi jika aksi sebelumnya gagal.
Deskripsi aksi ``A`` **or** ``B``

1) Eksekusi ``A``.
2) Jika ``A`` berhasil, berhenti mengeksekusi.
3) Jika ``A`` gagal, Eksekusi ``B``.

.. graphviz::
   :align: center
   :caption: Example of an action description, which tries to transfer to Alice
             and then Bob, if any of these fails, it will try to transfer to
             Charlie instead.

   digraph G {
       node [color=transparent]
       or1 [label = "Or"];
       and1 [label = "And"];
       transA [label = "Transfer x to Alice"];
       transB [label = "Transfer y to Bob"];
       transC [label = "Transfer z to Charlie"];

       or1 -> and1;
       and1 -> transA;
       and1 -> transB;
       or1 -> transC;
   }

.. seealso::

   Lihat :ref:`host-functions-actions` untuk referensi tentang cara membuat
   aksi.

Seluruh urutan aksi dijalankan **atomically**, dan mengarah ke pembaruan
ke semua akun dan instance yang relevan, atau, jika terjadi penolakan pembayaran
untuk eksekusi, tidak ada perubahan lain. Akun yang mengirim transaksi awal
akan membayar untuk eksekusi seluruh urutan.
