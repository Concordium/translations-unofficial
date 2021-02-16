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
     
     (e.g., right now it says that an instance is a module + state but the
     picture below shows instances as contracts + state).
   - Decide how exactly we should define smart-contract modules (as its own concept
     or as Wasm modules), and whether we should talk about them at all.
   - Decide whether we should have a concrete code example, and whether it should
     be in Wasm or Rust or perhaps pseudocode.
   - Consider having a picture that explains the relationship between modules and instances.

Ang **smart contract instance** ay smart contract module kapareha ang 
tiyak na estado at tiyak na halaga ng GTU tokens.
Madamihang smart contract instances ay pwede gawin sa parehong modulo.
Sa halimbawang, ang :ref:`auction <auction>` contract, pwede na maramihang pagkakataon, na bawat isang 
nakatuon sa pag-bid para sa isang tukoy na bagay at sa sarili nitong mga kalahok.

A **smart contract instance** is a smart contract module together with a
specific state and an amount of GTU tokens.
Multiple smart contract instances can be created from the same module.
For example, for an :ref:`auction <auction>` contract, there could be multiple instances, each
one dedicated to bidding for a specific item and with its own participants.

Ang Smart contract instances ay maaring gawin sa isang :ref:`smart contract
module<contract-module>` sa pamamagitan ng  ``init`` transaction na nag uugmok
sa hiniling na function sa smart contract module. Etong function na to ay makakauha
ng parameter.
Ang resulta neto ay kailangan para maging initial smart contract ng estado ng
instance.

Smart contract instances can be created from a deployed :ref:`smart contract
module<contract-module>` via the ``init`` transaction which invokes the
requested function in the smart contract module. This function can take a
parameter.
Its end result is required to be the initial smart contract state of the
instance.

.. note::

   Ang smart contract instance ay madalas tinatawag na *instance*.

.. graphviz::
   :align: center
   :caption: Example of smart contract module containing two smart contracts:
             Halimbawa  ng smart contract module na may dalawang smart contracts:
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

The state of a smart contract instance consists of two parts, the user-defined
state and the amount of GTU the contract holds, i.e., its *balance*. When
referring to state we typically mean only the user-defined state. The reason for
treating the GTU amount separately is that GTU can only be spent and
received according to rules of the network, e.g., contracts cannot create
or destroy GTU tokens.

.. _contract-instances-init-on-chain:

Pag gawa ng instansya sa on-chaon ng smart contract
===================================================

Ang bawat smart contract ay dapat mag talaga ng function para sa pag gawa ng smart contract 
na istansya. Ang function na ganoon ay tinatalaga bilang *init function*.

Every smart contract must contain a function for creating smart contract
instances. Such a function is referred to as the *init function*.

Para makagawa ng smart contract instance, ang account ay nag papadala ng espesyal na
transaksyon na nakadugtong sa naagawang smart contract module at ang pangalan ng
init function na gagamitin dito.

To create a smart contract instance, an account sends a special transaction with
a reference to the deployed smart contract module and the name of the
init function to use for instantiation.

Ang transaksyon ay maari din magkaroon ng halaga ng GTU, na kung saan ay dinadagdag
sa balanse ng smart contract instance. May tinatalagang bagay na kailangan sa function
na parte ng transaksyon at ibinibigay sa anyo ng isang hanay ng mga byte.

The transaction can also include an amount of GTU, which is added to the balance
of the smart contract instance. A parameter to the function is supplied as part
of the transaction in the form of an array of bytes.

To summarize, the transaction includes:
Upang buod, ang mga transaksyon ay binubuo:


- Reference to the smart contract module.
- Referensya sa smart contract module.
- Panggalan ng init function.
- Name of the init function.
- Parameter para sa init function.
- Parameter to the init function.
- Halaga ng GTU sa instansya.
- Amount of GTU for the instance.

Ang init function ay hindi nag hahangad na gumawa ng bagong istansya mula sa
mga parameters na yun. Kung ang init function ay tumatanggap ng parameters,
ito ay nag tatalaga ng initial na estado ng instansya ng inisyal na balanse.


