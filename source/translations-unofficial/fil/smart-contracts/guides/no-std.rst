.. _no-std-fil:

=======================
Pagbuo gamit ``no_std``
=======================

Itong gabay ay ipapakita kung paano paganahin ang ``no_std`` para sa rust smart contract,
may potensyal na bawasan ang laki ng resulta ng Wasm module ng iilang kilobytes.

Paghahanda
==========

Ang pag-compile ng ``concordium-std`` ng walang ``std`` feature ay kinakailangan gamit ang rust
nightly toolchain, na maaring ma-install gamit ang ``rustup``:

.. code-block:: console

   $rustup toolchain install nightly

Pag-setup ng module para sa ``no_std``
======================================

Ang ``concordium-std`` library ay nagpapakita ng ``std`` feature, na nagpapaganda sa paggamit ng 
rust standard library.
Itong feature ay gumagana by default.

Para hindi paganahin, maari mong i-disable and default feature para sa
``concordium-std`` sa loob ng dependencies ng iyong module.

.. code-block:: rust

   [dependencies]
   concordium-std = { version: "=0.2", default-features = false }

Para magawang mag-toggle sa mayroon or walang std, idagdag ang ``std`` sa iyong
sariling module, na nagpapagana sa ``std`` feature ng ``concordium-std``:

.. code-block:: rust

   [features]
   std = ["concordium-std/std"]

Ito ang mga halimbawa ng pag-setup ng smart contract, kung saan ang ``std`` sa bawat
smart contract module ay gumagana by default.

Pagbuo ng module
================

Para magamit ang nightly toolchain, idagdag ang ``+nightly`` pagkatapos ng
``cargo``:

.. code-block:: console

   $cargo +nightly concordium build

Kung gusto mong hindi paganahin ang default features ng iyong smart contract module,
maari mong ipasa ang extra arguments para sa ``cargo``:

.. code-block:: console

   $cargo +nightly concordium build -- --no-default-features
