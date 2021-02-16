.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node:

====================
Pagpapatakbo ng Node
====================

.. contents::
   :local:
   :backlinks: none

Sa gabay na ito, matutunan mo kung paano magpatakbo ng isang node 
sa iyong computer na makikilahok sa network ng Concordium. Nangangahulugan 
ito na natanggap mo mga blocks at transaksyon mula sa iba pang mga node, 
pati na rin magpalaganap impormasyon tungkol sa mga blocks at transaksyon 
sa mga node sa Concordium Network. Matapos sundin ang gabay na ito, magagawa mong

-  magpatakbo ng Concordium node
-  maobserbahan ito sa network dashboard at
-  mag-query ng estado ng Concordium blockchain direkta mula sa iyong compyuter.

Di mo kailangan ng account para magpatakbo ng node.

Bago ka magsimula
================

Bago ka magpatakbo ng Concordium node ay kakailanganin mong

1. Mag-install at magpatakbo ng Docker.

   -  Sa *Linux*, payagan ang Docker na patakbuhin bilang isang non-root user.

2. Mag-download at i-extract ang :ref:`concordium-node-and-client-download` software.

Mag-upgrade mula sa isang naunang bersyon ng Open Testnet
=========================================================

Upang mag-upgrade sa kasalukuyang software ng Concordium para sa Open Testnet 4:

-  Sundin ang mga hakbang sa itaas upang :ref:`download<downloads>` ang pinakabagong Concordium
   software.

-  Patakbuhin ang ``concordium-node-reset-data`` executable mula sa unzipped
   archive.

   -  Para sa mga *Mac* users: sa unang pagkakataon na buksan mo ang tool, right-click ang
      ``concordium-node-reset-data`` file at piliin ang **Open**. Isang mensahe
      lilitaw na ang software ay mula sa isang hindi kilalang developer.
      Piliin ang **Open** uli.
   -  Para sa mga *Windows* users: sa unang pagkakataon na buksan mo ang tool,
      double-click ang ``concordium-node-reset-data`` file. Isang mensahe
      lilitaw na ang software ay mula sa isang hindi kilalang developer.
      Piliin ang **More info** → **Run anyway**.

- Ang tool ay magtatanong ng:

      *Do you also want to remove saved keys?*

   Ang mga account na nilikha para sa mga naunang bersyon ay hindi na wasto para sa Open Testnet 3. Samakatuwid, kung mayroon kang mga nakaimbak na account mula sa naunang mga bersyon ay inirerekumenda namin ang pag-tayp ng **y** na magbubura sa lahat ng mga accounts keys.

.. _running-a-node:

Pagpapatakbo ng node
====================

Upang simulang patakbuhin ang isang client na sasali sa Open Testnet sundin ang mga ito
mga hakbang:

1. Buksan ang ``concordium-node`` executable mula sa unzipped archive.

-  Para sa mga *Mac* users: sa unang pagkakataon na buksan mo ang tool, i-right-click ang
   ``concordium-node`` binary at piliin ang **Open**. Isang mensahe
      lilitaw na ang software ay mula sa isang hindi kilalang developer.
      Piliin ang **Open** uli.
-  Para sa mga *Windows* users: sa unang pagkakataon na buksan mo ang tool, i-double-click
   ang ``concordium-node`` binary. Isang mensahe lilitaw na ang software ay mula sa isang hindi kilalang developer. Piliin ang **More info** →
   **Run anyway**.
-  Kapag *restarting* ang isang node ay ikonsidera na gumamit ng
   ``--no-block-state-import`` na opsyon. Ida-download lang nito ang mga updates sa Concordium blockchain
   na naganap habang hindi aktibo ang iyong node at pwedeng mapabilis nito ang proseso ng pag-boot.
   

2. Maglagay ng pangalan para sa iyong node. Itong pangalan na ito ay ang madi-display sa public dashboard.

3. Kung ang tool ay nasimulan dati tatanungin ka nito kung gusto mong burahin ang local node database bago magsimula. Ang pagpindot sa **y** ay ito ay matatanggal at pagkatapos ay muling gagawa ng impormasyon sa estado ng Concordium blockchain na na-save sa iyong computer. **Tandaan na
   ang pagtanggal ng lokal na database ng node ay nangangahulugang mas magtatagal ito para sa iyo
   node upang makahabol sa network ng Concordium.**

I-download na ngayon ng tool ang Concordium Client image at mai-load ito sa Docker. 
Ang client ay ilulunsad at magsisimulang maglabas ng impormasyon ang pag-log ng client
tungkol sa pagpapatakbo ng node.

Tignan ang iyong node sa dashboard
==================================

Pagkatapos patakbuhin ang ``concordium-node`` pwede kang

