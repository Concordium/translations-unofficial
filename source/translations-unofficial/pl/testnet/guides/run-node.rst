.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node:

==================
bieg węzeł (node)
==================

.. contents::
   :local:
   :backlinks: none

W tym przewodniku, dowiesz się, jak bieg węzeł(node) na swoim komputerze
uczestniczy w sieci Concordium. Oznacza to, że otrzymujesz
bloki i transakcje z innych węzłów, także tak jak propagować
Informacja o blokach i transakcjach do węzłów w Concordium
sieć. Po wykonaniu tego przewodnika, będziesz w stanie

-  uruchom węzeł(node) Concordium
-  obserwuj to na panelu sieciowym i
-  zapytaj o stan łańcucha blokowego Concordium bezpośrednio z twojego maszyna.

Nie potrzebujesz konta, aby bieg węzeł(node).

Zanim zaczniesz
================

Przed uruchomieniem węzła Concordium musisz to zrobić

1. Zainstaluj i uruchom Docker.
   -  Na *Linux*, zezwalaj na uruchamianie Dockera jako użytkownik inny niż root.

2. Pobierz i wypakuj the :ref:`concordium-node-and-client-download` oprogramowanie.

Uaktualnij z wcześniejszej wersji Open Testnet
===============================================

Aby dokonać aktualizacji do aktualnego oprogramowania Concordium dla Open Testnet 4:

-  Wykonaj powyższe kroki, aby :ref:`Download<downloads>` najnowsze 
   oprogramowanie Concordium.

-  Uruchom ``concordium-node-reset-data`` wykonywalny z rozpakowanego
   archiwum.

   -  Dla *Mac* użytkowników: the pierwszy raz otwierasz narzędzie, kliknij prawym przyciskiem myszy
      ``concordium-node-reset-data`` plik i wyselekcjonować **otwarty**. 
      Pojawi się komunikat, że oprogramowanie pochodzi od niezidentyfikowanego programisty.
      wyselekcjonować **otwarty** jeszcze raz.
   -  Dla *Windows* użytkowników: the pierwszy raz otwierasz narzędzie,
      podwójne-kliknięcie the ``concordium-node-reset-data`` plik. 
      Pojawi się komunikat, że oprogramowanie pochodzi od niezidentyfikowanego programisty.
      wyselekcjonować **Więcej informacji** → **Biegnij mimo wszystko**.

-  Narzędzie zapyta:

      *Czy chcesz również usunąć zapisane klucze?*

   Konta utworzone dla wcześniejszych wersji nie są już ważne w Open Testnet 3. 
   W związku z tym, jeśli masz zapisane konta z poprzednich wersji
   zalecamy wejście **y** co spowoduje usunięcie wszystkich kluczy 
   kont.

.. _running-a-node:

Uruchomienie węzła
===================

Aby rozpocząć uruchamianie klienta, który dołączy do Open Testnet postępuj zgodnie z nimi
kroki:

1. Otwarte the ``concordium-node`` wykonywalny z rozpakowanego archiwum.

-  Dla *Mac* użytkowników: the pierwszy raz otwórz narzędzie, kliknij prawym przyciskiem myszy
   ``concordium-node`` binarny i wyselekcjonować **otwarty**. 
   Pojawi się komunikat, że oprogramowanie pochodzi od niezidentyfikowanego programisty.
   wyselekcjonować **otwarty** jeszcze raz.
-  Dla *Windows* użytkowników: the pierwszy raz otwórz narzędzie, podwójne kliknięcie
   the ``concordium-node`` dwójkowy. Pojawi się komunikat, 
   że oprogramowanie pochodzi od niezidentyfikowanego programisty. wyselekcjonować **Więcej informacji** →
   **Biegnij mimo wszystko**.
-  Gdy *ponowne uruchamianie* węzeł(node) rozważać używając
   ``--no-block-state-import`` opcja. Spowoduje to pobranie tylko aktualizacji
   do łańcucha blokowego Concordium, który wystąpił, gdy węzeł(node) 
   był nieaktywny i może przyspieszyć proces uruchamiania.

2. Wpisz nazwę swojego węzła. Ta nazwa będzie wyświetlana na publicznym pulpicie 
   nawigacyjnym.

3. Jeśli narzędzie zostało uruchomione wcześniej, przed rozpoczęciem zostaniesz zapytany, 
   czy chcesz usunąć bazę danych węzłów lokalnych. pilny **y** 
   usunie i później odtwarzać informacje o the Stan z
   Blockchain Concordium, który został zapisany na Twoim komputerze. **Zauważ, że
   usunięcie bazy danych węzłów lokalnych oznacza, 
   że zajmie to więcej czasu węzeł(node) do dogonienia sieci Concordium.**

