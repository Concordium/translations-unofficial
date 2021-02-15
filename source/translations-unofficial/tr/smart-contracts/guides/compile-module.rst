.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rust-analyzer: https://github.com/rust-analyzer/rust-analyzer

.. _compile-moduletr:

========================================
Rust akıllı sözleşme modülünü derleyin
========================================

Bu kılavuz size Rust'ta yazılmış akıllı sözleşme modülünü bir Wasm modülüne
nasıl derleyeceğinizi gösterecektir.

Hazırlık
===========

Rust ve Cargo'nun kurulu olduğundan ve ``wasm32-unknown-unknown`` hedefinin,
``cargo-concordium`` ve bir akıllı sözleşme modülü için Rust kaynak kodu ile
birlikte, derlemeye çalıştığınızdan emin olun.

.. seealso::

   Geliştirici araçlarının nasıl kurulacağına ilişkin talimatlar için
   bkz :ref:`setup-tools`.

Wasm'a Derleme
=================

Akıllı sözleşme modüllerinin oluşturulmasına yardımcı olmak ve :ref:`contract
schemas <contract-schema>` gibi özelliklerden yararlanmak için, Rust_ akıllı
sözleşmeler oluşturmak için ``cargo-concordium`` aracını kullanmanızı öneririz.

Akıllı bir sözleşme oluşturmak için, alttaki komutu çalıştırın:

.. code-block:: console

   $cargo concordium build

Bu, Cargo_ yapilandirmasini kullanır, ancak sonuç üzerinde optimizasyonlar da yapar.

.. seealso::

   Akıllı sözleşme modülü için şema oluşturmak için, bazı :ref:`hazırlıklar gerekmektedir <build-schematr>`.

.. note::

   Doğrudan çalıştırarak Cargo_ kullanarak derlemek de mümkündür:

   .. code-block:: console

      $cargo build --target=wasm32-unknown-unknown [--release]

   ``--release`` parametresi ile bile, üretilen Wasm modülünün hata ayıklama
   bilgilerini içerdiğini unutmayın..

Bilgisayar bilgilerini derlemeden kaldırma
==============================================

Derlenmiş Wasm modülü, binary oluşturan makineden bilgi içerebilir; bu ``.cargo``
dizininin mutlak yolu gibi bilgiler olabilir.

Çoğu insan için bu hassas bir bilgi değildir, ancak bunu bilmeniz önemlidir.

Linux'ta PATH’ler çalıştırılarak incelenebilir:

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: Çözüm

İdeal çözüm, bu yolu tamamen kaldırmak olacaktır, fakat bu genel önemli bir
görevdir ve dikkat edilmelidir..

Sözleşmeyi derlerken ``--remap-path-prefix`` bayrağını kullanarak sorunu
çözmek mümkündür.
Unix benzeri sistemlerde bayrak, ``RUSTFLAGS`` ortam değişkeni kullanılarak doğrudan
``cargo concordium``' çağrısına geçirilebilir:

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

Bu, kullanıcıların home path’ini boş dizeyle değiştirir. Diğer PATH ler de
benzer bir şekilde değiştirilebilir. Genel olarak ``--remap-path-prefix=from=to``
kullanılması, herhangi bir gömülü yolun başında ``from`` ile ``to``' arasında
eşleşecektir..

Bayrak, Yapılandırma bölümünde; Kütüphanenizdeki ``.cargo/config``
dosyasini da kalici olarak ayarlayacaktır:

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

`<user>`, wasm modülünü oluşturan kullanıcıyla değiştirilmelidir.

Uyarılar
---------

Rust araç zinciri için ``rust-src`` bileşeni yüklü ise, yukarıdakiler sorunu
muhtemelen çözmeyecektir. Bu bileşen, rust-analyzer_ gibi bazı Rust araçları
tarafından gereklidir..

.. seealso::

   ``--remap-path-prefix``  ve ``rust-src`` ile sorunu bildiren bir sorun için bkz:
   https://github.com/rust-lang/rust/issues/73167
