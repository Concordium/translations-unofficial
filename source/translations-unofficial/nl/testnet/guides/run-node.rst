.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node:

==========
Start een Node
==========

.. contents::
   :local:
   :backlinks: none

In deze handleiding leer je hoe je een node kunt draaien op een computer 
in het Condordium netwerk. Dit betekent dat je blokken en transacties 
ontvangt van andere nodes, maar ook blokken en transacties verspreid 
naar andere nodes in het Concordium netwerk.
Na het volgen van deze handleiding kun je het volgende:

-  Een Concordium node draaien
-  deze waarnemen op het netwerk dashboard
-  de status opvragen van de Concordium blockchain direct vanaf je computer

Je hebt geen account nodig om een node te draaien.

Voordat je begint
================

Voordat je een Concordium node start heb je het volgende nodig

1. Installeer en draai Docker.

   -  Op *Linux*, geef Docker permissies om te draaien als non-root gebruiker.

2. Download en extraheer :ref:`concordium-node-and-client-download` software.

Upgrade een eerdere versie van Open Testnet.
===============================================

Om de huidige Concordium software te upgraden voor Open Testnet 4:

-  Volg de volgende stappen om de meerst recente Concordium software te :ref:`downloaden<downloads>`.

-  Start de ``concordium-node-reset-data`` executable nadat deze is uitgepakt uit het archief.

   -  Voor *Mac* gebruiker: de eerste keer dat je de tool opent, rechtermuis-klik op het
      ``concordium-node-reset-data`` bestand en selecteer **Open**. Een boodschap zal 
	  verschijnen dat de software van een onbekende uitgever is.
      Selecteer **Open** opnieuw.
   -  Voor *Windows* gebruikers: de eerste keer dat je de tool opent,
      dubbelklik het ``concordium-node-reset-data`` bestand. Een boodschap zal
	  verschijnen dat de software van een onbekende uitgever is.
      Selecteer **More info** → **Run anyway**.

-  De tool zal vervolgens het volgende vragen:

      *Do you also want to remove saved keys?*

   Accounts die eerder zijn gebruikt met oudere versies zijn niet langer geldig op
   Open Testnet 3. Daarom, als je accounts hebt opgeslagen uit vorige versies
   raden we aan om **y** te kiezen zodat alle account keys verwijderd worden.

.. _running-a-node:

Een node draaien
==============

Om een client op te starten die gaat deelnemen met het Open Testnet 
zul je deze stappen moeten volgen:

1. Start de ``concordium-node`` executable uit het uitgepakte zip archief.

-  Voor *Mac* gebruikers: de eerste keer dat je de tool opent, rechtermuisklik
   op het ``concordium-node`` bestand en selecteer **Open**. Een boodschap zal verschijnen
   dat de software van een onbetrouwbare uitgever komt. Selecteer **Open**
   opnieuw.
-  Voor *Windows* gebruikers: de eerste keer dat je de tool opent, dubbelklik
   op het ``concordium-node`` bestand. AEen boodschap zal verschijnen
   dat de software van een onbetrouwbare uitgever komt. Selecteer **More info** →
   **Run anyway**.
-  Als je een node gaat *herstarten* dan graag overwegen om de optie
   ``--no-block-state-import`` te gebruiken. Dit zal alleen de resterende updates downloaden
   van de Concordium blockchain toen de node inactief was en dit kan het boot proces
   versnellen.

2. Geef een naam voor je node. Deze naam zal zichtbaar worden op het publieke Dashboard.

3. Als de tool al eerder was gestart dan zul je de vraag krijgen of je 
   de lokale node database wil verwijderen voordat je de node start. Als je **y** kiest
   zal de database verwijderd worden en vervolgens wordt de database opnieuw aangemaakt met de informatie 
   uit de Concordium blockchain dat op je computer was opgeslagen.. **Let wel,
   het verwijderen van de database betekent dat het syncen langer kan duren voordat je node 
   weer in sync is met het Concordium network.**

De tool zal nu het Concordium Client image downloaden en in de docker laden. 
De client zal vervolgens starten en de informatie van de node op 
het vertonen.


