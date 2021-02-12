
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker:

=============================================
Bloklar oluşturmak için Fırıncı (Baker) olmak
=============================================

.. contents::
   :local:
   :backlinks: none

Bu bölümde fırıncı (baker) nedir, ağdaki rolleri nelerdir ve bir fırıncı çalıştırmak için neler gerekir konuları açıklanmaktadır.

Bu bölümü okuyarak şunları öğreneceksiniz:

- Fırıncı (baker) nedir ve bağlantılı kavramları nelerdir
- Düğümünüzü fırıncı (baker) olarak çalışacak şekilde nasıl yükseltebilirsiniz

Fırıncı (baker) olma süreci şu adımlarla özetlenebilir:

#. Bir hesap ve hesaba da bir miktar GTU alın.
#. Bir fırıncı anahtarı seti alın.
#. Fırıncı anahtarınızı hesabınıza kayıt edin.
#. Bilgisarınızda çalışan düğümü fırıncı anahtarıyla başlatın.

Bu adımları tamamladığınızda düğümünüz ağda bloklar üretmeye başlayacaktır. Eğer düğümünüzün ürettiği bloklar zincire eklenirse düğümünüz ödül kazanacaktır.

.. note::

   Bu bölümde, blok üretmek üzere kayıt olmak ve kayıt edilen fırıncıyı yönetmek için kullanılan hesabı ``bakerAccount`` olarak adlandıracağız.

Tanımlar
========

Fırıncı (Baker)
---------------
Ağda yeni blok üretmek üzere aktif olarak çalışan ve ürettiği blokları zincir ağına eklemeye çalışan düğümdür. Fırıncı, bloğa dahil olan işlkemleri korumak üzere, işlemeri sipariş eder, toplar ve zincirin bütünlüğünü doğrulamak üzere bunları doğrular.


Fırıncı anahtarları (Baker keys)
--------------------------------
Her fırıncı *baker keys* olarak adlandırılan bir kriptografik anahtara sahiptir. Düğüm bu anahtarı yarattığı blokları imzalamak için kullanır.   Yeni blok yaratmak için oluşturulan hesabın bu anahtar seti yüklenmiş olarak çalışması gerekir.

Fırıncı hesabı (Baker account)
------------------------------
Her hesap, zincire bir fırıncı kaydetmek için bir dizi fırıncı anahtarı kullanabilir.

Bir fırıncı zincire dahil edilecek geçerli bir blok ürettiği zaman ilişkendirilmiş olduğu hesaba bir ödül ödemesi yapılır.


Bahis ve piyango
----------------

.. todo::

   - Link to release schedule.

Hesap, GTU bakiyesinin bir kısmını *fırıncı hissesine* (*baker stake*) yatırabilir ve daha sonra manual olarak bu tutarın tamamını veya bir kısmını serbest bırakabilir. *Fırıncı hissesi* olarak yatırılan tutar serbest bırakılana kadar taşınamaz veya başka bir hesaba aktarılamaz.


.. note::

   Eğer bir hesap zamanlanmış serbest bırakma ile aktarılmış bir tutara sahipse bu miktar henüz serbest bırakılmamış olsa bile stake edilebilir.
   
Bir blok pişirmek için seçilecek bir fırıncının, kazanma olasılığı kabaca anlatmak gerekirse stake edilmiş tutarla orantılı olarak değişmektedir.

Aynı stake tutarı bulunan bloğun zincire dahil edilmesiyle ilgili sonladırmanın başarılı olması yada olmaması hesaplarında da kullanılır. Detaylarını görmek için See Finalization_.

.. _epochs-and-slots:

Çağlar ve yuvalar (Epochs and slots)
------------------------------------
Concordium zincir ağında, zaman *yuvalar* olarak adlandırılan alt kısımlara bölünmüştür. Yuvalar Genesis bloğunda sabit bir süreye sahiptir. Her yuva herhangi bir dalda en fazla bir bloğa sahip olabilir ancak aynı yuvada farklı dallar üzerinden bir çok blok üretilebilmektedir.

