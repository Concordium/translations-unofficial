
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker-id:

==================================
Menjadi Baker (membuat blok)
==================================

.. contents::
   :local:
   :backlinks: none

Bagian ini menjelaskan apa itu baker, perannya dalam jaringan, dan bagaimana menjadi
baker.

Dengan membaca bagian ini Anda akan belajar tentang:

-  Apa itu baker dan konsep yang terkait dengannya.
-  Cara meningkatkan node Anda untuk menjadi baker.

Proses untuk menjadi baker dapat diringkas dalam langkah-langkah berikut:

#. Dapatkan akun dan beberapa GTU.
#. Dapatkan akun dan beberapa GTU. Dapatkan satu set kunci baker.
#. Daftarkan kunci baker dengan akun tersebut.
#. Mulailah node dengan kunci baker.

Setelah menyelesaikan langkah-langkah ini, node baker akan membaking blok. Jika blok yang dibaking
ditambahkan ke rantai(chain)(chain), node baker akan menerima hadiah.

.. note::

   Pada bagian ini kita akan menggunakan nama ``bakerAccount`` sebagai nama
   akun yang akan digunakan untuk mendaftar dan mengelola baker.

Definisi
===========

Baker
-----

Node adalah *baker* (atau *baking*) ketika ia aktif berpartisipasi dalam
jaringan dengan membuat blok baru yang ditambahkan ke rantai(chain)(chain). Sebuah Baker mengumpulkan,
memesan dan memvalidasi transaksi yang termuat dalam blok untuk menjaga
integritas blockchain. Baker menandatangani setiap blok yang mereka panggang(bake)
agar blok tersebut dapat diperiksa dan dieksekusi oleh peserta lainnya didalam
jaringan.

Kunci Baker (baker keys)
------------------------

Setiap baker memiliki satu set kunci kriptografi yang disebut *kunci baker(baker keys)*. Node menggunakan
kunci ini untuk menandatangani blok yang dipanggang(bake). Untuk bake blok dapat ditandatangani oleh
Baker tertentu node harus berjalan dengan memuat set kunci baker(baker keys).

Akun Baker
-------------

Setiap akun dapat menggunakan satu set kunci baker untuk mendaftarkan sebuah baker.

Setiap kali baker membuat blok valid yang dimasukkan ke dalam rantai(chain)(chain), setelah beberapa
saat hadiah dibayarkan ke akun terkait.

Stake dan lotre
-----------------

.. todo::

   - Tautan ke jadwal rilis.

Akun tersebut dapat mempertaruhkan sebagian dari saldo GTU-nya ke dalam *baker stake* dan kemudian
secara manual dapat melepaskan semua atau sebagian dari jumlah yang dipertaruhkan(staked). Jumlah yang dipertaruhkan
tidak bisa dipindahkan atau ditransfer sampai dirilis oleh baker.

.. note::

   Jika akun memiliki Beberapa GTU yang ditransfer dengan jadwal rilis,
   GTU tersebut dapat dipertaruhkan meskipun belum dirilis.

Agar dipilih untuk membaking blok, baker harus berpartisipasi dalam sebuah
*lotere* di mana kemungkinan mendapatkan tiket menang kira-kira
sebanding dengan jumlah yang dipertaruhkan.

Stake yang sama digunakan saat menghitung apakah seorang baker termasuk dalam komite
finalisasi atau tidak. Lihat Finalisasi_.

.. _epochs-and-slot-id:

Epochs(masa) dan slot
----------------------

Di blockchain Concordium, Waktu dibagi menjadi *slot*. Slot punya waktu
durasi tetap di blok Genesis. Pada cabang tertentu, setiap slot dapat memiliki
kebanyakan hanya satu blok, tetapi beberapa blok di cabang yang berbeda dapat diproduksi di
slot yang sama.

.. todo::

   Mari tambahkan gambar.

Saat mempertimbangkan imbalan dan konsep terkait baking lainnya, kami menggunakan
konsep sebuah *epoch* sebagai satuan waktu yang mendefinisikan suatu periode di mana himpunan
dari baker dan stake saat ini sudah diperbaiki. Epoch memiliki durasi waktu tetap di
Blok Genesis. Di testnet, epoch memiliki durasi **1 jam**.

Memulai baking
==============

Mengelola akun
-----------------

Bagian ini memberikan ringkasan singkat tentang langkah-langkah yang relevan untuk mengimpor sebuah
Akun. Untuk penjelasan lengkap, lihat :ref:`managing_accounts`.

