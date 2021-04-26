.. _Discord: https://discord.gg/xWmQ5tp

.. _guide-account-transactions:

=========================================================
Concordium ID: Magsimula sa mga account at transaksyons
=========================================================

.. contents::
   :local:
   :backlinks: none

Bago sundin ang gabay na ito dapat ay tapos ka na humiling ng iyong paunang account at identity, tulad ng inilarawan sa :ref:`the previous chapter<testnet-get-started>`.

Gumawa nang bagong account
===========================
Bago pumunta kung paano gumagana ang mga account, balances at transaksyon, gumawa muna tayo ng isang pangalawang account. Magsimula sa pamamagitan ng pagpunta
sa pahina ng *Accounts*. Sa kanang sulok sa itaas ay makikita mo ang **plus sign**. Pindutin upang magpatuloy. Sa susunod na screen
hihilingin sa iyo na pangalanan ang iyong bagong account. Sa halimbawang ito pipiliin namin ang pangalang *Example Account 2*, 
ngunit maaari kang pumili ng alinmang pangalan na gusto mo.

.. image:: images/concordium-id/acc1.png
      :width: 32%
.. image:: images/concordium-id/acc2.png
      :width: 32%

Kapag pinindot mo ang **Next**, makikita mo ang screen kung saan ay dapat mong magpasya kung aling identity ang gagamitin mo upang buksan ang bagong account.
Sa ngayon marahil ay mayroon ka lamang isa, ngunit kung mayroon kang higit sa isa ay maaari kang pumili ng alinmang identity na nais mo mula sa listahan.
Sa pamamagitan ng pag-click sa isang identity, dadalhin ka sa susunod na screen. Kapag gagawa ka ng isang hindi paunang account, i.e.  isang account na 
hindi nilikha sa paggawa ng identity, ay maaari kang pumili upang magbunyag ng bilang ng :ref:`glossary-attribute`. ito ay hindi kinakailangan, at kung wala 
kang isang tukoy na dahilan upang gawin ito, inirerekumenda namin na huwag ilantad ang anuman, dahil ang mga isiniwalat na katangian ay mapupunta sa chain at hindi matatanggal.

.. image:: images/concordium-id/acc3.png
      :width: 32%
.. image:: images/concordium-id/acc4.png
      :width: 32%

Kung pipindutin mo ang **Reveal account attributes button**, dadalhin ka sa susunod na pahina. Maaari mong i-tick ang mga katangiang nais mong ibunyag, 
at pagkatapos ay pindutin ang **Submit account**. Sa pagpipindot nang **Submit account** dito or sa susnod na pahina ay, dadalhin ka sa huling pahina ng paglikha ng account, 
na magbibigay sa iyo ng isang maikling pangkalahatang ideya at sasabihin sa iyo na ang account ay naisumite.

.. image:: images/concordium-id/acc5.png
      :width: 32%
.. image:: images/concordium-id/acc6.png
      :width: 32%

Sa pamamagitan nang pagpindot sa **Ok, thanks** sa pangkalahatang ideya ng pagsusumite, ibabalik ka sa pahina ng account. Maaari mong makita na ang iyong bagong 
account ay nakabinbin pa rin, at maaaring tumagal ng ilang minuto upang ma finalize sa chain.Kung hindi mo pa nasubukang gawin ito, maaari mong subukang pindutin 
ang pababang nakaharap na arrow sa isa sa mga card ng account, upang makita na fino-fold nito ang card. Sisiniwalat nito ang dalawang bagong impormasyon, 
*at disposal*  at * staked *. Ang at disposal ay sasabihin sayo kung magkano ang balanse ng mga account na magagamit mo para sa ibinigay na sandali, at ang staked amount ay 
maaari kang magbasa nang higit pa tungkol sa :ref:`managing accounts<managing_accounts>` pahina.

.. image:: images/concordium-id/acc7.png
      :width: 32%
.. image:: images/concordium-id/acc8.png
      :width: 32%


