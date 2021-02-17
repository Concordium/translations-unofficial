
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker:

==========================================
Maging isang Baker (lumikha ng mga blocks)
==========================================

.. contents::
   :local:
   :backlinks: none

Ipinapaliwanag ng seksyong ito kung ano ang isang baker, ang papel nito sa network at kung paano maging isang baker.

Sa pamamagitan ng pagbasa sa seksyong ito malalaman mo:

-  Ano ang isang baker at ang mga konseptong nauugnay dito.
-  Paano i-upgrade ang iyong node upang maging isang baker.

Ang proseso ng pagiging isang baker ay maaaring ibuod sa mga sumusunod na hakbang:

#. Kumuha ng isang account at ilang mga GTU.
#. Kumuha ng isang hanay ng mga key ng baker.
#. Irehistro ang mga key ng baker sa account.
#. Simulan ang node gamit ang mga key ng baker.

Matapos makumpleto ang mga hakbang na ito, ang baker node ay magluluto ng mga blocks. Kung
ang isang inihurnong blocks ay idinagdag sa chain ang baker ng node ay makakatanggap ng gantimpala.

.. Note::

   Sa seksyong ito gagamitin namin ang pangalang ``bakerAccount`` bilang pangalan ng account na gagamitin
   upang magparehistro at pamahalaan ang isang baker.

Mga kahulugan
=============

Baker
-----

Ang isang node ay isang *baker* (o *nagbi-baking*) kapag aktibong lumahok sa network sa
pamamagitan ng paglikha ng mga bagong blocks na idinagdag sa chain. Kinokolekta, inuutos at
pinatutunayan ng isang baker ang mga transaksyon na kasama sa isang block upang mapanatili ang
integridad ng blockchain. Nilalagdaan ng baker ang bawat block na niluluto nila upang ang block ay
maaaring masuri at maipatupad ng natitirang mga kalahok ng network.

Baker keys
----------

Ang bawat baker ay may isang hanay ng mga cryptographic key na tinatawag na  *baker keys*.
Ginagamit ng node ang mga key na ito upang lagdaan ang mga block na lutuin nito. Upang
makapaghurno ng mga block na nilagdaan ng isang tukoy na baker ang node ay dapat na tumatakbo
kasama ang hanay ng mga key ng baker na na-load

Baker account
-------------

Ang bawat account ay maaaring gumamit ng isang hanay ng mga key ng baker upang
magrehistro ng isang baker.

Tuwing ang isang baker ay “nagluluto” ng isang wastong block na kasama sa chain,
pagkatapos ng ilang oras ang isang gantimpala ay binabayaran sa nauugnay na account.

Stake and lottery
-----------------

.. todo::

   - Link to release schedule.

Maaaring i-stake ng account ang bahagi ng balanse ng GTU nito sa *baker stake* at sa paglaon ay
manu-manong mailabas ang lahat o bahagi ng naipong halaga. Ang staked na halaga ay hindi maaaring
ilipat o ilipat hanggang sa ito ay pinakawalan ng baker.

.. note::

   Kung nagmamay-ari ang isang account ng isang halaga na inilipat na may iskedyul
   ng paglabas, ang halaga ay maaaring mai-stoke kahit hindi pa inilabas.

Upang mapili para sa pagluluto sa isang block, ang baker ay dapat lumahok sa isang
*lottery* kung saan ang posibilidad na makakuha ng isang panalong tiket
ay halos katimbang sa naipong halaga.

Ang parehong stake ay ginagamit kapag kinakalkula kung ang isang baker ay kasama sa
finalization committee o hindi.  Tingnan ang Finalization_.

.. _epochs-and-slots:

Epochs and slots
----------------

Sa Concordium blockchain, ang oras ay nahahati sa *slots*. May oras ang puwang
tagal na naayos sa block ng Genesis. Sa anumang naibigay na sangay, ang bawat
puwang ay maaaring magkaroon ng karamihan sa isang block, ngunit maraming mga
block sa iba't ibang mga sangay ay maaaring gawin sa parehong puwang.

