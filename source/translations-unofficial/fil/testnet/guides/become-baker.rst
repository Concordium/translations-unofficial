
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
===========

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
============

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
na ito ay inilarawan sa seksyong  `Restaking the earnings`_.

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

   +----------------------------------------------------------------+------------------------------------------------+--------------------------+
   |                                                                | Kapag ang transaksyon ay kasama sa isang block | Pagkatapos ng 2 epochs   |
   +================================================================+=========================================+=================================+
   | Makikita ang pagbabago sa pamamagitan ng pagtatanong sa node   |  ✓                                      |                                 |
   +----------------------------------------------------------------+-----------------------------------------+---------------------------------+
   | Ang Baker ay kasama sa baking committee                        |                                         | ✓                               |
   +----------------------------------------------------------------+-----------------------------------------+---------------------------------+

.. note::

   If the transaction for adding the baker was included in a block during epoch `E`, the
   baker will be considered as part of the baking committee when epoch
   `E+2` starts.

Managing the baker
==================

Checking the status of the baker and its lottery power
------------------------------------------------------

In order to see if the node is baking, you can check various sources that
offer different degrees of precision in the information displayed.

- In the `network dashboard <http://dashboard.testnet.concordium.com>`_, your
  node will show its baker ID in the ``Baker`` column.
- Using the ``concordium-client`` you can check the list of current bakers
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

- Using the ``concordium-client`` you can check that the account has
  registered a baker and the current amount that is staked by that baker.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- If the staked amount is big enough and there is a node running with the baker
  keys loaded, that baker should eventually produce blocks and you can see
  in your mobile wallet that baking rewards are being received by the account,
  as seen in this image:

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Updating the staked amount
--------------------------

To update the baker stake run

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

Modifying the staked amount modifies the probability that a baker gets elected
to bake blocks.

When a baker **adds stake for the first time or increases their stake**, that
change is executed on the chain and becomes visible as soon as the transaction
is included in a block (can be seen through ``concordium-client account show
bakerAccount``) and takes effect 2 epochs after that.

.. table:: Timeline: increasing the stake

   +----------------------------------------+-----------------------------------------+----------------+
   |                                        | When transaction is included in a block | After 2 epochs |
   +========================================+=========================================+================+
   | Change is visible by querying the node | ✓                                       |                |
   +----------------------------------------+-----------------------------------------+----------------+
   | Baker uses the new stake               |                                         | ✓              |
   +----------------------------------------+-----------------------------------------+----------------+

When a baker **decreases the staked amount**, the change will need *2 +
bakerCooldownEpochs* epochs to take effect. The change becomes visible on the
chain as soon as the transaction is included in a block, it can be consulted through
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

   In the testnet, ``bakerCooldownEpochs`` is set initially to 168 epochs. This
   value can be checked as follows:

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   As noted in the `Definitions`_ section, the staked amount is *locked*,
   i.e. it cannot be transferred or used for payment. You should take this
   into account and consider staking an amount that will not be needed in the
   short term. In particular, to deregister a baker or to modify the staked
   amount you need to own some non-staked GTU to cover the transaction
   costs.

Restaking the earnings
----------------------

When participating as a baker in the network and baking blocks, the account
receives rewards on each baked block. These rewards are automatically added to
the staked amount by default.

You can choose to modify this behavior and instead receive the rewards in
the account balance without staking them automatically. This switch can be
changed through ``concordium-client``:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Changes to the restake flag will take effect immediately; however, the changes
start affecting baking and finalizing power in the epoch after next. The current
value of the switch can be seen in the account information which can be queried
using ``concordium-client``:

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

When the baker is registered, it will automatically re-stake the earnings, but as
mentioned above, this can be changed by providing the ``--no-restake`` flag to
the ``baker add`` command as shown here:

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Finalization
------------

Finalization is the voting process performed by nodes in the *finalization
committee* that *finalizes* a block when a sufficiently big number of members of
the committee have received the block and agree on its outcome. Newer blocks
must have the finalized block as an ancestor to ensure the integrity of the
chain. For more information about this process, see the
:ref:`finalization<glossary-finalization>` section.

The finalization committee is formed by the bakers that have a certain staked
amount. This specifically implies that in order to participate in the
finalization committee you will probably have to modify the staked amount
to reach said threshold. In the testnet, the staked amount needed to participate
in the finalization committee is **0.1% of the total amount of existing GTU**.

Participating in the finalization committee produces rewards on each block that
is finalized. The rewards are paid to the baker account some time after the
block is finalized.

Removing a baker
================

The controlling account can choose to de-register its baker on the chain. To do
so you have to execute the ``concordium-client``:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

This will remove the baker from the baker list and unlock the staked amount on
the baker so that it can be transferred or moved freely.

When removing the baker, the change has the same timeline as decreasing
the staked amount. The change will need *2 + bakerCooldownEpochs* epochs to take effect.
The change becomes visible on the chain as soon as the transaction is included in a block and you
can check when this change will be take effect by querying the account information
with ``concordium-client`` as usual:

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

   Decreasing the staked amount and removing the baker cannot be done
   simultaneously. During the cooldown period produced by decreasing the staked
   amount, the baker cannot be removed and vice versa.

Support & Feedback
==================

If you run into any issues or have suggestions, post your question or
feedback on `Discord`_, or contact us at testnet@concordium.com.
