.. _inspect-instance-id:

==================================
Memerikasa instance smart contract
==================================

Panduan ini akan menunjukkan kepada Anda cara memeriksa instance smart contract.
Memeriksa sebuah instance akan menunjukkan kepada Anda nama, pemilik, referensi modul, saldo,
status dan fungsi terima:

Persiapan
===========

Pastikan bahwa anda :ref:`menjalankan node<run-a-node>` menggunakan :ref:`Concordium software<downloads>` terbaru dan anda
mempunyai sebuah smart-contract instance on-chain untuk diperiksa.

.. seealso::
   Untuk cara menerapkan modul smart contract lihat :ref:`deploy-module-id` dan untuk
   cara membuat sebuah instance :ref:`initialize-contract-id`.

Pemeriksaan
============

Untuk memeriksa, atau menunjukkan, informasi tentang insatnce smart contract dengan
indeks alamatnya ``0``, jalankan perintah berikut:

.. code-block:: console

   $concordium-client contract show 0

Hasilnya harus serupa dengan berikut ini:

.. code-block:: console

   Contract:        my_contract
   Owner:           '4Lh8CPhbL2XEn55RMjKii2XCXngdAC7wRLL2CNjq33EG9TiWxj' (default)
   ModuleReference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'
   Balance:         0.000000 GTU
   State:
       {
           "first_field": 0,
           "second_field": 42
       }
   Functions:
    - receive_one
    - receive_two

.. seealso::

   Untuk informasi lebih lanjut tentang alamat kontrak instance, lihat
   :ref:`references-on-chain`.

Tingkat detail inspeksi bergantung pada apakah perintah ``show`` memiliki
akses ke :ref:`skema kontrak <contract-schema-id>`.
Jika skema disematkan, itu akan digunakan secara implisit.
Jika tidak, skema dapat disediakan menggunakan parameter ``--schema /path/to/schema.bin``.

.. note::

   File skema yang disediakan menggunakan parameter ``--schema`` akan diutamakan
   daripada skema yang disematkan.

   :ref:`Baca lebih lanjut tentang mengapa dan bagaimana menggunakan skema smart contract  <contract-schema-id>`.