The init function can signal that it does not wish to create a new instance
with those parameters. If the init function accepts the parameters, it sets
up the initial state of the instance and its balance. The instance is given an
address on the chain and the account who sent the transaction becomes the owner
of the instance. If the function rejects, no instance is created and only the
transaction for attempting to create the instance is visible on-chain.

.. seealso::

   See :ref:`initialize-contract` guide for how to initialize a
   contract in practice.

Instance state
==============
Ang bawat smart contract instance ay may karagdagang estado na kung saan ito ay ni rerepresenta
sa isang chain sa pamamagitan ng anyo ng isang hanay ng mga byte. Ang instansya ay gumagamit ng functions
na bigay ng host environment upang basahin, isulat at palitan ang laki ng estado.

Every smart contract instance holds its own state which is represented on-chain
as an array of bytes. The instance uses functions provided by the host
environment to read, write and resize the state.

.. seealso::

   See :ref:`host-functions-state` for a reference of these functions.
   Tignan ang :ref:`host-functions-state` para sa reperensya sa mga functions na ito.

Smart contract state is limited in size. Currently the limit on smart contract
state is 16KiB.

Ang estado ng isang Smart contrct ay limitado sa laki neto. Sa kasalukuyan ang limitasyon neto
ay 16KiB.

.. seealso::

   Check out :ref:`resource-accounting` for more on this.
   Tignan ang :ref:`resource-accounting` para sa iba pang impormasyon.

Interaksyon sa Instansya
============================

Ang smart contract ay maaring ilantad ang zero or madami pang functions
para sa pakikipag ugnay sa instansya, tinutukoy eto na *receive functions*.

A smart contract can expose zero or more functions for interacting with an
instance, referred to as *receive functions*.

Kahalintulad ng init functions, recieve functions ay natatawag sa pamamagitan
ng transaksyon, na nag kakahalaga ng ka unting halaga ng GTU para sa kontrata
at argumento sa function sa kaanyuan ng bytes.

Just like with init functions, receive functions are triggered using
transactions, which contain some amount of GTU for the contract and an argument
to the function in the form of bytes.



To summarize, a transaction for smart-contract interaction includes:
Upang buod, ang isang transaksyon para sa pakikipag-ugnay sa smart-contract ay may kasamang:

- Address to smart contract instance.
- Address para sa smart contract instance.
- Name of the receive function.
- Pangalan para sa nakuhang function.
- Parameter to the receive function.
- Parametro para matanggap ang function
- Halaga ng GTU ng instansya.
- Amount of GTU for the instance.

.. _contract-instance-actions:

Pagtatala ng events
==============

.. todo::

   Explain what events are and why they are useful.
   Rephrase/clarify "monitor for events".
   
   Ipaliwanag kung ano ang mga events at bakit sila mahalaga.
   Linawin ang  "monitor for events".
   
   

Events can be logged during the execution of smart contract functions. This is
the case for both init and receive functions. The logs are designed for
off-chain use, so that actors outside of the chain can monitor for events and
react to them. Logs are not accessible to smart contracts, or any other actor on
the chain. Events can be logged using a function supplied by the host
environment.

Ang events ay pwede itala mula sa pagpapatupad ng smart contract functions. Ito ay
parehas na kaso sa init and recieve functions. Ang logs ay ginawa paara sa off-chain na
gamit, para ang mga aktor sa labas ng smart contracts, o aktor sa labas ng chain.
Ang mga events ay maaring itala gamit ang functiion na binigay ng host environment.


.. seealso::

   See :ref:`host-functions-log` for the reference of this function.
   Tignan ang :ref:`host-functions-log` para sa kaalaman sa function na ito.

These event logs are retained by bakers and included in transaction summaries.
Ang mga event logs na to ay pinanatili ng bakers at kasama sa mga buod ng transaksyon.

Logging an event has an associated cost, similar to the cost of writing to the
contract's state. In most cases it would only make sense to log a few bytes to
reduce cost.

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
