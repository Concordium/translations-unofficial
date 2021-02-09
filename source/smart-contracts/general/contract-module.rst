.. _contract-module:

======================
Ang Smart contract modules
======================

Ang mga Smart contracts ay naka-deploy sa chain ng * smart contract modules *

.. tandaan::

   Ang isang smart contract module ay madalas na tinutukoy bilang isang * module *.

Ang isang module ay maaaring maglaman ng isa o higit pang mga smart contracts, pinapayagan  na maibahagi ang code sa  mga kabilang sa kontrata at 
maaaring opsyonal na maglaman ng: ref: `mga contract schema 
<contract-schema>`.

.. graphviz::
   :align: center
   :caption: A smart contract module containing two smart contracts.

   digraph G {
       subgraph cluster_0 {
           node [fillcolor=white, shape=note]
           label = "Module";
           "Crowdfunding";
           "Escrow";
       }
   }

Ang module ay dapat na may sarili, at mayroon lamang isang pinaghihigpitang listahan ng mga pag-importsna 
nagbibigay-daan para sa pakikipag-interact sa chain.
Ang mga ito ay ibinibigay ng host environment at magagamit para sa smart contract sa pamamagitan ng pag-import ng isang 
module na pinangalanang "concordium".

.. tingnan::

   Check out :ref:`host-functions` for a complete reference.

On-chain language
=================

Sa Concordium blockchain ang smart contract language ay isang subset ng Web 
Assembly`_ ( Wasm) na idinisenyo upang maging isang portable compilation 
target at patatakbuhin sa mga sandboxed environments. Kapaki-pakinabang ito sapagkat ang smart 
contracts ay pinapatakbo ng mga baker sa network na hindi kinakailangang 
magtiwala sa code.

Ang Wasm ay isang low-level-language at hindi praktikal na isulat sa pamamagitan ng kamay. Sa halip ay maaaring isulat ang isang smart contracts 
sa isang more high-level language na 
kino-compile sa Wasm.

.. _mga-limitasyon-wasm:

Limitasyon
-----------

.. mgagagawin::

   Magdagdag ng iba pang mga limitasyon, tulad ng mga seksyon ng pagsisimula ...

Ang blockchain environment ay napaka partikular sa kahulugan na ang bawat node ay dapat
maipatupad ang contract sa eksaktong parehong paraan, gamit ang parehong dami nang pinagkukunan.
Kung hindi man ay bigo itong maabot ang pinagkasunduang estado ng chain. Sa kadahilanang ang smart 
contracts ay kailangang isulat sa isang 
pinaghihigpitang subset ng Wasm.

Floating point numbers
^^^^^^^^^^^^^^^^^^^^^^

Bagaman mayroong suporta si Wasm para sa mga floating point numbers, ang smart contract ay hindi 
pinapayagang gamitin ang mga ito. Ang dahilan nito ay ang Wasm floating-point numbers ay maaaring magkaroon ng isang espesyal na "NaN" 
("hindi isang numero") na value na nagre-resulta sa nondeterminism.

Ang paghihigpit ay nalalapat statiscally, nangangahulugang hindi naglalaman ang mga smart contracts ng mga uri ng mga floating points, 
at hindi rin sila maaaring maglaman ng anumang mga tagubilin na nasasangkot ang 
floating point values.


Deployment
==========

Ang pag-dedeploy ng isang module sa chain ay nangangahulugang pagsusumite ng module bytecode bilang isang 
transaksyon sa network ng Concordium. Kung * wasto * ang transaksyong ito isinasama ito sa 
isang block.. Ang transaksyong ito, tulad ng bawat iba pang transaksyon, ay mayroong nauugnay na gastos. 
Ang gastos ay batay sa laki ng bytecode at magkakaroon nang charge para sa parehong 
pag-check ng bisa ng module at on-chain storage.

Ang pag-deploy mismo ay hindi nag e-execute
nang smart contract. Upang ma execute, ang isang user ay dapat munang lumikha ng isang * instance * ng contract.

.. tingnandin::

   Tingnan :ref:`contract-instances` para sa karagdagang impormasyon.

.. _smart-contracts-on-chain:

.. _smart-contracts-on-the-chain:

.. _contract-on-chain:

.. _contract-on-the-chain:

Smart contract on the chain
===========================

Ang isang smart contract sa chain ay isang koleksyon ng mga functions na na e-export mula sa isang na-deploy na 
module. Ang konkretong mekanismo na ginamit para dito ay ang `Web Assembly`_ export 
seksyon.. Ang isang smart contract ay dapat na mag-export ng isang functio para sa pagsisimula 
ng mga bagong instances at maaaring mag-export ng zero o higit pang mga functions para sa pag-update ng instance..

Dahil ang isang module ng smart contract ay maaaring mag-export ng mga functions para sa magkakaibang mga smart contracts,
naiuugnay namin ang mga functions gamit ang naming scheme:

- ``init_<contract-name>``: Ang function para sa pagsisimula ng isang smart contract  ay dapat magsimula sa "init_" 
   na sinusundan ng isang pangalan ng smart contract. Ang contract ay dapat na binubuo lamang ng ASCII alphanumeric o 
   bantas na mga character, at hindi pinapayagan na maglaman ng 
   "." simbolo.

- ``<contract-name>.<receive-function-name>``: Ang mga functions para sa pakikipag-ugnay sa isang 
smart contract ay pauna sa pangalan ng contract, na sinusundan ng isang "." 
At pangalan para sa function. Kapareho sa init function, hindi pinapayagan ang pangalan ng 
kontrata na maglaman ng simbolong "."

.. tandaan::

   Kung gagawa ka ng smart contracts gamit ang Rust at "concordium-std", ang 
   pamaraan macros ``#[init(...)]`` and ``# [receive(...)`` 
   i-set up ang tamang scheme ng pagbibigay ng pangalan.

.. _Web Assembly: https://webassembly.org/
