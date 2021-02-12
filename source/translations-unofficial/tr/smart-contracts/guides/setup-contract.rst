.. highlight:: toml

.. _setup-contract:

===================================
Bir akıllı sözleşme projesi kurma
===================================

Rust dilindeki bir akıllı sözleşme, standart Rust kütüphaneleri kullanılarak yazılır.
Kütüphane sonrasında Rust kullanarak Wasm'a derlenir
``wasm32-unknown-unknown`` ve, sadece bir Rust kütüphanesi olduğu sürece, bağımlılık yönetimi için
Cargo_ kullanılabilir.

Yeni bir akıllı sözleşme projesi kurma, bir proje klasörü yaratılarak başlar. Terminal üzerinde bu proje klasörünün içine girilir ve aşağıdaki komut çalıştırılır:

.. code-block:: console

   $cargo init --lib

Bu komut, varsayılan Rust proje kütüphanelerini yeni birkaç dosya ve klasör oluşturarak kuracak.
Terminalde bulunula klasör kontrol edildiğinde bir ``Cargo.toml`` dosyası ve bir ``src``
klasörü ve bazı gizli dosyalar olduğu görülecektir.

Wasm'ın derlenebilmesi için, cargo'ya doğru ``crate-type`` belirtilmelidir.
``Cargo.toml`` dosyasına aşağıdaki satırlar eklenerek bu yapılır::

   [lib]
   crate-type = ["cdylib", "rlib"]

Standart akıllı sözleşme kütüphanelerinin eklenmesi
=====================================================

Bu adımda gerekli olan ``concordium-std`` eklenecektir.
Bu, Rust dilinde kısa ve etkili bir akıllı sözleşmenin yazılabilmesi için gerekli olan
prosedürel makro ve fonksiyonları içermektedir.

Kütüphane, ``Cargo.toml`` dosyası açılarak  ``[dependencies]`` alanının içine ``concordium-std = "*"`` (tercihen, `*` yerine`concordium-std`_ son versiyonu yazılmalıdır) satırı eklenerek tanımlanır::

   [dependencies]
   concordium-std = "0.4"

Kütüphanenin tüm dökümantasyonuna docs.rs_ linkinden ulaşılabilinir.

.. note::

   Eğer bu kütüphanenin değiştirilmiş bir versiyonu kullanılmak istenirse, ``concordium-std`` kopyalanamalı ve ``Cargo.toml`` dosyasına, yukarıda belirtilen satır yerine, aşağıdaki satır eklenmelidir.::

      [dependencies]
      concordium-std = { path = "./path/to/concordium-std" }

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rustup: https://rustup.rs/
.. _repository: https://gitlab.com/Concordium/concordium-std
.. _docs.rs: https://docs.rs/crate/concordium-std/
.. _`concordium-std`: https://docs.rs/crate/concordium-std/

İşlem tamam! Kendi akıllı sözleşmenizi geliştirmeye başlayabilirsiniz.