Gumawa ng isang transaksyon
===========================
Susunod, subukang pindutin ang **Balance** area ng iyong bagong nilikha na account. Sa screen na ito maaari mong makita ang kasalukuyang balanse 
ng iyong account, at sa puntong  ito, papayagan ka ring humiling ng 100 GTU na para gamitin sa Testnet. Ang kahilingan para sa 100 GTU ay isang tampok sa Testnet, 
at para sa Testnet 4 ay ililipat nito ang 2000 GTU sa account, kahit na ang nakalagay ay 100. Ang drop ng GTU ay magagamit lamang sa isang account nang isang beses. 
Sa pamamagitan ng pagpindot dito, mapapansin mo ang paglitaw ng isang transaksyon. Hihintayin ito nang kaunti, at makalipas ang ilang sandali, ang 2000 GTU ay 
madadagdag sa iyong account

.. image:: images/concordium-id/acc9.png
      :width: 32%
.. image:: images/concordium-id/acc10.png
      :width: 32%

Ngayon ay mayroon na tayong ilang GTU sa ating account, susubukan nating gumawa ng isang transaksyon. Pindutin ang ** SEND ** upang magawa iyon. Sa susunod na 
pahina maaari mong i-input ang halaga nang nais mong ilipat, at pumili ng tatanggap. Sa halimbawang ito ililipat natin ang 10 GTU.

.. image:: images/concordium-id/acc11.png
      :width: 32%
.. image:: images/concordium-id/acc12.png
      :width: 32%

Pag napagpasyahan na ang halaga, pipili ngayon tayo nang receipient.. Para magawa to, pindutin ang Select **Recipient or shield amount** button.
Sa pahinang ito maaari kang maghanap nang mga receipient sa iyong *address book* o mag-dagdag nang recipient sa pamamagitan nang pag-scan sa recipient's accounts QR code.
Tulad ng nakikita mo sa screenshot, isa lang ang recipient na nai-saved, *Example Account 1*. bukod pa rito ay mayroon tayong pagpipilian na *Shield an
amount*, ngunit babalik din tayo sa dito mamaya. Pipiliin natin ang *Example Account 1* bilang ating recipient sa halimbawang ito.

.. image:: images/concordium-id/acc13.png
      :width: 32%
.. image:: images/concordium-id/acc14.png
      :width: 32%

Dahil nakapili na tayo nang halaga at recipient, maaari  na nating pindutin ang **Send Funds** upang magpatuloy. Sa pamamagitan nito, lalabas ang isang confirmation 
screen kung saan maaari nating mapatunayan ang halaga, recipient at sending account. Sa pamamagitan nang pagpindot sa **Yes, send funds**, I-veverify natin ang ating sarili 
gamit ang isang passcode o biometric, at pagkatapos ay isusumite ang transaksyon sa chain. Maaaring tumagal nang kaunti para mag-finalize ang transaksyon.

.. image:: images/concordium-id/acc15.png
      :width: 32%
.. image:: images/concordium-id/acc16.png
      :width: 32%

Makikita natin ngayon na ang *Example Account 2's Transfers* log ay nagpapakita na ang halaga ay nabawasan, kasama ang isang *fee*. Ang lahat ng mga transaksyon ay 
nagkakahalaga ng isang fee, at depende sa uri ng transaksyon na maaaring magkakaiba ang bayad. Ang pagpindot sa transaksyon ay magbibigay-daan sa iyo na makita ang higit pang
mga detalye.

.. image:: images/concordium-id/acc17.png
      :width: 32%
.. image:: images/concordium-id/acc18.png
      :width: 32%

.. _move-an-amount-to-the-shielded-balance:

