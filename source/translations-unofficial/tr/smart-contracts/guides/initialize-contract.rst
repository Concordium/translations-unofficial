.. _initialize-contract:

====================================
Akıllı sözleşme örneğini başlatma
====================================

Bu kılavuz, dağıtımı yapılmış bir akıllı sözleşme üzerinden JSON veya binary (2li sistem) 
formatında yeni bir akıllı sözleşmenin nasıl başlatıcağını gösterecektir.
Ek olarak, bu sözleşme örneğinin isimlendirilmesi de gösterilecektir.

Ön hazırlık
============

:ref:`Concordium<downloads>` yazılımının son versiyonu ile :ref:`çalışan bir düğümün (node)<run-a-node>` ve 
zincir üzerinde :ref:`dağıtılmış bir akıllı sözleşme modülün <deploy-module>` olduğundan emin olunuz.

Bir akıllı sözleşme başlatılması, bir tür “işlem (transaction)” olması sebebiyle,
bu işlemi yapmayı ödeyebilecek miktarda GTU’ya sahip bir hesap ile 
``concordium-client`` kullanılmalıdır.

.. note::

   İşlem masrafı, init işlevine gönderilen parametrelerin boyutuna
   ve işlevin kendisinin karmaşıklığına bağlıdır.

Başlatma
=========

1000'e kadar NRG'nin kullanılmasına izin veren ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` 
referans numaralı dağıtılmış bir modülden parametresiz akıllı sözleşmenin bir örneğini ``my_contract`` 
başlatmak için aşağıdaki komut çalıştırılır:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

Eğer yukarıdaki işlem başarılı sonuçlanırsa, çıktı aşağıdaki gibi olmalıdır:

.. todo::

   Update address format to ``<i, s>`` from ``{"index": i, "subindex": s}``
   (throughout the documentation).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Bu mesaj, zincir üzerinde yeni bir sözleşme örneğinin gösterilen adres ile 
başarılı bir şekilde yaratıldığı anlamına gelmektedir.

.. seealso::

   Sözleşme başlatmayı daha iyi anlayabilmek için
   :ref:`contract-instances-init-on-chain` dokümanına bakabilirsiniz.

   Modül referansları ve örnek adresler hakkında daha fazla bilgi için,
   :ref:`references-on-chain` dokümanına bakabilirsiniz.

   Modül referansları kullanmak zahmetlı olabilmektedir. Modül referanslarını daha basit isimler
   ile kullanabilmek için :ref:`naming-a-module` dokümanına bakabilirsiniz.

.. _init-passing-parameter-json:

Parametreleri JSON formatında kullanma
----------------------------------------

JSON formatındaki bir parametre, bir dosya olarak veya modüle gömülü olarak bir 
:ref:`akıllı sözleşme şeması<contract-schema>` sağlanırsa iletilebilir.
Şema aynı zamanda JSON'ı ikili formata dönüştürmek için de kullanılır.

.. seealso::

   :ref:`Akıllı sözleşme şemasının nasıl ve neden kullanıldığı hakkında daha fazla bilgi <contract-schema>`.

   :ref:`Parametreleri ikili (binary) formatında kullanma<init-passing-parameter-bin>`.

JSON formatında ``my_parameter.json`` parametre dosyası ve
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` referans numaralı 
modülden ``my_parameter_contract`` isimli bir sözleşme örneği oluşturmak için
aşağıdaki komut çalıştırılır:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

Eğer yukarıdaki işlem başarılı sonuçlanırsa, çıktı aşağıdaki gibi olmalıdır:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Aksi halde, sorunu gösteren bir hata mesajı görülecektir.
Sıklıkla karşılaşılan hata mesajları bir sonraki bölümde gösterilmektedir.

.. note::

   Eğer JSON dosyası içindeki parametreler, şema içindeki veri türleri ile uyuşmuyorsa,
   hata mesajı alınır. Örneğin:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Eğer verilen modül gömülü bir şema içermiyorsa, ``--schema /path/to/schema.bin`` parametresi 
   kullanılarak şema sağlanabilir.

.. note::

   Bir sözleşme örneğinin başlatılması sırasında, GTU'lar ``--amount AMOUNT`` parametresi 
   kullanılarak transfer edilebilir.


.. _init-passing-parameter-bin:

Parametreleri ikili (binary) formatında kullanma
-------------------------------------------------

Parametreler ikili (binary) formatında kullanıldığı zaman, bir :ref:`sözleşme şemasına
<contract-schema>` ihtiyaç yoktur.

İkili (binary) formatında ``my_parameter.bin`` parametre dosyası ve 
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` referans numaralı 
modülden ``my_parameter_contract`` isimli bir sözleşme örneği oluşturmak için
aşağıdaki komut çalıştırılır::

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


Eğer yukarıdaki işlem başarılı sonuçlanırsa, çıktı aşağıdaki gibi olmalıdır:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. seealso::

   Akıllı sözleşmelerde parametreleri kullanımıyla ilgili daha fazla bilgi için
   :ref:`working-with-parameters` dokümanına bakabilirsiniz.

.. _naming-an-instance:

Bir sözleşme örneğinin isimlendirilmesi
========================================

Bir sözleşme örneği, referans vermeyi kolaylaştırmak amacıyla bir takma ad veya gerçek bir *ad*  
ile isimlendirilebilinir. Bu isim sadece yerel (local) olarak ``concordium-client`` tarafından
saklanır ve zincir üzerinde görünür değildir.

.. seealso::

   İsimlerin ve yerel ayarların nasıl ve nerede tutulduğu ile ilgili detaylar
   :ref:`local-settings` dokümanında yer almaktadır.

Başlama sırasında isimlendirmek için, ``--name`` parametresi kullanılır.

Burada, ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` referans 
numaralı modülden oluşturulmuş  
``my_contract`` sözleşmesi başlatılıyor, ve ``my_named_contract`` olarak isimlendiriliyor:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


Eğer yukarıdaki işlem başarılı sonuçlanırsa, çıktı aşağıdaki gibi olmalıdır:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

Sözleşme örneği ayrıca ``name`` komutu kullanılarak da isimlendirilebilinir. 
``0`` adres dizinli bir sözleşme örneğini ``my_named_contract`` olarak isimlendirmek için,
aşağıdaki komut çalıştırılır:

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

Eğer yukarıdaki işlem başarılı sonuçlanırsa, çıktı aşağıdaki gibi olmalıdır:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. seealso::

   Sözleşme örneği adresleri hakkında daha fazla bilgi
   :ref:`references-on-chain` dokümanında yer almaktadır.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
