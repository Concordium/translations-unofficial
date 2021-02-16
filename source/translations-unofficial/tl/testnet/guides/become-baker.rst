
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker:

=====================================
Maging Baker (mag-create nang blocks)
=====================================

.. contents::
   :local:
   :backlinks: none

Ipinapaliwanag ng seksyong ito kung ano ang isang baker, ang papel nito sa network at kung paano 
maging isa.

Sa pamamagitan ng pagbasa sa seksyong ito malalaman mo:

-  Ano ang isang baker at ang mga konseptong nauugnay dito.
-  Paano i-upgrade ang iyong node upang maging isang baker.

Ang proseso ng pagiging baker ay maaaring nakabuod sa mga sumusunod na hakbang:

#. Kumuha ng account at ilang mga GTU
#. Kumuha ng isang hanay ng mga key ng baker.
#. I-rehistro ang baker keys sa account.
#. Simulan ang node gamit ang mga baker keys.

Matapos makumpleto ang mga hakbang na ito, ang baker node ay magluluto ng mga blocks. Kung ang isang baker blocks ay idinagdag sa chain 
ang node nang baker ay makakatanggap ng reward.

.. note::

   Sa seksyong ito gagamitin natin ang pangalan``bakerAccount`` bilang pangalan ng account na gagamitin upang 
   magparehistro at mag-manage nang baker

Mga kahulugan
==============

Baker
-----

Ang node ay isang *baker* o (*is baking*) kapag aktibong nagpa-participate sa network sa pamamagitan 
ng paglikha ng mga bagong blocks na idinagdag sa chain. Nangongolekta, nag-uutos at pinatutunayan ng isang baker 
ang mga transaksyon na kasama sa isang bloke upang mapanatili ang integridad ng blockchain. Ina-asign nang baker ang bawat 
blocks sa pag be-bake upang suriin at maipatupad ang blocks ng mga kalahok sa network.

Baker keys
----------

Ang bawat baker ay may isang hanay ng mga cryptographic key na tinatawag na *baker keys*. Ginagamit ng node ang mga key na 
ito upang lagdaan ang mga blocks na nae-bake nito. Upang makapag bake ng blocks na nilagdaan ng isang specific na baker, Ang node 
ay dapat na nagra-run kasama ang nae-load na mga set na baker keys.

Baker account
-------------

Ang bawat account ay pwedeng gumamit ng isang set nang baker keys upang marehistro bilang isang baker.

Tuwing ang isang baker ay nakapag-bake ng isang wastong blocks na kasama sa chain ay ilang oras din 
ay bibigyan nang reward ang nauugnay na account.

Stake and lottery
-----------------

.. todo::

   - Link to release schedule.

Maaaring mag-stake nang ilang  GTU balance ang isang account sa *baker stake* at pwede
ring mai-release ang ilang bahagi nang na staked na amount. Ang na-staked na account ay hindi 
maaaring malipat o ma transfer hanggang sa ito ay mai-release ng baker

.. note::

  Kung ang account ay nagmamay-ari nang isang transferred amount gamit ang release schedule, 
  ang amount ay pwedeng mai-stake kahit hindi pa ito nai-rerelease.

Upang mapili para makag-bake sa isang blocks, ang bakeray dapat lumahok sa isang *lottery* 
kung saan ang posibilidad na makakuha ng isang panalong tiket ay bahagyang 
katimbang sa naipong halaga.

Ang parehong stake ay ginagamit kapag kinakalkula kung ang isang baker ay kasama sa 
finalization committee o hindi. Tingnan ang Finalization_.

.. _epochs-and-slots:

Epochs and slots
----------------

Sa Concordium blockchain, ang oras ay nahahati sa *slots*. Ang mga slots ay may tagal ng oras na naayos sa 
block ng Genesis. Sa anumang naibigay na sangay, ang bawat slots ay maaaring magkaroon ng 
halos isang blocks, ngunit maraming mga blocks sa iba't ibang mga sangay ang maaaring 
gawin sa parehong slot.

.. todo::

   Let's add a picture.

kapag isinasaalang-alang ang mga rewards at iba pang mga konsepto na nauugnay sa pagbe-bake, ay ginagamit natin 
ang konsepto ng isang *epoch* bilang isang yunit ng oras na tumutukoy sa isang panahon kung saan naayos ang hanay ng 
mga kasalukuyang bakers at stakes. Ang mga epoch ay may tagal ng oras na naayos sa block ng Genesis. Sa testnet, ang mga epoch ay may tagal na **1 oras**.

