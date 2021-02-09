.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node:

====================
Betreiben einer Node
====================

.. contents::
   :local:
   :backlinks: none

In dieser Anleitung lernen Sie, wie Sie eine Node auf Ihrem Computer betreiben, die
am Concordium-Netzwerk teilnimmt. Das bedeutet, dass Sie
Blöcke und Transaktionen von anderen Nodes empfangen, sowie
Informationen über Blöcke und Transaktionen an die Nodes im Concordium
Netzwerk weiterleiten. Nachdem Sie diese Anleitung befolgt haben, werden Sie in der Lage sein

- eine Concordium-Node zu betreiben
- sie auf dem Netzwerk-Dashboard zu beobachten und
- den Zustand der Concordium-Blockchain direkt von Ihrer Maschine aus abzufragen.

Sie benötigen kein Konto, um eine Node zu betreiben.

Bevor Sie beginnen
==================

Bevor Sie eine Concordium-Node starten, müssen Sie

1. Installieren und starten Sie Docker.

   -  Lassen Sie unter *Linux* zu, dass Docker als Nicht-Root-Benutzer ausgeführt wird.

2. Laden Sie die Software :ref:`concordium-node-and-client-download` herunter und extrahieren Sie sie.

Aktualisieren Sie von einer früheren Version von Open Testnet
=============================================================

So aktualisieren Sie auf die aktuelle Concordium-Software für Open Testnet 4:

-  Folgen Sie den obigen Schritten, um :ref:`download<downloads>` die aktuellste Concordium
   Software zu erhalten.

-  Starten Sie die ausführbare Datei ``concordium-node-reset-data`` aus dem entpackten
   Archiv.

   -  Für *Mac*-Benutzer: Wenn Sie das Programm zum ersten Mal öffnen, klicken Sie mit der rechten Maustaste auf die Datei
      Datei ``concordium-node-reset-data`` und wählen Sie **Öffnen**. Eine Meldung
      erscheint die Meldung, dass die Software von einem nicht identifizierten Entwickler stammt.
      Wählen Sie erneut **Öffnen**.
   -  Für *Windows*-Benutzer: Wenn Sie das Programm zum ersten Mal öffnen,
      doppelklicken Sie auf die Datei ``concordium-node-reset-data``. Eine Meldung
      erscheint, dass die Software von einem nicht identifizierten Entwickler stammt.
      Wählen Sie **Mehr Info** → **Trotzdem ausführen**.

-  Das Tool wird Sie fragen:

      *Do you also want to remove saved keys?* / *Möchten Sie auch gespeicherte Schlüssel entfernen?*
 
   Konten, die für frühere Versionen erstellt wurden (Testnet 3), sind nicht mehr gültig.
   Wenn Sie also Konten aus früheren Versionen gespeichert haben,
   empfehlen wir die Eingabe von **y**, wodurch alle Kontenschlüssel gelöscht werden.
   Schlüssel.

.. _running-a-node:

Ausführen einer Node
====================

Um einen Client zu starten, der dem Open Testnet beitritt, führen Sie folgende
Schritte:

1. Öffnen Sie die ausführbare Datei ``concordium-node`` aus dem entpackten Archiv.

-  Für *Mac*-Benutzer: Wenn Sie das Programm zum ersten Mal öffnen, klicken Sie mit der rechten Maustaste auf die Datei
   ``concordium-node``-Binary und wählen Sie **Öffnen**. Es wird eine Meldung erscheinen
   dass die Software von einem nicht identifizierten Entwickler stammt. Wählen Sie **Öffnen**
   erneut.
-  Für *Windows*-Benutzer: Wenn Sie das Programm zum ersten Mal öffnen, doppelklicken Sie auf
   das ``concordium-node``-Binary. Es erscheint eine Meldung, dass die
   Software von einem nicht identifizierten Entwickler stammt. Wählen Sie **Mehr Info** →
   **Trotzdem ausführen**.
-  Beim *Neustart* einer Node sollten Sie die Option
   "no-block-state-import" verwenden. Dies lädt nur die
   Aktualisierungen der Concordium-Blockchain herunter, die aufgetreten sind, während die Node inaktiv war und kann den Startvorgang beschleunigen.

2. Geben Sie einen Namen für Ihre Node ein. Dieser Name wird auf dem öffentlichen
   Dashboard angezeigt.

3. Wenn das Tool bereits gestartet wurde, werden Sie gefragt, ob Sie
   die lokale Nodedatenbank vor dem Start löschen möchten. Durch Drücken von **y** wird
   gelöscht und anschließend ebenfalls die Informationen über den Zustand der
   Concordium-Blockchain, die auf Ihrem Computer gespeichert wurden. **Beachten Sie, dass
   das Löschen der lokalen Node-Datenbank bedeutet, dass es länger dauert, bis Ihre
   Node gestartet ist.**