Akun dibuat menggunakan aplikasi :ref:`concordium_id`. Setelah sebuah akun
berhasil dibuat, menavigasi ke tab **Lainnya** dan memilih **Ekspor**
memungkinkan Anda mendapatkan file JSON yang berisi informasi akun.

Untuk mengimpor akun ke dalam toolchain, jalankan

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount

``concordium-client`` akan meminta kata sandi untuk mendekripsi file yang diekspor dan
mengimpor semua akun. Kata sandi yang sama akan digunakan untuk mengenkripsi
kunci penandatanganan transaksi dan kunci transfer terenkripsi.

Membuat kunci untuk baker dan mendaftarkannya
---------------------------------------------

.. note::

   Untuk proses ini, akun perlu memiliki beberapa GTU, jadi pastikan untuk meminta
   pemberian 100 GTU untuk akun di aplikasi seluler.

Setiap akun memiliki baker ID unik yang digunakan saat mendaftarkan baker. ID
ini harus disediakan oleh jaringan dan saat ini tidak dapat dihitung . ID
ini harus diseratkan di dalam file kunci baker ke node, sehingga node dapat menggunakan
kunci baker untuk membuat blok. ``Concordium-client`` akan secara otomatis mingisi
bagian ini saat melakukan operasi berikut.

Untuk membuat satu set kunci baru, jalankan:

.. code-block:: console

   $concordium-client baker generate-keys <keys-file>.json

dimana Anda dapat memilih nama arbitrer untuk file kunci. untuk
mendaftarkan kunci ke jaringan, yang Anda perlukan adalah :ref:`menjalankan node <running-a-node-id>`
dan kirim transaksi ``baker add`` ke jaringan:

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

ganti

- ``<amountToStake>`` dengan jumlah GTU untuk stake awal baker
- ``<concordium-data-dir>`` dengan direktori data berikut:

  * di Linux dan MacOS: ``~/.local/share/concordium``
  * di Windows: ``%LOCALAPPDATA%\\concordium``.

(Nama file yang dihasilkan harus tetap ``baker-credentials.json``).

Berikan tanda ``--no-restake`` untuk menghindari penambahan otomstis
hadiah untuk jumlah yang distake di baker. Perilaku ini dijelaskan di
bagian `Re-stake penghasilan`_.

Untuk memulai node dengan kunci baker ini dan mulai memproduksi blok. pertama,
anda harus mematikan node yang sedang berjalan (baik dengan menekan
``Ctrl + C`` di terminal tempat node berjalan atau eksekusi menggunakan
``concordium-node-stop``).

Setelah menempatkan file di direktori yang sesuai (sudah dilakukan di
perintah sebelumnya saat menentukan file output), mulai node lagi menggunakan
``concordium-node``. Node secara otomatis akan mulai baking saat baker
disertakan dalam baker untuk epoch saat ini.

Perubahan ini akan dijalankan
segera dan akan berlaku saat menyelesaikan epoch setelah epoch yang mana
transaksi untuk menambahkan baker sudah termasuk dalam satu blok.

.. table:: Timeline: menambahkan baker

   +-------------------------------------------+-----------------------------------------+-----------------+
   |                                           | Saat transaksi termasuk dalam satu blok | setelah 2 epoch |
   +===========================================+=========================================+=================+
   | Perubahan terlihat dengan menanyakan node |  ✓                                      |                 |
   +-------------------------------------------+-----------------------------------------+-----------------+
   | Baker termasuk dalam panitia baking       |                                         | ✓               |
   +-------------------------------------------+-----------------------------------------+-----------------+

.. note::

   Jika transaksi untuk menambahkan baker dimasukkan dalam blok selama epoch `E`,
   baker akan dianggap sebagai bagian dari panitia baking saat epoch
   `E + 2` dimulai.

Mengelola baker
==================

Memeriksa status baker dan kekuatan loterenya
------------------------------------------------------

Untuk melihat apakah node sedang membaking, Anda dapat memeriksa berbagai sumber yang
menawarkan tingkat presisi yang berbeda dalam informasi yang ditampilkan.

- Dalam `network dashboard <http://dashboard.testnet.concordium.com>`_, node
  anda akan menunjukkan baker ID di kolom ``baker``.
