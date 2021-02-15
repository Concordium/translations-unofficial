.. _list of types implementing the SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schematr:

=============================
Bir sözleşme şeması oluşturma
=============================

Bu kılavuz, ``cargo-concordium`` kullanarak akıllı sözleşme şemasının nasıl
oluşturulacağını, dosyaya nasıl aktarılacağını ve / veya şemanın akıllı sözleşme
modülüne nasıl yerleştirileceğini gösterecektir.

Hazırlık
===========

Öncelikle, ``cargo-concordium`` kurulu olduğundan emin olun. Kurulu değilse
:ref:`setup-toolstr` dokümanı size yardımcı olacaktır.

Ayrica şema oluşturmak istediğiniz akıllı sözleşmenin Rust kaynak koduna
da ihtiyacınız olacaktır.

Bir şema için gerekli sözleşmeyi ayarlayın
===========================================

Bir sözleşme şeması oluşturmak için önce akıllı sözleşmemizi hazırlamalıyız.

Akıllı sözleşmemizin hangi kısımlarının şemaya dahil edileceğini seçebiliriz.
Sözleşme durumu şeması ve / veya başlatma (init) ve alma fonksiyonlarinin her
bir parametresini seçenek olarak ekleyebiliriz.

Şemaya dahil etmek istediğimiz her tür, ``SchemaType`` özelliğini uygulamalıdır.
Bu, tüm temel türler ve diğer bazı türler için zaten yapılmıştır (bkz. `list of
types implementing the SchemaType`_). çoğu durumda, ``#[derive(SchemaType)]``
kullanılarak otomatik olarak da elde edilebilir. ::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

``SchemaType`` özelliğini manuel olarak uygulamak, yalnızca bu türün bayt olarak
nasıl temsil edildiğini ve nasıl temsil edileceğini açıklayan bir ``schema::Type``
için alıcı olan bir fonksiyonun belirtilmesini gerektirmektedir.

.. todo::

   Create an example showing how to manually implement ``SchemaType`` and link
   to it from here.

Sözleşme durumu dahil etme
----------------------------

Sözleşme durumu için şema üretmek ve dahil etmek için kullanacağımız
``#[contract_state(contract = ...)]`` makrosunu şu şekilde kullanabiliriz::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

Veya daha basit olarak; eğer sözleşme durumu zaten ``SchemaType`` uygulayan
bir türde ise, u32 kullanılabilir::

   #[contract_state(contract = "my_contract")]
   type State = u32;

Fonksiyon parametreleri dahil etme
-------------------------------------

Başlangıç (init) ve alma fonksiyonlarının parametreleri ile şema üretmek ve dahil
etmek için; ``#[init(..)]`` - ve ``#[receive(..)]``-macro için isteğe bağlı
``parameter`` özniteliğini ayarlıyoruz.::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }

Şema oluşturmak
===================

Şimdi, `` cargo-concordium '' kullanarak gerçek şemayı oluşturmaya hazırız bu
aşamada şemayı yerleştirme ve / veya şemayı bir dosyaya yazma seçeneklerimiz de var.

.. seealso::

   Bu konuda daha fazla bilgi almak için bkz.
   :ref:`here<contract-schema-which-to-choosetr>`.

Şemayı yerleştirme (Embedding)
--------------------------------

Şemayı akıllı sözleşme modülüne yerleştirmek için build komutuna
``--schema-embed`` ekliyoruz.

.. code-block:: console

   $cargo concordium build --schema-embed

Eğer işlem başarılı olursa, komutun çıktısı size şemanın bayt cinsinden
toplam boyutunu söyleyecektir.

Bir şema dosyası çıktı alma
------------------------------

Şemayı dosyaya çıkarmak için, ``--schema-out=FILE``  parametresini
kullanabiliriz; burada ``FILE``, oluşturulacak dosyanın PATH’i ve ismi olacaktir.:

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