-  pwede mong tignan ang iyong node sa `Network Dashboard`_
-  :ref:`query<testnet-query-node>` imporasyon patungkol sa mga blocks, mga transakyon, at mga accounts.

Network dashboard
-----------------

Aabutin ng ilang sandali ang client upang abutin ang estado ng
Concordium blockchain. Kasama ito, halimbawa, sa pag-download
impormasyon tungkol sa lahat ng mga blocks sa chain.

Kabilang sa iba pang impormasyon, sa `Network Dashboard`_ pwede ka magkaroon 
ng ideya kung gaano katagal aabutin ang iyong node upang abutin ang chain. 
Para dyan pwede mong ikumpara ang haba ng node sa pamamagitan ng **Length** value (number of
blocks your node received) kasama ang **Chain Len** value (bilang ng
mga blocks sa pinakamahabang chain sa network) na makikita sa pinakataas ng dashboard.


Pagpapagana ng mga papasok na koneksyon
=======================================

Kung pinapatakbo mo ang iyong node sa likod ng isang firewall, o sa likod ng 
iyong home router, kung gayon pwede ka lang kumonekta sa ibang mga nodes,
pero ang ibang mga nodes ay hindi makakapagsimula ng koneksyon sa iyo.
Ito ay ok lang, at ang iyong node ay makakasali pa rin sa Concordium network.
Pwede pa rin itong makapag-padala ng mga transaksyon at,
:ref:`if so configured<become-a-baker>`, makapag-bake at makapag-finalize.

Gayunpaman maaari mo ring gawing mas mahusay na kalahok sa network ang iyong node
sa pamamagitan ng pagpapagana ng mga inbound connections. Bilang default, ``concordium-node`` 
ay nakikinig sa port ``8888`` para sa inbound connections. Nakasalalay sa iyong network at
pag-configure ng platform na kakailanganin mo upang ipasa ang isang external port
sa ``8888`` sa iyong router, buksan mo ito sa iyong firewall, o pareho. Ang mga 
detalye kung paano ito gawin ay nakadepende sa kung paano mo ito kinonfigure.

Configuring ports
-----------------

Ang node ay nakikinig sa apat na mga ports, na maaaring mai-configure sa pamamagitan 
ng pagbibigay ng naaangkop na mga command line arguments kapag sinisimulan ang node. 
Ang mga ports na ginagamit ng node ay ang mga sumusunod:

-  8888, ang port para sa peer-to-peer networking, na pwedeng i-set kasama ng
   ``--listen-node-port``
-  8082, ang port na ginagamit ng middleware, na pwedeng i-set kasama ng ``--listen-middleware-port``
-  10000, ang gRPC port, na pwedeng i-set kasama ang ``--listen-grpc-port``

Kapag binabago ang mga mappings sa itaas ang docker container ay dapat itigil 
(:ref:`stop-a-node`), i-reset, at simulan uli. Para i-reset ang container pwede kang gumamit ng
``concordium-node-reset-data`` o patakbuhin mo ang ``docker rm concordium-client`` sa terminal.

*Lubos naming inirerekumenda* na ang iyong firewall ay dapat na-configure lamang
para payagan ang mga koneksyon na publiko sa port 8888 (ang peer-to-peer networking
port). Ang isang tao na may access sa iba pang mga ports ay maaaring makakuha
kontrol ng iyong node o sa mga accounts na nai-save mo sa node.

.. _stop-a-node:

Pagpapatigil sa node
====================

Para mapatigil ang node, pindutin ang **CTRL+c**, at maghinty para sa node na gawin nya ang isang malinis na shutdown.

Kung aksidente mong naisara ang window na hindi pa napapatay ang client, ito ay patuloy
na tatakbo sa likod sa Docker. Sa ganung pagkakataon, gamitin ang 
``concordium-node-stop`` binary sa katulad na paraan kung paano mo 
binuksan ang ``concordium-node`` executable.

Suporta at Katugunan
====================

Ang impormasyon sa pag-log para sa iyong node ay maaaring makuha gamit ang
tool na ``concordium-node-retrieve-logs`` Magse-save ito ng mga logs mula 
sa tumatakbong image sa isang file. Bilang karagdagan, kung bibigyan ng 
pahintulot, gagawin nitong kunin ang impormasyon tungkol sa mga program na 
kasalukuyang tumatakbo sa system.

Pwede mong ipadala ang iyong mga logs, impormasyon ng iyong sistema, 
mga katanungan at katugunan sa testnet@concordium.com. You can also reach out at our `Discord`_, 
o makipag-ugnay sa amin sa  :ref:`troubleshooting page<troubleshooting-and-known-issues>`