.. todo::

   Let's add a picture.

Kapag isinasaalang-alang ang mga gantimpala at iba pang mga konsepto na nauugnay sa
pagluluto sa hurno, ginagamit namin ang konsepto ng isang *epoch* bilang isang yunit
ng oras na tumutukoy sa isang panahon kung saan ang set ng mga kasalukuyang baker at
stakes ay naayos na. Ang mga panahon ay may tagal ng oras na naayos sa Genesis block.
Sa testnet, ang mga epoch ay may tagal na **1 oras**.

Pagsisimula ng baking
=====================

Pamamahala ng mga account
-------------------------

Nagbibigay ang seksyong ito ng isang maikling recap ng mga nauugnay na hakbang para sa
pag-import ng isang account. Para sa isang kumpletong paglalarawan,  tignan :ref:`managing_accounts`.

Ang mga account ay nilikha gamit ang: :ref:`concordium_id` app. Kapag matagumpay na nagawa ang
isang account, ang pag-navigate sa tab na **More** at pagpili sa **Export**
ay nagbibigay-daan sa iyo upang makakuha ng isang JSON file na naglalaman ng impormasyon ng account.

Upang mag-import ng isang account sa toolchain run

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount

``concordium-client`` hihingi ng isang password upang mai-decrypt ang
nai-export na file at mai-import ang lahat ng mga account. Gagamitin
ang parehong password para sa pag-encrypt ng mga susi sa pag-sign ng
transaksyon at ang naka-encrypt na mga transfer key.

Paggawa ng mga keys para sa baker at pagrerehistro nito
-------------------------------------------------------

.. note::

   Para sa prosesong ito ang account ay kailangang pagmamay-ari ng ilang GTU kaya
   siguraduhing humiling ng drop ng 100 GTU para sa account sa mobile app.

Ang bawat account ay may natatanging baker ID na ginagamit kapag nagrerehistro ng
baker nito. Ang ID na ito ay dapat ibigay ng network at sa kasalukuyan ay hindi
maaring precompute. Ang ID na ito ay dapat ibigay sa loob ng file ng mga baker key
sa node upang magamit nito ang mga key ng baker upang lumikha ng mga block.
Ang ``concordium-client`` ay awtomatikong pupunan ang patlang na ito kapag
nagsasagawa ng mga sumusunod na operasyon.

Upang lumikha ng isang sariwang hanay ng mga key na tatakbo:

.. code-block:: console

   $concordium-client baker generate-keys <keys-file>.json

kung saan maaari kang pumili ng isang di-makatwirang pangalan para sa
mga key file. Upang irehistro ang mga susi sa network na kailangan mong
maging :ref:`running a node <running-a-node>`
at magpadala ng isang  ``baker add`` ng transaksyon sa network:

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

pagpapalit

- ``<amountToStake>`` na may halagang GTU para sa paunang stake ng baker
- ``<concordium-data-dir>`` kasama ang sumusunod na direktoryo ng data:

  * sa Linux at MacOS: ``~/.local/share/concordium``
  * sa Windows: ``%LOCALAPPDATA%\\concordium``.

(Ang pangalan ng file ng output ay dapat manatili ``baker-credentials.json``).

Magbigay ng isang ``--no-restake`` na flag upang maiwasan ang awtomatikong
pagdaragdag ng mga gantimpala sa staked na halaga sa baker. Ang pag-uugali
na ito ay inilarawan sa seksyong  `Restaking ng mga kita`.

Upang masimulan ang node gamit ang mga key ng baker at simulang gumawa
ng mga block kailangan mo munang i-shut down ang kasalukuyang running node
(alinman sa pamamagitan ng pagpindot sa
``Ctrl + C`` sa terminal kung saan tumatakbo ang node o gamit ang
``concordium-node-stop`` executable).

