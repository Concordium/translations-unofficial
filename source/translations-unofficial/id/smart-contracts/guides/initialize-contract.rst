.. _initialize-contract-id:

====================================
Inisialisasi instance smart contract
====================================

Panduan ini akan menunjukkan kepada Anda cara menginisialisasi smart contract
dari modul smart contract yang diterapkan dengan parameter dalam JSON atau format biner.
Selain itu, ini akan menunjukkan bagaimana memberi nama sebuah instance.

Persiapan
===========

Pastikan bahwa anda :ref:`menjalankan node<run-a-node>` menggunakan :ref:`Concordium software<downloads>` terbaru dan
anda memiliki smart contract :ref:`terpasanhg <deploy-module-id>` di beberapa modul on-chain.

Karena penerapan modul kontrak pintar dilakukan dalam bentuk transaksi,
Anda juga perlu memiliki setup ``concordium-client`` dengan sebuah akun
dengan GTU yang cukup untuk membayar transaksi.

.. note::

   Biaya transaksi ini bergantung pada ukuran parameter yang dikirim
   ke fungsi init dan kompleksitas fungsi itu sendiri.

Inisialisasi
==============

Untuk menginisialisasi sebuah instance smart contract tanpa parameter ``my_contract``
dari modul yang diterapkan dengan referensi
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` sambil
mengizinkan hingga 1000 NRG untuk digunakan, jalankan
perintah berikut:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

Jika berhasil, hasilnya akan serupa dengan:

.. todo::

   Perbarui format alamat menjadi ``<i, s>`` dari ``{"index": i, "subindex": s}``
   (sepanjang dokumentasi).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Melihat pesan ini berarti instance kontrak on-chain baru telah dibuat
dengan alamat yang ditampilkan.

.. seealso::

   Untuk mendapatkan pemahaman yang lebih dalam tentang inisialisasi kontrak, lihat
   :ref:`contract-instances-init-on-chain-id`.

   Untuk informasi lebih lanjut tentang referensi modul dan alamat instance,
   see :ref:`references-on-chain`.

   Menggunakan referensi modul secara langsung dapat merepotkan; untuk menamai mereka, lihat
   :ref:`naming-a-module`.

.. _init-passing-parameter-json-id:

Meneruskan parameter dalam format JSON
--------------------------------------

Parameter dalam format JSON dapat diteruskan jika sebuah :ref:`skema smart contract
<contract-schema-id>` diberikan, baik sebagai file atau disematkan dalam modul.
Skema ini digunakan untuk membuat JSON menjadi biner.

.. seealso::

   :ref:`Baca lebih lanjut tentang mengapa dan bagaimana menggunakan skema smart contract <contract-schema-id>`.

   :ref:`Parameter juga dapat dikirimkan dalam format biner <init-passing-parameter-bin>`.

Untuk menginisialisasi instance sebuah kontrak ``my_parameter_contract``
dari modul dengan referensi
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` dengan sebuah
file parameter ``my_parameter.json`` di format JSON, jalankan perintah berikut:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

Jika berhasil, hasilnya akan serupa dengan:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Jika tidak, kesalahan yang menjelaskan masalah akan ditampilkan.
Kesalahan umum dijelaskan di bagian selanjutnya.

.. note::

   Jika parameter yang diberikan dalam format JSON tidak sesuai dengan jenis yang
   ditentukan dalam skema, pesan kesalahan akan ditampilkan. Sebagai contoh:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Jika modul yang diberikan tidak berisi skema yang disematkan, maka dapat diberikan
   menggunakan parameter ``--schema /path/to/schema.bin``.

.. note::

   GTU juga dapat ditransfer ke instance kontrak selama inisialisasi
   menggunakan parameter ``--amount AMOUNT``.


.. _init-passing-parameter-bin-id:

Meneruskan parameter dalam format biner
---------------------------------------

Saat meneruskan parameter dalam format biner, sebuah :ref:`skema kontrak
<contract-schema-id>` tidak diperlukan.

Untuk menginisialisasi instance sebuah kontrak ``my_parameter_contract``
dari modul dengan referensi
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` dengan sebuah
file parameter ``my_parameter.json``di format JSON, jalankan perintah berikut:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


Jika berhasil, hasilnya akan serupa dengan:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. seealso::

   Untuk informasi tentang cara bekerja dengan parameter dalam smart contracts, lihat
   :ref:`working-with-parameters`.

.. _naming-an-instance-id:

Memberi nama instance kontrak
=============================

Contoh kontrak dapat diberi alias lokal, atau *nama*,
yang membuat referensi lebih mudah.
Nama hanya disimpan secara lokal oleh ``concordium-client``,
dan tidak terlihat secara on-chain.

.. seealso::

   Untuk penjelasan tentang bagaimana dan di mana nama dan pengaturan lokal lainnya
   disimpan, lihat :ref:`local-settings`.

Untuk menambahkan nama selama inisialisasi, sertakan parameter ``--name``.

Disini, kita menginisialisasi kontark ``my_contract`` dari modul yang telah diterapkan
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` dan menamainya
``my_named_contract``:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


Jika berhasil, hasilnya akan serupa dengan:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

Instances kontrak juga dapat dinamai menggunakan perintah ``name``.
Untuk memberi nama instance dengan indeks alamat ``0`` sebagai ``my_named_contract``, jalankan
perintah berikut:

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

Jika berhasil, hasilnya akan serupa dengan:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. seealso::

   Untuk informasi lebih lanjut tentang alamat instances kontrak, lihat
   :ref:`references-on-chain`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
