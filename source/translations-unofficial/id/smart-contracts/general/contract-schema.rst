.. Should answer:
..
.. - Why should I use a schema?
.. - What is a schema?
.. - Where to use a schema?
.. - How is a schema embedded?
.. - Should I embed or write to file?
..

.. _`custom section`: https://webassembly.github.io/spec/core/appendix/custom.html
.. _`implementation in Rust`: https://github.com/Concordium/concordium-contracts-common/blob/main/src/schema.rs

.. _contract-schema-id:

======================
Skema smart contract
======================

Skema smart contract adalah deskripsi tentang cara merepresentasikan byte dalam representasi yang
lebih terstruktur. Ini dapat digunakan oleh alat eksternal saat menampilkan
status instance smart contract dan untuk menentukan parameter menggunakan
representasi terstruktur, seperti JSON.

.. seealso::

   Untuk petunjuk tentang cara membuat skema untuk modul smart contract di
   Rust, lihat :ref:`build-schema-id`.

Mengapa menggunakan skema kontrak
=================================

Data di blockchain, seperti status instance dan parameter yang diteruskan
ke fungsi init dan terima, diserialkan sebagai urutan byte.
Serialisasi dioptimalkan untuk efisiensi,daripada untuk keterbacaan manusia.

.. todo::

   Pertimbangkan untuk menulis ulang sub-bagian ini karena mungkin agak sulit
   untuk dipahami; khususnya, mungkin hanya mengatakan bahwa demi kenyamanan, pengguna
   dapat meneruskan data yang tidak berserial ke dalam suatu fungsi selama mereka juga menyediakan
   skema yang menjelaskan cara membuat (de)serialisasi data.

Biasanya byte ini memiliki struktur dan struktur ini dikenal oleh kontrak
pintar sebagai bagian dari fungsi kontrak, tetapi di luar fungsi ini, mungkin
sulit untuk memahami byte. Hal ini terutama terjadi saat memeriksa keadaan
kompleks dari instance kontrak atau saat meneruskan parameter kompleks
ke fungsi smart contract. Dalam kasus terakhir, byte harus diserialkan dari
data terstruktur atau ditulis secara manual.

Solusi untuk menghindari penguraian byte secara manual adalah dengan menangkap
informasi ini dalam *skema smart contract*, yang menjelaskan cara membuat struktur
dari byte, dan dapat digunakan oleh alat eksternal.

.. note::

   tool ``concordium-client`` dapat menggunakan skema untuk
   :ref:`membuat serial parameter JSON <init-passing-parameter-json-id>`
   dan untuk deserialisasi status instance kontrak ke JSON.

Skema tersebut kemudian disematkan ke dalam modul smart contract yang diterapkan
ke rantai, atau ditulis ke file dan diedarkan di luar rantai.

Haruskah Anda menyematkan atau menulis ke file?
===============================================

Apakah skema kontrak harus disematkan atau ditulis ke file, tergantung pada
situasi anda.

Menyematkan skema ke dalam modul smart contract, mendistribusikan skema bersama
dengan kontrak yang memastikan skema yang benar sedang digunakan dan juga
memungkinkan siapa saja untuk menggunakannya secara langsung. Kelemahannya adalah
modul smart contract menjadi lebih besar ukurannya dan karena itu lebih mahal
untuk diterapkan. Tetapi kecuali smart contract menggunakan jenis yang sangat
kompleks untuk status dan parameter, ukuran skema kemungkinan dapat diabaikan
dibandingkan dengan ukuran smart contract itu sendiri.

Memiliki skema dalam file terpisah memungkinkan Anda memiliki skema tanpa membayar
byte tambahan saat menerapkan.
Sisi negatifnya adalah Anda malah harus mendistribusikan file skema melalui beberapa
saluran lain dan memastikan bahwa pengguna kontrak menggunakan file yang benar
dengan smart contract Anda.

Format skema
=================

.. todo::

   Memperjelas apakah kita berbicara tentang skema abstrak *apa pun* yang dapat diterapkan pengguna,
   atau skema khusus yang disediakan oleh Concordium. Kemudian hanya berbicara tentang satu atau yang lain,
   atau setidaknya dengan jelas memisahkan pembahasan itu.

Sebuah Skema dapat berisi

- informasi struktur untuk modul smart contract
- deskripsi status smart contract
- parameter untuk fungsi init dan terima dari smart contract.

Masing-masing deskripsi ini disebut sebagai *jenis skema*. Jenis skema selalu
opsional untuk disertakan dalam skema.

Saat ini, jenis skema yang didukung terinspirasi oleh apa yang biasa digunakan di
bahasa pemrograman Rust:

.. code-block:: rust

   enum Type {
       Unit,
       Bool,
       U8,
       U16,
       U32,
       U64,
       I8,
       I16,
       I32,
       I64,
       Amount,
       AccountAddress,
       ContractAddress,
       Timestamp,
       Duration,
       Pair(Type, Type),
       List(SizeLength, Type),
       Set(SizeLength, Type),
       Map(SizeLength, Type, Type),
       Array(u32, Type),
       Struct(Fields),
       Enum(List (String, Fields)),
   }

   enum Fields {
       Named(List (String, Type)),
       Unnamed(List Type),
       Empty,
   }


Di sini, ``SizeLength`` menjelaskan jumlah byte yang digunakan untuk menggambarkan panjangnya
dari jenis panjang variabel, seperti ``Daftar``.

.. code-block:: rust

   enum SizeLength {
       One,
       Two,
       Four,
       Eight,
   }

Untuk referensi tentang bagaimana jenis skema diserialkan menjadi byte, kami merujuk
pembaca ke`implementation in Rust`_.

.. _contract-schema-which-to-choose-id:

Menyematkan skema secara on-chain
=================================

Skema disematkan ke dalam modul smart contract menggunakan fitur `custom section`_
dari modul Wasm.
Ini memungkinkan modul Wasm menyertakan bagian bernama dari byte, yang
tidak mempengaruhi semantik menjalankan modul Wasm.

Semua skema dikumpulkan dan ditambahkan dalam satu bagian kustom bernama
``concordium-schema-v1``.
Koleksi ini adalah daftar pasangan, berisi nama kontrak yang dikodekan
di UTF-8 dan byte skema kontrak.