Matapos mailagay ang file sa naaangkop na direktoryo (tapos na sa nakaraang
utos kapag tumutukoy sa output file), simulang muli ang node gamit ang
``concordium-node``. . Ang node ay awtomatikong magsisimulang magbe-bake kapag
ang baker ay isinama sa mga baker para sa kasalukuyang  epoch.

Ang pagbabagong ito ay agad na naisasagawa at magkakabisa kapag natapos ang epoch
pagkatapos ng isa kung saan ang transaksyon para sa pagdaragdag ng baker
ay kasama sa isang block.

.. table:: Timeline: pagdaragdag ng isang baker

   +----------------------------------------------------------------+-------------------------------------------------+--------------------------+
   |                                                                | Kapag ang transaksyon ay kasama sa isang block  | Pagkatapos ng 2 epochs   |
   +================================================================+=================================================+==========================+
   | Makikita ang pagbabago sa pamamagitan ng pagtatanong sa node   |  ✓                                              |                          |
   +----------------------------------------------------------------+-------------------------------------------------+--------------------------+
   | Ang Baker ay kasama sa baking committee                        |                                                 | ✓                        |
   +----------------------------------------------------------------+-------------------------------------------------+--------------------------+

.. note::

   Kung ang transaksyon para sa pagdaragdag ng baker ay kasama sa isang block sa panahon
   ng epoch  `E`, ang baker ay isasaalang-alang bilang bahagi ng baking committee kapag
   nagsimula ang epoch `E+2`.

Pamamahala sa baker
===================

Sinusuri ang katayuan ng baker at ang lakas nito sa loterya
-----------------------------------------------------------

Upang makita kung ang node ay nagluluto sa hurno, maaari mong suriin
ang iba't ibang mga mapagkukunan na nag-aalok ng iba't ibang antas ng
katumpakan sa ipinakitang impormasyon.

- Sa `network dashboard <http://dashboard.testnet.concordium.com>`_, ipapakita
  ng iyong node ang baker ID nito sa ``Baker`` kolum.
- Gamit ang ``concordium-client`` maaari mong suriin ang listahan ng mga
  kasalukuyang baker at ang kamag-anak na staked na halaga na hawak nila, ibig
  sabihin ang kanilang lakas sa loterya. Matutukoy ng lakas ng lottery kung
  gaano ito posibilidad na ang isang naibigay na baker ay mananalo sa lottery
  at magluto ng isang block.

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

- Gamit ang ``concordium-client`` maaari mong suriin kung mayroon ang account
  nakarehistro ng isang baker at ang kasalukuyang halaga na natipon ng baker na iyon.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- Kung ang staked na halaga ay sapat na malaki at mayroong isang node na
  tumatakbo kasama ang mga key ng baker na na-load, ang baker na iyon ay dapat
  na gumawa ng mga block at maaari mong makita sa iyong mobile wallet na ang
  mga gantimpala sa pagbe-bake ay natanggap ng account, tulad ng nakikita sa
  imaheng ito:

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Ang pag-update sa staked amount
-------------------------------

Upang mai-update ang pagtakbo ng stake ng baker

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

Binabago ng naka-stak na halaga ang posibilidad na mapili ang isang baker upang maghurno ng mga block.

