.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node-tr:

================
Node çalıştırmak
================

.. contents::
   :local:
   :backlinks: none

Rehberin bu bölümünde, bilgisayarınızda nasıl bir düğüm (node) kurabileceğinizi, bu düğümü Concordium ağına nasıl dahil edeceğinizi öğreneceksiniz. Bu bilgisayarınızda çalışan düğümün Concordium ağındaki diğer düğümlerden nasıl bloklar ve işlemler alıp ağa yayarak çalışabileceğini tanımlamak anlamına gelir.

Bu kılavuzda anlatılanları tamamladığınızda şunları yapabileceksiniz:

-  Bir Concordium düğümü çalıştırmak
-  Çalıştırdığınız düğümü Concordium Ağ Panelinde izleyebilmek
-  Concordium zincirinin durumunu doğrudan düğüm olarak çalışan bilgisaranızdan sorgulayabilmek

Bir düğüm kurmak ve çalıştırmak için hesaba ihtiyacınız yoktur.

Başlamadan önce
===============

Bir Concordium düğümü çalıştırmadan önce yapmanız gerekenler :

1. Docker'ı kurmalı ve çalıştırmalısınız

   -  *Linux* üzerinde çalıştıracaksanız, Docker'ın root yetkileri olmayan bir kullanıcı ile çalışabilmesine izin vermeniz gerekmektedir.

2. :ref:`concordium-node-and-client-download` yazılımını bilgisayarınıza indirin ve arşivden çıkartın.

Open Testnet'in önceki sürümünden yükseltim yapmak
==================================================

Open Testnet 4 için mevcut en güncel Concordium yazılına yükseltim:

-  En güncel Concordium yazılımını yukarıda açıklanan adımda belirtildiği gibi :ref:`indirin<downloads>` ve arşivden çıkartın.

-  Arşivden çıkarttığınız dosyalar içerisinden ``concordium-node-reset-data`` komutunu çalıştırın.

   -  *Mac* kullanıyorsanız : Aracı ilk kez açtığınızda ``concordium-node-reset-data`` dosyasına sağ tıklayın ve **Open**'ı seçin. Bunu yaptığınızda karşınıza bunun tanınmayan bir geliştirici tarafından oluşturulan bir yazılım olduğu mesajı gelecektir yeniden **Open**'a basın.

   -  *Windows* kullanıyorsanız : Aracı ilk kez açtığınızda ``concordium-node-reset-data`` dosyasına çift tıklayın. Bunu yaptığınızda karşınıza yazılımın tanınmayan bir geliştirici tarafından oluşturulduğu mesajı gelecektir.
      **More info** (Daha fazla bilgi) seçeneğine tıkladıktan sonra → **Run anyway** (Çalıştırmaya devam et) seçeneğiyle devam edin.

-  Bu işlem sonrasında araç şu şekilde bir soru görüntülemenizi sağlayacaktır:

      *Kaydedilmiş anahtarları da kaldırmak istiyor musunuz?* (Bu mesaj İngilizce olarak şöyle de görüntüleniyor olabilir : *Do you also want to remove saved keys?*)

   Önceki sürümler için yaratılan hesaplar Open Testnet'in güncel sürümünde artık geçerli değildir. Bu nedenle eğer daha önceki sürümde bilgisayarınızda saklanmış hesaplar varsa bu adımda *Y* seçeneğiyle devam ederek tüm hesap anahtarlarını silmenizi tavsiye ederiz.


.. _running-a-node-tr:

Düğüm çalıştırmak
=================

Bilgisayarınız üzerinde bir düğüm çalıştırarak Open Testnet'e dahil etmek için aşağıdaki adımları izleyin:

1. Açmış olduğunuz arşiv dosyası içerisinden ``concordium-node`` dosyasını çalıştırın.

-  *Mac* kullanıyorsanız : Aracı ilk kez açtığınızda ``concordium-node`` dosyasına sağ tıklayın ve **Open**'ı seçin. Bunu yaptığınızda karşınıza bunun tanınmayan bir geliştirici tarafından oluşturulan bir yazılım olduğu mesajı gelecektir yeniden **Open**'a basın.

