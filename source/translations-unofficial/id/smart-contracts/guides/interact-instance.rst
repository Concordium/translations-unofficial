.. _interact-instance-id:

===========================================
Berinteraksi dengan instance smart contract
===========================================

Panduan ini akan menunjukkan kepada Anda, cara berinteraksi dengan
instance smart contract, yang berarti memicu fungsi terima, yang mungkin,
memperbarui status instans.

Persiapan
===========

Pastikan bahwa anda :ref:`menjalankan node<run-a-node>` menggunakan :ref:`Concordium software<downloads>` terbaru dan anda
mempunyai sebuah smart-contract instance on-chain untuk diperiksa.

.. seealso::
   Untuk cara menerapkan modul smart contract lihat :ref:`deploy-module-id` dan untuk
   cara membuat sebuah instance :ref:`initialize-contract-id`.

Karena interaksi dengan smart contract adalah transaksi, Anda juga harus memastikan
untuk menyiapkan ``concordium-client`` dengan akun yang berisi GTU yang cukup untuk
membayar transaksi.

.. note::

   Biaya transaksi ini bergantung pada ukuran parameter yang dikirim
   ke fungsi terima dan kompleksitas fungsi itu sendiri.

Interaksi
===========

Untuk memperbarui instance dengan indeks alamat ``0`` menggunakan fungsi terima
tanpa parameter ``my_receive`` sambil mengizinkan hingga 1000 energi untuk digunakan,
jalankan perintah berikut:

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

Jika berhasil, hasilnya akan serupa dengan berikut ini:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

Meneruskan parameter dalam format JSON
--------------------------------------

Parameter dalam format JSON dapat diteruskan jika sebuah :ref:`skema smart contract
<contract-schema-id>` diberikan, baik sebagai file atau disematkan dalam modul.
Skema ini digunakan untuk membuat JSON menjadi biner.

.. seealso::

   :ref:`Baca lebih lanjut tentang mengapa dan bagaimana menggunakan skema smart contract
   <contract-schema-id>`.

Untuk memperbarui sebuah instance dengan indeks alamat ``0`` menggunakan fungsi terima
``my_parameter_receive`` dengan file parameter ``my_parameter.json`` dalam format
JSON, jalankan perintah berikut:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json

Jika berhasil, hasilnya akan serupa dengan berikut ini:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

Jika tidak, kesalahan yang menjelaskan masalah akan ditampilkan.
Kesalahan umum dijelaskan di bagian selanjutnya.

.. seealso::

   Untuk informasi lebih lanjut tentang alamat contract instance, lihat
   :ref:`references-on-chain`.

.. note::

   Jika parameter yang diberikan dalam format JSON tidak sesuai dengan jenis
   yang ditentukan dalam skema, pesan kesalahan akan ditampilkan. Sebagai contoh:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Jika modul yang diberikan tidak berisi skema yang disematkan, itu dapat diberikan
   menggunakan parameter ``--schema /path/to/schema.bin``.

.. note::

   GTU juga dapat ditransfer ke kontrak selama pembaruan menggunakan
   parameter ``--amount AMOUNT``.

Meneruskan parameter dalam format biner
---------------------------------------

Saat meneruskan parameter dalam format biner, sebuah
:ref:`skema kontrak <contract-schema-id>` tidak diperlukan.

Untuk memperbarui sebuah instance dengan indeks alamat ``0`` menggunakan fungsi terima
``my_parameter_receive`` dengan file parameter ``my_parameter.bin`` dalam format
binary, jalankan perintah berikut:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

Jika berhasil, hasilnya akan serupa dengan berikut ini:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. seealso::

   Untuk informasi tentang cara bekerja dengan parameter dalam smart contract, lihat
   :ref:`working-with-parameters`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