Kapag ang isang baker ay **adds stake for the first time or increases their stake**, ang
pagbabago na iyon ay isinasagawa sa kadena at nakikita kaagad na ang transaksyon ay isinasama
sa isang block (maaaring makita sa pamamagitan ng (can be seen through ``concordium-client account show
bakerAccount``) at magkakabisa 2 epoch pagkatapos nito.

.. table:: Timeline: pagdaragdag ng stake

   +--------------------------------------------------------------+------------------------------------------------+----------------+
   |                                                              | Kapag ang transaksyon ay kasama sa isang block | After 2 epochs |
   +==============================================================+================================================+================+
   | Makikita ang pagbabago sa pamamagitan ng pagtatanong sa node | ✓                                              |                |
   +--------------------------------------------------------------+------------------------------------------------+----------------+
   | Gumagamit si Baker ng bagong stake                           |                                                | ✓              |
   +--------------------------------------------------------------+------------------------------------------------+----------------+

Kapag ang isang baker ay **decreases the staked amount**, kailangan ng pagbabago ng *2 +
bakerCooldownEpochs* upang magkabisa. Ang pagbabago ay nakikita sa chain sa sandaling
ang transaksyon ay kasama sa isang block, maaari itong kumunsulta sa pamamagitan ng
``concordium-client account show bakerAccount``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: Timeline: pagbawas ng stake

   +--------------------------------------------------------------+------------------------------------------------+------------------------------------------------+
   |                                                              | Kapag ang transaksyon ay kasama sa isang block | Pagkatapos ng *2 + bakerCooldownEpochs* epochs |
   +==============================================================+================================================+================================================+
   | Makikita ang pagbabago sa pamamagitan ng pagtatanong sa node | ✓                                              |                                                |
   +--------------------------------------------------------------+------------------------------------------------+------------------------------------------------+
   | Gumagamit si Baker ng bagong stake                           |                                                | ✓                                              |
   +--------------------------------------------------------------+------------------------------------------------+------------------------------------------------+
   | Maaaring mabawasan muli ang                                  | ✗                                              | ✓                                              |
   | stake o matanggal ang baker                                  |                                                |                                                |
   +--------------------------------------------------------------+------------------------------------------------+------------------------------------------------+

.. note::

   Sa testnet, ``bakerCooldownEpochs`` ay itinakda nang una sa 168 epochs. Maaaring suriin
   ang halagang ito tulad ng sumusunod:

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   Tulad ng nabanggit sa `Mga Kahulugan`_ seksyon, ang naka-stak na halaga ay naka-*locked*,
   ibig sabihin hindi ito maaaring ilipat o magamit para sa pagbabayad. Dapat mong isaalang-
   alang ito at isaalang-alang ang pagtutuon ng isang halaga na hindi kakailanganin sa maikling
   panahon. Sa partikular, upang alisin ang pagpapatala ng isang baker o upang baguhin ang naipong
   halaga na kailangan mo upang pagmamay-ari ng ilang hindi naka-istak na GTU upang masakop ang mga
   gastos sa transaksyon.

Restaking ng mga kita
----------------------

Kapag nakikilahok bilang isang baker sa network at mga baking block,
tumatanggap ang account ng mga gantimpala sa bawat lutong block. Ang
mga gantimpala na ito ay awtomatikong idinagdag sa naka-stak na halaga bilang default.

Maaari mong piliing baguhin ang pag-uugali na ito at sa halip ay makatanggap
ng mga gantimpala sa balanse ng account nang hindi awtomatiko nitong itinutuon.
Ang switch na ito ay maaaring mabago  ``concordium-client``:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Ang mga pagbabago sa bandila ng muling ibalik ay magkakabisa kaagad;
gayunpaman, ang mga pagbabago ay nagsisimulang makaapekto sa baking at pagtatapos
ng kapangyarihan sa panahon pagkatapos ng susunod. Ang kasalukuyang halaga ng switch
ay maaaring makita sa impormasyon ng account na maaaring ma-query gamit  ``concordium-client``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: pag-update sa restake

   +--------------------------------------------------------------+------------------------------------------------+-------------------------------+
   |                                                              | Kapag ang transaksyon ay kasama sa isang block | 2 epoch matapos gantimpalaan  |
   +==============================================================+================================================+===============================+
   | Makikita ang pagbabago sa pamamagitan ng pagtatanong sa node | ✓                                              |                               |
   +--------------------------------------------------------------+------------------------------------------------+-------------------------------+
   | Ang mga kita ay [hindi] awtomatikong maibabalik muli         | ✓                                              |                               |
   +--------------------------------------------------------------+------------------------------------------------+-------------------------------+
   |Kung awtomatikong magpapatuloy, nakakaapekto                  |                                                | ✓                             |
   | ang nakuhang stake sa lakas ng loterya                       |                                                |                               |
   +--------------------------------------------------------------+------------------------------------------------+-------------------------------+

Kapag nakarehistro ang baker, awtomatiko nitong muling mai-stake ang mga kita,
ngunit tulad ng nabanggit sa itaas, maaari itong mabago sa pamamagitan ng pagbibigay ng ``--no-restake`` flag sa ``baker add`` command na tulad ng
ipinapakita dito:

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Finalization
------------

Ang Finalization ay ang proseso ng pagboto na isinagawa ng mga node sa *finalization
committee* na *nagtatapos* sa isang block kapag ang isang sapat na malaking bilang ng
mga miyembro ng komite ay nakatanggap ng block at sumang-ayon sa kinalabasan nito. Ang
mga mas bagong block ay dapat magkaroon ng finalized block bilang isang ninuno upang
matiyak ang integridad ng chain. Para sa karagdagang impormasyon tungkol sa prosesong ito,
tingnan ang seksyong: :ref:`finalization<glossary-finalization>` section.

Ang finalization committee ay nabuo ng mga baker na mayroong isang tiyak na naitalagang halaga.
Partikular nitong ipinahihiwatig na upang lumahok sa komite ng finalization malamang na kailangan
mong baguhin ang staked na halaga upang maabot ang nasabing threshold. Sa testnet, ang staked na
halaga na kinakailangan upang lumahok sa finalization committee ay **0.1% of the total amount of existing GTU**.

Ang pakikilahok sa finalization committee ay gumagawa ng mga gantimpala sa bawat block na natapos.
Ang mga gantimpala ay binabayaran sa account ng baker ilang oras matapos ang pag-block ay natapos na.

Pagtatanggal sa isang baker
===========================

Ang pagpipiliang account ay maaaring pumili upang i-de-rehistro ang baker nito
sa chain. Upang gawin ito kailangan mong isagawa ang  ``concordium-client``:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

Aalisin nito ang baker mula sa listahan ng baker at i-unlock ang naimbak na
halaga sa baker upang maaari itong ilipat o malayang ilipat.

Kapag tinatanggal ang baker, ang pagbabago ay may parehong timeline tulad ng
pagbawas ng staked na halaga. Kailangan ng pagbabago ng  *2 + bakerCooldownEpochs* epochs
upang magkabisa. Ang pagbabago ay makikita sa chain sa sandaling ang transaksyon ay kasama
sa isang block at maaari mong suriin kung kailan magkakabisa ang pagbabagong ito sa
pamamagitan ng pagtatanong sa impormasyon ng account sa ``concordium-client`` tulad ng dati:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: Pagtatanggal sa isang baker

   +------------------------------------------------------------------+------------------------------------------------+----------------------------------------+
   |                                                                  | Kapag ang transaksyon ay kasama sa isang block | After *2 + bakerCooldownEpochs* epochs |
   +==================================================================+================================================+========================================+
   | Makikita ang pagbabago sa pamamagitan ng pagtatanong sa node     | ✓                                              |                                        |
   +------------------------------------------------------------------+------------------------------------------------+----------------------------------------+
   | Ang Baker ay tinanggal mula sa baking committee                  |                                                 | ✓                                     |
   +------------------------------------------------------------------+------------------------------------------------+----------------------------------------+

.. warning::

   Ang pagbawas ng staked na halaga at pag-aalis ng baker ay
   hindi maaaring gawin nang sabay-sabay. Sa panahon ng cooldown
   na ginawa sa pamamagitan ng pagbawas ng staked na halaga,
   ang baker ay hindi maaaring alisin at kabaligtaran.

Suporta at Katugunan
====================

Kung nagkakaroon ka ng anumang mga isyu o may mga mungkahi, i-post ang
iyong katanungan o puna sa `Discord`_, o makipag-ugnayan sa amin sa testnet@concordium.com.
