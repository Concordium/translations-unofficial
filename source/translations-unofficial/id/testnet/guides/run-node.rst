.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _menjalankan-node:

================
Menjalankan Node
================

.. contents::
   :local:
   :backlinks: none

Dalam panduan ini, Anda mempelajari cara menjalankan node di komputer Anda
berpartisipasi dalam jaringan Concordium. Ini berarti Anda menerima
blok dan transaksi dari node lain, serta menyebarluaskan
informasi tentang blok dan transaksi ke node di jaringan
Concordium. Setelah mengikuti panduan ini, Anda akan bisa

-  menjalankan Concordium node
-  amati di dasbor jaringan dan
-  menanyakan status blockchain Concordium langsung dari mesin
   Anda

Anda tidak memerlukan akun untuk menjalankan node.

Sebelum anda memulai
====================

Sebelum menjalankan node Concordium, Anda harus melakukan

1. Instal dan menjalankan docker

   - Di *Linux*, izinkan Docker dijalankan sebagai pengguna non-root.

2. Unduh dan ekstrak software :ref:`concordium-node-and-client-download`.

Upgrade dari Open Testnet versi sebelumnya
===========================================

Untuk meningkatkan ke software Concordium saat ini, untuk Open Testnet 4:

-  Ikuti langkah-langkah di atas untuk :ref:`download<downloads>` software concordium
   terbaru.

-  Jalankan ``concordium-node-reset-data`` yang dapat dieksekusi dari arsip
   unzip.

   -  Untuk pengguna *Mac*: pertama kali Anda membuka alat, klik kanan file
      ``concordium-node-reset-data`` dan pilih **Buka**. Sebuah pesan
      akan terlihat bahwa perangkat lunak tersebut berasal dari pengembang yang tidak dikenal.
      Pilih **Buka** lagi.
   -  Untuk pengguna *Windows*: pertama kali Anda membuka alat,
      klik dua kali file ``concordium-node-reset-data``. Sebuah pesan
      akan terlihat bahwa perangkat lunak tersebut berasal dari pengembang yang tidak dikenal.
      Pilih **Lainnya** → **Jalankan saja**.

-  Alat tersebut akan menanyakan:

     *Apakah Anda juga ingin menghapus kunci yang disimpan?*

   Akun yang dibuat untuk versi sebelumnya tidak lagi valid di
   Open Testnet 3. Oleh karena itu, jika Anda telah menyimpan akun dari versi
   sebelumnya kami sarankan memasukkan **y** yang akan menghapus semua kunci
   akun.

.. _menjalankan-node:

Menjalankan Node
================

Untuk mulai menjalankan klien yang akan bergabung dengan Open Testnet ikuti
Langkah ini:

1. Buka ``concordium-node`` yang dapat dieksekusi dari arsip yang telah di unzip.

-  Untuk pengguna *Mac*: pertama kali Anda membuka alat, klik kanan file
   ``concordium-node`` binary dan pilih **Buka**. Sebuah pesan akan muncul
   bahwa perangkat lunak tersebut berasal dari pengembang yang tidak dikenal. Pilih **Buka**
   lagi.
-  Untuk pengguna *Windows*: pertama kali Anda membuka alat, klik dua kali
   binary ``concordium-node``. Sebuah pesan akan muncul bahwa
   perangkat lunak berasal dari pengembang tak dikenal. Pilih **Lainnya** →
   **Jalankan saja**.
-  Saat *memulai ulang* node pertimbangkan untuk menggunakan
   Opsi `` --no-block-state-import``. Ini hanya akan mengunduh file
   pembaruan ke blockchain Concordium yang terjadi saat node itu
   tidak aktif dan mungkin mempercepat proses boot.

2. Masukkan nama untuk node Anda. Nama ini akan ditampilkan di depan dasbor
   umum.

3. Jika alat telah dijalankan sebelumnya Anda akan ditanya apakah Anda mau
   hapus database node lokal sebelum memulai. Menekan **y** akan
   menghapus dan kemudian membuat ulang informasi tentang status
   Blockchain Concordium yang disimpan di komputer Anda. **Perhatikan itu
   menghapus database node lokal berarti akan memakan waktu lebih lama untuk node
   anda untuk mengejar ketinggalan dengan jaringan Concordium.**

Alat sekarang akan mengunduh  Klien image Concordium dan memuatnya ke dalam
Docker. Klien akan meluncurkan dan mulai mengeluarkan informasi logging
tentang pengoperasian node.