De node op het dashboard zien
=================================

Na het starten van de ``concordium-node`` kun je:

-  de node zien op het `Network Dashboard`_
-  :ref:`query<testnet-query-node>` geeft informatie over de blocks, transacties en de accounts

Netwerk dashboard
-----------------

Het zal even duren voordat de client in sync is met laatste status van de
Concordium blockchain. Dit betekent bijvoorbeeld, het downloaden 
van de informatie van alle blokken van de blockchain.

Naast alle andere informatie op het`Network Dashboard`_ kun je ook
zien hoe lang het ongeveer duurt voordat je node weer in sync is met de
chain. Om dit te zien moet je de node's **Length** waarde (totaal 
ontvangen blocks op je node) vergelijken met de **Chain Len** waarde (totaal aantal
blocks aanwezig op de langste chain binnen het netwerk). Deze laatste is 
zichtbaar bovenaan in het dashboard.


Inkomende verbindingen open zetten
============================

Als je node achter een firewall, of achter je thuis router verbonden is,
dan zul je ook alleen maar naar buiten toe met andere nodes kunnen verbinden, 
maar andere nodes zullen geen verbinding kunnen starten richting jou node. 
Dit is geen probleem en de node zal evengoed volledig deelnemen in het 
Concordium network. De node kan evengoed transacties versturen, 
:ref:`als in gesteld<become-a-baker>`, baker zijn en finalizen.

Maar, om je node nog beter te laten participeren in het netwerk kun ook
inkomende verbindingen accepteren. Standaard luistert de ``concordium-node``
op poort ``8888`` voor inkomende verbindingen. Afhankelijk van je netwerk en/of
je platform configuratie zul je een externe poort moeten forwarden naar je 
router op poort ``8888``, de firewall moeten open zetten of zelfs beide. 
De details hoe dit gedaan moet worden zijn afhankelijk van je configuratie.


Poorten configureren
-----------------

De node luistert op vier poorten, welke allemaal geconfigureerd kunnen worden 
door een juist parameter mee te geven als de node gestart wordt.
De poorten die gebruikt worden zijn als volgt:

-  8888, de poort voor peer-to-peer netwerken, deze kan aangepast worden met
   ``--listen-node-port``
-  8082, de poort die wordt gebruikt door middleware, deze kan aangepast 
   worden met ``--listen-middleware-port``
-  10000, de gRPC poort, welke aangepast kan worden met ``--listen-grpc-port``

Voordat bovenstaande poorten worden aangepast moet de docker container eerst 
worden gestopt.(:ref:`stop-a-node`), gereset en vervolgens weer gestart worden. 
Om de container te resetten kan ``concordium-node-reset-data`` gebruikt worden of 
het commando ``docker rm concordium-client`` in een terminal scherm.

We *adviseren* om de firewall zo in te stellen dat deze alleen op poort 8888 
publieke verbindingen accepteert. (de peer-to-peer netwerk poort). 
Iemand die toegang krijgt tot andere poorten van buitenaf kan soms toegang of 
controle krijgen tot je node of accounts die je op je node bewaard.

.. _stop-a-node:

De node stoppen
=================

Om een node te stoppen druk je op **CTRL+c**, wacht dan rustig totdat de node 
netjes is gestopped.

Als je perongeluk het scherm afsluit voordat je de node expliciet hebt gestop 
blijft deze draaien op de achtergrond in Docker. In dat geval gebruik je het commando
``concordium-node-stop`` op dezelfde manier zoals je ook je node hebt gestart met
``concordium-node`` .

Ondersteuning & Feedback
==================

Als je de logging informatie van je node wil bewaren dan gebruikt je de 
``concordium-node-retrieve-logs`` tool. Deze tool bewaard de logs van de 
draaiende node naar een bestand. Additioneel kun je ook toestemming geven 
om informatie op te slaan van alle programma's die op dat moment op je 
systeem draaien.


De logs, systeem informatie, vragen en feedback kun je sturen naar 
testnet@concordium.com. Je kunt ook vragen stellen in `Discord`_, of
bekijk onze :ref:`troubleshoot pagina<troubleshooting-and-known-issues>`

