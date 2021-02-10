.. _contract-instances:

==========================
Akıllı sözleşme örnekleri
==========================

.. todo::

   - Clarify how instances relate to smart contracts relate to modules
     (e.g., right now it says that an instance is a module + state but the
     picture below shows instances as contracts + state).
   - Decide how exactly we should define smart-contract modules (as its own concept
     or as Wasm modules), and whether we should talk about them at all.
   - Decide whether we should have a concrete code example, and whether it should
     be in Wasm or Rust or perhaps pseudocode.
   - Consider having a picture that explains the relationship between modules and instances.

Bir **akıllı sözleşme örneği**, belirli bir durum ve bir miktar GTU ile birlikte akıllı bir
sözleşme modülüdür.Aynı modülden birden fazla akıllı sözleşme örneği oluşturulabilir.
Örneğin, bir :ref:`auction <auction>` sözleşmesi için, her biri belirli bir öğe için
ve kendi katılımcıları ile teklif vermeye adanmış birden çok örnek olabilir.


Akıllı sözleşme örnekleri, akıllı sözleşme modülünde istenen işlevi çağıran ``init``
işlemi aracılığıyla yüklenmiş bir :ref:`smart contract module<contract-module>` nden
oluşturulabilir. Bu fonksiyon bir parametre alabilir.
Nihai sonucun, örneğin, ilk akıllı sözleşme durumunda olması gerekir.

.. note::

   Akıllı sözleşme örneği genellikle sadece *örnek* olarak adlandırılır.

.. graphviz::
   :align: center
   :caption: İki adet akıllı sözleşme içeren akıllı sözleşme modülü örneği:
             Emanet ve Kitle Fonlaması. Her sözleşmenin iki örneği vardır.

   digraph G {
       rankdir="BT"

       subgraph cluster_0 {
           label = "Modül";
           labelloc=b;
           node [fillcolor=white, shape=note]
           "Kitle fonlaması";
           "Emanet";
       }

       subgraph cluster_1 {
           label = "Örnekler";
           style=dotted;
           node [shape=box, style=rounded]
           Ev;
           Araba;
           Aygıtlar;
           "Masa oyunu";
       }

       Ev:n -> Emanet;
       Araba:n -> Emanet;
       Aygıtlar:n -> "Kitle fonlaması";
       "Masa oyunu":n -> "Kitle fonlaması";
   }

Akıllı sözleşme örneğinin durumu
==================================

Akıllı sözleşme örneğinin durumu iki bölümden oluşur; kullanıcı tanımlı durum ve
sözleşmenin sahip olduğu GTU miktarı, yani *bakiye*. Durumdan bahsederken
tipik olarak yalnızca kullanıcı tanımlı durumu kastediyoruz. GTU tutarını ayrı
ayrı ele almanın nedeni, GTU'nun yalnızca ağın kurallarına göre harcanması ve
alınabilmesidir, örneğin, sözleşmeler GTU jetonları yaratamaz veya yok edemez.

.. _contract-instances-init-on-chain:

Zincir üzerinde akıllı sözleşme örneği oluşturmak
=======================================================

Her akıllı sözleşme, akıllı sözleşme örnekleri oluşturmak için bir fonksiyon içermelidir.
Bu fonksiyonlara *init fonksiyonu* denir.

Bir akıllı sözleşme örneği oluşturmak için, bir hesap, yüklenmiş akıllı sözleşme
modülüne ve örnekleme için kullanılacak *init fonksiyonu* adına bir referansla özel bir işlem gönderir.

İşlem aynı zamanda akıllı sözleşme örneğinin bakiyesine eklenen bir GTU miktarını da içerebilir ve
İşlemin bir parçası olarak bir bayt dizisi biçiminde işleve bir parametre sağlanır.

Özetlemek gerekirse, işlem şunları içerir:

- Akıllı sözleşme modülüne referans..
- init fonksiyonunun adı.
- init fonksiyonunun parametresi.
- Örnek için GTU miktarı.

İnit fonksiyonu, bu parametrelerle yeni bir örnek oluşturmak istemediğini işaret edebilir.
Eğer İnit fonksiyonu parametreleri kabul ederse, örneğin başlangıç durumunu ve bakiyesini
ayarlar. Örneğe zincir üzerinde bir adres verilir ve işlemi gönderen hesap, örneğin sahibi
olur. Eğer fonksiyon işlemi reddederse, hiçbir örnek oluşturulmaz ve yalnızca örneği
yaratmaya yönelik işlem zincir üzerinde görünür.

.. seealso::

   Bir sözleşmenin nasıl başlatılacağı için :ref:`initialize-contract` kılavuzuna bakın..

Örnek durumu
==============

Her akıllı sözleşme örneği, kendi durumunu zincir üzerinde bir bayt dizisi olarak tutar.
Örnek, durumu okumak, yazmak ve yeniden boyutlandırmak için bilgisayar ortamı tarafından
sağlanan fonksiyonları kullanır.

.. seealso::

   Bu fonksiyon referansı için bkz :ref:`host-functions-state`.

Akıllı sözleşme durum boyutu sınırlıdır. Şu anda akıllı sözleşme durum boyutu 16KiB ile
sinirlandirilmistir.