Maglipat nang amount sa shielded balance
=========================================
Kung babalik tayo sa screen ng *Accounts*, makikita natin ngayon na ang 10 GTU ay nailipat sa *Balance of Example Account 1*. Tulad ng napansin mo dati, ang mga account ay 
mayroon ding :ref:`glossary-shielded-balance`. In short, ang shielded balance ay para sa pagpapanatili ng shielded (encrypted) na mga halaga nang GTU sa account. Subukan nating 
magdagdag ng ilang shielded GTU sa ating *Example Account 2*. Magsimula sa pamamagitan ng pagpindot sa **Shielded Balance** area sa ating account card

.. image:: images/concordium-id/acc19.png
      :width: 32%
.. image:: images/concordium-id/acc20.png
      :width: 32%

Susunod ay, pindutin ang **SEND** button muli at mag-enter nang amount ng GTU sa *shield*, na nag papakita nang pagkilos sa pagdaragdag ng ilang GTU sa *Shielded Balance*. 
Sa halip na pumili ng isang recipient, sa oras na ito ay pipindutin natin ang **Shield amount**.

.. image:: images/concordium-id/acc21.png
      :width: 32%
.. image:: images/concordium-id/acc22.png
      :width: 32%

Maaari na nating ipagpatuloy at kumpirmahin ang transaksyon, tulad ng ginawa natin dati sa regular na transfer. Maaaring magtagal ang 
transaksyon para ma-finalize sa chain.

.. image:: images/concordium-id/acc23.png
      :width: 32%
.. image:: images/concordium-id/acc24.png
      :width: 32%

Sa pamamagitan ng pag-back sa pahina ng *Accounts*, makikita na ngayon na mayroong 10 GTU sa *Shielded Balance*of*Example Account 2*. Kung ang *Shielded Balance* area ng 
account card ay pinindot, makikita natin na mayroong *Shielded amount* transaksyon sa shielded balance transfer log.
Ang paggawa ng isang shielding transksyon ay mayroon din na fee, ngunit ang fee na ito ay mababawas mula sa regular na balanse ng account. Subukan mong pumunta bumalik at 
tingnan ang transfer log ng regular na *Balance*.

.. image:: images/concordium-id/acc25.png
      :width: 32%
.. image:: images/concordium-id/acc26.png
      :width: 32%

Gumawa ng isang Shielded Transfer
==================================
Pagkakaroon nang available shielded GTU, maaari na nating subukan ang paggawa ng isang *Shielded transfer*, na nangangahulugang maaari tayong gumawa ng isang transfer na may  
encrypted amount nang GTU. Ang unang hakbang ay mag-browse sa pahina ng *shielded balance* ng account na naglalaman ng shielded GTU, kung hindi mo pa ito nagagawa noon.
Pagkatapos ay pindutin ang **SEND** button. pwede ka nang mag enter nang amount at pumili nang recipient. Sa halimbawang ito napili nating
ilipat ang 2 GTU. Kapag pinindot ang **Select Recipient or unshield amount** button, ay maaari kang pumili ng isang recipient. Pipiliin natin ang
*Example Account 2* sa halimbawang ito.

.. image:: images/concordium-id/acc27.png
      :width: 32%
.. image:: images/concordium-id/acc28.png
      :width: 32%

Dahil may amount at recipient na tayo pwede na tayong magpatuloy. Tulad ng iba pang mga transaksyon, makakakita ka ngayon ng isang confirmation screen, at sa pamamagitan ng 
pagpapatuloy mula doon, ma-e-verify mo ang iyong sarili gamit ang isang passcode o biometric, at pagkatapos ay isusumite ang shielded transaction sa chain, Muli 
ang transaksyon ay maaaring tumagal ng ilang sandali upang mai-finalize sa chain.

.. image:: images/concordium-id/acc29.png
      :width: 32%
.. image:: images/concordium-id/acc30.png
      :width: 32%


Ngayon, kung babalik ka sa screen ng *Accounts*. Makikita mo ang isang maliit na shield ay lumitaw sa tabi ng amount sa *Shielded Balance* sa receiving account.
Ipinapahiwatig nito na may mga bagong natanggap na mga shielded transaction sa shielded balance.
Subukang pindutin ang shielded balance, at pansinin na kailangan mong maglagay ng isang passcode o gamitin ang iyong biometric upang makapasok dito.
Nangyayari ito dahil kailangan mong i-decrypt ang natanggap na mga shielded transactions, bago mo makita ang amount.

