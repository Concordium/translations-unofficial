.. Should answer:
    - What is a smart contract
    - Why use a smart contract
    - What are the use cases
    - What are not the use cases

.. _introduction:

===============================
Akıllı sözleşmelere giriş
===============================

Akıllı sözleşme, Concordium blok zincirine gönderilen ve doğrudan çekirdek protokolün
parçası olmayan davranışı tanımlamak için kullanılan, kullanıcı tarafından sağlanan bir kod parçasıdır.
Akıllı sözleşmeler, Concordium ağındaki düğümler tarafından önceden tanımlanmış kurallara göre yürütülür.
Yürütülmeleri tamamen şeffaftır ve tüm düğümler, yürütmenin sonucunun yalnızca halka açık bilgilere dayandığı
konusunda hemfikir olmalıdır.

Akıllı bir sözleşme GTU alabilir, tutabilir ve gönderebilir, zincirin bazı yönlerini gözlemleyebilir
ve kendi durumunu koruyabilir. Akıllı sözleşmeler her zaman **harici** eylemlere (ör. Mesaj gönderen bir hesap)
yanıt olarak yürütülür. Pratikte akıllı sözleşmeler, zincir içi ve zincir dışı işlevselliği birleştiren genellikle
daha büyük bir sistemin küçük bir parçası olacaktır. Zincir dışı işlevselliğe bir örnek olarak, hisse senedi
fiyatları veya hava durumu bilgileri gibi gerçek dünyadaki verilere dayalı olarak akıllı sözleşmeyi başlatan bir sunucu olabilir.

Akıllı sözleşmeler ne içindir?
===============================

Akıllı sözleşmeler, üçüncü şahıslara duyulan güven miktarını azaltabilir, bazı durumlarda güvenilir bir üçüncü
şahsa olan ihtiyacı ortadan kaldırabilir veya yeteneklerini azaltarak onlara duyulan güven miktarını azaltabilir.

çünkü Akıllı sözleşmeler tamamen şeffaf bir şekilde yürütüldüğünden, bir düğüme erişimi olan herkesin doğrulayabileceği
şekilde, taraflar arasında anlaşmayı sağlamak için çok yararlı olabilirler

.. _auction:

Açık artırma akıllı sözleşme örneği
-----------------------------------

Akıllı sözleşmeler için bir kullanım örneği, bir açık artırma düzenlemek olabilir;
Akıllı sözleşmeyi herkesten farklı teklifleri kabul edecek ve en yüksek teklifi
vereni takip etmesini sağlayacak şekilde programlayabiliriz.
Açık artırma bittiğinde, akıllı sözleşme kazanan teklif GTU'sunu satıcıya gönderir ve
diğer tüm teklifleri geri gönderir. Satıcı daha sonra ürünü kazanana göndermelidir

Akıllı sözleşme, müzayedecinin ana rolünün yerini alır. Sözleşmenin kendisi yalnızca
teklif bölümünü ve GTU'ların zincir üzerindeki dağıtımını yönetir. Satıcı yükümlülüklerini
yerine getirmezse, muhtemelen en yüksek teklifi verene geri ödeme yapmak için bir iş
akışına ihtiyaç duyacaktır. Bu, büyük olasılıkla, sözleşmenin, satıcının gerçekten
yükümlülüğünü yerine getirdiğine veya en yüksek teklifi verenin şikayette bulunabilmesi
için bir şekilde bazı kanıtlari sunmasina olanak sağlayan kavramları desteklemesi gerektiği
anlamına gelecektir. Akıllı sözleşmeler bu gerçek dünya sorunlarını otomatik olarak çözemez
ve en uygun çözüm muhtemelen açık artırmanın özelliklerine bağlı olarak değişecektir.

Akıllı sözleşmeler ne için uygun *değildir*?
=============================================

Akıllı sözleşmeler çok heyecan verici bir teknolojidir ve insanlar bunlardan yararlanmanın
yeni yollarını bulmaya devam etmektedir.
Bununla birlikte, akıllı sözleşmelerin iyi bir çözüm olmadığı bazı durumlar vardır.

Akıllı sözleşmelerin en önemli avantajlarından biri kod yürütülmesine duyulan güvendir ve
bunu başarmak için blok zinciri ağındaki çok sayıda düğümün aynı kodu çalıştırması ve sonucun
kabul edilmesini sağlaması gerekir.
Doğal olarak bu, aynı kodu bazı bulut hizmetlerinde tek bir düğümde çalıştırmaya kıyasla pahalı hale gelir.

Akıllı sözleşmenin yoğun hesaplamalara dayandığı durumlarda, hesaplamaları akıllı sözleşmenin
dışına çıkarmak ve akıllı sözleşmenin, önemli bölümlerinin doğru şekilde yürütülmesini
sağlamak için kriptografik teknikler kullanabilir. Bu teknikler ile gereken hesapların doğru bir
şekilde yürütüldüğünden emin olunabilir

Son olarak, akıllı sözleşmelerin gizliliği olmadığını ve akıllı sözleşmenin içerisindeki **her şeye**
Concordium ağındaki herkes tarafından erişilebileceğini hatırlamak önemlidir, bu da hassas verileri
akıllı bir sözleşmelerde ele almanın zor olduğu anlamına gelecektir. Bazı durumlarda, doğrudan verilerle
çalışmak yerine, kriptografik araçların kullanılması mümkün olabilir, bunun yerine akıllı sözleşmeler,
gerçek verileri gizleyen şifreleme veya taahhütler gibi türetilmiş kavramlarla çalışması sağlanabilir.

Akıllı bir sözleşmenin yaşam döngüsü
=====================================

Akıllı bir sözleşme ilk olarak zincire :ref:`Contract Module <contract-moduletr>`’un parçası olarak yüklenir.
Bundan sonra akıllı sözleşme, :ref:`smart contract instance <contract-instancestr>`  sağlayarak *başlatılır*.
Son olarak, akıllı bir sözleşme örneği, kendi iş akışına göre tekrar tekrar güncellenebil