.. seealso::

   Bu konuyla ilgili daha fazla bilgi için :ref:`host-functions-state` sayfasına bakın.

Bir örnekle etkileşim kurma
============================

Akıllı bir sözleşme, *alma fonksiyonları* olarak adlandırılan bir örnekle etkileşim için,
birden çok fonksiyon ortaya çıkarabileceği gibi hiç fonksiyonda cikarmayabilir.

Tıpkı init fonksiyonlarinda olduğu gibi, alma fonksiyonlari, sözleşme için bir miktar
GTU ve bayt biçiminde işleve yönelik bir argüman içeren işlemler kullanılarak tetiklenir.

Özetlemek gerekirse, akıllı sözleşme etkileşimi için bir işlem şunları içerir::

- Akıllı sözleşme örneğinin adresi.
- Alma fonksiyonunun adı.
- Alma fonksiyonu parametresi.
- Örnek için GTU miktarı. 

.. _contract-instance-actions:

Olayları günlüğe kaydetme
============================

.. todo::

   Explain what events are and why they are useful.
   Rephrase/clarify "monitor for events".

Akıllı sözleşme fonksiyonlarının yürütülmesi sırasında olaylar günlüğe kaydedilebilir.
Bu, hem init hem de alma fonksiyonları için geçerlidir. Günlükler zincir dışı kullanım
için tasarlanmıştır, böylece zincirin dışındaki aktörler olayları izleyebilir ve
bunlara tepki verebilir. Günlüklere akıllı sözleşmeler veya zincirdeki başka herhangi
bir aktör erişilemez. Olaylar, bilgisayar tarafından sağlanan bir fonksiyon kullanılarak
günlüğe kaydedilebilir.

.. seealso::

   Bu fonksiyonun referansı için bkz :ref:`host-functions-log`

Bu olay günlükleri fırıncılar tarafından tutulur ve işlem özetlerine dahil edilir.

Bir olayın günlüğe kaydedilmesinin, sözleşmenin durumuna yazma maliyetine benzer
bir maliyeti vardır. Çoğu durumda, maliyeti düşürmek için yalnızca birkaç bayt
kaydetmek mantıklı olacaktır..

.. _action-descriptions:

Eylem açıklamaları
===================

Alma fonksiyonu, zincir üzerindeki bilgisayar ortamı tarafından yürütülecek
*eylemlerin açıklamasını* döndürür.

Bir sözleşmenin üretebileceği olası eylemler şunlardır:

- **Accept** (Kabul et) her zaman başarılı olan bir eylemdir.
- **Simple transfer** (Basit aktarım)  GTU'nun örnekten belirtilen hesaba aktarımı.
- **Send** (Gönder): belirtilen akıllı sözleşme örneğinin alma fonksiyonunu çağırır, ve isteğe bağlı olarak GTU'ları gönderen örnekten alıcı örneğe aktarir

Eğer bir eylem yürütülemezse, alma işlevi geri döndürülür ve örneğin durumu ve
bakiyesi değişmeden kalır. Ancak,

- alma işlevini tetikleyen (başarısız)(unsuccessful) işlem yine de zincire eklenir ve
- başarısız eylemi gerçekleştirme maliyeti dahil işlem maliyeti, gönderen hesaptan düşülür.

Birden çok işlem açıklamasını işleme
---------------------------------------

**and** (ve) birleştiricisini kullanarak eylem açıklamalarını zincirleyebilirsiniz.
Bir eylem açıklaması dizisi ``A`` **and** ``B``


1) Çalıştır ``A``.
2) Eğer ``A`` başarılı olursa, Çalıştır ``B``.
3) Eğer ``B`` başarısız olursa tüm eylem dizisi başarısız olur (ve ``A`` nın sonucu geri alınır)..

Hataları işleme
-----------------

Önceki bir eylemin başarısız olması durumunda bir eylemi gerçekleştirmek için
**or** (veya) birleştiriciyi kullanın. Eylem açıklaması ``A`` **or** ``B``

1) Çalıştır ``A``.
2) Eğer ``A`` başarılı olursa, çalıştırmayı durdur.
3) Eğer ``A`` başarısız olursa, Çalıştır ``B``.

.. graphviz::
   :align: center
   :caption: Alice'e ve ardından Bob'a aktarmaya çalışan bir eylem açıklaması örneği,bunlardan herhangi biri başarısız olursa, bunun yerine Charlie'ye aktarmayı deneyecektir.

   digraph G {
       node [color=transparent]
       or1 [label = "Veya"];
       and1 [label = "Ve"];
       transA [label = "Transfer x den Alice'e"];
       transB [label = "Transfer y den Bob'a"];
       transC [label = "Transfer z den Charlie'ye"];

       or1 -> and1;
       and1 -> transA;
       and1 -> transB;
       or1 -> transC;
   }

.. seealso::

   Eylemlerin nasıl oluşturulacağına ilişkin referans için bkz :ref:`host-functions-actions`.

Eylem ağacının tamamı **atomik olarak** yürütülür ve, ya tüm ilgili örneklerde ve hesaplarda
güncellemelere ya da reddedilmesi durumunda uygulama için ödeme yapılmasına yol açar, ancak
başka hiçbir değişiklik yapılmaz. Başlatan işlemi gönderen hesap, tüm ağacın yürütülmesi
için ödeme yapar.
