
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker:

==================================
Wordt een baker (creeër blocks)
==================================

.. contents::
   :local:
   :backlinks: none

In deze sectie wordt uitgelegd wat een baker is, zijn rol in het netwerk en hoe je een baker kunt worden.

Door onderstaande sectie te lezen zul je het volgende leren:

-  Wat is een baker en de concepten die er bij horen
-  Hoe kun je de node upgraden en een baker worden.

Het proces om een baker te worden kan als volgt samengevat worden:

#. Verkrijg een account en wat GTU's
#. Verkrijg een set baker sleutels
#. Registreer de baker sleutels met het account
#. Start de node met de baker sleutels

Nadat je deze stappen hebt voltooid zal de baker beginnen met het 'bakken' van blocks. 
Als een baker een block aan de chain toevoegd krijgt de baker hier een beloning voor.


.. note::

   In deze sectie gebruiken we de naam ``bakerAccount`` als de naam voor het account dat we zullen gebruiken
   voor het registreren en beheren van een baker.
   
Definitions
===========

Baker
-----

Een node is een *baker* (of zoals in het nederlands gezegd 'aan het bakken') wanneer
het actief nieuwe blokken genereerd die worden toegevoegd aan de blockchain. 
Een baker verzameld orders en valideert vervolgens de transacties die aan een block 
worden toegevoegd zodat de integriteit van de blockchain gewaarborgd blijft.
Een baker zal iedere block ondertekenen die wordt 'gebakken'. Hierdoor kunnen 
anderen in het netwerk dit controleren en uitvoeren.

Baker keys
----------

Een baker heeft een set cryptografische sleutels genoemd *baker keys*. De node gebruikt
deze sleutels om blokken te ondertekenen die worden 'gebakken'. Voordat er blokken ondertekent 
kunnen worden van specifieke baker moet de node voorzien zijn van een set 'baker keys'.

Baker account
-------------

Elk account kan een set 'baker keys' gebruiken om een baker te registreren.

Elke keer als een baker een geldige blok ondertekent die vervolgens wordt opgenomen in 
de chain krijgt het account ndat hiermee wordt geassocieerd na een tijdje een beloning.


Stake and lotterij
-----------------

Een account kan een gedeelte van zijn GTU balans 'staken' (vast zetten) op de 'baker stake' en 
kan eventueel later alles of een gedeelte weer manueel vrijgeven (unstaken). Een staked bedrag kan dus
niet worden verplaatst of verzonden worden totdat de baker deze heeft vrijgegeven.

.. note::

   Als een account een bedrag heeft dat is verstuurd met een geplande vrijgave (release schedule)
   dan kan dit bedrag evengoed worden gebruikt om te staken.

Voordat een baker gekozen wordt om een blok te bakken moet de baker deelnemen in een loterij waarbij de kans
om gekozen te worden ongeveer proportioneel gelijk is aan het 'staked' bedrag.

Hetzelfde 'staked' bedrag wordt gebruikt om te berekenen of de baker mee mag doen in set van bakers (finalization
committee) of niet. See Finalization_.

.. _epochs-and-slots:

Epochs and slots
----------------

In de Concordium blockchain wordt tijd verdeeld in blokken genaamd *slots*. Slots hebben een vastgestelde tijdsduur 
die is vastgelegd in de oorsprong block genaamd 'Genesis block'. Op elke vertakking kan elk slot maar 1 block bevatten, 
maar er kunnen meerdere blocks op verschillende vertakkingen geproduceerd worden binnen hetzelfde (tijds) slot.

Als we nadenken over de beloningen en andere baker gerelateerde zaken gebruiken we het concept genaamd 'epoch' 
als eenheid van tijd die een periode representeerd waarin een set bakers en stakers vast staat.
De epochs hebben een vaste tijd lengte die is vastgesteld in de Genesis block. In het testnet is deze lengte op **1 uur** gezet.

Start baking
============

Beheren van accounts
-----------------

Deze sectie laat in een korte overzicht zien wat de relevante stappen zijn die genomen moeten worden om een account te importeren.
Voor een meer complete omschrijving zie :ref:`managing_accounts`.

Accounts worden aangemaakt met de :ref:`concordium_id` app. Als een account 
succesvol is aangemaakt navigeer je naar de **More** tab en selecteer je **Export**
Deze export geeft een JSON bestand met daarin alle account informatie.

