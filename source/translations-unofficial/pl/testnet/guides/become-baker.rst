
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker:

==================================
Zostań piekarzem (twórz bloki)
==================================

.. contents::
   :local:
   :backlinks: none
   
Ta sekcja wyjaśnia, czym jest piekarz, jego rolę w sieci i jak nią zostać.

Czytając tę sekcję, dowiesz się:

-  Co to jest piekarz i pokrewne pojęcia do tego.
-  Jak ulepszyć swój węzeł(node), aby zostać piekarzem.

Proces zostania piekarzem można podsumować w następujących krokach:

#. Zdobądź konto i niektóre GTUs.
#. Zdobądź zestaw kluczy do piekarza.
#. Zarejestruj klucze piekarza na koncie.
#. Uruchom węzeł(node) za pomocą klawiszy piekarza.

Po wykonaniu tych czynności, węzeł(node) piekarza będzie piec bloki. Jeśli upieczony blok
jest dodawany do łańcucha piekarz węzła(node) otrzyma nagrodę.

.. note::

   W tej sekcji użyjemy nazwy ``bakerAccount`` jako nazwa
   konto które posłużą do zarejestrowania piekarza i zarządzania nim.

Definicje
===========

Piekarz
-----

Węzeł(node) to *piekarz* (lub *pieczenie*) kiedy aktywnie uczestniczy w
sieć tworząc nowe bloki, które są dodawane do łańcucha. Piekarz zbiera,
Orders i uprawomocnić the transakcje które są zawarte w bloku utrzymać
integralność łańcucha bloków. Piekarz podpisuje każdy blok że oni piec 
więc że blok można sprawdzić i wykonany przez resztę uczestników
sieć.

Piekarz Klucze
----------

Każdy piekarz ma zestaw kluczy kryptograficznych nazywa *Piekarz Klucze*. Węzeł(node) używa
te klucze podpisać bloki, które piecze. W celu wypieku bloków sygnowanych przez a
konkretnego piekarza węzeł(node) musi działać z załadowanym zestawem kluczy piekarza.

Konto Piekarz 
-------------

Każde konto może używać zestawu kluczy piekarza do rejestracji piekarza.

Kiedy tylko Piekarz piecze prawidłowy blok, który zostanie włączony do łańcucha, po jakimś
czas nagroda jest wypłacana na powiązane konto.

Stake i loteria
-----------------

.. todo::

   - Link to release schedule.

Konto może postawić część swojego salda GTU na *stake piekarza*, a później może ręcznie
zwolnić całość lub część postawionej kwoty. Postawiona kwota nie może zostać przesunięta 
ani przeniesiona, dopóki nie zostanie zwolniona przez piekarza.

.. note::

   Jeśli konto posiada kwotę, która została przekazana z harmonogramem wydań,
   kwota może zostać stake, nawet jeśli nie została jeszcze zwolniona.

aby być wybranym do pieczenia bloku, piekarz musi uczestniczyć w
*loteria* w którym prawdopodobieństwo otrzymania zwycięskiego kuponu jest przybliżone
proporcjonalny do postawionej kwoty.

Ta sama stake jest używana przy obliczaniu, czy piekarz zostanie uwzględniony w finalizacji
komitet albo nie. Widzieć Finalizaji_.

.. _epochs-and-slots-pl:

Epochs i sloty
----------------

W Concordium blockchain, czas jest podzielony na *sloty*.Sloty mieć czas
czas trwania ustalony w bloku Genesis. na dowolnym oddziale, każdy slot może mieć 
w najbardziej jeden blok, ale wiele bloków na różnych gałęziach można wytworzony w
ten sam slot.

.. todo::

   Let's add a picture.

Gdy rozważając nagrody i inne pieczenie-związane z koncepcje, Używamy the
koncepcja *epoki* jako jednostka czasu definiująca okres w którym zestaw
obecnych piekarzy i stake są ustalone. Epoki mieć czas Trwanie naprawione na
Blok Genesis. w testnet, epoki mają czas trwania z **1 hour**.

Rozpocznij pieczenie
============

Zarządzanie kontami
-----------------

Ta sekcja zapewnia krótkie podsumowanie odpowiednich kroków, dla Importowanie konto. 
Aby uzyskać pełny opis, widzieć :ref:`managing_accounts`.

Konta są tworzone przy użyciu :ref:`concordium_id` aplikacja. Kiedyś konto 
pomyślnie utworzony, nawigacja do the **więcej** tab i wybór **Eksport**
pozwala Ci aby uzyskać plik JSON zawierające informacje o koncie.