- Dengan menggunakan ``concordium-client`` Anda dapat memeriksa daftar baker saat ini
  dan jumlah stake relatif yang mereka pegang, yaitu kekuatan lotre mereka.
  kekuatan lotere akan menentukan seberapa besar kemungkinan sebuah baker akan memenangkan
  lotere dan membuat blok.

   .. code-block:: console

     $concordium-client consensus show-parameters --include-bakers
     Election nonce:      07fe0e6c73d1fff4ec8ea910ffd42eb58d5a8ecd58d9f871d8f7c71e60faf0b0
     Election difficulty: 4.0e-2
     Bakers:
                                  Account                       Lottery power
             ----------------------------------------------------------------
         ...
         34: 4p2n8QQn5akq3XqAAJt2a5CsnGhDvUon6HExd2szrfkZCTD4FX   <0.0001
         ...

- Dengan menggunakan ``concordium-client`` Anda dapat memeriksa bahwa akun tersebut telah
  mendaftarkan baker dan jumlah yang distake saat ini oleh baker tersebut.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- Jika jumlah stakenya cukup besar dan ada node yang berjalan dengan memuat
  kunci baker, baker itu akhirnya akan menghasilkan blok dan Anda bisa melihatnya
  di dompet seluler Anda bahwa hadiah baking diterima oleh akun,
  seperti yang terlihat pada gambar ini:

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Memperbarui jumlah yang distake
-------------------------------

Untuk memperbarui stake baker jalankan

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

Memodifikasi jumlah yang distake mengubah kemungkinan bahwa sebuah baker akan terpilih
untuk membake blok.

Ketika sebuah baker **menambahkan stake untuk pertama kalinya atau meningkatkan stakenya**, perubahan
itu dijalankan pada rantai(chain)(chain) dan akan terlihat segera setelah transaksi
termasuk dalam blok (dapat dilihat melalui ``concordium-client account show
bakerAccount``)  dan berlaku 2 periode setelah itu.

.. table:: Timeline: meningkatkan stake

   +-------------------------------------------+-----------------------------------------+----------------+
   |                                           | Saat transaksi termasuk dalam satu blok |Setelah 2 epoch |
   +===========================================+=========================================+================+
   | Perubahan terlihat dengan menanyakan node | ✓                                       |                |
   +-------------------------------------------+-----------------------------------------+----------------+
   | Baker menggunakan stake baru              |                                         |  ✓             |
   +-------------------------------------------+-----------------------------------------+----------------+

ketika sebuah baker **menurunkan jumlah stake**, perubahan akan membutuhkan *2 +
bakerCooldownEpochs* epochs untuk diterapkan. Perubahan akan terlihat di
rantai(chain)(chain) segera setelah transaksi dimasukkan dalam blok, dapat dikonsultasikan melalui
``concordium-client account show bakerAccount``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: Timeline: mengurangi stake

   +-------------------------------------------+-----------------------------------------+-----------------------------------------+
   |                                           | Saat transaksi termasuk dalam satu blok | setelah *2 + bakerCooldownEpochs* epoch |
   +===========================================+=========================================+=========================================+
   | Perubahan terlihat dengan menanyakan node | ✓                                       |                                         |
   +-------------------------------------------+-----------------------------------------+-----------------------------------------+
   | Baker menggunakan stake baru              |                                         |  ✓                                      |
   +-------------------------------------------+-----------------------------------------+-----------------------------------------+
   | Stake dapat di kurangi lagi atau          | ✗                                       |  ✓                                      |
   | baker dapat dibuang                       |                                         |                                         |
   +-------------------------------------------+-----------------------------------------+-----------------------------------------+

.. note::

   Di testnet, ``bakerCooldownEpochs`` awalnya disetel ke 168 epoch. Nilai
   ini dapat diperiksa sebagai berikut:

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   Seperti disebutkan di bagian `Definisi`_, jumlah yang dipertaruhkan *dikunci*,
   yaitu tidak dapat ditransfer atau digunakan untuk pembayaran. Anda harus mengambil ini
   ke dalam akun dan pertimbangkan menstake dengan jumlah yang tidak akan dibutuhkan pada
   jangka pendek. Secara khusus, untuk membatalkan pendaftaran seorang baker atau untuk memodifikasi jumlah
   stake yang Anda perlukan untuk memiliki beberapa GTU yang tidak dipertaruhkan untuk menutupi biaya
   transaksi.

Re-stake Penghasilan
----------------------

Saat berpartisipasi sebagai baker di jaringan dan membaking blok, akun
menerima hadiah di setiap blok yang dibake. Hadiah ini otomatis ditambahkan ke
jumlah yang distake secara default.