Om een account te importeren op de toolchain moeten we het volgende draaien:

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount

``concordium-client`` zal vragen om een wachtwoord om het export bestand te 
ontsleutelen om vervolgens alle accounts te importeren. Hetzelfde wachtwoord
zal gebruikt worden om de transactie 'signing keys' en 'transfer keys' te encrypten

Keys aanmaken voor een baker en deze registreren
--------------------------------------------

.. note::

   Voor dit proces heeft het account wat GTU nodig, dus vergeet niet om in de mobiele app
   de 100GTU aan te vragen.
   
Elk account heeft een unieke baker ID dat wordt gebruikt om een baker te registeren.
Dit ID zal door het netwerk geleverd moeten worden en kan niet van te voren aangeleverd
worden. Dit ID, wat in de baker keys zit, wordt aan de node gegeven zo dat deze blokken
ermee kan genereren. De ``concordium-client`` zal deze automatisch gebruiken bij de volgende
commando's.

Om een nieuwe set keys te genereren:

.. code-block:: console

   $concordium-client baker generate-keys <keys-file>.json

Je kunt zelf een naam kiezen voor het keys-file bestand. Om de keys te registreren in het 
netwerk moet je :ref:`een node draaien <running-a-node>` en een ``baker add`` transactie 
sturen naar het netwerk.

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

vervang

- ``<amountToStake>`` met het GTU bedrag voor de initiele baker stake
- ``<concordium-data-dir>`` met de volgende data directory:

  * on Linux and MacOS: ``~/.local/share/concordium``
  * on Windows: ``%LOCALAPPDATA%\\concordium``.

(Het verkregen bestand moet voor nu dezefde naam blijven behouden als ``baker-credentials.json``).

Voorzie het commando met ``--no-restake`` om te voorkomen dat de verkregen vergoedingen 
automatisch worden toegevoegd aan de baker stake.  Dit gedrag is omschreven in de sectie 
`Restaking the earnings`_.

Om de node te starten met de nieuwe baker keys om vervolgens blocks te genereren moeten we eerst
de huidige node stoppen. (Dit kan door ``Ctrl + C`` op de terminal te gebruiken of het volgende 
commando te gebruiken``concordium-node-stop`` ).

Nadat het bestand in de juiste directory is geplaatst (is gebeurd door het vorige commando 
waar de bestandsnaam is gespecificeerd), start je de node opnieuw door het commando 
``concordium-node``.
Vervolgens zal de node automatisch beginnen met het 'bakken' als de baker wordt opgenomen bij 
de rest van de bakers in de huidige epoch.
Deze wijziging zal per direct worden uitgevoerd maar zal pas effectief zijn na de epoch die komt nadat
de baker is opgenomen in een block, is afgelopen.

.. table:: Tijdlijn: toevoegen van een baker   

   +---------------------------------------------+-----------------------------------------------+-----------------+
   |                                             | Als een transactie in een block is opgenomen. | Na 2 epochs     |
   +=============================================+===============================================+=================+
   | De wijziging is op te vragen op de node     |  ✓                                            |                 |
   +---------------------------------------------+-----------------------------------------------+-----------------+
   | De baker opgenomen in de baking committee   |                                               | ✓               |
   +---------------------------------------------+-----------------------------------------------+-----------------+

.. note::

   Als de transactie voor het toevoegen van de baker is opgenomen in een block tijdens epoch `E`, dan
   zal de baker lid worden van de bakers set, ook wel baking committee genoemd, ndat de epoch
   `E+2` begint.

Baker beheren
==================

Controleer de status van de baker en de loterij power
------------------------------------------------------

Je hebt meerdere mogelijkheden om te zien of de node ook aan het 'baken'. Iedere methode laat deze informatie 
op een iets andere getailleerde manier zien.

- Op het `network dashboard <http://dashboard.testnet.concordium.com>`_, kun
  je de node vinden met een baker ID in de ``Baker`` kolom.
- Als je de ``concordium-client`` gebruikt kun je in een lijst zien met alle actieve bakers
  en de baker stake die erbij hoort. (loterij power)
  De loterij power zal de bepalende factor zijn hoe groot de kans is dat een baker de loterij 
  kan winnen en een block kan 'bakken'
  

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

