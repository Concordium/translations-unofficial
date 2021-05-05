
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker:

==================================
Baker werden (Blöcke erstellen)
==================================

.. contents::
   :local:
   :backlinks: none

Dieser Abschnitt erklärt, was ein Baker ist, welche Rolle er im Netzwerk spielt und wie man
einer wird.

Durch das Lesen dieses Abschnitts werden Sie lernen:

-  Was ein Baker ist und welche Konzepte mit ihm verbunden sind.
-  Wie Sie Ihre Node upgraden, um ein Baker zu werden.

Der Prozess, ein Baker zu werden, lässt sich mit den folgenden Schritten zusammenfassen:

#. Besorgen Sie sich ein Konto und einige GTUs.
#. Besorgen Sie sich Baker-Schlüssel.
#. Registrieren Sie die Baker-Schlüssel mit dem Konto.
#. Starten Sie die Node mit den Baker-Schlüsseln.

Nachdem diese Schritte ausgeführt wurden, backt die Baker-Node Blöcke. Wenn ein gebackener Block zur Kette hinzugefügt wird, erhält der Bäcker der Node eine Belohnung (Rewards).

.. note::
 
   In diesem Abschnitt verwenden wir den Namen ``bakerAccount`` als den Namen des
   Kontos, das zur Registrierung und Verwaltung eines Bakers verwendet wird.

Definitionen
===========

Baker
-----

Ein Knoten ist ein *Baker* (oder *backt*), wenn er aktiv am Netzwerk teilnimmt, indem er neue Blöcke erzeugt, die der Blockchain hinzugefügt werden. Ein Baker sammelt,
ordnet und validiert die Transaktionen, die in einem Block enthalten sind, um die
die Integrität der Blockchain aufrechtzuerhalten. Der Baker signiert jeden Block, den er bäckt, damit der Block von den übrigen Teilnehmern des Netzwerks geprüft und ausgeführt werden kann.

Baker-Schlüssel
----------

Jeder Baker hat einen Satz kryptografischer Schlüssel, die *Baker-Schlüssel* genannt werden. Der Knoten verwendet diese Schlüssel, um die Blöcke zu signieren, die er backt. Um Blöcke zu backen, die von einem bestimmten Baker signierte wurden, muss die Node mit seinem Satz von Baker-Schlüsseln geladen sein.

Baker account
-------------

Jedes Konto kann einen Satz von Baker-Schlüsseln verwenden, um einen Baker zu registrieren.

Wann immer ein Baker einen gültigen Block backt, der in die Blockchain aufgenommen wird, wird nach einiger
Zeit eine Belohnung an das zugehörige Konto ausgezahlt.

Stake and Lotterie
-----------------

.. todo::

   - Link to release schedule.
   
Das Konto kann einen Teil seines GTU-Guthabens in den *Bakereinsatz* setzen und
später manuell den gesamten oder einen Teil des eingesetzten Betrags freigeben. Der eingesetzte Betrag
kann nicht bewegt oder übertragen werden, bis er vom Baker freigegeben wird.

.. note::

   Wenn ein Konto einen Betrag besitzt, der mit einem Freigabeplan übertragen wurde,
   kann der Betrag eingesetzt werden, auch wenn er noch nicht freigegeben wurde.
   
Um für das Backen eines Blocks ausgewählt zu werden, muss der Baker an einer
*Lotterie* teilnehmen, bei der die Wahrscheinlichkeit, ein Gewinnticket zu erhalten, ungefähr
proportional zum gesetzten Betrag ist.

Der gleiche Einsatz wird verwendet, um zu berechnen, ob ein Baker in das Finalisierungskomitee aufgenommen wird oder nicht.

.. _epochs-and-slots:

Epochen and slots
----------------

In der Concordium-Blockchain ist die Zeit in *Slots* eingeteilt. Slots haben eine Zeitdauer, die im Genesis-Block festgelegt ist. Auf jedem gegebenen Zweig kann jeder Slot höchstens einen Block enthalten, aber mehrere Blöcke auf verschiedenen Zweigen können im
gleichen Slot produziert werden.

.. todo::

   Let's add a picture.

Wenn wir die Belohnungen und andere baking-bezogene Konzepte betrachten, verwenden wir das
Konzept einer *Epoche* als eine Zeiteinheit, die einen Zeitraum definiert, in dem die Menge
von aktuellen Bakern und Einsätzen festgelegt ist. Epochen haben eine Zeitdauer, die im
Genesis-Block festgelegt ist. Im Testnetz haben Epochen eine Dauer von **1 Stunde**.

Start baking
============

Konten verwalten
-----------------

Dieser Abschnitt enthält eine kurze Zusammenfassung der relevanten Schritte zum Importieren eines
Kontos. Eine vollständige Beschreibung finden Sie unter :ref:`managing_accounts`.