Aby zaimportować konto do łańcucha narzędzi uruchom

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount

``concordium-client`` zapyta o hasło, aby odszyfrować wyeksportowany plik i zaimportować wszystkie konta.
To samo hasło będzie używane do szyfrowania kluczy podpisywania transakcji i zaszyfrowanego klucza transferów.

Tworzenie kluczy dla piekarza i rejestracyjny tego
--------------------------------------------

.. note::

   Do tego procesu konto musi posiadać jakąś GTU więc upewnij się zażądać the
   100 GTU upuść na konto w aplikacji mobilnej.

Każde konto ma unikalny identyfikator piekarza który jest używany podczas rejestracji piekarza. To
Identyfikator musi zostać dostarczony przez sieć i obecnie nie można go wstępnie obliczyć.
Ten identyfikator należy podać wewnątrz kluczy piekarza do węzła, aby mógł on używać kluczy
piekarza do tworzenia bloków. The ``concordium-client`` automatycznie wypełni to 
pole podczas wykonywania poniższych operacji.

Aby utworzyć nowy zestaw kluczy, uruchom:

.. code-block:: console

   $concordium-client baker generate-keys <keys-file>.json

gdzie możesz wybrać nieprzewidywalną nazwę na klucze plik. Do
zarejestrować klucze w sieci musisz być :ref:`bieganie węzeł(node) <running-a-node>`
i wyślij ``piekarz dodaj`` transakcja do sieci:

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

będzie zastąpiony

- ``<amountToStake>`` z kwotą GTU for the baker's stawka początkowa.
- ``<concordium-data-dir>`` z następującym katalogiem danych:

  * on Linux and MacOS: ``~/.local/share/concordium``
  * on Windows: ``%LOCALAPPDATA%\\concordium``.

(Nazwa pliku wyjściowego powinna pozostać ``baker-credentials.json``).

Zapewnij ``--no-restake`` flaga, której należy unikać automatyczne dodawanie
nagrody do postawiona kwota na piekarzu. To zachowanie jest opisane w
Sekcja `Restaking the earnings`_.

w celu uruchomić węzeł tym kluczem piekarzas i zacznij produkować bloki 
najpierw musisz zamknąć aktualnie działający węzeł (albo naciskając
``Ctrl + C`` na terminalu gdzie jest węzeł(node) bieganie lub używając
``concordium-node-stop`` wykonywalny).

Po umieszczeniu pliku w odpowiednim katalogu (już zrobione w
poprzednie polecenie podczas określania pliku wyjściowego), start węzeł(node) ponownie używając
``concordium-node``. Węzeł(node) automatycznie rozpocząć pieczenie kiedy piekarz
dostaje zawarte w piekarzach dla bieżącej epoki.

Ta zmiana zostanie wykonana
natychmiast i zacznie obowiązywać, kiedy wykończeniowy epoka po w którym
transakcja za dodanie piekarza był zawarty w bloku.

.. table:: Oś czasu: dodanie piekarza

   +-------------------------------------------+-----------------------------------------+-----------------+
   |                                           | Gdy transakcja jest zawarta w bloku     | Po 2 epochs     |
   +===========================================+=========================================+=================+
   | Zmiana jest widoczna przez querying węzeł |  ✓                                      |                 |
   +-------------------------------------------+-----------------------------------------+-----------------+
   | Baker is included in the baking committee |                                         | ✓               |
   +-------------------------------------------+-----------------------------------------+-----------------+

.. note::

   Jeśli transakcja za dodanie piekarza został zawarty w bloku w czasie epoki `E`, piekarz będzie brany pod uwagę
   jako część komitetu pieczenia w epoce
   `E+2` starts.

Zarządzający piekarz
==================

Sprawdzanie statusu piekarza i jego moc loterii
------------------------------------------------------

W celu zobacz, czy węzeł(node) się piecze, możesz sprawdzić różne źródłas że
oferują różne stopnie precyzja w wyświetlanych informacjach.

- W `sieć dashboard <http://dashboard.testnet.concordium.com>`_, Twój
  węzeł pokaże swój identyfikator piekarza w ``Baker`` column.