Start baking
============

Mag-manage nang accounts
------------------------

Nagbibigay ang seksyong ito ng isang maikling recap ng mga nauugnay na 
hakbang para sa pag-import ng isang account. Para sa isang kumpletong paglalarawan, tingnan :ref:`managing_accounts`.

Ang mga account ay nilikha gamit ang :ref:`concordium_id` app. Kapag ang isang account ay naging
matagumpay na nilikha, pagna-navigate sa **More** tab at pagpili sa **Export**
Pinapayagan kang makakuha ng isang JSON file na naglalaman ng impormasyon ng account

Upang mag-import ng isang account sa toolchain run

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount

``concordium-client`` hihingi ng isang password upang mai-decrypt ang nai-export na file at
i-import ang lahat ng account. Gagamitin ang parehong password para sa pag-encrypt ng
mga susi sa pag-sign ng transaksyon at ang encrypted transfers key.

Paglikha ng mga keys para sa isang baker at pagrerehistro nito
---------------------------------------------------------------

.. note::

   Para sa prosesong ito ang account ay kailangang magmay-ari ng ilang GTU kaya siguraduhing 
   humiling ng drop ng 100 GTU para sa account sa mobile app.
   
Ang bawat account ay may natatanging baker ID na ginagamit kapag nagrerehistro ng baker nito. This
ID has to be provided by the network and currently cannot be precomputed. This
ID must be given inside the baker keys file to the node so that it can use the
baker keys to create blocks. The ``concordium-client`` will automatically fill
this field when performing the following operations.

Upang lumikha ng isang sariwang hanay ng mga key:

.. code-block:: console

   $concordium-client baker generate-keys <keys-file>.json

Kung saan maaari kang pumili ng isang arbitrary na pangalan para sa mga keys file. Upang irehistro ang mga keys 
sa network kailangan mong :ref:`running a node <running-a-node>`
at mag send nang``baker add`` transaction to the network:

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

papalitan

- ``<amountToStake>`` gamit ang GTU amount para sa baker's initial stake
- ``<concordium-data-dir>`` kasama ang mga sumusunod na direktoryo ng data:

  * on Linux and MacOS: ``~/.local/share/concordium``
  * on Windows: ``%LOCALAPPDATA%\\concordium``.

(Ang pangalan ng output file ay dapat manatili``baker-credentials.json``).

Kailangan ang ``--no-restake`` upang awtomatically maiwasan ang pag add nang rewards sa stake amount nang baker. 
Ang behaviour na ito ay inilarawan sa seksyon `Restaking the earnings`_.

Upang masimulan ang node gamit ang mga key ng baker at simulang gumawa ng mga blocks kailangan mo munang
i-shut down ang kasalukuyang node na tumatakbo (either pindutin ang
``Ctrl + C`` sa the terminal kung saan nagra-run
``concordium-node-stop`` executable).

Matapos mailagay ang file sa naaangkop na direktoryo (tapos na sa dating command kapag 
tumutukoy sa output file), I-start ulit ang node
``concordium-node``. Ang node ay awtomatikong magsisimulang magbe-bake kapag ang baker  
ay napapasama sa mga baker para sa kasalukuyang panahon.

Isasagawa ang pagbabagong ito
kaagad at magkakabisa pagkatapos nang epoch kung saan ang transaksyon para sa 
pagdaragdag ng baker ay kasama sa isang blocks.

.. table:: Timeline: adding a baker

   +-------------------------------------------+-----------------------------------------+-----------------+
   |                                           | When transaction is included in a block | After 2 epochs  |
   +===========================================+=========================================+=================+
   | Change is visible by querying the node    |  ✓                                      |                 |
   +-------------------------------------------+-----------------------------------------+-----------------+
   | Baker is included in the baking committee |                                         | ✓               |
   +-------------------------------------------+-----------------------------------------+-----------------+

.. note::

   Kung ang transaksyon para sa pagdaragdag ng baker ay kasama sa isang blocks sa panahon ng epoch `E`, ang 
   baker ay isasaalang-alang bilang bahagi ng baking committee
   `E+2` start.

Pagma-manage nang baker
==================

Sinusuri ang katayuan ng baker at ang lakas nito sa loterya
-----------------------------------------------------------