.. todo::

   Let's add a picture.

Ödüller ve pişirme ile ilgili diğer kavramları değerlendirirken *çağ* (*epoch*) kavramını ağda mevcut fırıncıların ve hisselerin sabit olduğu bir dönemi tanımlayan zaman birimini isimlendirmek için kullanıyoruz. Tüm çağ'ların (epoch) Genesis bloğı içerisinde sabit bir zaman periyodu vardır. Testnet'te çağların periyodu **1 saat**'dir.


Pişirmeye başlama
=================

Hesapları yönetmek
------------------
Bu bölüm, bir hesabı içe aktarma süreciyle ilgili kısa bir özet bilgi sağlar. Tam bir açıklamayı incelemek için lütfen şuraya tıklayın : :ref:`managing_accounts`.

Hesaplar :ref:`concordium_id` uygulaması kullanılarak oluşturulur. Bir hesap başarıyla oluşturulduktan sonra **More** sekmesine giderek **Export** (dışa aktar) seçeneğiyle hesap bilgilerini içeren bir JSON dosyasının dışarı aktarılması sağlanabilir.

Dışarı aktarmış olduğunuz hesap bilgilerini zincir ağına aktarmak için aşağıdaki komutu çalıştırın:

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount

``concordium-client`` size dosyayı dışarı aktarırken belirlemiş olduğunuz şifreyi soracaktır. Bu şifre aynı zamanda işlemleri imzalamak için kullanılacak şifreli transfer anahtarıdır.


Fırıncı (Baker) için anatar oluşturma ve kayıt ettirme
------------------------------------------------------

.. note::

   Bu süreci yürütebilmeniz için hesapta GTU bulunması gerekmektedir. Kullanacağınız mobil uygulamadan 100 GTU talep ettiğinizden emin olun.
   
Her hesabın fırıncısını kayıt ederken kullanılan benzersiz bir fırıncı kimliği vardır. Bu kimlik, ağ tarafından sağlanmalıdır ve şu anda önceden hesaplanması mümkün değildir. Bu kimlik, blokları oluşturmak için blokları oluşturmak üzere baker keys dosyası içinde düğüme verilemlidir. Aşağıdaki işlemleri gerçekleştirirken ``concordium-client`` bu alanı otomatik olarak dolduracaktır.

Yeni bir anahtar seti oluşturmak için şu komutu çalıştırın:

.. code-block:: console

   $concordium-client baker generate-keys <keys-file>.json

Yukarıdaki kodda gördüğünüz *<keys-file>* kısmı anahtar dosyası için kendi belirlediğiniz herhangi bir ismi belirleyip yazacağınız yerdir.   Anahtarları ağa kayıt edebilmek için ihtiyacınız olan öncelikle :ref:`düğümünüzü çalıştırmak <running-a-node>` ve sonra ``baker add`` işlemini ağa göndermektir. Bunu aşağıda göreceğiniz komutla yapabilirsiniz:

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

kodda yapılacak değişiklik açıklamaları aşağıda belirtilmiştir:

- ``<amountToStake>`` Fırıncının ilk hissesi için ne kadar GTU tutarı yatırılmasını istiyorsanız bu tutar yazılmalıdır
- ``<concordium-data-dir>`` aşağıda belirtilen veri dizinine uygun şekilde değiştirilmelidir:

  * Linux ve MacOS için: ``~/.local/share/concordium``
  * Windows için: ``%LOCALAPPDATA%\\concordium``.

(Komutun sonunda gördüğünüz çıktı dosyasının adı ise ``baker-credentials.json``) olarak sabit kalmalıdır.

``--no-restake`` bayrağı kullanmak sizi oluşacak ödülleri otomatik olarak fırıncının stake edilmiş tutarına eklemekten korur. Bu davranışın detayları şu bölümde açıklanmıştır : `Restaking the earnings`_.

