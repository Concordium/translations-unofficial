.. _Discord: https://discord.gg/xWmQ5tp

.. _testnet-get-started:

===============================================
Concordium ID: Magsimula sa pag-gamit nang app
===============================================

.. contents::
   :local:
   :backlinks: none

Bago sundin ang gabay na ito dapat mo nang natapos ang pag-install ng Concordium ID, tulad ng inilarawan sa :ref:`the previous chapter<testnet-get-the-app>`.

Mag set up ng passcode at biometrics
=====================================

Kapag binuksan mo ang app ng Concordium ID sa kauna-unahang pagkakataon, sasalubungin ka ng isang flow na makakatulong sa iyong pag-set up 
ng isang passcode at pagpapatotoo ng biometric, paglikha ng isang :ref:`glossary-initial-account`,
at gagabayan ka rin nito hanggang sa pagkuha ng :ref:`glossary-identity`. Ang paunang account ay isang espesyal na uri ng account,
na isinumite sa chain ng :ref:`glossary-identity-provider`, sa paglikha ng isang identity. Maaari kang gumawa ng parehong mga transaksyon 
mula sa isang paunang account tulad ng mula sa mga regular na account, ngunit ang may-ari ng paunang account ay
kilala ng identity provider. Matapos malikha ang iyong identity magagawa mong magsumite ng mga account sa iyong sariling 
chain, at ang mga ito ay hindi malalaman ng identity provider. Maaari kang matuto nang higit pa tungkol sa mga account sa :ref:`Identities
at accounts<reference-id-accounts>` page.

Ang unang screen na makikita mo kapag bubuksan mo ang Concordium ID ay ang isang ito. Ipapaliwanag nito na
kailangan mong dumaan sa prosesong ito upang makapagsimula.

Kung handa ka nang magpatuloy, maaari mong pindutin ang **Yes, let's go!** Hihilingin sa iyo sa susunod na screen na maglagay ng isang anim na 
digit na passcode. Kung mas gugustuhin mong gumamit ng isang buong password kasama ang mga titik, maaari mo ring piliing gawin ito dito.

.. image:: images/concordium-id/int1.png
      :width: 32%
.. image:: images/concordium-id/int2.png
      :width: 32%

.. todo::

   Write a directive to make two or more images side-by-side centered


Napili alinman sa isang passcode o isang buong password, ay makakakuha ka ng pagpipilian na gumamit din ng biometric kung sinusuportahan 
ito ng iyong telepono, ang pagkilala sa mukha o fingerprint. Inirerekumenda namin na gamitin ang biometric kung mayroon kang option na gawin ito.

.. image:: images/concordium-id/int3.png
      :width: 32%
      :align: center

Hilingin ang iyong paunang account at identity
=====================================================

Susunod, makakakuha ka ng pagpipilian sa pagitan ng paggawa ng isang bagong paunang account at identity o pag-import ng isang gawa na na account.
Ipagpalagay na ito ang unang pagkakataon na gumagamit ka ng Concordium ID, maaari kang pumili **I want to create my initial account** upang magpatuloy.

.. image:: images/concordium-id/int4.png
      :width: 32%
      :align: center


Sa susunod na screen makikita mo ang isang paglalarawan kung ano ang paunang account at ang tatlong mga hakbang na kailangan mong matapos upang makuha ito,
kasama ang iyong identity. Sa madaling salita, ang paunang account ay isang account na isinumite sa chain nang pinili mong identity provider,
na nangangahulugang malalaman nila na ikaw ang may-ari ng account. Mamaya maaari kang magsumite ng mga account sa
sa sarili mong chain, na nangangahulugang ang may-ari ng mga account na ito ay ikaw lamang.

.. image:: images/concordium-id/int5.png
      :width: 32%
      :align: center

Ang tatlong mga hakbang na nabanggit sa itaas ay:

1. Pangalan ng iyong paunang account
2. Pangalan ng iyong identity
3. Humihiling ng paunang account at identity mula sa pinili mong :ref:`glossary-identity-provider`