- *Windows* kullanıyorsanız : Aracı ilk kez açmak için ``concordium-node`` dosyasına çift tıklayın. Bunu yaptığınızda karşınıza yazılımın tanınmayan bir geliştirici tarafından oluşturulduğu mesajı gelecektir. **More info** (Daha fazla bilgi) seçeneğine tıkladıktan sonra → **Run anyway** (Çalıştırmaya devam et) seçeneğiyle devam edin.


-  Düğümünüzü *yeniden başlatmanız* gerekirse
   ``--no-block-state-import`` seçeneğini kullanabilirsiniz. Bu seçeneği kullandığınızda düğümünüz Concordium zincirinden sadece aktif olmadığı süreçte oluşan güncellemeleri alacak ve sıfırdan tüm veriyi indirmesi gerekmeyecektir. Bu sayede düğümü yeniden başlatma süresini hızlandırabilirsiniz.

2. Düğümünüze bir isim verin. Bu isim herkese açık gösterge panelinde görüntülenecetir.

3. Eğer bilgisarınızda daha önce bu araçla bir düğüm aktifleştirdiyseniz araç yeniden çalıştığında size veri tabanını tamamen silmek isteyip istemediğinizi soracaktır. (Bu mesaj İngilizce olarak şu şekilde görüntülenebilir : *if you want to
   delete the local node database before starting*.) Bu soruya **y** diyerek cevap vermeniz durumunda tüm veri tabanı silinerak sıfırlanacak ve Concordium zincir ağınan sıfırdan tamamı bilgisayarınıza indirilecektir.

**Bilgisayarınızda çalışan düğümü, veritabanını silerek yeniden başlatmanız durumunda Concordium Zincir ağından yüklenecek veri miktarı nedeniyle bunun uzun sürebileceğini unutmayın.**

Bu araç şimdi Concordium Client imajını indirecek ve Docker'a yükleyecektir. Aracın başlatacağı istemci ile birlikte düğümün çalışması hakkındaki günlük bilgileri ekranda akmaya başlayacaktır.

Düğümünüzü kontrol panelinde (Network dashboard) görmek
=======================================================
``concordium-node``'u çalıştırdıktan sonra şunları yapabilirsiniz:

-  Düğümünüzü `Network Dashboard`_'da görüntüleyebilirsiniz
-  Bloklar, işlemler ve hesaplar hakkında :ref:`sorgu çalıştırabilirsiniz<testnet-query-node>`

Ağ Kontrol Paneli (Network dashboard)
-------------------------------------
Düğüm aracınızın Concordium Zincir ağının güncel durumunu yakalayarak eş zamanlı hale gelmesi biraz zaman alacaktır. Bu eş zamanlama işlemi, örneğin zincirdeki tüm bloklar hakkındaki bilgilerin bilgisayarınızdaki düğüme indirilmesi gibi bilgileri içermektedir.

Diğer bilgilerin yanı sıra, `Network Dashboard`_ 'a baktığınızda bilgisayarınızda çalışan düğümün zincir ağıyla ne kadar zamanda senkronize olabileceği konusunda bir fikir edinebilirsiniz.

Bunun için düğümünüze ait kontrol panelinde gördüğünüz, ne kadar bloğun düğümünüze senkronize edildiğini gösteren **Length** değeri ile kontrol panelinin en üst kısmından ulaşabileceğiniz, zincir ağının uaştığı en uzun noktayı gösteren **Chain Len** değerini karşılaştırabilirsiniz.



Gelen bağlantıları etkinleştirme
================================
Eğer düğümünüzü bir firewall yada ev tipi yönlendirici arkasında çalıştırıyorsanız sadece diğer düğümlere bağlanıyorsunuz. Diğer düğüm çalıştıran bilgisayarlar ise sizin düğümünüzle doğru şekilde bağlantı kuramıyorlar. Bu bir problem yaratmaz ve düğümünüz Concordium ağıyla sorunsuz bir işbirliği yaparak çalışabilir. Düğümünüz ağa sorunsuz şekilde işlemleri gönderebilecek ve eğer :ref:`düğümünüzde baker çalıştırmak<become-a-baker>` için ayar yaptıysanız bu işlemleri de sorunsuz yapacaktır.

