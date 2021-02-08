.. Dapat masagot:
    - Ano ang isang smart contract
    - Ano ang gamit nang smart contract
    - Ano-ano ang mga use cases
    - Ano ang mga hindi use cases

.. _introduksiyon:

===============================
Introduksiyon sa smart contracts
===============================

Ang isang smart contract ay isang piraso ng code na ibinibigay ng gumagamit na isinumite sa Concordium
blockchain, ginamit upang tukuyin ang pag-uugali na hindi direktang bahagi ng core
protokol Ang mga smart contract ay naisakatuparan ng mga node sa network ng Concordium
ayon sa paunang natukoy na mga patakaran. Ang kanilang pagpapatupad ay ganap na transparent, at lahat
nang mga node ay dapat sumang-ayon sa kung ano ang kinalabasan ng pagpapatupad. Nakabatay lamang sa publiko
ang magagamit na impormasyon.

Ang isang smart contract ay maaaring makatanggap, humawak at magpadala ng GTU, na-oobserbahan nito ang ilang
mga aspeto ng chain, at mapanatili ang sarili nitong estado. Ang mga smart contract ay palaging naisakatuparan 
bilang isang tugon sa ** panlabas ** mga pagkilos, hal. isang account na nagpapadala ng isang mensahe. 
Sa pagsasagawa ng smart contract ay madalas na isang maliit na bahagi ng isang mas malaking system, pagsasama at 
pag-andar ng off-chain. Isang halimbawa ng off-chain na functionalidad ay ang maaaring isang  server na 
nag-aanyaya sa smart contract batay sa data mula sa totoong mundo, tulad ng mga 
presyo ng stocks, o impormasyon sa panahon.

Para saan ang mga smart contracts?
=============================

Maaaring bawasan ng smart contract ang kinakailangang halaga ng pagtitiwala sa mga third-party, sa ilang mga kaso
inaalis ang pangangailangan para sa isang pinagkakatiwalaang third-party, sa ibang mga kaso binabawasan ang kanilang
mga kakayahan at sa gayon binabawasan ang dami ng tiwala na kinakailangan sa kanila.

Sapagkat ang smart contract ay naisakatuparang ganap na may transparency, sa isang paraan na ang sinumang may access sa 
isang node ay maaaring mapatunayang, maaari silang maging napaka kapaki-pakinabang para matiyak ang kasunduan sa pagitan 
ng mga partido.

.. _subasta:

Halimbawa ng smart contract na subasta
------------------------------

Ang isang kaso ng paggamit para sa smart contract ay maaaring para sa paghawak ng isang subasta; dito pino-program namin
ang smart contract upang tanggapin ang iba't ibang mga bid mula sa sinuman at subaybayan ito hanggang sa 
pinakamataas na bidder. Kapag natapos na ang subasta, ibinabalik ng smart contract ang nagwagi na bid na 
GTU sa nagbebenta at lahat ng iba pang mga bid. Dapat na ipadala ng nagbebenta ang item sa nagwagi.

Pinapalitan ng smart contract ang pangunahing papel ng auctioneer. Ang kontrata mismo ang siyang 
tanging namamahala sa bahagi ng pag-bid, at ang on-chain na pamamahagi ng mga GTU. 
Kinakailangan din sa malamang ang ilang lohika para sa pagbabayad sa pinakamataas na bidder kung ang nagbebenta ay hindi 
natutupad ang kanilang mga obligasyon. Nangangahulugan ito na ang contract ay kailangang sumuporta sa mga kuru-kuro ng 
patunay na natupad talaga ng nagbebenta ang kanilang mga obligasyon, o ilang paraan para sa pinakamataas na bidder na 
pwedeng mag-file ng isang reklamo. Hindi malulutas ng mga smart contracts ang mga 
totoong isyu sa mundo, at ang pinakamahusay
na solusyon ay nakasalalay sa mga detalye ng subasta.

Ano ano ang mga * hindi * para sa smart contracts?
===================================

Ang mga smart contracts ay isang kapanapanabik na teknolohiya at ang mga tao ay naghahanap pa rin ng mga 
bagong paraan upang samantalahin ang mga ito. 
Gayunpaman, may ilang mga kaso kung saan ang smart contracts ay hindi isang mahusay na solusyon. 

Ang isa sa mga pangunahing bentahe ng mga smart contracts ay ang pagtitiwala sa code pagpapatupad, at para makamit ito ay 
dapat na e-execute nang isang malakihang bilang nang blockchain network ang parehas na 
code at matiyak ang kasunduan ng resulta. Natural na nagiging mahal ito kumpara sa 
pagpapatakbo ng parehong code sa isang node
sa ilang cloud service

Sa mga kaso kung saan ang isang smart contract ay nakasalalay sa mabibigat na kalkulasyon, maaaring ito ay posibleng 
ilipat ang pagkalkula nito mula sa smart contract at hayaan ang smart contract na magpapatupad lamang ng ilang mga 
pangunahing bahagi ng pagkalkula, gamit ang cryptographic na mga diskarte upang matiyak na ang iba pang mga bahagi ay 
naisakatuparan nang tama.

Panghuli, mahalagang tandaan na ang mga smart contracts ay walang privacy at 
** lahat ** nang maa-access nang smart contracts ay maa-access din ng iba sa network ng Concordium,
nangangahulugang mahirap hawakan ang sensitibong data sa 
smart contract. Sa ilang mga kaso maaring gumamit ng mga cryptographic tools upang hindi direktang gagana sa data, 
sa halip ay hayaan ang smart contracts na ang gagawa sa mga 
derived notions tulad ng mga pag e-encrypt at commitments, na  siyang nagtatago sa aktwal na mga data.

Life cycle ng isang smart contract
==============================

Ang isang smart contract ay unang ide-niploy sa chain bilang bahagi ng isang :ref:`contract
module <contract-module>`. Pagkatapos nito ang isang smart contract ay maaaring ma *initialized*para
kumuha ng isang :ref:`smart contract instance<contract-instances>`. Panghuli ang isang smart 
contract instances ay maaaring paulit-ulit na naii-update ayon sa sarili nitong lohika.
