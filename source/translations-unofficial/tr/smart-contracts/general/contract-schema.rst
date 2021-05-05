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

.. _contract-schematr:

=========================
Akıllı sözleşme şemaları
=========================

Akıllı sözleşme şeması, baytların daha yapılandırılmış bir sunumda nasıl temsil
edileceğinin bir açıklamasıdır. Bu sema; bir akıllı sözleşme örneğinin durumunu
görüntülemek ve JSON gibi yapılandırılmış bir sunum kullanarak parametreleri
belirlemek için harici araçlar tarafından kullanılabilir.

.. seealso::

   Rust dilinde akıllı bir sözleşme modülü için şemanın nasıl oluşturulacağına ilişkin talimatlar için bkz :ref:`build-schematr`.

Neden bir sözleşme şeması kullanmalı?
===========================================

Bir örneğin durumu, başlatma (init) ve alma fonksiyonlarina aktarılan parametreler gibi blok zincirindeki veriler, bir bayt
dizisi olarak serileştirilir. Serileştirme, insan tarafından okunabilirlik yerine verimlilik için optimize edilmiştir

.. todo::

   Consider rewriting this subsection as it can be somewhat difficult to
   understand; in particular, possibly just say that for convenience, the user
   can pass unserialized data into a function as long as they also provide a
   schema that spells out how to (de)serialize the data.

Genellikle bu baytların yapısı vardır ve bu yapı, sözleşme fonksiyonlarının bir
parçası olarak akıllı sözleşme tarafından bilinir, ancak bu fonksiyonların dışında
baytları anlamak zor olabilir. Bu, özellikle bir sözleşme örneğinin karmaşık bir
durumunu incelerken veya karmaşık parametreleri bir akıllı sözleşme fonksiyonlarına
aktarırken geçerlidir. İkinci durumda, baytlar ya yapılandırılmış verilerden
serileştirilmeli ya da manuel olarak yazılmalıdır.

Baytların manuel olarak ayrıştırılmasını önlemenin çözümü, bu bilgileri baytlardan
nasıl yapı yapılacağını açıklayan ve harici araçlar tarafından kullanılabilen bir
*akıllı sözleşme şemasında* yakalamaktır.

.. note::

   ``concordium-client`` uygulaması sözleşme örneklerinin durumunu JSON'a serileştirmek
   için bir şema kullanabilir :ref:`serialize JSON parameters<init-passing-parameter-jsontr>`

Şema daha sonra zincire yerleştirilen bir akıllı sözleşme modülüne yerleştirilir
veya bir dosyaya yazılır ve zincir dışına aktarılır.

Bir dosyaya eklemeli mi yoksa yazmalı mısınız?
===============================================

Bir sözleşme şemasının bir dosyaya gömülmesi veya yazılması, durumunuza bağlıdır.

Şemayı akıllı sözleşme modülüne yerleştirmek, şemayı sözleşmeyle birlikte dağıtır
ve doğru şemanın kullanılmasını sağlar ve ayrıca herkesin doğrudan kullanmasına
izin verir. Olumsuz yanı, akıllı sözleşme modülünün boyut olarak daha büyük hale
gelmesi ve bu nedenle dağıtımının daha pahalı olmasıdır. Ancak akıllı sözleşme,
durum ve parametreler için çok karmaşık türler kullanmadıkça, şemanın boyutu,
akıllı sözleşmenin boyutuna kıyasla muhtemelen ihmal edilebilir olacaktır.

Şemayı ayrı bir dosyada bulundurmak, dağıtım sırasında fazladan bayt ödemeden
şemaya sahip olmanızı sağlar. Bunun dezavantajı, şema dosyasını başka bir kanal
üzerinden dağıtmanız ve sözleşmeli kullanıcıların akıllı sözleşmemizle doğru
dosyayı kullanmasını sağlamanız gerektiğidir.

Şema biçimi
=================

.. todo::

   Clarify whether we talk about *any* abstract schema that a user could implement,
   or a specific schema supplied by Concordium. Then only talk about one or the other,
   or at least clearly separate the discussion of those.

Bir şema şunları içerebilir:

- akıllı sözleşme modülü için yapı bilgileri
- akıllı sözleşme durumunun açıklaması
- akıllı bir sözleşmenin başlatma (init) ve alma fonksiyonları için parametreler.

Bu açıklamaların her birine *şema türü* adı verilir. Şema türlerinin bir şemaya
dahil edilmesi her zaman isteğe bağlıdır.

Şu anda desteklenen şema türleri, Rust programlama dilinde yaygın olarak
kullanılanlardan esinlenmiştir:

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


Burada, ``SizeLength``, ``List`` gibi bir değişken uzunluk türünün uzunluğunu
tanımlamak için kullanılacak bayt miktarını açıklar.

.. code-block:: rust

   enum SizeLength {
       One,
       Two,
       Four,
       Eight,
   }

Bir şema türünün bayt olarak nasıl serileştirildiğine ilişkin daha fazla bilgi
için, `implementation in Rust`’ı okumanızı öneririz.

.. _contract-schema-which-to-choosetr:

Zincir üzerine şemaları gömme
=================================

Şemalar, Wasm modüllerinin `custom section`_  özelliğini kullanarak akıllı
sözleşme modüllerine yerleştirilir. Bu, Wasm modüllerinin, Wasm modülünü
çalıştırmanın anlamını etkilemeyen adlandırılmış bir bayt bölümünü
içermesine olanak tanımaktadır.

Tüm şemalar toplanır ve ``concordium-schema-v1`` adlı özel bir bölüme eklenir.
Bu koleksiyon, UTF-8'de kodlanmış sözleşmenin adını ve sözleşme şeması
baytlarını içeren çiftlerin bir listesidir.
