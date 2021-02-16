.. _contract-instances:

========================
Smart contract instances
========================

.. todo::

   - Linawin kung pano nag uugnay ang mga instance at smart contracts sa modulo
     (halimbawa, ngayon na sinabi na ang instance ay module at state pero ang 
     litrato sa ibaba ay pinapakita na ang instances ay contracts +state).
   - Pumili kung papano dapat i talaga ang smart contract modules ( mula sa sarili neto concepto o
     kaya ay Wasm modules), o kung dapat ba natin silang isali.
   - Pumili kung dapat ba mag karoon ng kong kretong halimbawa, at kung dapat ba 
      ito maging Wasm o Rust o pseudocode.
   - Isaisip at mag karoon ng ideya na nag papaliwanag ng relasyon ng modules at instances.  
     
Ang **smart contract instance** ay smart contract module kapareha ang 
tiyak na estado at tiyak na halaga ng GTU tokens.
Madamihang smart contract instances ay pwede gawin sa parehong modulo.
Sa halimbawang, ang :ref:`auction <auction>` contract, pwede na maramihang pagkakataon, na bawat isang 
nakatuon sa pag-bid para sa isang tukoy na bagay at sa sarili nitong mga kalahok.

Ang Smart contract instances ay maaring gawin sa isang :ref:`smart contract
module<contract-module>` sa pamamagitan ng  ``init`` transaction na nag uugmok
sa hiniling na function sa smart contract module. Etong function na to ay makakauha
ng parameter.
Ang resulta neto ay kailangan para maging initial smart contract ng estado ng
instance.

.. note::

   Ang smart contract instance ay madalas tinatawag na *instance*.

.. graphviz::
   :align: center
   :caption: Halimbawa  ng smart contract module na may dalawang smart contracts:
             Escrow at Crowdfunding. Ang parehong kontrata ay may dalawanginstances.

   digraph G {
       rankdir="BT"

       subgraph cluster_0 {
           label = "Module";
           labelloc=b;
           node [fillcolor=white, shape=note]
           "Crowdfunding";
           "Escrow";
       }

       subgraph cluster_1 {
           label = "Instances";
           style=dotted;
           node [shape=box, style=rounded]
           House;
           Car;
           Gadget;
           Boardgame;
       }

       House:n -> Escrow;
       Car:n -> Escrow;
       Gadget:n -> Crowdfunding;
       Boardgame:n -> Crowdfunding;
   }

Estado ng smart contract instance
==================================

Ang estado ng smart contract instance ay may dalawang parte, ang tinukoy ng gumagamit
at halaga ng GTU na mayroon ang kontrat. halimbawa, ang *balance*. Kapag iniiuugnay ang state
ibig sabihin neto ay ang tinukoy ng gumagamit na estado. Ang rason kung bakit kailangang
ihiwalay ang GTU amount ay dahil ang GTU ay maari lamang gastusin at makuha
sa pamamagitan ng panununtunan ng network, halimbawa ang mga kontrata ay di makakagawa
o sumira ng GTU tokens.

.. _contract-instances-init-on-chain:

Pag gawa ng instansya sa on-chaon ng smart contract
===================================================

Ang bawat smart contract ay dapat mag talaga ng function para sa pag gawa ng smart contract 
na istansya. Ang function na ganoon ay tinatalaga bilang *init function*.

Para makagawa ng smart contract instance, ang account ay nag papadala ng espesyal na
transaksyon na nakadugtong sa naagawang smart contract module at ang pangalan ng
init function na gagamitin dito.

Ang transaksyon ay maari din magkaroon ng halaga ng GTU, na kung saan ay dinadagdag
sa balanse ng smart contract instance. May tinatalagang bagay na kailangan sa function
na parte ng transaksyon at ibinibigay sa anyo ng isang hanay ng mga byte.

Upang buod, ang mga transaksyon ay binubuo:

- Referensya sa smart contract module.
- Panggalan ng init function.
- Parameter para sa init function.
- Halaga ng GTU sa instansya.

Ang init function ay hindi nag hahangad na gumawa ng bagong istansya mula sa
mga parameters na yun. Kung ang init function ay tumatanggap ng parameters,
ito ay nag tatalaga ng initial na estado ng instansya ng inisyal na balanse.
Ang instance ay binibigyan ng address sa chain at sa account ng nagpadala
ay siyang nag mamay ari ng instance. Kung ang function ay tumanggi, walang
instance na nagawa at ang transaksyon sa sa pagsubok sa pag gawa ng instance
ay makikita sa on-chain.

