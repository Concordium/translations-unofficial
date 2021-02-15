.. _contract-moduletr:

==========================
Akıllı sözleşme modülleri
==========================

Akıllı sözleşmeler, zincirde *akıllı sözleşme modüllerine* yüklenir.

.. note::

   Akıllı sözleşme modülü genellikle sadece *modül* olarak isimlendirilir.

Bir modül, kodun sözleşmeler arasında paylaşılmasına izin veren bir veya daha fazla akıllı sözleşme içerebilir
ve isteğe bağlı olarak şunları içerebilir :ref:`contract schemas<contract-schema>`


.. graphviz::
   :align: center
   :caption: İki akıllı sözleşme içeren akıllı bir sözleşme modülü.

   digraph G {
       subgraph cluster_0 {
           node [fillcolor=white, shape=note]
           label = "Module";
           "Crowdfunding";
           "Escrow";
       }
   }

Modül, bağımsız olmalı ve yalnızca zincirle etkileşime izin veren kısıtlı bir
girdi listesine sahip olmalıdır. Bunlar, bilgisayar ortamı tarafından sağlanır ve
``concordium`` adlı girdi modülü tarafindan akıllı sözleşme için kullanılabilir
hale getirilir.

.. seealso::

   Referansların tamamı için :ref:`host-functions`'a göz atın.

Zincir üzerindeki programlama dili
====================================

Concordium blok zincirinde akıllı sözleşme dili, taşınabilir bir derleme hedefi
olan ve izole ortamlarda çalıştırılmak üzere tasarlanmış olan `Web Assembly`_ (Kisaca Wasm)
dilinin alt kümesidir. Ağdaki koda güvenmeleri gerekmeyen fırıncılar tarafından bu
kod çalistirilacağından, Wasm oldukça yararlıdır.

Wasm düşük seviyeli bir dildir ve elle yazmak pratik değildir. Bunun yerine,
daha yüksek seviyeli bir dilde akıllı sözleşmeler yazılabilir ve bunlar daha
sonra Wasm'a derlenebilir.

.. _wasm-limitationstr:

Limitler
-----------

.. todo::

   Add other limitations, such as start sections...

Blok zinciri ortamı, her düğümün sözleşmeyi tamamen aynı şekilde, tamamen aynı
miktarda kaynak kullanarak yürütebilmesi açısından çok özeldir. Aksi takdirde
düğümler, zincirin durumu hakkında fikir birliğine varamazlar. Bu nedenle akıllı
sözleşmelerin sınırlı bir Wasm alt kümesine yazılması gerekir.

Kayan nokta sayıları
^^^^^^^^^^^^^^^^^^^^^^

Wasm'ın kayan nokta sayılarını desteklemesine rağmen, akıllı bir sözleşmenin
bunları kullanmasına izin verilmez. Bunun nedeni, Wasm kayan noktalı sayıların
özel bir ``NaN`` ("not a number")(Sayi olmayan bir değer) değerine sahip olabilmesidir
ve bu değerin işlenmesi belirsizlikle sonuçlanabilir.

Kısıtlama statik olarak uygulanır, yani akıllı sözleşmeler kayan nokta türleri
içeremez veya kayan nokta değerleri içeren herhangi bir talimat içeremez.


Dağıtım
==========

Zincire bir modül yüklemek, modül bayt kodunu Concordium ağına bir işlem olarak
göndermek anlamına gelir. Eger *Geçerli* bir işlem ise bir bloğa dahil edilecektir.
Bu işlem, diğer her işlem gibi, ilişkili bir maliyete sahiptir. Bu işlemin maliyeti,
bayt kodunun boyutuna bağlıdır ve hem modülün geçerliliğini kontrol etmek hem de
zincir üzerinde depolamak için ücretlendirilir.

Yükleme işleminin kendisi akıllı sözleşme çalıştırmaz. Çalıştırmak için,
kullanıcı önce bir kontrat *örneğini* oluşturmalıdır.

.. seealso::

   Daha fazla bilgi için bkz :ref:`contract-instancestr`.

.. _smart-contracts-on-chaintr:

.. _smart-contracts-on-the-chaintr:

.. _contract-on-chaintr:

.. _contract-on-the-chaintr:

Zincir üzerindeki akıllı sözleşmeler
======================================

Zincir üzerindeki akıllı sözleşme, yüklenmiş bir modülden dışa aktarılan işlevlerin
bir koleksiyonudur. Bunun için kullanılan somut mekanizma `Web Assembly`_ dışa
aktarma bölümüdür. Akıllı bir sözleşme, yeni örnekleri başlatmak için bir fonksiyonu
dışa aktarmalıdır fakat bu örneği güncellemek için herhangi bir fonksiyonu aktarmasina
gerek olmayabileceği gibi daha fazla işlevi dışa aktarmasıda gerekebilir.

Bir akıllı sözleşme modülü birden fazla farklı akıllı sözleşme için işlevleri dışa aktarabildiğinden,
işlevleri bir isimlendirme şeması kullanarak birbirleri ile ilişkilendiririz:

- ``init_<contract-name>``: Bir akıllı sözleşmeyi başlatma işlevi, ``init_`` ile başlamalı ve ardından akıllı sözleşmenin bir adı gelmelidir. Sözleşme yalnızca ASCII alfasayısal karakterlerden veya noktalama işaretlerinden oluşmalıdır ve ``.`` Sembolünü içermesine izin verilmez.

- ``<contract-name>.<receive-function-name>``: Akıllı bir sözleşmeyle etkileşimde bulunacak fonksiyon isminin önüne sözleşme adı gelmeli, ve aralarinda ``.`` sembolu bulunmalıdır. Init fonksiyonların da oldugu gibi, sözleşme adının ``.`` sembolünü içermesine izin verilmez.

.. note::

   Rust dilini ve ``concordium-std`` kullanarak akıllı sözleşmeler geliştiriyorsanız,
   prosedurel makrolar olan ``#[init(...)]`` ve ``#[receive(...)]`` doğru adlandırma
   şemasını düzenler.

.. _Web Assembly: https://webassembly.org/
