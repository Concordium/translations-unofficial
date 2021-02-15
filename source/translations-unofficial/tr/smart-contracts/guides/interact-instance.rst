.. _interact-instance:

=============================================
Akıllı sözleşme örneği ile etkileşim kurun
=============================================

Bu kılavuz size akıllı bir sözleşme örneğiyle nasıl etkileşimde bulunacağınızı
gösterecektir; bu etkileşim; örneğin durumunu güncelleyen bir alma işlevinin
tetiklenmesi anlamına gelir.

Hazırlık
===========

:ref:`Concordium software<downloads>`  ile :ref:`running a node<run-a-node>`
kullanarak bir düğüm çalıştırıyor oldugunuzdan emin olun. Bu şekilde zincir
üzerinde bir akıllı sözleşme örneğini inceleyebilirsiniz.

.. seealso::
   Akıllı sözleşme modülünün nasıl dağıtılacağı için bkz :ref:`deploy-module`
   dokumanına ve bir örneğin nasıl oluşturulacağı için dokumanına bkz: :ref:`initialize-contract`.

Akıllı sözleşmeyle etkileşimler ağ üzerine bir işlem olduğundan, işlemlerin
ücretini ödeyebilmek için yeterli GTU'ya sahip bir hesapla ``concordium-client``
kurulduğundan emin olmalısınız.

.. note::

   Bu işlemin maliyeti, alma işlevine gönderilen parametrelerin boyutuna ve
   fonksiyonun kendisinin karmaşıklığına bağlıdır.

Etkileşim
===========

Parametresiz alma işlevi olan ``my_receive`` kullanarak, adres indeksi ``0``
olan bir örneği güncellemek için aşağıdaki komutu çalıştırın. Bu komut 1000
enerji kullanacaktir:

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

Komut başarılı olursa, çıktı aşağıdakine benzer olmalıdır:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

Parametreleri JSON biçiminde aktarma
---------------------------------------

Eger :ref:`smart contract schema <contract-schematr>` is saglanir ise, parametre
JSON formatı içerisinde; bir dosya yada gömülü olarak geçilebilir. Şema, JSON
objesini ikili (binary) olarak serileştirmek için kullanılır.

.. seealso::

   :ref:`Akıllı sözleşme şemalarının neden ve nasıl kullanılacağı hakkında daha
   fazla bilgi edinmek için <contract-schematr>`.

``my_parameter_receive`` alma fonksiyonunu kullanarak JSON formatında bir
parametre dosyası ``my_parameter.json`` ile adres indeksi `` 0 '' olan bir örneği
güncellemek için, aşağıdaki komutu çalıştırın:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json

Komut başarılı olursa, çıktı aşağıdakine benzer olmalıdır:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

Aksi takdirde, sorunu açıklayan bir hata görüntülenir.
Yaygın hatalar sonraki bölümde açıklanmaktadır.

.. seealso::

   Sözleşme örneği adresleri hakkında daha fazla bilgi için, bakınız
   :ref:`references-on-chain`.

.. note::

   JSON biçiminde sağlanan parametre şemada belirtilen türe uymuyorsa, bir hata
   mesajı görüntülenecektir. Örneğin:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Belirli bir modül gömülü bir şema içermiyorsa, ``--schema /path/to/schema.bin``
   parametresi kullanılarak şema sağlanabilir.

.. note::

   GTU, güncellemeler sırasında ``--amount AMOUNT`` parametresi kullanılarak
   bir sözleşmeye de aktarılabilir.

Parametreleri ikili (binary) biçimde aktarma.
----------------------------------------------

Parametreleri ikili(binary) biçimde iletirken, :ref:`contract schema <contract-schematr>`
gerekli değildir.

``my_parameter_receive`` alma fonksiyonunu kullanarak ``my_parameter.bin``
parametre dosyası ile ikili formatta adres indeksi ``0`` olan bir örneği güncellemek
için, aşağıdaki komutu çalıştırın:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

Eğer komut başarılı olursa, çıktı aşağıdakine benzer olmalıdır:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. seealso::

   Akıllı sözleşmelerde parametrelerle nasıl çalışılacağı hakkında bilgi için bkz
   :ref:`working-with-parameters`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