Makikita mo ang unang hakbang sa susunod na pahina, na mang-hihingi sa iyo na magpasok ng isang pangalan para sa iyong paunang account. 
Pindutin ang continue at magdadala ito sa iyo sa susunod na pahina, kung saan kailangan mong pangalanan ang iyong identity. 
Ang parehong mga pangalang ito ay ikaw lang nakakalaam. upang mapangalanan mo sila nang higit pa o mas kaunti kahit anong 
gusto mo(Mayroong ilang mga hadlang sa kung anong mga titik at palatandaan ang maaari mong gamitin).

Sa halimbawa sa ibaba, pinili naming tawagan ang aming paunang account *Example Account 1* at ang aming identity *Example Identity*. Tulad ng nabanggit,
maaari kang pumili ng alinmang mga pangalan na gusto mo.

.. image:: images/concordium-id/int6.png
      :width: 32%
.. image:: images/concordium-id/int7.png
      :width: 32%

Sa pamamagitan ng pagpindot sa **Continue to identity providers**, dadalhin ka sa isang pahina kung saan kailangan mong pumili sa pagitan ng *identity providers*.
Ang isang identity provider ay isang external entity na papatunayan kung sino ka, bago ibalik ang isang object ng identity na gagamitin sa chain.
Sa ngayon maaari kang pumili sa pagitan ng:

* *Notabene Development* na magbibigay sa iyo ng isang test identity na walang pagpapatunay sa tunay na buhay.
* *Notabene* na nagpapatunay nang iyong totoong identity sa buhay.

.. image:: images/concordium-id/int8.png
      :width: 32%
      :align: center

Sa pamamagitan ng pagpili ng Notebene Development, bibigyan ka ng test identity nang walang karagdagang pagtatalo. Kung pipiliin mo ang Notabene dadalhin ka
sa kanilang panlabas na pahina na nagbibigay ng identity, na gagabay sa iyo sa proseso ng pag-verify para sa isang object ng identity. 
Pagkatapos nito ay ibabalik ka sa Concordium ID.

Pagkatapos mo sa alinman sa mga pagbibigay ng identity, mapupunta ka sa susunod na screen. Ipapakita nito sa iyo ang isang pangkalahatang ideya 
ng iyong identity at ang paunang account.

.. image:: images/concordium-id/int9.png
      :width: 32%
      :align: center

Depende sa napili mong identity provider, ang layout ng identity card ay maaaring bahagyang magkakaiba. Maaari mong makita na ang
Ang Halimbawa ng Account 1 ay hawak ng identity example identity. Ang account na nilikha sa panahon ng prosesong ito ay mamarkahan 
ng *(Initial)* sa app, upang malaman mo kung aling account ang paunang account na isinumite sa chain  ng identity provider.

Sa pamamagitan ng pagpindot sa **Finish** dadalhin ka sa *Accounts screen*. Sa screen na ito ay makikita mo ang iyong bagong likhang paunang
account Maaaring nagpapakita ito ng isang *Pending icon*, na nangangahulugang ang identity provider ay pino-proseso pa ang pagsusumite at paglikha ng iyong
paunang account at identity. Maaari ka ring mag-navigate sa screen ng *Identities screen* sa pamamagitan ng pag-click sa **Identities** sa ilalim ng
display. Sa screen na ito maaari mong makita ang iyong bagong nilikha na identity, na maaaring nakabinbin pa rin kung sakaling ang identity provider ay hindi pa tapos. 
Ang kailangan mo lang gawin ngayon, ay maghintay na matapos sila.

.. image:: images/concordium-id/int10.png
      :width: 32%
.. image:: images/concordium-id/int11.png
      :width: 32%


Suporta at Puna
==================

Kung magkakaroon ka ng anumang mga isyu o may mga mungkahi, ay magpost o
mag feedback sa `Discord`_, o I-contact kami sa testnet@concordium.com.