Ancak düğümünüzün ağa daha iyi bir performansla bağlanması ve daha fazla fayda sağlaması için gelen bağlantı isteklerini açmanızda fayda olacaktır. Varsayılan olarak ``concordium-node`` ağdaki ``8888`` portundan geliş isteklerini dinler. Sizin ağ yapınıza bağlı olarak iç ve dış ağınızdan ''8888'' portunu düğümün çalıştığı bilgisayarınızın IP adresine yönlendirin. Bu işlemi nasıl yapabileceğiniz tamamen düğümünüzün çalıştığı ağ ortamındaki yönlendirici ve firewall yapısına göre değişecektir.

Port ayarlama
-------------
Düğüm dört bağlantı noktasını dinler. Dinlenecek bağlantı noktaları düğümü çalıştırmak için kullanılan komut satırı argümanları ile yapılandırılabilir. Düğüm tarafından kullanılan varsayılan bağlantı noktaları aşağıdaki gibidir :

-  8888, *peer-to-peer* ağları için bağlantı noktası, bu portu ayarlamak için komut satırında ``--listen-node-port`` argümanı kullanılabilir.
-  8082, ara yazılım tarafından (middleware) kullanılan port, bu portu ayarlamak için komut satırında ``--listen-middleware-port`` argümanı kullanılabilir.
-  10000, gRPC portu, bu portu ayarlamak için komut satırında ``--listen-grpc-port`` argümanı kullanılabilir.

Docker üzerinde bu port eşleştirmelerini değiştirmek istediğinizde düğüm durdurulmalı (:ref:`stop-a-node`), sıfırlanmalı ve yeniden başlatılmaldır.
Konteyneri sıfırlamak için ``concordium-node-reset-data`` veya terminal ekranında ``docker rm concordium-client`` komutlarını kullanabilirsiniz.

Güvenlik duvarınızı yalnızca 8888 (peer-to-peer bağlantı portu) portundan genel bağlantılara izin verecek şekilde yapılandırmanızı *şiddetle tavsiye ederiz*. Diğer portlara erişimi olan bir başkası düğğümünüzün kontrolünü ele geçirebilir, üzerindeki hesaplara ve düğümde kayıtlı bilgilerinize ulaşabilir.


.. _stop-a-node-tr:

Düğümü durdurmak
================
Düğümü durdurmak için çalıştırmak için kullandığınız ve bilgilerin akmakta olduğu ekranda **CTRL+c** tuşlarına basın ve düğümün düzgün ve temiz bir şekilde kapanmasını bekleyin.

Eğer istemcinizi düzgün bir şekilde durdurmadan penecereyi yanlışlıkla kapatırsanız Docker arka planda çalışmaya devam edecektir. Bu durumda ``concordium-node`` komutunu çalışırdığınız dosyalarla aynı dizinde bulunan ``concordium-node-stop`` komutunu diğeriyle aynı şekilde çalıştırarak düğümünüzü düzgün bir şekilde durdurabilirsiniz.

Destek ve Geri Bildirim
=======================
Bilgisayarınızda çalıştırdığınız düğümle ilgili kayıt kütük dosyalarına ``concordium-node-retrieve-logs`` aracıyla ulaşabilirsiniz. Bu kayıt kütük dosyalarını çalışan imajdan bir dosyaya kayıt edecektir. Ek olarak eğer yetki verildiyse mevcut sistem üzerinde çalışan programlarla ilgili verileri de bu dosyaya ekleyecektir.

Kayıt kütük dosyalarınızı, sistem bilgilerinizi de ekleyerek soru ve yorumlarınızı elektronik postayla testnet@concordium.com adresine gönderebilirsiniz.
Bize tüm soru, yorum ve geri bildirimleriniz için her zaman `Discord`_ kanalımızdan ulaşabilir veya :ref:`troubleshooting page<troubleshooting-and-known-issues>` sayfamızı ziyaret edebilirsiniz.