Das Tool lädt nun das Concordium-Client-Image herunter und lädt es in
Docker. Der Client wird gestartet und beginnt mit der Ausgabe von Protokollierungsinformationen
über den Betrieb der Node.

Sehen Sie Ihre Node auf dem Dashboard
=================================

Nachdem Sie ``concordium-node`` ausgeführt haben, können Sie

- Ihre Node auf dem ``Netzwerk-Dashboard`` sehen
- :ref:`query<testnet-query-node>` Informationen über Blöcke, Transaktionen und Konten

Netzwerk-Dashboard
-----------------

Der Client wird eine Weile brauchen, um sich mit dem Zustand der
Concordium-Blockchain zu synchronisieren. Dies beinhaltet zum Beispiel das Herunterladen von
Informationen über alle Blöcke in der Blockchain.

Neben anderen Informationen können Sie auf dem `Network Dashboard`_ eine
Vorstellung davon bekommen, wie lange Ihre Node brauchen wird, um mit der
Blockchain zu synchronisieren. Dazu können Sie den Wert **Länge** der Node (Anzahl der
Blöcke, die Ihre Node empfangen hat) mit dem **Chain Len**-Wert (Anzahl der
Blöcke im Netzwerk) vergleichen, der oben im Dashboard angezeigt wird.


Zulassen von eingehenden Verbindungen
=====================================

Wenn Sie Ihre Node hinter einer Firewall oder hinter Ihrem Heim-Router betreiben, dann können Sie wahrscheinlich nur eine Verbindung zu anderen Nodes herstellen, aber andere Nodes sind nicht in der Lage, Verbindungen zu Ihrer Node zu initiieren.
Dies ist völlig in Ordnung, und Ihre Node nimmt vollständig am
Concordium-Netzwerk teil. Er wird in der Lage sein, Transaktionen zu senden und,
:ref:`if so configured<become-a-baker>`, zu backen und abzuschließen.

Sie können Ihre Node aber auch zu einem noch besseren Netzwerkteilnehmer machen, indem Sie eingehende Verbindungen aktivieren. Standardmäßig lauscht ``concordium-node`` auf
dem Port ``8888`` für eingehende Verbindungen. Abhängig von Ihrem Netzwerk und
Plattformkonfiguration müssen Sie entweder einen externen Port
auf ``8888`` auf Ihrem Router weiterleiten, ihn in Ihrer Firewall öffnen oder beides. Die
Details, wie dies zu tun ist, hängen von Ihrer Konfiguration ab.

Ports konfigurieren
-------------------

Die Node lauscht auf vier Ports, die durch Angabe der entsprechenden Kommandozeilenargumente beim Start der Node konfiguriert werden können.
Die Ports die von der Node verwendet werden, sind wie folgt:

-  8888, der Port für Peer-to-Peer-Netzwerke, der wie folgt gesetzt werden kann:
   ``--listen-node-port``
-  8082, der von der Middleware verwendete Port, der mit ``--listen-middleware-port`` eingestellt werden kann
-  10000, der gRPC-Port, der mit ``--listen-grpc-port`` eingestellt werden kann

Beim Ändern des obigen Mappings muss der Docker-Container
gestoppt (:ref:`stop-a-node`), zurückgesetzt und wieder gestartet werden. Um den Container zurückzusetzen, verwenden Sie entweder
``concordium-node-reset-data`` oder führen Sie ``docker rm concordium-client`` in
einem Terminal aus.

Wir empfehlen *strengstens*, dass Ihre Firewall so konfiguriert sein sollte, dass sie nur
öffentliche Verbindungen auf Port 8888 (dem Peer-to-Peer-Netzwerkport) zulassen. Jemand, der Zugriff auf die anderen Ports hat, könnte in der Lage sein, die
Kontrolle über Ihre Node oder über Nodes, die Sie auf den Nodes gespeichert haben.

.. _stop-a-node:

Anhalten der Node
=================

Um die Node anzuhalten, drücken Sie **Strg+c**, und warten Sie, bis die Node sauber
herunterfährt.

Wenn Sie versehentlich das Fenster schließen, ohne den Client explizit herunterzufahren, läuft der Client in Docker im Hintergrund weiter. In diesem Fall verwenden Sie das ``concordium-node-stop``-Binary auf die gleiche Weise, wie Sie die ausführbare Datei ``concordium-node`` verwenden.

Support & Feedback
==================
Logging-Informationen für Ihren Knoten können Sie mit dem Befehl ``concordium-node-retrieve-logs`` abrufen. Dies speichert Protokolle vom laufenden Image in eine Datei. Wenn Sie die Erlaubnis dazu haben, wird es außerdem Informationen über die Programme, die gerade auf dem System laufen, abrufen.

Sie können Ihre Protokolle, Systeminformationen, Fragen und Rückmeldungen an
testnet@concordium.com senden. Sie können sich auch an unser `Discord`_ wenden, oder
besuchen Sie unsere :ref:`troubleshooting page<troubleshooting-and-known-issues>`.
