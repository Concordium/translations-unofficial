.. _deploy-module-id:

================================
Menerapkan modul smart contract
================================

Panduan ini akan menunjukkan cara menerapkan modul smart contract *on-chain* dan
bagaimana menamainya.

Persiapan
===========

Pastikan bahwa Anda :ref:`menjalankan node<run-a-node>` menggunakan ref:`Concordium software <downloads>` terbaru dan
bahwa Anda memiliki :ref:`modul smart-contract<setup-tools-id>` yang siap untuk diterapkan.

Karena penerapan modul kontrak pintar dilakukan dalam bentuk transaksi,
Anda juga perlu memiliki setup ``concordium-client`` dengan sebuah akun
dengan GTU yang cukup untuk membayar transaksi.

.. note::

   Biaya transaksi bergantung pada ukuran modul smart contract.
   ``concordium-client`` menunjukkan biaya dan meminta konfirmasi
   sebelum menjalankan transaksi apa pun.

Penerapan
==========

Untuk menerapkan modul smart contarct ``my_module.wasm`` menggunakan akun
dengan nama account-name, jalankan perintah sebagai berikut:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. note::

   opsi --sender dapat dihilangkan jika akun "default" akan digunakan. kami akan melakukan dibawah ini
   untuk singkatnya.

Jika berhasil, hasilnya akan serupa dengan:

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'.

Catat referensi modul seperti yang digunakan saat membuat instance smart contract.

.. seealso::

   Untuk panduan tentang cara menginisialisasi smart contract dari modul yang diterapkan, lihat
   :ref:`initialize-contract-id`.

   Untuk informasi lebih lanjut tentang referensi modul, lihat :ref:`references-on-chain`.

.. _naming-a-module-id:

Memberi nama modul
==================

Sebuah modul dapat diberi alias lokal, atau *nama*, yang membuatnya lebih
mudah.
Nama tersebut hanya disimpan secara lokal oleh ``concordium-client``, dan tidak terlihat
secara on-chain.

.. seealso::

   Untuk penjelasan tentang bagaimana dan dimana nama serta pengaturan lokal lain
   disimpan, lihat :ref:`local-settings`.

Untuk menambakan nama saat penerapan, parameter ``--name`` digunakan.
Disini, kita menamai modul dengan nama ``my_deployed_module``:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

Jika berhasil, hasilnya akan serupa dengan:

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

Modules juag dapat diberi nama dengan menggunakan perintah ``name``.
Untuk memberi nama modul yang diterapkan dengan referensi
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` sebagai
``some_deployed_module``, jalankan perintah sebagai berikut:

.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

Hasilnya harus serupa dengan ini:

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module'.