.. seealso::

  Tignan :ref:`initialize-contract` gabay sa mismong initial na pag gawa ng kontrata.
   

Instance state
==============
Ang bawat smart contract instance ay may karagdagang estado na kung saan ito ay ni rerepresenta
sa isang chain sa pamamagitan ng anyo ng isang hanay ng mga byte. Ang instansya ay gumagamit ng functions
na bigay ng host environment upang basahin, isulat at palitan ang laki ng estado.

.. seealso::

   Tignan ang :ref:`host-functions-state` para sa reperensya sa mga functions na ito.

Ang estado ng isang Smart contrct ay limitado sa laki neto. Sa kasalukuyan ang limitasyon neto
ay 16KiB.

.. seealso::

   Tignan ang :ref:`resource-accounting` para sa iba pang impormasyon.

Interaksyon sa Instansya
============================

Ang smart contract ay maaring ilantad ang zero or madami pang functions
para sa pakikipag ugnay sa instansya, tinutukoy eto na *receive functions*.

Kahalintulad ng init functions, recieve functions ay natatawag sa pamamagitan
ng transaksyon, na nag kakahalaga ng ka unting halaga ng GTU para sa kontrata
at argumento sa function sa kaanyuan ng bytes.

Upang buod, ang isang transaksyon para sa pakikipag-ugnay sa smart-contract ay may kasamang:

- Address para sa smart contract instance.
- Pangalan para sa nakuhang function.
- Parametro para matanggap ang function
- Halaga ng GTU ng instansya.

.. _contract-instance-actions:

Pagtatala ng events
==============

.. todo::
 
   Ipaliwanag kung ano ang mga events at bakit sila mahalaga.
   Linawin ang  "monitor for events".
 
Ang events ay pwede itala mula sa pagpapatupad ng smart contract functions. Ito ay
parehas na kaso sa init and recieve functions. Ang logs ay ginawa paara sa off-chain na
gamit, para ang mga aktor sa labas ng smart contracts, o aktor sa labas ng chain.
Ang mga events ay maaring itala gamit ang functiion na binigay ng host environment.

.. seealso::

   Tignan ang :ref:`host-functions-log` para sa kaalaman sa function na ito.

Ang mga event logs na to ay pinanatili ng bakers at kasama sa mga buod ng transaksyon.

Ang pag tatala ng event ay nakadugtong sa gastos, kaparehas sa gastos sa pagsusulat ng estado
ng kontrata. Sa madalas na mga sitwasyon mas naayong mag tala lang ng kaunti upang makatipid sa gastos.

.. _action-descriptions:

Action descriptions
===================

A receive function returns a *description of actions* to be executed by
the host environment on the chain.

The possible actions that a contract can produce are:

- **Accept** is a primitive action that always succeeds.
- **Simple transfer** of GTU from the instance to the specified account.
- **Send**: invoke receive function of the specified smart contract instance,
  and optionally transfer some GTU from the sending instance to the receiving
  instance.

If an action fails to execute, the receive function is reverted, leaving
the state and the balance of the instance unchanged. However,

- the transaction that triggers the (unsuccessful) receive function is still added to the chain, and
- the transaction cost, including the cost of executing the failed action,
  is deducted from the sending account.

Processing multiple action descriptions
---------------------------------------

You can chain action descriptions using the **and** combinator.
An action-description sequence ``A`` **and** ``B``

1) Executes ``A``.
2) If ``A`` succeeds, executes ``B``.
3) If ``B`` fails the whole action sequence fails (and the result of ``A`` is reverted).

Handling errors
---------------

Use the **or** combinator to execute an action in case that a previous action fails.
An action description ``A`` **or** ``B``

1) Executes ``A``.
2) If ``A`` succeeds, stops executing.
3) If ``A`` fails, executes ``B``.

.. graphviz::
   :align: center
   :caption: Example of an action description, which tries to transfer to Alice
             and then Bob, if any of these fails, it will try to transfer to
             Charlie instead.

   digraph G {
       node [color=transparent]
       or1 [label = "Or"];
       and1 [label = "And"];
       transA [label = "Transfer x to Alice"];
       transB [label = "Transfer y to Bob"];
       transC [label = "Transfer z to Charlie"];

       or1 -> and1;
       and1 -> transA;
       and1 -> transB;
       or1 -> transC;
   }

.. seealso::

   See :ref:`host-functions-actions` for a reference of how to create the
   actions.

The whole action tree is executed **atomically**, and either leads to updates
to all the relevant instances and accounts, or, in case of rejection, to payment
for execution, but no other changes. The account which sent the initiating
transaction pays for the execution of the entire tree.
