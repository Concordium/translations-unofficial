.. _no-std:

======================================
``no_std`` kullanarak yapılandırma
======================================

Bu kılavuz, Rust akıllı sözleşmeniz için ``no_std``’yı nasıl etkinleştireceğinizi
ve sonucunda ortaya çıkan Wasm modülünün boyutunun potansiyel olarak birkaç
kilobayt azaltacağını gösterir.

Hazırlık
===========

``std`` özelliği olmadan ``concordium-std`` derlemek, ``rustup`` kullanılarak
kurulabilen Rust nightly uygulamalarını kullanılmasını gerektirir:

.. code-block:: console

   $rustup toolchain install nightly

``no_std`` icin gerekli modülü kurma
========================================

``concordium-std`` kitaplığı, Rust standart kitaplığının kullanılmasını sağlayan
bir ``std`` özelliğini ortaya çıkarır ve bu özellik varsayılan olarak aktif
haldedir.

Bu ozelligi devre dışı bırakmak için, modülünüzün bağımlılıklarındaki
``concordium-std`` için varsayılan özellikleri devre dışı bırakmanız yeterlidir.

.. code-block:: rust

   [dependencies]
   concordium-std = { version: "=0.2", default-features = false }

Std ile ve std olmadan modülleleriniz arasında geçiş yapabilmek için, kendi
modülünüze ``std`` ekleyin. Bu ``concordium-std`` nin ``std`` özelliğini
etkinleştirecektir:

.. code-block:: rust

   [features]
   std = ["concordium-std/std"]

Bu özellik, her akıllı sözleşme modülü için  ``std``  nin varsayılan olarak
etkinleştirildiği akıllı sözleşme örneklerinin kurulumudur.

Modülü oluşturmak
===================

Nightly uygulamalarını kullanmak için, ``cargo`` dan hemen sonra ``+nightly``
ekleyin:

.. code-block:: console

   $cargo +nightly concordium build

Eğer kendi akıllı sözleşme modülünüzün varsayılan özelliklerini devre dışı
bırakmak istiyorsanız, ``cargo`` için ekstra argümanlar ekleyebilirsiniz:

.. code-block:: console

   $cargo +nightly concordium build -- --no-default-features
