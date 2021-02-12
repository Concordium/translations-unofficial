.. _inspect-instance:

======================================
Akıllı sözleşme örneğini inceleyin
======================================

Bu kılavuz size akıllı bir sözleşme örneğini nasıl inceleyeceğinizi gösterecektir.
Bir örneği incelemek size adını, sahibini, modül referansını, dengesini, durumunu
ve alma işlevlerini gösterecektir.

Hazırlık
===========

:ref:`Concordium software<downloads>`  ile :ref:`running a node<run-a-node>`
kullanarak bir düğüm çalıştırıyor oldugunuzdan emin olun. Bu şekilde zincir
üzerinde bir akıllı sözleşme örneğini inceleyebilirsiniz.


.. seealso::
   Bir akıllı sözleşme modülünün nasıl dağıtılacağı için :ref:`deploy-module`
   dokumanina ve bir örneğin nasıl oluşturulacağı :ref:`initialize-contract` dokumanina bakınız.

Sözleşmeyi inceleme
====================

Adres indeksi ``0`` olan bir akıllı sözleşme örneği hakkındaki bilgileri
incelemek veya göstermek için aşağıdaki komutu çalıştırın:

.. code-block:: console

   $concordium-client contract show 0

Çıktı aşağıdakine benzer olmalıdır:

.. code-block:: console

   Contract:        my_contract
   Owner:           '4Lh8CPhbL2XEn55RMjKii2XCXngdAC7wRLL2CNjq33EG9TiWxj' (default)
   ModuleReference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'
   Balance:         0.000000 GTU
   State:
       {
           "first_field": 0,
           "second_field": 42
       }
   Functions:
    - receive_one
    - receive_two

.. seealso::

   Sözleşme örneği adresleri hakkında daha fazla bilgi için
   :ref:`references-on-chain` dokümanına bakabilirsiniz..

Bir incelemenin ayrıntı düzeyi, ``show`` komutunun  :ref:`contract schema <contract-schema>`
erişimine sahip olup olmadığına bağlıdır. Eger şema gömülü bir şema ise, dolaylı olarak
kullanılacaktır. Aksi takdirde, ``--schema /path/to/schema.bin`` parametresi kullanılarak
bir şema sağlanabilir.


.. note::

   ``--schema`` parametresi kullanılarak sağlanan bir şema dosyası, gömülü bir
   şemaya göre öncelikli olacaktır.

.. seealso::

   :ref:`Akıllı sözleşme şemalarının neden ve nasıl kullanılacağı hakkında daha fazla bilgi edinin <contract-schema>`.
