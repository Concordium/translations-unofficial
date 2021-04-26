.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node:

==========
Mag Run ng Node
==========

.. contents::
   :local:
   :backlinks: none

Sa gabay na ito, matutunan mo kung paano makapag-run ng isang node sa iyong computer upang makalahok sa network ng Concordium. 
Nangangahulugan ito na natanggap mo ang mga bloke at transaksyon mula sa iba pang mga node, pati na rin magpalaganap nang impormasyon 
tungkol sa mga bloke at transaksyon sa mga node sa Concordium network. Matapos sundin ang gabay na ito, magagawa mong

-  makapag-run nang Concordium node
-  obserbahan ito sa network dashboard at
-  mag-query sa estado ng Concordium blockchain direkta 
   mula sa iyong machine.

Hindi mo kailangan ng isang account upang makapag-run ng isang node.

Bago ka magsimula
================

Bago ka makapag-run ng isang Concordium node ay kakailanganin mong

1. Mag-Install at magpa-run nang Docker.

   -  Sa *Linux*, e-allow ang Docker bilang non-root user.

2. I-Download at e-ektract ang :ref:`concordium-node-and-client-download` software.

Mag-upgrade mula sa isang naunang bersyon ng Open Testnet
===============================================

Upang mag-upgrade sa kasalukuyang software ng Concordium para sa Open Testnet 4:

-  Sundin ang steps sa :ref:`download<downloads>` ang pinakabagong software ng 
   Concordium.

-  I-run ang ``concordium-node-reset-data`` mula sa hindi naka-zip na 
   archive.

   -  Para sa *Mac* users: sa unang pagkakataon na bubuksan mo ang tool, i-right-click mo ang
      ``concordium-node-reset-data`` file at piliin ang **Open**. Lalabas ang isang mensahe na ang software ay mula sa isang hindi 
      kilalang developer
      Piliin ang **Open** ulit.
   -  Para sa *Windows* users: sa unang pagkakataon na bubuksan mo ang tool,
      I-double-click ang ``concordium-node-reset-data`` file. Lalabas ang isang mensahe na ang software ay mula sa isang hindi 
      kilalang developer
      I-select ang **More info** → **Run anyway**.

-  Magtatanong ang tool

      *Nais mo ring alisin ang mga naka-save na key?*

   Ang mga account na nilikha para sa mga naunang bersyon ay hindi na wastong
   buksan ang Testnet 3. Samakatuwid, kung mayroon kang nakaimbak na mga account mula sa mga naunang bersyon inirerekumenda namin ang pagpasok ng ** y ** 
   na tatanggalin ang lahat ng account keys.

.. _running-a-node:

Running ng node
==============

Upang masimulan nang isang client ang pagsali sa Open Testnet ay dapat sundin ang mga hakbang na ito
hakbang na ito:

1. Buksan ang ``concordium-node`` ma-eexecute mula sa hindi naka-zip na mga archive.

-  Para sa *Mac* users: sa unang pagkakataon na bubuksan mo ang tool, I-right-click ang
   ``concordium-node`` binary at piliin ang **Open**. Lilitaw ang isang mensahe na ang software ay mula sa isang hindi kilalang developer. 
   piliin ang **Open** ulit.
-  Para sa *Windows* users: sa unang pagkakataon na bubuksan mo ang tool, I-right-click at, I-double-click
   lang ang ``concordium-node`` binary. Lilitaw ang isang mensahe na ang software ay mula sa isang hindi kilalang developer. 
   I-Select ang **More info** →
   **Run anyway**.
-  pag *restarting* ang node I-consider na gumamit nang
   ``--no-block-state-import`` na option. I-dadownload lamang nito ang mga pag-update sa Concordium blockchain na naganap habang ang node 
   ay hindi aktibo at maaaring mapabilis ang proseso ng boot.

2. Mag enter ka ng isang pangalan para sa iyong node. At makikita mo ang pangalang ito sa 
   pampublikong dashboard.

3. Kung ang tool ay nasimulan na dati tatanungin ka kung nais mong tanggalin ang lokal na database ng node bago simulan. Pagpinindot mo ang  ** y ** ay 
   buburahin nito ang database na naka save at pagkatapos ay muling gagawa nang bagong impormasyon sa estado nang Concordium blockchain mo na naka-save sa iyong computer. 
   **Tandaan na ang pagtanggal ng lokal na database ng node ay nangangahulugang mas matatagalan para maka-catch-up ang iyong node sa network ng Concordium.**

Ngayon Ida-download nang tool ang imahe ng Concordium Client at mai-load ito sa Docker.  
Magsisimula nang mag launch at maglalabas ng impormasyon ang client patungkol sa 
logging information ng node