Melihat node Anda di dasbor
===========================

Setelah menjalankan ``concordium-node`` Anda bisa

-  melihat node Anda di `Network Dashboard`_
-  :ref:`query <testnet-query-node>` informasi tentang blok, transaksi, dan akun

Network dashboard
-----------------

Klien memerlukan beberapa saat untuk mengetahui status file
Blockchain Concordium. Ini melibatkan, misalnya, mengunduh
informasi tentang semua blok dalam rantai.

Di antara informasi lainnya, di `Network Dashboard`_ Anda bisa
mendapatkan gambaran tentang berapa lama waktu yang dibutuhkan node Anda untuk mengejar
rantai. Untuk itu, Anda dapat membandingkan nilai **Length** node (jumlah
blok node yang Anda diterima) dengan nilai **Chain Len** (jumlah
blok dalam rantai terpanjang dalam jaringan) yang ditampilkan di
atas dasbor.


Mengaktifkan koneksi masuk
============================

Jika Anda menjalankan node Anda di belakang firewall, atau di belakang router rumah
anda, maka Anda mungkin hanya dapat terhubung ke node lain,
tetapi node lain tidak akan dapat memulai koneksi ke node Anda.
Ini tidak maslah, dan node Anda akan sepenuhnya berpartisipasi dalam
Jaringan Concordium. dan akan dapat mengirim transaksi dan,
:ref:`jika demikian dikonfigurasi<become-a-baker>`, untuk bake dan menyelesaikan.

Namun Anda juga dapat membuat node Anda menjadi peserta jaringan yang lebih baik
dengan mengaktifkan koneksi masuk. Secara default, ``concordium-node`` mendengarkan
di port ``8888`` untuk koneksi masuk. Tergantung pada jaringan
dan konfigurasi platform Anda juga perlu meneruskan port eksternal
ke ``8888`` di router Anda, buka di firewall, atau keduanya. Itu
rincian bagaimana ini dilakukan akan tergantung pada konfigurasi Anda.

Konfigurasi port
-----------------

Node mendengarkan empat port, yang dapat dikonfigurasi dengan menyediakan
argumen baris perintah yang sesuai saat memulai node. Port
yang digunakan oleh node adalah sebagai berikut:

-  8888, port untuk jaringan peer-to-peer, yang dapat disetel dengan
   ``--listen-node-port``
-  8082, port yang digunakan oleh middleware, yang dapat disetel dengan ``--listen-middleware-port``
-  10000, port gRPC, yang dapat disetel dengan ``--listen-grpc-port``

Saat mengubah pemetaan di atas, kontainer Docker  harus
berhenti (:ref:`stop-a-node`), setel ulang, dan mulai lagi. Untuk mengatur ulang kontainer, gunakan baik
``concordium-node-reset-data`` atau jalankan ``docker rm concordium-client`` di
sebuah terminal.

Kami *sangat menyarankan* bahwa firewall Anda harus dikonfigurasi hanya
izinkan koneksi publik pada port 8888 (port jaringan peer-to-peer
). Seseorang dengan akses ke port lain mungkin dapat mengambil
kontrol dari node Anda atau akun yang telah Anda simpan di node.

.. _menghentikan-node:

Menghentikan node
=================

Untuk menghentikan node, tekan **CTRL+c**, dan tunggu node melakukan penghentian
bersih.

Jika Anda tidak sengaja menutup jendela tanpa mematikan secara eksplisit
klien, itu akan tetap berjalan di latar belakang di Docker. Karena
case, gunakan binary ``concordium-node-stop`` dengan cara yang sama seperti Anda membuka
``concordium-node`` yang dapat dieksekusi.

Dukungan & Umpan Balik
======================

Informasi pencatatan untuk node Anda dapat diambil menggunakan
alat ``concordium-node-retrieve-logs``. Ini akan menyimpan log dari
menjalankan image ke sebuah file. Selain itu, jika diberi izin, ini akan
mengambil informasi tentang program yang sedang berjalan di sistem.

Anda dapat mengirim log, informasi sistem, pertanyaan, dan umpan balik Anda ke
testnet@concordium.com. Anda juga dapat menghubungi di `Discord`_ kami, atau
lihat :ref: `halaman pemecahan masalah <troubleshooting-and-known-issues>`