Fırıncı anahtarları ile düğümünüzü çalıştırabilmek için şu anda çalışan düğümünüzü durdurmanız gerekir. (Bunu çalışır durumdaki terminal ekranında ``Ctrl + C`` komutu girerek veya ``concordium-node-stop`` komutunu kullanarak yapabilirsiniz.

Oluşan dosyayı uygun olan dizine koyduktan sonra (bir önceki komutla belirlenen çıktı dosyasının bulunduğu dizin) düğümünüzü yeniden çalıştırmak için ``concordium-node`` komutunu kullanabilirsiniz. Artık fırıncı, o anki dönem için düğüme dahil olacak ve blok arama (pişirme) işlemine başlayacaktır.

Yapılan bu değişiklik hemen uygulanacak ve sonrasında fırıncı ekleme işleminin bir bloğa dahil edildiği döenemin sonunda da yürürlüğe girecektir.


.. table:: Zaman çizelgesi: fırıncı ekleme

   +-------------------------------------------+-----------------------------------------+-----------------+
   |                                           | İşlem bir bloğa dahil edildiğinde       | 2 dönem sonra   |
   +===========================================+=========================================+=================+
   | Düğüm sorgulanarak değişikliği görebilmek |  ✓                                      |                 |
   +-------------------------------------------+-----------------------------------------+-----------------+
   | Fırıncı pişirme komitesine dahildir       |                                         | ✓               |
   +-------------------------------------------+-----------------------------------------+-----------------+

.. note::

  Eğer fırıncı dahil etme işlemini **E** döneminde yaptıysanız komiteye dahil edilmesi **E+2** dönemi başlangıcında devreye alınacaktır.
  

Fırıncıyı yönetmek
==================

Fırımcının durumunu ve çekiliş (piyango) gücünü kontrol etmek
-------------------------------------------------------------

Düğümünüzün pişirme işlemlerini yapıp yapmadığını farklı kaynaklardan kontrol edebilirsiniz. Bu kaynaklarda görüntülenen bilgilerin hassasiyet ve anlık durumlar farklılık gösterebilir.

- `Ağ kontrol panelinizde (network dashboard) <http://dashboard.testnet.concordium.com>`_ düğümünüzün ``Baker`` kolonunda baker ID görünür.
- ``concordium-client`` 'ı kulklanarak mecvut fırıncınızın durumunu ve ilişkilendirilmiş staked tutarını ve piyango(çekiliş) gücünü görebilirsiniz.  Lottery Power fırıncınızın bir blok bulduğunda kazanabileceği ödül miktarını belirlemekte kullanılır.

  .. code-block:: console

     $concordium-client consensus show-parameters --include-bakers
     Election nonce:      07fe0e6c73d1fff4ec8ea910ffd42eb58d5a8ecd58d9f871d8f7c71e60faf0b0
     Election difficulty: 4.0e-2
     Bakers:
                                  Account                       Lottery power
             ----------------------------------------------------------------
         ...
         34: 4p2n8QQn5akq3XqAAJt2a5CsnGhDvUon6HExd2szrfkZCTD4FX   <0.0001
         ...

- ``concordium-client`` 'ı kullanarak hesabınıza kayıtlı fırıncıyı ve bu fırıncı için stake edilmiş tutarı görebilirsiniz.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- Eğer staked amount yeterince büyükse ve düğümünüz fırıncı anahtarları yüklenmiş şekilde çalışıyorsa blok üretme işlemlerini mobil cüzdanınızda hesabınıza ulaşan ödlüllerle birlikte görebilirsiniz. Örneği resimde gösterilmiştir:

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Staked amount'u güncellemek
---------------------------
Fırıncı hissesini güncellemek için aşağıdaki komutu çalıştırın :


.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

Stake edilen miktarın değiştirilmesi bir fırıncının blokları pişirmek için seçilme olasılığını değiştirmektedir.

Bir fırıncı hesabı için **ilk kez stake eklediğinizde yada daha önce eklenmiş stake'i arttırdığınızda** bu değişimin zincire iletilmesi hemen görünür hale gelecektir. Bunu ``concordium-client account show bakerAccount`` komutuyla görebilirsiniz  ve 2 dönem (epoch) sonra yürürlüğe girecektir.


.. table:: Zaman çizelgesi: Stake edilmiş tutarı arttırmak durumunda

   +-------------------------------------------+--------------------------------------+----------------+
   |                                           | İşlem bir bloğa dahil edildiğinde    | 2 dönem sonra  |
   +===========================================+======================================+================+
   | Düğüm sorgulanarak değişikliği görebilmek | ✓                                    |                |
   +-------------------------------------------+--------------------------------------+----------------+
   | Fırıncının yeni stake tutarını kullanması |                                      | ✓              |
   +-------------------------------------------+--------------------------------------+----------------+

Fırıncının **stake edilmiş tutarını düşürdüğünüzde** bu değişimin aktif olması için *2 +bakerCooldownEpochs* dönemi gerekir. Değişim işlem başarıyla yapıldığı anda görünür hale gelecek ve ``concordium-client account show bakerAccount``: komutu kullanılarak aşağıdaki komutla kontrol edilebilecektir.

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: Zaman çizelgesi: Stake edilmiş tutarı azaltmak (düşürmek) durumunda

   +-------------------------------------------+------------------------------------+----------------------------------------+
   |                                           | İşlem bir bloğa dahil edildiğinde  | *2 + bakerCooldownEpochs* dönem sonra  |
   +===========================================+====================================+========================================+
   | Düğüm sorgulanarak değişikliği görebilmek | ✓                                  |                                        |
   +-------------------------------------------+------------------------------------+----------------------------------------+
   | Fırıncının yeni stake tutarını kullanması |                                    | ✓                                      |
   +-------------------------------------------+------------------------------------+----------------------------------------+
   | Stake tutarının yeniden düşürülmesi       | ✗                                  | ✓                                      |
   | veya fırıncının hesaptan kaldırılması     |                                    |                                        |
   +-------------------------------------------+------------------------------------+----------------------------------------+

.. note::

   Testnet içerisinde, ``bakerCooldownEpochs`` süresi 168 dönem olarak tanımlanmıştır. Bu değer aşağıdaki komutla kontrol edilebilir :

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   `Tanımlar`_ bölümünde açıklandığı üzere , stake edilmiş tutarlar *kilitlidir*, transfer edilemez, ödeme için kullanıalamaz ve buna benzer işlemler yapılamaz. Bunu hesaba katarak kısa vadede ihtiyaç duymayacağız bir tutarı stake etmelisiniz. Özellikle bir fırıncı kaydını silmek veya stake edilen tutarı değiştirmek için bu işlemin maliyetini karşılayacak serbest GTU'lara sahip olmanız gerektiğini unutmayın.
   
Kazançları yeniden düzenlemek
-----------------------------
Ağa ve bloklara fırıncı hesabıyla dahil olduğunuzda her pişirilen blok için fırıncı hesabı bir ödül kazanır. Bu ödüller varsayılan olarak bahis tutarına (Staked Amount) otomatik olarak eklenir.

İstediğinz zaman kazanılan ödüllerin bahis tutarına (staked amount) eklenmesi yerine doğrudan hesap bakiyenize eklenmesini de sağlayabilirsiniz. Bu ayarlar düğümünüz üzerinde ``concordium-client`` komutuyla gerçekleştirilebilir. Örnekleri aşağıda bulunmaktadır:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Kazanılan ödülleri re-stake etmek veya etmemeye yönelik yapılan değişiklikler bir sonraki dönemde pişirme ve sonlandırma gücünü etkilemeye başlar. Anahtarın mevcut değerini ``concordium-client`` komutuyla hesap bilgilerini sorgulayarak görebilirsiniz. Komut kullanım örneği aşağıda bulunmaktadır:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Zaman Çizelgesi: Bahis miktarı güncelleme (Restake Updates)

   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   |                                               | İşlem bir bloğa dahil edildiğinde       | Ödülden 2 dönem (epoch) sonra |
   +===============================================+=========================================+===============================+
   | Düğüm sorgulanarak değişikliği görebilmek     | ✓                                       |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   | Kazançlar bahis tutarına eklenmez             | ✓                                       |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   | Kazancın bahis tutarına eklenmesi halinde     |                                         | ✓                             |
   | bahis ve piyango şansının değişmesi           |                                         |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+

Fırıncı düğüm üzerinden ağa kayıt edildiğinde kazancını otomatik olarak yeniden yatıracaktır, ancak yukarıda belirtildiği gibi değiştirilebileceği gibi aşağıda gösterilen ``--no-restake`` argümanının 
``baker add`` komutuna eklenmesiyle de değiştirilebilir :

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Kesinleştirme
-------------
Kesinleştirme, *kesinleştirme komitesindeki* düğümler tarafından gerçekleştirilen ve yeterli sayıdaki komite üyesinin bloğu alıp sonucu üzerinde anlaştığında bşr bloğu *sonuçlandıran* oylama süresidir. Daha yeni bloklar zincirin bütünlüğünü 
sağlamak için mevcut bir bloğa atanmış olmalıdır. Bu işlem hakkında daha fazla bilgi edinmek için şuraya bakın : :ref:`finalization<glossary-finalization>`.

Kesinleştirme komitesi, belli bir miktar hisseye sahip fırıncılar tarafından oluşturulur. Bu, özellikle kesinleştirme komitesine katılmak için bahsi geçen eşiğe ulaşabilmeniz için bahis tutarınızı (staked amount) değiştirmeniz gerekeceği anlamına gelir.
Testnet'te, kesinleştirme komitesine katılmak için gereken bahis miktarı, **mevcut toplam GTU'nun %0.1**'idir.

Kesinleştirme komitesine katılmak, sonuçlandırılan (pişirilen) her blokta ödüller üretir. Blok (üretim) kesinleştikten bir süre sonra ödüller fırıncı hesabına ödenir.

Fırıncı hesabını silmek
=======================
Düğüm sahibi, fırıncısının zincirdeki kaydını silebilir. Bunu yapmak için çalıştırılması gereken ``concordium-client`` komutu aşağıda gösterilmiştir:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

Bu komutun çalıştırılması, fırıncınızı, fırıncı listesinden kaldıracak aynı zamanda bahis için yatırılan (staked) tutarın transfer edilebilmesi veya serbest hesaba taşınabilmesi için tutarın kilidini de açacaktır.

Fırıncı kaldırılmasıyla, bahis miktarının (staked amount) azaltılması aynı zaman çizelgesine sahiptir. Değişikliğin yürürlüğe girebilmesi için *2 + bakerCooldownEpochs* dönemi geçmesi gerekir. İşlem bir bloğa dahil edilir edilmez değişiklik zincirde görünür hale gelir
ve siz hesap bilgilerinizi sorgulayarak bu değişikliğin ne zaman geçerli olacağını görebilirsiniz. Bu sorgu işlemini, her zaman olduğu gibi ``concordium-client`` ile gerçekleştirebilirsiniz, aşağıda örneği gösterilmiştir:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Zaman çizelgesi: fırıncıyı silmek

   +--------------------------------------------+-----------------------------------------+----------------------------------------+
   |                                            | İşlem bir bloğa dahil edildiğinde       | *2 + bakerCooldownEpochs* dönem sonra  |
   +============================================+=========================================+========================================+
   | Düğüm sorgulanarak değişikliği görebilmek  | ✓                                       |                                        |
   +--------------------------------------------+-----------------------------------------+----------------------------------------+
   | Fırıncının komiteden silinmesi             |                                         | ✓                                      |
   +--------------------------------------------+-----------------------------------------+----------------------------------------+

.. warning::

   Stake edilmiş tutarı azatlma ve fırıncı hesabını silme işlemi eş zamanlı olarak yapılamaz. Stake edilen tutarın yansıtılma dönemi boyunca fırıncı hesabı silinemez veya tersi de mümkün değildir.
   

Destek ve Geri Bildirim
=======================

Herhangi bir sorunla karşılaşırsanız veya bir öneriniz varsa, sorunuzu veya geri bildirimlerinizi `Discord`_ üzerinden gönderin veya testnet@concordium.com adresine e-posta yazarak bize ulaşın.
