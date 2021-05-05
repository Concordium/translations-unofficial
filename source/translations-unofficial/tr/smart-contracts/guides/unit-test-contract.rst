.. _unit-test-contracttr:

==============================================
Rust'ta bir sözleşme için birim testi yapma
==============================================

Bu kılavuz size Rust'ta yazılmış akıllı bir sözleşme için birim testlerinin
nasıl yazılacağını gösterecektir.
Akıllı sözleşme Wasm modülünü test etmek için bkz :ref:`local-simulatetr`.

Rust'ta akıllı sözleşme bir kütüphane olarak yazılır ve ``#[test]`` özelliğiyle
işlevlere açıklama ekleyerek onu bir kütüphane gibi test edebiliriz.

.. code-block:: rust

    // contract code
    ...

    #[cfg(test)]
    mod test {

        #[test]
        fn some_test() { ... }

        #[test]
        fn another_test() { ... }
    }

Testin çalıştırılması ``cargo`` kullanılarak yapılabilir:

.. code-block:: console

   $cargo test

Varsayılan olarak, bu komut sözleşmeyi derler ve yerel hedefiniz (büyük olasılıkla ``x86_64``)
için makine kodunu test eder ve ardından çalıştırır. Bu tür testler, ilk geliştirmede
ve fonksiyonel doğruluğun test edilmesinde yararlı olabilir.

Kapsamlı testler için hedef platformu dahil etmek önemlidir. örn: `Wasm32`
Platformlar arasında, bir sözleşmenin davranışını değiştirebilecek bazı ince
farklar vardır.
Örneğin bir fark, işaretçilerin boyutuyla ilgilidir; burada `Wasm32`, çoğu
platform için ortak olan sekiz bayt yerine dört bayt kullanır.

Birim testleri yazma
=====================

Birim testleri genellikle üç bölümden oluşan bir yapıyı izler: bir durum hazirlanmasi,
bazı kod birimlerinin çalıştırılması ve kodun durumu ve çıktısı hakkında çıkarımlarda
bulunulması.

Kontrat fonksiyonları ``#[init(..)]``  veya ``#[receive(..)]`` kullanılarak yazılırsa,
bu fonksiyonları doğrudan birim testinde test edebiliriz..

.. code-block:: rust

   use concordium_std::*;

   #[init(contract = "my_contract", payable, enable_logger)]
   fn contract_init(
      ctx: &impl HasInitContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
   ) -> InitResult<State> { ... }

   #[receive(contract = "my_contract", name = "my_receive", payable, enable_logger)]
   fn contract_receive<A: HasActions>(
      ctx: &impl HasReceiveContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
      state: &mut State,
   ) -> ReceiveResult<A> { ... }

Fonksiyon argümanları için test saplamaları, ``test_infrastructure`` adı verilen
``concordium-std`` alt modülünde bulunur..

.. seealso::

   Daha fazla bilgi ve örnek için concordium-std belgelerine bakın.

.. todo::

   Show more of how to write the unit test

Wasm'da testleri çalıştırma
=============================

Testlerin yerel makine koduna derlenmesi çoğu durumda yeterlidir, ancak testleri
Wasm olarak derlemek ve düğümler tarafından kullanılan yorumlayıcıyı kullanarak
çalıştırmak da mümkündür.
Bu durum, test ortamını zincir üzerindeki çalışma ortamına daha yakın hale getirir
ve bazı durumlarda daha fazla hata yakalanmasına yardımcı olabilir.

Geliştirme aracı ``cargo-concordium`` Wasm için bir test çalıştırıcısı içermektedir.
Bu çalıştırıcı Concordium düğümlerinde de bulunan Wasm yorumlayıcısı ile aynıdır.

.. seealso::

   ``cargo-concordium`` un nasıl kurulacağına dair :ref:`setup-toolstr` kılavuzu'na bakabilirsiniz.

Birim testi, ``#[test]`` yerine ``#[concordium_test]`` ile yapılmalıdır ve ``#[cfg(test)]``
yerine ``#[concordium_cfg_test]`` kullanılmasını önermekteyiz:

.. code-block:: rust

   // contract code
   ...

   #[concordium_cfg_test]
   mod test {

       #[concordium_test]
       fn some_test() { ... }

       #[concordium_test]
       fn another_test() { ... }
   }

``#[concordium_test]`` makrosu, ``concordium-std`` ``wasm-test`` özelliği ile
derlendiğinde testlerimizi Wasm'da çalıştırılacak şekilde ayarlar. Bu ozellik
kullanilmadigi durumda; ``#[test]`` gibi davranir , yani ``cargo test`` kullanarak
yerel kodu hedefleyen birim testlerini yapmak hala mümkün olacaktır.

Benzer şekilde, ``#[concordium_cfg_test]`` makrosu, ``wasm-test`` ile
``concordium-std`` yapılandırırken modülümüzü de ekler, aksi takdirde ``#[test]``
gibi davranir ve testleri yapılandırmaya eklediğimizde kontrol edebilmemizi sağlar.

Testler artık aşağıdakiler kullanılarak oluşturulabilir ve çalıştırılabilir:

.. code-block:: console

   $cargo concordium test

Bu komut, ``concordium-std`` için etkinleştirilen ``wasm-test`` özelliği ile Wasm
testlerini derler ve ``cargo-concordium``’dan test çalıştırıcısını kullanır.

.. warning::

   ``panic!``’den gelen hata mesajları ve dolayısıyla``assert!``’İn farklı
   varyasyonları da Wasm'a derlenirken *gösterilmez*..

   Bunun yerine, test yaparken cikarimlarda bulunabilmek için ``fail!`` ve ``claim!``
   varyantlarını kullanın, çünkü rapor testte başarısız olmadan *önce* test
   çalıştırıcısına hata mesajlarını geri gönderir.
   Her ikisi de ``concordium-std`` nin bir parçasıdır.

.. todo::

   Use link concordium-std: docs.rs/concordium-std when crate is published.