Narzędzie pobierze teraz obraz klienta Concordium i załaduje go do
Doker. Klient uruchomi się i zacznij wyświetlać informacje logowania
o działaniu węzła.

Widząc swój węzeł(node) na pulpicie nawigacyjnym
==================================================

Po bieganiu ``concordium-node`` możesz

-  zobacz swój węzeł(node) the `Network Dashboard`_
-  :ref:`query<testnet-query-node>` informacje o blokach, transakcje, i kont.

Sieć deska rozdzielcza
-----------------------

Poprawienie stanu pliku zajmie klientowi trochę czasu
Blockchain Concordium. wiąże, na przykład, Ściągnij
informacje o wszystkich blokach w łańcuchu.

Między innymi, na `Network Dashboard`_ możesz
otrzymać pomysł ile czasu zajmie węzłowi nadrobienie zaległości z łańcuch.
Za to możesz porównać węzeł(node) **Długość** wartość (Liczba
Bloki twój węzeł(node) otrzymał) z the **Chain Len** wartość 
(Liczba Bloki w najdłuższym łańcuchu w sieci) który jest wyświetlany
w górnej części deski rozdzielczej.


Włącz przychodzące połączenia
==============================

Jeśli jesteś bieganie Twój węzeł(node) za firewallem, lub za domem
router, wtedy prawdopodobnie będziesz mógł łączyć się tylko z innymi węzłami,
ale inne węzły nie będą mogły inicjować połączeń z Twoim węzłem.
To jest całkowicie w porządku, a Twój węzeł(node) będzie w pełni uczestniczył
Sieć Concordium. Będzie mógł wysyłać transakcje i,
:ref:`if so configured<become-a-baker>`, upiec i sfinalizować.

Jednak możesz również uczynić swój węzeł(node) jeszcze lepszym uczestnikiem sieci
poprzez włączenie połączeń przychodzących. Domyślnie, ``concordium-node`` listens
on port ``8888`` dla połączeń przychodzących. W zależności od Twojej sieci i
konfiguracja platformy będziesz zarówno trzeba przekazać dalej na port zewnętrzny
aby ``8888`` na Twoim router, otwórz go w swoim firewallu, lub oba.
szczegóły tego, jak to się robi, zależą od twojej konfiguracji.

Konfigurowanie portów
----------------------

Węzeł(node) nasłuchuje na czterech portach, that can be configured przez dostarczanie
odpowiednie argumenty wiersza poleceń podczas uruchamiania węzła. Porty
używane przez węzeł(node) są następujące:

-  8888, port dla sieci peer-to-peer, które można ustawić za pomocą
   ``--listen-node-port``
-  8082, port używany przez oprogramowanie pośredniczące, które można ustawić za pomocą ``--listen-middleware-port``
-  10000, the gRPC port, które można ustawić za pomocą ``--listen-grpc-port``

Podczas zmiany mapowań powyżej kontenera Dockera musi być
już się zatrzymał (:ref:`stop-a-node`), Resetowanie, i zaczął jeszcze raz. Aby zresetować kontener albo użyj
``concordium-node-reset-data`` lub biegnij ``docker rm concordium-client`` w
terminal.

My *zdecydowanie zalecane* że twoja zapora powinna być skonfigurowana tylko
zezwalaj na połączenia publiczne na porcie 8888 (sieci peer-to-peer
Port).Ktoś z dostępem do innych portówmoże być w stanie wziąć
kontrola twojego węzła lub konta zapisane w węźle.

.. _stop-a-node:

Zatrzymywanie węzła
=====================

Aby zatrzymać węzeł(node), naciśnij **CTRL+c**, i poczekaj, aż węzeł(node) wyczyści
zamknąć.

Jeśli przypadkowo zamkniesz okno bez jawnego wyłączania
Klient, będzie dalej działać w tle w Dockerze. kiedy to się dzieje, 
Użyj ``concordium-node-stop`` binarny w ten sam sposób, w jaki otworzyłeś
the ``concordium-node`` wykonywalny.

Wsparcie i informacje zwrotne
===============================

Rejestrowanie informacji dla twojego węzła można odzyskać używając
``concordium-node-retrieve-logs`` narzędzie. Spowoduje to zapisanie dzienników z pliku
uruchomiony obraz do pliku. Dodatkowo, jeśli otrzyma pozwolenie, to będzie
uzyskać informację o programach aktualnie uruchomionych w systemie.

Możesz wysłać swoje logi, informacje o systemie, pytania i informacje zwrotne aby
testnet@concordium.com.Możesz również skontaktować się w nasz `Discord`_, lub
sprawdź nasze :ref:`troubleshooting page<troubleshooting-and-known-issues>`