- Używając ``concordium-client`` możesz sprawdzić listę aktualnych piekarzy
  i krewny postawiona kwota że oni trzymają, i.e. ich moc loterii.
  Moc loterii określi jak prawdopodobne to ten piekarz wygra
  loteria i upiecz blok.

  .. code-block:: console

     $concordium-client consensus show-parameters --include-bakers
     Election nonce:      07fe0e6c73d1fff4ec8ea910ffd42eb58d5a8ecd58d9f871d8f7c71e60faf0b0
     Election difficulty: 4.0e-2
     Piekarze:
                                  Konto                       Moc loterii
             ----------------------------------------------------------------
         ...
         34: 4p2n8QQn5akq3XqAAJt2a5CsnGhDvUon6HExd2szrfkZCTD4FX   <0.0001
         ...

- Używając ``concordium-client`` możesz sprawdzić, czy konto ma
  zarejestrował piekarza i aktualną kwotę postawioną przez tego piekarza.

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- Jeśli postawiona kwota jest wystarczająco duży i jest uruchomiony węzeł z piekarzem
  klucze załadowane, ten piekarz powinien w końcu produkować bloki i możesz zobaczyć
  w portfelu mobilnym że nagrody za wypieki są odbierane na koncie,
  jak widać na tym obrazku:

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px

Aktualizacja postawioną kwotę
--------------------------

Aby zaktualizować piekarza bieg stake

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount
   
Modyfikacja postawionej kwoty modyfikuje prawdopodobieństwo, że piekarz zostanie wybrany
piec bloki.

Kiedy piekarz **dodaje stawkę po raz pierwszy lub zwiększa swoją stawkę**, że
zmiana jest wykonywana w łańcuchu i staje się widoczny jak tylko transakcja
jest zawarty w bloku(może być widziane poprzez ``concordium-client account show
bakerAccount``) i zaczyna obowiązywać 2 epoki po tym.

.. table:: Oś czasu: zwiększenie stake

   +-------------------------------------------+--------------------------------------+----------------+
   |                                           |  Gdy transakcja jest zawarta w bloku | Po 2 epochs    |                    
   +===========================================+======================================+================+
   | Zmiana jest widoczna przez querying węzeł | ✓                                    |                |
   +-------------------------------------------+--------------------------------------+----------------+
   | Baker korzysta z nowej stake              |                                      | ✓              |
   +-------------------------------------------+--------------------------------------+----------------+

Kiedy piekarz **zmniejsza postawioną kwotę**, zmiana będzie potrzebować *2 +
bakerCooldownEpochs* epoki, aby odniosły skutek. Zmiana stanie się widoczna na
łańcuch gdy tylko transakcja zostanie zawarta w bloku, to może być konsultowane poprzez
``concordium-client account show bakerAccount``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: Oś czasu: maleje stake

   +------------------------------------------+--------------------------------------+----------------------------------------+
   |                                          |  Gdy transakcja jest zawarta w bloku |   Po *2 + bakerCooldownEpochs* epochs  |
   +========================================+========================================+========================================+
   | Zmiana jest widoczna przez querying węzeł| ✓                                    |                                        |
   +------------------------------------------+--------------------------------------+----------------------------------------+
   | Baker korzysta z nowej stake             |                                      | ✓                                      |
   +------------------------------------------+--------------------------------------+----------------------------------------+
   | Stake can be decreased again or          | ✗                                    | ✓                                     |
   | baker can be removed                     |                                      |                                        |
   +------------------------------------------+--------------------------------------+----------------------------------------+

.. note::
To
   wartość można sprawdzić w następujący sposób:
   w testnet, ``bakerCooldownEpochs`` jest początkowo ustawiony na 168 epoki. To
   wartość można sprawdzić w następujący sposób:

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

   Jak zaznaczono w `Definitions`_ Sekcja, postawiona kwota wynosi *zablokowany*,
   i.e. nie można go przenieść ani wykorzystać do zapłaty. Powinieneś to wziąć
   na konto i rozważ postawienie kwoty, która nie będzie potrzebna w
   krótkoterminowe. W szczególności, aby wyrejestrować piekarza lub zmodyfikować postawioną piekarz
   kwotę, której potrzebujesz, aby posiadać nieobstawione GTU, aby pokryć transakcję
   koszty.

Restaking zarobki
----------------------

Uczestnicząc jako piekarz w sieci i blokach do pieczenia, konto
otrzymuje nagrody za każdy upieczony blok. Te nagrody są automatycznie dodawane do
domyślnie postawiona kwota.

Możesz wybrać aby zmodyfikować to zachowanie a zamiast tego otrzymuj nagrody w
saldo konta bez staking je automatycznie. Ten przełącznik może być
zmieniony poprzez ``concordium-client``:

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