Upang makita kung ang node ay nagbe-bake, maaari mong suriin ang iba't ibang mga mapagkukunan 
na offer ng iba't ibang antas ng katumpakan sa ipinakitang impormasyon

- Sa `network dashboard <http://dashboard.testnet.concordium.com>`_, Iyong
  node ay ipapakita ang baker ID sa ``Baker`` column.
- Gamit ang ``concordium-client`` maaari mong suriin ang listahan ng mga kasalukuyang baker
  and the relative staked amount that they hold, i.e. their lottery power.  The
  lottery power will determine how likely it is that a given baker will win the
  lottery and bake a block.

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

- Gamit ang``concordium-client`` maaari mong suriin na ang account ay nakarehistro sa isang 
baker at ang kasalukuyang halaga na natipon ng baker na iyon.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- Kung ang staked amount ay sapat na malaki at mayroong isang node na tumatakbo kasama ang mga baker 
  keys na na-load, ang baker na iyon ay dapat na gumawa ng mga blocks at maaari mong makita sa iyong 
  mobile wallet na ang mga gantimpala sa pagbe-bake ay natatanggap ng account,
  tulad ng nakikita sa imaheng ito

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Pag-update sa staked amount
--------------------------

Upang mai-update ang baker stake run

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

Ang pagbabago sa staked amount ay nag reresulta sa probabilidad na ang baker ay ma-elect 
na mag bake nang blocks.

Pag ang baker ay nag **adds stake for the first time or increases their stake**,ang pagbabago na 
iyon ay isinasagawa sa chain at ay nakikita kaagad hanggang ang transaksyon
ay mai-sama sa isang blocks (can be seen through ``concordium-client account show
bakerAccount``) at gagana nang 2 epochs pagkatapos nito.

.. table:: Timeline: increasing the stake

   +----------------------------------------+-----------------------------------------+----------------+
   |                                        | When transaction is included in a block | After 2 epochs |
   +========================================+=========================================+================+
   | Change is visible by querying the node | ✓                                       |                |
   +----------------------------------------+-----------------------------------------+----------------+
   | Baker uses the new stake               |                                         | ✓              |
   +----------------------------------------+-----------------------------------------+----------------+

Pag ang baker ay nag **decreases the staked amount**, kailangan magbago *2 +
bakerCooldownEpochs* epochs upang mag-effect. Ang pagbabago ay nakikita sa chain sa sandaling ang transaksyon
ay nakasama sa isang blocks, maaari itong ikunsulta sa pamamagitan ng
``concordium-client account show bakerAccount``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: Timeline: decreasing the stake

   +----------------------------------------+-----------------------------------------+----------------------------------------+
   |                                        | When transaction is included in a block | After *2 + bakerCooldownEpochs* epochs |
   +========================================+=========================================+========================================+
   | Change is visible by querying the node | ✓                                       |                                        |
   +----------------------------------------+-----------------------------------------+----------------------------------------+
   | Baker uses the new stake               |                                         | ✓                                      |
   +----------------------------------------+-----------------------------------------+----------------------------------------+
   | Stake can be decreased again or        | ✗                                       | ✓                                      |
   | baker can be removed                   |                                         |                                        |
   +----------------------------------------+-----------------------------------------+----------------------------------------+

.. note::

   sa Testnet, ``bakerCooldownEpochs`` ay naka set sa 168 epochs. Maaaring suriin ang value na 
   ito ng mga sumusunod:

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   Tulad ng nabanggit sa `Definitions`_ seksyon, ang staked amount ay *naka-lock*, ibig sabihin hindi ito 
   maaaring ilipat o magamit para sa pagbabayad. Dapat mong isaalang-alang ito at I-consider na ang pag I-stake nang 
   amount ay hindi kakailanganin sa maikling panahon. Sa partikular, upang alisin ang isang baker o upang baguhin ang 
   staked amount ay kailangan mong magmay-ari ng ilang hindi naka-istake na GTU upang masakop ang mga gastos sa transaksyon.

Pagre-restake nang kita
-----------------------

kapag nakikilahok bilang isang baker sa network at mag bake nang blocks, makakatanggap ang account 
nang rewards sa bawat baked blocks. Ang mga rewards na ito ay awtomatikong 
idinagdag sa staked amount bilang default.