- Als je de ``concordium-client`` kun je ook controleren of het account een baker heeft geregistreerd 
  en werk bedrag dat stakes is bij die betreffende baker.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- Als het stake bedrag groot genoeg is en er draait een node met de betreffende baker keys
  dan zal uiteindelijk de baker blokken gaan produceren en  zul je die beloningen ook terug zien
  op je telefoon in de wallet van het betreffende account zoals je kunt zien in onderstaande 
  afbeelding:

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Update een 'staked' bedrag
--------------------------

Om een baker stake te updaten kun je het volgende starten

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

Door het stake bedrag aan te passen verander je ook de kansen om als baker gekozen te worden en blokken te bakken.

Als een baker **voor de eerste keer zijn stake gaat toevoegen of vergroten** dan wordt deze wijziging op de chain
geschreven en pas zichtbaar als deze in een block wordt opgenomen. (Dit kun je zien door ``concordium-client account show
bakerAccount`` en vervolgens zal dit na 2 epochs effectief zijn.

.. table:: Tijdlijn: vergroot de stake

  +---------------------------------------------+------------------------------------------------+-----------------+
   |                                             | Als een transactie in een block is opgenomen. | Na 2 epochs     |
   +=============================================+===============================================+=================+
   | De wijziging is op te vragen op de node     |  ✓                                            |                 |
   +---------------------------------------------+-----------------------------------------------+-----------------+
   | De baker gebruikt de nieuwe stake           |                                               | ✓               |
   +---------------------------------------------+-----------------------------------------------+-----------------+
   
Als een baker **zijn stake verkleind** dan zal deze wijziging *2 + bakerCooldownEpochs* 
epochs nodig hebben voordat het effectief is. De wijziging wordt zichtbaar op de blockchain
zodra de transactie is opgenomen in een block. Dit kan opgevraagd worden via
``concordium-client account show bakerAccount``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: Tijdlijn: de stake verkleinen

   +----------------------------------------+-----------------------------------------------+----------------------------------------+
   |                                        | Als een transactie in een block is opgenomen. | Na *2 + bakerCooldownEpochs* epochs    |
   +========================================+=========================================+==============================================+
   | De wijziging is op te vragen op de node| ✓                                             |                                        |
   +----------------------------------------+-----------------------------------------------+----------------------------------------+
   | De baker gebruikt de nieuwe stake      |                                               | ✓                                      |
   +----------------------------------------+-----------------------------------------------+----------------------------------------+
   | De stake kan weer verkleind worden of  | ✗                                             | ✓                                      |
   | de baker van verwijderd worden         |                                               |                                        |
   +----------------------------------------+-----------------------------------------------+----------------------------------------+

.. note::

   Voor het testnet zijn de ``bakerCooldownEpochs`` initieel op 168 epochs gezet. Deze 
   waarde kan gecontroleerd worden via:

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   Zoals aangegeven in de `Definitions`_ sectie is een 'staked' bedrag *locked*,
   dus het kan bijvoorbeeld niet worden verplaatst of gebruikt worden voor te betalen.
   Je zult dus, voordat je gaat staken, hier goed over na moeten denken omdat je dit bedrag 
   niet op korte termijn kunt gebruiken.
   Zeker in het geval wanneer je een baker wil deregistreren of een staked bedrag gaat wijzigen
   zul je altijd een beetje non-staked GTU nodig hebben om de transactie kosten te betalen.
   
Herstaken van de beloningen
----------------------

Als je als baker gaat fungeren in het netwerk en blocks gaat 'bakken' dan zul je ook beloond 
worden voor elk 'baked block'. Deze beloningen zullen standaard automatisch worden toegevoegd 
aan de stake.

Je kunt dit aanpassen en in plaats van de beloningen gelijk te laten staken kun je de 
belongingen automatisch laten toevoegen aan het account zonder dat ze gestaked worden.
Het commando om dit te wijzigen gaat via ``concordium-client``:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Zodra je bovenstaande uitvoerd zal dit per direct effectief zijn, maar de wijzigingen 
die betrekking hebben op het 'bakken' en de uiteindelijke loterij power zijn pas 
effectief in de epoch na de volgende epoch.
De huidige waarde van de wijziging kun je in de account informatie terug vinden en 
is op te vragen met het commando ``concordium-client``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Tijdlijn: de restake updaten

   +-----------------------------------------------+-----------------------------------------------+-------------------------------------------+
   |                                               | Als een transactie in een block is opgenomen. | 2 epochs na het ontvangen van de beloning |
   +===============================================+===============================================+===========================================+
   | De wijziging is op te vragen op de node       | ✓                                             |                                           |
   +-----------------------------------------------+-----------------------------------------------+-------------------------------------------+
   | Beloningen die wel (of niet) automatisch      |                                               |                                           |
   | restaked worden                               | ✓                                             |                                           |
   +-----------------------------------------------+------------------------------------------ ----+-------------------------------------------+
   | Als er automatisch gerestaked worden, dan     |                                               | ✓                                         |
   | heeft dit effect op de loterij power          |                                               |                                           |
   +-----------------------------------------------+-----------------------------------------------+-------------------------------------------+

Als een baker is geregistreerd zal deze automatisch de beloningen restaken maar zoals 
aangegeven hierboven kan dit gewijzigd worden met de parameter ``--no-restake`` achter
het commando ``baker add`` zoals hieronder is weergegeven:

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Finalization
------------

Finalization is de engelse benaming voor het stem proces dat door de nodes die lid zijn van de *finalization
committee* wordt uitgevoerd. Nodes *finalizen* een block als er voldoende leden uit dit 
commitee de blok hebben ontvangen en overeen zijn gekomen hoe dit block eruit moet komen te zien.
Nieuwere blokken moeten altijd een vorige 'finalized block' als erfgenaam hebben zodat de integriteit 
van de blockchain gewaarborgt blijft.
Als je meer informatie over dit proces wil lezen kun je dit in het :ref:`finalization<glossary-finalization>` 
sectie vinden.

Het 'finalization committee' wordt gevormd door bakers met een bepaald stake bedrag.
In andere woorden, dit betekent dus dat je pas kunt deelnemen in het 'finalization committee'
als je ook voldoende hebt gestaked. Je zult wellicht het stake bedrag moeten aanpassen om
die grens te bereiken. In het testnet is het benodigde stake bedrag om in het 
'finalization committee' te komen **0.1% van de totale bestaande hoeveelheid GTU**.

Als je deel neemt in dit 'finalization committee' zul je beloningen krijgen voor elk 
block dat je finalized. De beloningen worden betaald aan de baker nadat een block 
is finalized.

Verwijder een baker
================

Het account dat de controle heeft over de baker kan er voor kiezen een baker te de-registreren op de blockchain.
Om dit te doen maak je gebruik van de ``concordium-client``:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

Dit commando verwijderd de baker uit de baker lijst en zal het stake bedrag vrijgeven zodat dit weer verplaats kan worden.

Als een baker wordt verwijderd zal deze wijziging dezelfde tijdlijn volgen als het verminderen van een stake.
In andere woorden, de wijziging heeft *2 + bakerCooldownEpochs* epochs nodig voordat het effectief is.
De wijziging wordt zichtbaar op de blockchain zodra de transactie is opgenomen in een block en dit kun je zoals 
gewoonlijk controleren door de account informatie op te vragen met ``concordium-client`` 

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Tijdlijn: verwijderen van een baker

   +--------------------------------------------+-----------------------------------------------+----------------------------------------+
   |                                            | Als een transactie in een block is opgenomen. | Na *2 + bakerCooldownEpochs* epochs    |
   +============================================+=========================================+==============================================+
   | De wijziging is op te vragen op de node    | ✓                                             |                                        |
   +--------------------------------------------+-----------------------------------------------+----------------------------------------+
   | De baker opgenomen in de baking committee  |                                               | ✓                                      |
   +--------------------------------------------+-----------------------------------------------+----------------------------------------+

.. warning::

   Verminderen van een stake en het verwijderen van een baker kan niet tegelijkertijd uitgevoerd worden.
   Tijdens de cooldown periode na het verminderen van een stake kan een baker niet verwijderd worden en visa versa.
   
Hulp & Feedback
==================

Als je tegen problemen aanloopt of suggesties hebt kun je je vragen 
of feedback posten in `Discord`_, of contact opnemen via testnet@concordium.com.