.. image:: images/concordium-id/acc31.png
      :width: 32%
.. image:: images/concordium-id/acc32.png
      :width: 32%

I-unshield ang isang amount
============================
Matapos ang decryption, ang amount ay makikita na ngayon sa *shielded balance* at sa account card sa screen ng *Accounts*. Ngayon, paano kung nais nating ilipat ang ilang GTU 
mula sa isang shielded balance sa regular balance? Subukan nating ilipat ang 2 GTU sa regular balance sa pamamagitan ng function ng *Unshielding* an amount. Upang magawa ito, 
pindutin ang **SEND** na button sa may shielded balance. Mag-enter nang 2 bilang amount, at pagkatapos ay pindutin ang **Select Recipient o unshield amount**. 
**Choose Unshield amount**.

.. image:: images/concordium-id/acc33.png
      :width: 32%
.. image:: images/concordium-id/acc34.png
      :width: 32%

Tapusin ang transaksyong tulad ng ginawa mo sa iba pa, at subukang mag-browse sa regular na balance ng account upang makita ang unshielding.
Kung ang transaksyon ay na-finalize na sa chain, dapat mo na ngayong makita na ang *Unshielded amount* na naka-ticked in sa regular balance.
Pansinin kung paano naging hindi 2 GTU, kahit na ang halaga na na-unshield mo lang ay 2. Ito ay dahil ang fee para sa paggawa ng anumang transaksyon, 
kasama ang unshielding ay ibabawas mula sa regular balance ng account na responsable para sa transaksyon.

.. image:: images/concordium-id/acc35.png
      :width: 32%
.. image:: images/concordium-id/acc36.png
      :width: 32%

Ibahagi ang iyong account address
==================================
Kung nais mong ibahagi ang address ng iyong account, madali itong magagawa sa pamamagitan ng pagpindot sa **Address** na button. Dadalhin ka nito sa isang pahina kung saan 
mayroon kang maraming mga pagpipilian ng pagbabahagi ng account address. Subukang pindutin ang **Share** na button, at ibahagi ang iyong address sa sinuman.
.. image:: images/concordium-id/acc37.png
      :width: 32%
.. image:: images/concordium-id/acc38.png
      :width: 32%

I-inspect ang release schedule
===============================
Sa Concordium blockchain posible na gumawa ng isang transaksyon na nagre-release ng transferred amount over time. Tinawag 
itong *transfer with a schedule*. Sa ngayon, hindi muna natin gagawin kung pano yung pag transfer dahil hindi ito maaaring gawin mula sa Concordium ID, 
ngunit suriin natin kung paano ang isang release schedule pwedeng ma-inspect. Kung nakatanggap ka ng isang transfer na schedule release, maaari mong pindutin 
ang **burger menu** sa kanang sulok sa itaas ng balance screen. Papayagan ka nitong pindutin ang **Release schedule**, at sa pamamagitan nito ay dadalhin ka sa isang screen na 
naglalaman ng impormasyon tungkol sa kung magkano ang ilalabas na GTU at kailan. Kung nais mong matuto nang higit pa tungkol sa kung paano gumawa ng isang transfer with a 
release schedule, maaari kang tumingin sa :ref:`concordium_client` at :ref:`transactions` na pahina.

.. image:: images/concordium-id/rel1.png
      :width: 32%
.. image:: images/concordium-id/rel2.png
      :width: 32%
.. image:: images/concordium-id/rel3.png
      :width: 32%

Suporta at Puna
==================

Kung magkakaroon ka ng anumang mga isyu o may mga mungkahi, ay magpost o mag feedback 
sa `Discord`_, o I-contact kami sa testnet@concordium.com.
