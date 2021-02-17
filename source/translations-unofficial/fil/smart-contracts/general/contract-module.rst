.. _contract-module-fil:

======================
Smart contract module
======================

Ang smart contracts ay naka-deploy sa loob ng chain ng *smart contract modules*

.. note::

   Ang smart contact module ay kadalasan natutukoy sa madaling sabi ay *module*.

Ang module ay maaring maglaman ng isa or maraming smart contracts, na nag papahintulot
sa code na ma-share sa mga contracts and opsyonal na magkaroon ng :ref:`contract schemas
<contract-schema>`.

.. graphviz::
   :align: center
   :caption: Ang smart contract module ay naglalaman ng dalawang smart contracts.

   digraph G {
       subgraph cluster_0 {
           node [fillcolor=white, shape=note]
           label = "Module";
           "Crowdfunding";
           "Escrow";
       }
   }

Ang module ay dapat self-contained, at may mahigpit na listahan ng imports lamang
na maaring mag-intereact sa chain.
Ito ay naka-provide ng host environment at naka-available para sa smart contract
sa pamamagitan ng pag-import ng module na ``concordium``.

.. seealso::

   Tignan ito :ref:`host-functions` para sa kumpletong sanggunian.

On-chain wika
=============

Sa loob ng Concordium blockchain ang smart contract language ay isang subset ng `Web
Assembly`_ (Wasm pag pinaikli) na na-disenyo para maging portable compilation
target at para tumakbo sa sandboxed environments. Ito ay mahalaga dahil ang smart
contracts ay ipapatakbo ng bakers sa network na hindi pinagkakatiwalaan and code.

Wasm ay isang low-level language at ito ay impractical para isulat ng kamay. Sa halip
ito ay kayang mag-sulat ng smart contracts na mas high-level language tapos
i-cocompile sa Wasm.

.. _wasm-limitations-fil:

Limitasyon
-----------

.. todo::

   Dagdagan ng ibang limitasyon tulad ng start sections...

Ang blockchain environment ay napaka-partikular na ang bawat node ay dapat
makapag-execute ng contract sa eksaktong paraan, gamit ang eksaktong amount
ng resources. Kung hindi man ang nodes ay mabibigong ma-reach and consensus sa
state ng chain. Para sa gantong rason ang smart contracts ay dapat masulat sa restriktong
subset ng Wasm.

Mga Floating na Point Numbers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Sapagkat ang Wasm ay may support para sa floating point numbers, ang smart contract ay
hindi maaring magamit. And rason para dito ay ang Wasm floating-point numbers
ay maaring magkaroon lang ng espesyal ``NaN`` ("not a number") na halaga para sa solusyon
na maaring mag-resulta ng nondeterminism.

Ang restriksyon ay na-aapply statically, ibig sabihin ang smart contract ay hindi pwede magkaroon
ng floting point types, at hindi rin pwede magkaroon ng kahit anong instrukyon na nagsasama ng floating
point values.


Pag-deploy
==========

Ang pag-deploy ng module sa chain ay pagsubmit sa module bytecode bilang
transaksyon sa Concordium network. Kapag *valid* ang transaksyon ay masasama
sa block. Itong transaksyon, bilang kada-ibang transaksoyn, ay may
naka-ugnay na gastos. Ang gastos ay naka-base sa sukat ng bytecode at naniningil
sa checking validity ng module at on-chain storage.

Ang pag-deploy ng sarili ay hindi nagpapatupad
ng smart contract. Para mag-patupad, ang user ay dapat gumawa ng *instance* ng contract.

.. seealso::

   Tignan ang :ref:`contract-instances` para sa iba pang impormasyon.

.. _smart-contracts-on-chain-fil:

.. _smart-contracts-on-the-chain-fil:

.. _contract-on-chain-fil:

.. _contract-on-the-chain-fil:

Smart contract sa loob ng chain
===============================

Ang smart contract sa loob ng chain ay isang koleksyon ng functions na naka-export sa deployed
na module. Ang concrete mechanism na ginagamit para dito ay `Web Assembly`_ export
section. Ang smart contract ay dapat mag-export ng isang function para sa pag-initiliaze
ng bagong instance at maaring mag-export ng wala or maraming functions para sa pag-update ng instance.

Dahil ang smart contract module ay maaring mag-export ng functions para sa iba`t-ibang smart
contracts, inuugnay namen ang functions gamit ang naming scheme:

- ``init_<contract-name>``: Ang function para sa pag-initialize ng smart contract ay dapat
  nagsisimula sa ``init_`` na sinusundan ng pangalan ng smart contract. Ang contract
  ay dapat naglalaman lang dapat ng ASCII alphanumeric o mga bantas na charater, at hindi
  maaring magkaroon ng simbolo na ``.``.

- ``<contract-name>.<receive-function-name>``: Functions para sa pag-interact sa
  smart contract ay naka-paunang salita ng pangalan ng contract at sinusundan ng ``.`` at ang
  pangalan ng function. Ganoon rin para sa init functon, ang pangalan ng contract ay hindi pinapayagan
  na magkaroon ng ``.`` symbol.

.. note::

   Kapag nag-develop ka ng smart contracts gamit ang Rust at ``concordium-std``, ang
   procedural macros ``#[init(...)]`` at ``#[receive(...)]`` ay inaayos ang
   tamang naming scheme.

.. _Web Assembly: https://webassembly.org/