Konten werden mit der App :ref:`concordium_id` erstellt. Sobald ein Konto
erfolgreich erstellt wurde, navigieren Sie zur Registerkarte **More** und wählen **Export**. Somit können Sie eine JSON-Datei abrufen, die die Kontoinformationen enthält.

Um ein Konto in die Toolchain zu importieren, führen Sie folgenden Befehl aus:

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount
   
``concordium-client`` wird nach einem Passwort fragen, um die exportierte Datei zu entschlüsseln und
alle Konten zu importieren. Das gleiche Passwort wird für die Verschlüsselung der
Transaktionssignierschlüssel und den verschlüsselten Übertragungsschlüssel verwendet.

Erstellen von Schlüsseln für einen Baker und dessen Registrierung
--------------------------------------------

.. note::

   Für diesen Vorgang muss das Konto einige GTU besitzen, also stellen Sie sicher, dass Sie die
   100 GTU für das Konto in der mobilen App haben.

Jedes Konto hat eine eindeutige Baker-ID, die bei der Registrierung des Bakers verwendet wird. Diese
ID muss vom Netzwerk bereitgestellt werden und kann derzeit nicht vorberechnet werden. Diese
ID muss in der Baker-Schlüsseldatei an den Knoten übergeben werden, damit dieser die
baker keys zum Erstellen von Blöcken verwenden kann. Der ``concordium-client`` füllt automatisch
dieses Feld, wenn er die folgenden Operationen durchführt.

Um einen neuen Satz von Schlüsseln zu erzeugen, führen Sie aus:

.. code-block:: console

   $concordium-client baker generate-keys <keys-file>.json

wo Sie einen beliebigen Namen für die Schlüsseldatei wählen können. Um
die Schlüssel im Netzwerk zu registrieren, müssen Sie :ref:`running a node <running-a-node>` sein
und eine ``baker add`` Transaktion an das Netzwerk senden:

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

Folgende Parameter müssen ersetzt werden:

- ``<amountToStake>`` durch den GTU-Betrag für den ersten Einsatz des Bakers
- ``<concordium-data-dir>`` mit dem folgenden Datenverzeichnis:

  * on Linux and MacOS: ``~/.local/share/concordium``
  * on Windows: ``%LOCALAPPDATA%\\concordium``.

(Der Name der Ausgabedatei sollte ``baker-credentials.json`` bleiben).

Geben Sie ein ``--no-restake`` Flag an, um zu verhindern, dass die
Belohnungen nicht automatisch zu den Einsätzen auf dem Baker addiert werden. Dieses Verhalten ist beschrieben in dem
Abschnitt `Restaking the earnings`_.