Maaari mong piliing baguhin ang pag-uugali na ito at sa halip ay makatanggap ng mga 
gantimpala sa account balance nang hindi awtomatikong mai-stake. Ang switch na ito ay maaaring
mabago sa ``concordium-client``:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Ang mga pagbabago sa  restake flag ay magkakabisa kaagad; gayunpaman, ang mga pagbabago ay nagsisimulang 
makaapekto sa baking at finalizing power sa epoch pagkatapos. Ang kasalukuyang halaga ng 
switch ay maaaring makita sa impormasyon ng account na pwedeng ma query.
gamit ang``concordium-client``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: updating restake

   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   |                                               | When transaction is included in a block | 2 epochs after being rewarded |
   +===============================================+=========================================+===============================+
   | Change is visible by querying the node        | ✓                                       |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   | Earnings will [not] be restaked automatically | ✓                                       |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   | If restaking automatically, the gained        |                                         | ✓                             |
   | stake affects the lottery power               |                                         |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+

Kapag nakarehistro ang baker, awtomatiko nitong muling mai-stake ang mga kita, ngunit bilang
nabanggit sa itaas, maaari itong mabago sa pamamagitan ng pagbibigay ng ``--no-restake`` 
flag sa ``baker add`` kung anong command ang nandito:

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Finalization
------------

Ang finalization ay ang proseso ng pagboto na isinagawa ng mga node sa *finalization committee* na 
nagfa *finalize* nang isang blocks  kapag ang isang sapat na malaking bilang ng mga miyembro ng komite 
ay nakatanggap ng blocks at sumang-ayon sa kinalabasan nito. Ang mga mas bagong blocks ay dapat magkaroon ng finalized 
block bilang isang ancestor upang matiyak ang integridad ng chain. Para sa karagdagang impormasyon tungkol sa prosesong ito, 
tingnan ang :ref:`finalization<glossary-finalization>` seksyon.

Ang finalization committee ay binubuo ng mga bakers na mayroong isang 
tiyak na staked amount. Partikular nitong ipinahihiwatig na upang makalahok sa
finalization committee marahil ay kailangan mong baguhin ang staked namount 
upang maabot ang nasabing threshold. Sa testnet, ang staked amount na kinakailangan 
upang makalahok sa finalization committee ay **0.1% ng kabuuang halaga ng mayroon kang GTU**.

Ang paglahok sa finalization committee ay gumagawa ng mga gantimpala 
sa bawat blocks na natapos. Ang mga rewards ay binabayaran sa account ng baker ilang 
oras pagkatapos ng ma-finalized ang block.

Pag-remove nang isang baker
===========================

Ang controlling account ay maaaring pumili nang i-de-rehistro na baker sa chain. Upang gawin 
ito kailangan mong isagawa ang``concordium-client``:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

Ina-alis nito ang baker mula sa listahan ng baker at i-unlock ang staked amount 
sa baker upang maaari itong malipat o malayang ilipat.

Kapag tinatanggal ang baker, ang pagbabago ay parehas ang timeline sa 
pagbabawas ng staked amount. Kainakailangan lang nang *2 + bakerCooldownEpochs* epochs upang magkabisa.
Ang pagbabago ay makikita sa chain sa sandaling ang transaksyon ay kasama sa isang blocks at maaari mong 
suriin kung kailan magkakabisa ang pagbabagong ito sa pamamagitan ng 
pagtatanong sa impormasyon ng account sa ``concordium-client`` gaya ng dati:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: removing a baker

   +--------------------------------------------+-----------------------------------------+----------------------------------------+
   |                                            | When transaction is included in a block | After *2 + bakerCooldownEpochs* epochs |
   +============================================+=========================================+========================================+
   | Change is visible by querying the node     | ✓                                       |                                        |
   +--------------------------------------------+-----------------------------------------+----------------------------------------+
   | Baker is removed from the baking committee |                                         | ✓                                      |
   +--------------------------------------------+-----------------------------------------+----------------------------------------+

.. warning::

   Ang pagbabawas ng staked amount at pag-alis ng baker ay hindi maaaring gawin 
   nang sabay-sabay. Sa panahon ng cooldown na ginawa sa pamamagitan ng pagbawas ng staked amount, 
   ang baker ay hindi maaaring alisin o vice versa.

Suporta at Puna
==================

Kung magkakaroon ka ng anumang mga isyu o may mga mungkahi, ay magpost o 
mag feedback sa `Discord`_, o I-contact kami sa testnet@concordium.com.