Zmiany flagi restake zaczną obowiązywać natychmiast; jednak, Zmiany
początek wpływające na pieczenie i finalizacja moc w epoki po następnym. Obecny
Wartość przełącznika można zobaczyć w informacjach o koncie, które można zapytać
używając ``concordium-client``:

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Oś czasu: aktualizacja restake

   +-------------------------------------------------+-----------------------------------------+-------------------------------+
   |                                                 | Gdy transakcja jest zawarta w bloku     | 2 epochs po byciu wynagrodzony|
   +=================================================+=========================================+===============================+
   | Zmiana jest widoczna przez querying węzeł       | ✓                                       |                               |
   +-------------------------------------------------+-----------------------------------------+-------------------------------+
   | Zarobki będą [not] być automatycznie restaked   | ✓                                       |                               |
   +-------------------------------------------------+-----------------------------------------+-------------------------------+
   | jeśli restaking automatycznie, zdobyte          |                                         | ✓                             |
   | stake wpływa na moc loterii                     |                                         |                               |
   +-------------------------------------------------+-----------------------------------------+-------------------------------+

Kiedy piekarz jest zarejestrowany, to będzie automatycznie re-stake zarobki, ale tak jak
wspomniano powyżej, można to zmienić dostarczając ``--no-restake`` flaga do
the ``baker add`` polecenie, jak pokazano tutaj:

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

Finalizacja
------------

Finalizacja to proces głosowania wykonywany przez węzły w *Finalizacja komisja* 
że *finalizuje* blok kiedy wystarczająco duża liczba członków 
komisja otrzymałem the blok i uzgodnij jego wynik. Nowsze bloki
musi mieć sfinalizowany blok jako przodek, aby zapewnić integralność
łańcuch. Więcej informacji o tym procesie, zobacz
:ref:`finalization<glossary-finalization>` sekcja.

Komitet finalizacyjny jest utworzona przez piekarzy które mają pewne staked
kwota. To konkretnie implikuje że aby wziąć udział w
komitet finalizacyjny prawdopodobnie będziesz musiał zmodyfikować postawioną kwotę
osiągnąć wspomniany próg. w testnet, postawiona kwota potrzebna do udziału
w komisji finalizacyjnej jest **0.1% całkowitej kwoty istniejącej GTU**.

Uczestnictwo w komitecie finalizacyjnym produkuje nagrody za każdy blok że
jest sfinalizowana. Nagrody są wypłacane na konto piekarza jakiś czas po
blok jest sfinalizowane.

Usunięcie piekarza
================

Konto kontrolujące może zdecydować o wyrejestrowaniu swojego piekarza jego piekarz na łańcuchu. do zrobienia
więc musisz wykonać ``concordium-client``:

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

Spowoduje to usunięcie piekarza z listy piekarzy i odblokowanie postawionej kwoty
Piekarz aby można go było transfer lub poruszał się swobodnie.

Podczas wyjmowania piekarza,zmiana ma tę samą oś czasu, co malejąca
postawioną kwotę. Zmiana będzie potrzebować *2 + bakerCooldownEpochs* epoki odniosły skutek.
Zmiana staje się widoczna w łańcuchu, gdy tylko transakcja zostanie włączona do bloku a ty
można sprawdzić kiedy ta zmiana wejdzie w życie, sprawdzając informacje o koncie
z ``concordium-client`` jak zwykle: 

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Oś czasu: usunięcie piekarza

   +--------------------------------------------------+-------------------------------------------+----------------------------------------+
   |                                                  | Gdy transakcja jest zawarta w bloku       | Po *2 + bakerCooldownEpochs* epochs    |
   +==================================================+===========================================+========================================+
   | Zmiana jest widoczna przez querying węzeł        | ✓                                         |                                        |
   +--------------------------------------------------+-------------------------------------------+----------------------------------------+
   | Baker zostaje usunięty z Komitetu ds. Pieczenia  |                                           | ✓                                      |
   +--------------------------------------------------+-------------------------------------------+----------------------------------------+

.. warning::

   Zmniejszenie postawionej kwoty i usunięcie piekarza nie może być zrobione
   równocześnie. W okresie cooldown produkowany przez zmniejszenie postawionej
   kwota, piekarza nie można usunąć i odwrotnie.

Wsparcie i informacje zwrotne
==================


Jeśli napotkasz żadnych problemów lub mieć sugestie, opublikuj swoje pytanie lub
informacje zwrotne na `Discord`_, lub Skontaktuj się z nami na testnet@concordium.com.
