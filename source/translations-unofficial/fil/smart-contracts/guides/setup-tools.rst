.. _setup-tools-fil:

===================================
Pag-install ng gamit sa development
===================================

Bago tayo magsimula sa pag-develop ng smart contracts, dapat nating i-setup ang
environment.

Rust at Cargo
==============

Una, `install rustup`_, na mag-iinstall ng Rust at Cargo sa iyong
machine.
Tapos gamitin ang ``rustup`` para mag-install ng Wasm target, na gagamitin para sa compilation:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium ay ang ginagamit para sa pag-develop ng smart contracts para sa Concordium
blockchain.
Maari din magamit para sa :ref:`compiling<compile-module>` at
:ref:`testing<unit-test-contract>` smart contracts, at nagpapagana ng features tulad ng
:ref:`building contract schemas<build-schema>`.

.. todo::

   Magdagdag ng links para sa testing at schemas.

Cargo Concordium ay ipinamamahagi bilang parte ng :ref:`Concordium software<downloads>` package.
Ang tool ay dapat nakalagay sa iyong PATH.

Para sa detalye kung paano gamitin ang Cargo Concordium patakbuhin ito:

.. code-block:: console

   $cargo concordium --help

Concordium software
===================

Ang tool para mag-deploy at mag-interact sa smart contract ay
:ref:`concordium-client<concordium_client>`. Ito ay ipinamamahagi bilang parte ng
:ref:`Concordium software<downloads>` package.

.. note::

   Para mag-deploy ng smart contract modules at mag-interact sa chain, siguraduhin
   na nagawa mo to :ref:`running a node<run-a-node>`.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _install rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