Um den Knoten mit diesen Baker-Schlüsseln zu starten und mit der Produktion von Blöcken zu beginnen, müssen Sie zuerst die aktuell laufende Node herunterfahren (entweder durch Drücken von
``Strg + C`` auf dem Terminal, auf dem die Node läuft, oder mit dem Befehl
``concordium-node-stop`` (ausführbare Datei).

Nachdem Sie die Datei in das entsprechende Verzeichnis gelegt haben (was bereits im
vorherigen Befehl bei der Angabe der Ausgabedatei geschehen ist), starten Sie die Node erneut mit
``concordium-node``. Die Node wird automatisch mit dem Backen beginnen, wenn der Baker
in die Baker für die aktuelle Epoche aufgenommen wird.

Diese Änderung wird sofort ausgeführt und wird beim Beenden der Epoche nach derjenigen wirksam, in der
die Transaktion zum Hinzufügen des Bakers in einen Block aufgenommen wurde.

.. table:: Timeline: Hinzufügen eines Bakers

   +-------------------------------------------------+-----------------------------------------------------+-----------------+
   |                                                 | Wenn die Transaktion in einen Block eingefügt wurde | Nach 2 Epochen  |
   +=================================================+=====================================================+=================+
   | Änderung ist durch Abfrage des Knotens sichtbar |  ✓                                                  |                 |
   +-------------------------------------------------+-----------------------------------------------------+-----------------+
   | Bäcker wird in das baking committee aufgenommen |                                                     | ✓               |
   +-------------------------------------------------+-----------------------------------------------------+-----------------+

.. note::

   Wenn die Transaktion zum Hinzufügen des Bakers in einem Block während der Epoche `E` enthalten war, wird der Baker als Teil des Backkomitees betrachtet, wenn die Epoche
   `E+2` beginnt.

Verwaltung the Bakers
=====================

Überprüfen des Status des Bakers und seiner Lotteriekraft
------------------------------------------------------

Um zu sehen, ob die Node gerade backt, können Sie verschiedene Quellen prüfen, die
unterschiedliche Genauigkeitsgrade bei den angezeigten Informationen bieten..

- Im `Netzwerk-Dashboard <http://dashboard.testnet.concordium.com>`_ zeigt Ihre
  Node seine Baker-ID in der Spalte ``Baker`` an.
- Mit dem ``concordium-client`` können Sie die Liste der aktuellen Baker überprüfen
  und den relativen Einsatz, den sie halten, d.h. ihre Lotteriekraft.  Die
  Lotteriemacht bestimmt, wie wahrscheinlich es ist, dass ein bestimmter Baker die Lotterie gewinnt und einen Block backt.

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

- Mit dem Befehl ``concordium-client`` können Sie überprüfen, ob das Konto
  einen Baker registriert hat und den aktuellen Betrag, der von diesem Baker eingesetzt wird.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- Wenn der eingesetzte Betrag groß genug ist und eine Node läuft, auf dem die Baker
  Schlüssel geladen sind, sollte dieser Baker schließlich Blöcke produzieren und Sie können
  in Ihrer mobilen Geldbörse sehen, dass Back-Belohnungen auf dem Konto eingegangen sind,
  wie in diesem Bild zu sehen:

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Aktualisieren des staked amount
--------------------------

Um den Bakereinsatz zu aktualisieren, führen Sie folgenden Befehl aus:

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

Das Ändern des Einsatzes ändert die Wahrscheinlichkeit, dass ein Baker gewählt wird
Blöcke zu backen.

Wenn ein Baker **zum ersten Mal einen Stake hinzufügt oder seinen Stake erhöht**, wird diese
Änderung auf der Blockchain ausgeführt und wird sichtbar, sobald die Transaktion
in einem Block enthalten ist (zu sehen über ``concordium-client account show
bakerAccount``) und wird 2 Epochen danach wirksam.

.. table:: Timeline: Erhöhung des Stakes

   +-----------------------------------------------+------------------------------------------------+----------------+
   |                                               | Wenn Transaktion in einem Block enthalten ist  | Nach 2 Epochen |
   +===============================================+================================================+================+
   | Änderung ist durch Abfrage der Node sichtbar  | ✓                                              |                |
   +-----------------------------------------------+------------------------------------------------+----------------+
   | Baker verwendet den neuen Stake               |                                                | ✓              |
   +-----------------------------------------------+------------------------------------------------+----------------+

Wenn ein Baker den **Stake** verringert, benötigt die Änderung *2 +
bakerCooldownEpochs* Epochen, um wirksam zu werden. Die Änderung wird auf der Blockchain sichtbar, sobald die Transaktion in einem Block enthalten ist, sie kann durch
``concordium-client account show bakerAccount``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: Timeline: Verringerung des Stakes

   +-----------------------------------------------+-----------------------------------------------+----------------------------------------+
   |                                               | Wenn Transaktion in einem Block enthalten ist | Nach *2 + bakerCooldownEpochs* Epochen |
   +===============================================+===============================================+========================================+
   | Änderung ist durch Abfrage der Node sichtbar  | ✓                                             |                                        |
   +-----------------------------------------------+-----------------------------------------------+----------------------------------------+
   | Baker erwendet den neuen Stake                |                                               | ✓                                      |
   +-----------------------------------------------+-----------------------------------------------+----------------------------------------+
   | Stake kann wieder verringert werden oder      | ✗                                             | ✓                                      |
   | Baker kann entfernt werden                    |                                               |                                        |
   +-----------------------------------------------+-----------------------------------------------+----------------------------------------+

.. note::

   Im Testnetz ist ``bakerCooldownEpochs`` zunächst auf 168 Epochen eingestellt. Dieser
   Wert kann wie folgt überprüft werden:

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   Wie im Abschnitt `Definitionen`_ erwähnt, ist der eingesetzte Stake *gesperrt*,
   d.h. er kann nicht übertragen oder zur Zahlung verwendet werden. Sie sollten dies berücksichtigen
   und einen Stake einsetzen, der kurzfristig nicht benötigt wird.
   Insbesondere zum Abmelden eines Bakers oder zum Ändern des Stakes
   müssen Sie einige nicht eingesetzte GTU besitzen, um die Transaktionskosten zu
   Kosten zu decken.
   
Restaking der Erträge
----------------------

Wenn Sie als Baker am Netzwerk teilnehmen und Blöcke backen, erhält das Konto
für jeden gebackenen Block Rewards. Diese Rewards werden automatisch zu dem eingesetzten Betrag addiert.

Sie können dieses Verhalten ändern und erhalten stattdessen die Rewards in
den Kontostand, ohne dass sie automatisch wieder eingesetzt werden. Diese Option kann
über ``concordium-client`` geändert werden:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Änderungen am restake-Flag werden sofort wirksam; allerdings wirken sich die Änderungen
erst in der übernächsten Epoche auf die Back- und Finalisierungsleistung aus. Der aktuelle
Wert des Schalters kann in den Kontoinformationen eingesehen werden, die abgefragt werden können mittels ``concordium-client``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: Updaten des restake

   +--------------------------------------------------------+-----------------------------------------------+-------------------------------+
   |                                                        | Wenn Transaktion in einem Block enthalten ist | 2 Epochen nach Belohnung      |
   +========================================================+===============================================+===============================+
   | Änderung ist durch Abfrage des Knotens sichtbar        | ✓                                             |                               |
   +--------------------------------------------------------+-----------------------------------------------+-------------------------------+
   | Das Ergebnis wird [nicht] automatisch neu gestaked     | ✓                                             |                               |
   +--------------------------------------------------------+-----------------------------------------------+-------------------------------+
   | Wenn restaking aktiviert ist, wird dies                |                                               | ✓                             |
   | die Lotterieleistung beeinflussen                      |                                               |                               |
   +--------------------------------------------------------+-----------------------------------------------+-------------------------------+

Wenn der Baker registriert ist, wird er den Gewinn automatisch wieder einsetzen, aber wie
oben erwähnt, kann dies geändert werden, indem man das ``--no-restake``-Flag an den
dem Befehl ``baker add`` hinzufügt, wie hier gezeigt:

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Finalisierung
------------

Die Finalisierung ist der Abstimmungsprozess, der von der Node im *Finalisierungs
Komitee* durchgeführt wird, der einen Block *finalisiert*, wenn eine ausreichend große Anzahl von Mitgliedern des Komitees den Block erhalten haben und dem Ergebnis zustimmen. Neuere Blöcke müssen den finalisierten Block als Vorgänger haben, um die Integrität der
Blockchain zu gewährleisten. Weitere Informationen über diesen Prozess finden Sie in dem
Abschnitt :ref:`Finalisierung<Glossar-Finalisierung>`.

Das Finalisierungskomitee wird von den Bakern gebildet, die einen bestimmten
Betrag haben. Das bedeutet konkret, dass wenn Sie am Finalisierungskomitee teilnehmen wollen, den Einsatzbetrag um den besagten Schwellenwert erreichen müssen. Im Testnetz beträgt der Einsatz, der für die Teilnahme am Finalisierungskomitee notwendig ist, **0,1 % des Gesamtbetrags der vorhandenen GTU**.

Die Teilnahme am Finalisierungskomitee erzeugt Belohnungen für jeden Block, der
finalisiert wird. Die Rewards werden einige Zeit nach dem Abschluss des Blocks auf das Bakerkonto ausgezahlt.

Entfernen eines Bakers
================

Das kontrollierende Konto kann sich entscheiden, seinen Baker von der Blockchain abzumelden. Um dies zu tunmüssen Sie dazu den ``concordium-client`` ausführen:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

Dies entfernt den Baker aus der Baker-Liste, so dass er frei übertragen oder verschoben werden kann.

Wenn Sie den Baker entfernen, hat die Änderung denselben Zeitrahmen wie das Verringern
des Einsatzes. Die Änderung benötigt *2 + bakerCooldownEpochs* Epochen, um wirksam zu werden.

Die Änderung wird auf der Blockchain sichtbar, sobald die Transaktion in einen Block aufgenommen wird und Sie können überprüfen, wann diese Änderung in Kraft tritt, indem Sie die Kontoinformationen mit ``concordium-client`` wie gewohnt abfragen:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: Entfernen eines Bakers

   +--------------------------------------------------+-----------------------------------------------+----------------------------------------+
   |                                                  | Wenn Transaktion in einem Block enthalten ist | Nach *2 + bakerCooldownEpochs* Epochen |
   +==================================================+===============================================+========================================+
   | Änderung ist durch Abfrage der Node sichtbar     | ✓                                             |                                        |
   +--------------------------------------------------+-----------------------------------------------+----------------------------------------+
   | Bäcker wird aus dem Ausschuss entfernt           |                                               | ✓                                      |
   +--------------------------------------------------+-----------------------------------------------+----------------------------------------+

.. warning::

   Das Verringern des Stakes und das Entfernen des Bakers können nicht
   gleichzeitig erfolgen. Während der Abkühlphase, die durch das Verringern des Stakes
   entsteht, kann der Baker nicht entfernt werden und andersherum.

Support & Feedback
==================

Wenn Sie auf Probleme stoßen oder Vorschläge haben, posten Sie Ihre Frage oder
Feedback auf `Discord`_, oder kontaktieren Sie uns unter testnet@concordium.com.
