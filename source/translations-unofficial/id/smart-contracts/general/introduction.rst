.. Should answer:
    - What is a smart contract
    - Why use a smart contract
    - What are the use cases
    - What are not the use cases

.. _introduction-id:

===============================
Pengenalan smart contract
===============================

smart contract adalah bagian kode yang disediakan pengguna yang dikirimkan ke blocakchain,
digunakan untuk menentukan perilaku yang bukan merupakan bagian langsung dari protokol
inti. smart contract dijalankan oleh node di jaringan Concordium sesuai dengan
aturan yang telah ditentukan sebelumnya. Eksekusi mereka sepenuhnya transparan, dan semua
node harus setuju pada apapun hasilnya, eksekusi hanya didasarkan pada informasi
yang tersedia untuk umum.

smart contract dapat menerima, menahan, dan mengirim GTU, ia dapat mengamati beberapa
aspek rantai, dan mempertahankan kondisinya sendiri. smart contract selalu
dieksekusi sebagai tanggapan atas tindakan **eksternal**, misalnya, akun yang mengirim
pesan. Dalam praktiknya, smart contract sering kali menjadi bagian kecil dari sistem
yang lebih besar, menggabungkan fungsionalitas on dan off-chain. Contoh fugsionalitas ialah
bisa menjadi server yang menjalankan smart contract berdasarkan data dari dunia nyata,
seperti harga saham, atau informasi cuaca.

Untuk apa smart contract?
=============================

smart contract dapat mengurangi jumlah kepercayaan yang dibutuhkan pada pihak ketiga, dalam
beberapa kasus menghilangkan kebutuhan akan pihak ketiga tepercaya, dalam kasus lain mengurangi
kemampuan mereka dan dengan demikian mengurangi jumlah kepercayaan yang dibutuhkan di dalamnya.

Karena smart contract dijalankan sepenuhnya secara transparan, dengan cara yang dapat diverifikasi
oleh siapa pun yang memiliki akses ke sebuah node, kontrak tersebut dapat sangat berguna
untuk memastikan kesepakatan antar pihak.

.. _auction-id:

Contoh smart contract pelelangan
--------------------------------

Kasus penggunaan untuk smart contract ialah bisa untuk mengadakan lelang; di sini kami memprogram
smart contract untuk menerima tawaran berbeda dari siapa pun dan terus melacaknya
dari penawar tertinggi.
Saat lelang selesai, smart contract mengirimkan GTU tawaran pemenang kepada penjual dan semua tawaran lainnya kembali.
Penjual kemudian harus mengirimkan barang tersebut kepada pemenang.

smart contract menggantikan peran utama juru lelang. Kontrak itu sendiri
hanya mengatur bagian penawaran, dan distribusi GTU dalam rantai. kemungkinan
juga akan membutuhkan logika untuk mengganti penawar tertinggi jika penjual
tidak memenuhi kewajiban mereka. Hal ini kemungkinan besar akan berarti bahwa
kontrak perlu mendukung beberapa gagasan bukti bahwa penjual telah memenuhi
kewajiban mereka, atau cara tertentu untuk penawar tertinggi untuk mengajukan
keluhan. smart contract tidak dapat menyelesaikan masalah dunia nyata ini secara otomatis,
dan solusi terbaik kemungkinan besar akan bergantung pada spesifikasi lelang.

smart contract *bukan* untuk apa?
===================================

smart contract adalah teknologi yang sangat menarik dan orang-orang masih mencari cara
baru untuk memanfaatkannya.
Namun, ada beberapa kasus di mana smart contract bukan solusi yang baik.

Salah satu keuntungan utama dari smart contract adalah kepercayaan pada eksekusi
eksekusi kode, dan untuk mencapai ini, sejumlah besar node dalam jaringan
blockchain harus mengeksekusi kode yang sama dan memastikan kesepakatan hasilnya.
Secara alami, ini menjadi mahal dibandingkan menjalankan kode yang sama pada
satu node di beberapa layanan cloud.

Dalam kasus di mana smart contract bergantung pada perhitungan yang berat, dimungkinkan
untuk memindahkan perhitungan ini dari smart contract dan membuat smart contract hanya
mengeksekusi beberapa bagian penting dari perhitungan, menggunakan teknik kriptografi
untuk memastikan bagian lain dijalankan dengan benar.

Terakhir, penting untuk diingat bahwa smart contract tidak memiliki privasi dan
**semua** yang dapat diakses oleh smart contract dapat diakses oleh semua orang
di jaringan Concordium, yang berarti sulit untuk menangani data sensitif dalam
smart contract. Dalam beberapa kasus dimungkinkan untuk menggunakan alat kriptografik
untuk tidak bekerja dengan data secara langsung,melainkan membuat smart contract bekerja
dengan gagasan turunan seperti enkripsi dan komitmen, yang menyembunyikan data aktual.

Siklus hidup smart contract
==============================

Sebuah smart contract pertama kali diterapkan ke raintai sebagai :ref:`modul
kontrak <contract-module-id>`. setalah itu smart contract dapat *diinisialisasi* untuk
mendapatkan :ref:`instance smart contract <contract-instances-id>`. akhirnya sebuah smart
contract dapat berulang kali diperbarui sesuai dengan logikanya sendiri.