Anda dapat memilih untuk mengubah perilaku ini dan sebagai gantinya menerima hadiah dalam
saldo akun tanpa merestake-nya secara otomatis. Pengaturan ini bisa
diubah melalui ``concordium-client``:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Perubahan pada bendera restake akan segera berlaku; Namun, perubahannya
mulai mempengaruhi baking dan kekuatan finalisasi dalam epoch selanjutnya. Nilai pengaturan
saat ini dapat dilihat pada informasi akun yang dapat ditanyakan
menggunakan ``concordium-client``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: memperbarui restake

   +-------------------------------------------+-----------------------------------------+-----------------------------------------+
   |                                           | Saat transaksi termasuk dalam satu blok | setelah *2 + bakerCooldownEpochs* epoch |
   +===========================================+=========================================+=========================================+
   | Perubahan terlihat dengan menanyakan node | ✓                                       |                                         |
   +-------------------------------------------+-----------------------------------------+-----------------------------------------+
   | Penghasilan [tidak] akan di Re-stake      | ✓                                       |                                         |
   | secara otomatis                           |                                         |                                         |
   +-------------------------------------------+-----------------------------------------+-----------------------------------------+
   | jika restaking otomatis, stake yang       |                                         | ✓                                       |
   | diperoleh mempengaruhi kekuatan lotere    |                                         |                                         |
   +-------------------------------------------+-----------------------------------------+-----------------------------------------+

Ketika baker terdaftar, secara otomatis akan me re-stake kembali pendapatannya, tetapi sebagai
disebutkan di atas, ini dapat diubah dengan memberikan tanda ``--no-restake`` ke
perintah ``baker add`` seperti yang ditunjukkan di sini:

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Finalisasi
------------

Finalisasi adalah proses pemungutan suara yang dilakukan oleh node di *finalisasi
komite* yang *menyelesaikan* blok ketika jumlah anggota panitia yang cukup
besar telah menerima blok tersebut dan menyetujui hasilnya. Blok baru
harus memiliki blok yang diselesaikan sebagai leluhur untuk memastikan integritas
rantai(chain)(chain). Untuk informasi lebih lanjut tentang proses ini, lihat
dibagian :ref:`finalisasi <glossary-finalization>`.

Panitia finalisasi dibentuk oleh baker yang memiliki jumlah stake
tertentu. Ini secara khusus menyiratkan bahwa untuk berpartisipasi dalam
panitia finalisasi Anda mungkin harus mengubah jumlah yang distake
untuk mencapai ambang tersebut. Di testnet, jumlah stake yang dibutuhkan untuk berpartisipasi
dalam panitia finalisasi adalah **0,1% dari total GTU yang ada**.

Berpartisipasi dalam panitia finalisasi menghasilkan hadiah di setiap blok yang
diselesaikan. Hadiah dibayarkan ke akun baker beberapa saat setelah
blok selesai.

Menghapus baker
================

Akun pengendali dapat memilih untuk membatalkan pendaftaran baker di jaringan. Untuk melakukan
itu Anda harus menjalankan ``concordium-client``:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

Ini akan menghapus baker dari daftar baker dan membuka jumlah yang di stake
baker sehingga dapat ditransfer atau dipindahkan dengan bebas.

Saat menghapus baker, perubahan memiliki garis waktu yang sama dengan menurunkan
jumlah yang distake, Untuk mnerapkan perubahan ini membutuhkan waktu *2 + bakerCooldownEpochs*.
Perubahan menjadi terlihat di rantai(chain) segera setelah transaksi dimasukkan ke dalam blok dan Anda
dapat memeriksa kapan perubahan ini akan diterapkan dengan menanyakan informasi akun
dengan ``concordium-client`` seperti biasa:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: Menghapus baker

   +--------------------------------------------+-----------------------------------------+-----------------------------------------+
   |                                            | Saat transaksi termasuk dalam satu blok | setelah *2 + bakerCooldownEpochs* epoch |
   +============================================+=========================================+=========================================+
   | Perubahan terlihat dengan menanyakan node  | ✓                                       |                                         |
   +--------------------------------------------+-----------------------------------------+-----------------------------------------+
   | Baker dikeluarkan dari panitia baking      |                                         | ✓                                       |
   +--------------------------------------------+-----------------------------------------+-----------------------------------------+

.. warning::

   Mengurangi jumlah yang distake dan mengeluarkan baker tidak dapat dilakukan
   serentak. Selama periode cooldown yang dihasilkan dengan mengurangi jumlah
   stake-nya, baker tidak bisa dilepas begitupula sebaliknya.

Dukungan & Umpan Balik
======================

Jika Anda mengalami masalah atau memiliki saran, kirim pertanyaan atau
umpan balik anda ke `Discord`_, atau hubungi kami di testnet@concordium.com.