Nakikita ang iyong node sa dashboard
=================================

Pagkatapos mai-run ang ``concordium-node`` pwede mong

-  tingnan ang iyong node sa `Network Dashboard`_
-  :ref:`query<testnet-query-node>` inpormasyon nang blocke, transaksiyon, at accounts

Network dashboard
-----------------

Aabutin ng ilang sandali ang client upang abutin ang estado ng
Concordium blockchain. Nagsasangkot ito, halimbawa, sa pag-download nang impormasyon 
tungkol sa lahat ng mga bloke sa chain.

Kabilang sa iba pang impormasyon, sa `Network Dashboard`_ ay maaari kang makakuha ng isang ideya kung gaano katagal 
aabutin ang iyong node upang maka catch-up sa chain. Dahil diyan, maari mong mai-compare ang node's **Length** value (bilang ng mga bloke 
na natanggap ng iyong node) sa **Chain Len** value (bilang ng bloke 
sa pinkamahabang chain sa network) na makikita mo sa bandang 
itaas ng dashboard.


Pagpapagana ng mga papasok na koneksyon
============================

Kung pinapa-run mo ang iyong node sa likod ng isang firewall, o sa likod ng iyong router,
kung gayon marahil ay makakakonekta ka lamang sa ibang mga node, ngunit ang ibang mga node 
ay hindi makapagsimula ng mga koneksyon sa iyong node. Ito ay okay lang, at ang iyong node ay 
ganap na lumalahok sa Network ng Concordium. Magagawa nitong magpadala ng mga transaksyon at,
:ref:`if so configured<become-a-baker>`, mag-bake at mag-finalize.

Gayunpaman maaari mo ring gawin ang iyong node na isang mas mahusay na kalahok sa network sa 
pamamagitan ng pagpapagana ng mga papasok na koneksyon. gamit ang default, ``concordium-node`` listens
on port``8888``para sa papasok na connections. Nakasalalay sa iyong network at pagsasaayos ng 
platform na kakailanganin mong ma-ipasa sa isang panlabas na port sa``8888``sa iyong router, 
buksan ang iyong firewall, or pareho. Ang mga detalye ng kung paano ito gawin ay 
depende sa iyong pagsasaayos.

Pag-configure ng mga port
-----------------

Ang node ay nakakonek sa apat na ports, na maaaring mai-configure sa pamamagitan ng pagbibigay 
ng naaangkop na mga argumento or command line kapag sisimulan ang node.. Ang mga port na 
ginamit ng node ay ang mga sumusunod:

-  8888, ito ang port para sa peer-to-peer networking, na pwedeng ma e-set gamit ang ``--listen-node-port``
-  8082, ito ang port para sa middleware, na pwedeng ma e-set gamit ang ``--listen-middleware-port``
-  10000, ang gRPC port, na pwedeng ma e-set gamit ang ``--listen-grpc-port``

Kapag binabago mo ang mga mappings ay dapat i-stop ang docker 
container. (:ref:`stop-a-node`), e-reset, at e-start ulit. Para ma e-reset ang container pwedeng gamitin ang
``concordium-node-reset-data`` o run ``docker rm concordium-client``
sa terminal.

*sina-suggest namin* na ang iyong firewall ay dapat na mai-configure lamang na payagan ang mga koneksyon 
sa publiko sa port na 8888 (the peer-to-peer networking port). 
Ang isang tao na may access sa iba pang mga port ay maaaring makakuha nang kontrol sa iyong node 
o mga account na nai-save mo sa node.

.. _stop-a-node:

Pag stop nang node
=================

Para mai-stop ang node, pindutin ang **CTRL+c**, at hinatayin ang node na mag clean
shutdown.

Kung hindi mo sinasadyang ma-isara ang window nang hindi malinaw na nakastop ang client, 
ito ay patuloy na nagra-run sa background nang Docker. Sa ganyang
kaso, gamitin mo ang ``concordium-node-stop`` binary sa parehong paraan sa pagbukas
nang ``concordium-node`` executable.

Suporta at Puna
==================

Ang Information log para sa iyong node ay maaaring makuha gamit ang
``concordium-node-retrieve-logs`` tool. Magse-save ito ng mga logs mula sa
pagra-run ng imahe sa isang file. Bukod pa rito, kung bibigyan ng permiso, kukuha ito 
ng impormasyon tungkol sa mga program na kasalukuyang tumatakbo sa system.

pwede mong ipadala ang iyong logs at system information, katanungan at puna sa
testnet@concordium.com. maaari ka ring makipag-ugnay sa amin sa `Discord`_, o
mai-check ang :ref:`troubleshooting page<troubleshooting-and-known-issues>`

