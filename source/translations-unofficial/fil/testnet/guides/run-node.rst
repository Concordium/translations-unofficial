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

However you can also make your node an even better network participant
by enabling inbound connections. By default, ``concordium-node`` listens
on port ``8888`` for inbound connections. Depending on your network and
platform configuration you will either need to forward an external port
to ``8888`` on your router, open it in your firewall, or both. The
details of how this is done will depend on your configuration.

Configuring ports
-----------------

The node listens on four ports, which can be configured by supplying the
appropriate command line arguments when starting the node. The ports
used by the node are as follows:

-  8888, the port for peer-to-peer networking, which can be set with
   ``--listen-node-port``
-  8082, the port used by middleware, which can be set with ``--listen-middleware-port``
-  10000, the gRPC port, which can be set with ``--listen-grpc-port``

When changing the mappings above the docker container must be
stopped (:ref:`stop-a-node`), reset, and started again. To reset the container either use
``concordium-node-reset-data`` or run ``docker rm concordium-client`` in
a terminal.

We *strongly recommend* that your firewall should be configured to only
allow public connections on port 8888 (the peer-to-peer networking
port). Someone with access to the other ports may be able to take
control of your node or accounts you have saved on the node.

.. _stop-a-node:

Stopping the node
=================

To stop the node, press **CTRL+c**, and wait for the node to do a clean
shutdown.

If you accidentally close the window without explicitly shutting down
the client, it will keep running in the background in Docker. In that
case, use the ``concordium-node-stop`` binary in the same way you opened
the ``concordium-node`` executable.

Support & Feedback
==================

Logging information for your node can be retrieved using the
``concordium-node-retrieve-logs`` tool. This will save logs from the
running image to a file. Additionally, if given permission, it will
retrieve information about the programs currently running on the system.

You can send your logs, system information, questions and feedback to
testnet@concordium.com. You can also reach out at our `Discord`_, or
check out our :ref:`troubleshooting page<troubleshooting-and-known-issues>`

