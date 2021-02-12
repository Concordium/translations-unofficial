.. _deploy-module:

======================================
Bir akıllı sözleşme modulünü dağıtmak
======================================
Bu klavuz zincir üzerindeki (*on-chain*) bir akıllı sözleşmeyi nasıl dağıtacağınızı ve nasıl isimlendireceğinizi gösterecektir.

Ön Hazırlık
============

:ref:`Concordium<downloads>` yazılımının son versiyonu ile :ref:`çalışan bir düğümün (node)<run-a-node>`
ve dağıtıma hazır bir :ref:`akıllı sözleşme modülün<setup-tools>` olduğundan emin olunuz.

Bir akıllı sözleşme dağıtımı, bir tür "işlem (transaction)" olması sebebiyle, 
bu işlemi yapmayı ödeyebilecek miktarda GTU'ya sahip bir hesap ile ``concordium-client`` 
kurulmuş olmalıdır.

.. note::

   İşlem masrafı, akıllı sözleşme modülünün boyutuna bağlı olarak değişmektedir.
   ``concordium-client`` işlem masrafını gösterir ve işlemi tamamlamadan önce
   bunun onaylanmasını ister.

Dağıtım
========

Bir hesap ismi  kullanarak akıllı sözleşme modülü ``my_module.wasm`` dağıtmak için aşağıdaki komut çalıştırılır:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. note::

   Eğer varsayılan "default" hesap kullanılıyorsa, --sender parametresi kullanılmayabilinir. Bu aşağıda yapılacak.

Eğer yukarıdaki işlem başarılı sonuçlanırsa, çıktı aşağıdaki gibi olmalıdır:

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'.

Akıllı sözleşme örnekleri oluştururken, kullanılan modül referansı
not edilmelidir.

.. seealso::

   Dağıtımı yapılmış bir modülden akıllı sözleşmeler başlatabilmek için
   :ref:`initialize-contract` dokümanı incelenmelidir.

   Modül referansları hakkında daha fazla bilgi almak için :ref:`references-on-chain` dokümanı incelenmelidir.

.. _naming-a-module:

Bir modülün isimlendirilmesi
=============================

Bir modül, referans vermeyi kolaylaştırmak amacıyla bir takma ad veya
gerçek bir *ad* ile isimlendirilebilinir.
Bu isim sadece yerel (local) olarak ``concordium-client`` tarafından saklanır ve
zincir üzerinde görünür değildir.

.. seealso::

   İsimlerin ve yerel ayarların nasıl ve nerede tutulduğu ile ilgili detaylar
   :ref:`local-settings` dokümanında yer almaktadır.

Dağıtım sırasında isimlendirmek için, ``--name`` parametresi kullanılır.
Burada modül ``my_deployed_module`` olarak isimlendirilmektedir:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

Eğer yukarıdaki işlem başarılı sonuçlanırsa, çıktı aşağıdaki gibi olmalıdır:

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

Modüller ayrıca ``name`` komutu kullanılarak da isimlendirilebilinir. 
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` referans numaralı modülü ``some_deployed_module`` olarak isimlendirmek için aşağıdaki komut çalıştırılır:

.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

Çıktı aşağıdaki gibi olmalıdır:

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module'.
