.. _list of types implementing the SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schema-fil:

=========================
Gumawa ng contract schema
=========================

Itong guide na ito ay ipapakita sa iyo kung paano gumawa ng smart contract schema, paano i-export ito
sa isang file, at/o i-embed ant schema sa smart contract moduel, gamit ang
``cargo-concordium``.

Paghahanda
==========

Una, siguraduhin na meron kang naka-install na ``cargo-concordium`` at kung wala naman ang guide na
:ref:`setup-tools` ang tutulong sayo.

Kailangan rin natin ng Rust source code ng smart contract na gusto mong gawan
ng schema.

Mag-setup ng contract para sa schema
====================================

Para makagawa ng contract schema, kailangan muna nating ihanda ang ating smart
contract para sa paggawa ng schema.

Pwede nating piliin kung anong mga parte ng ating smart contract ang isasama sa schema.
Ang mga pagpipilian ay ang isama ang schema para sa contract state, at/o para sa bawat
parameters ng init at receive funtions.

Ang bawat type na gusto nating isama sa schema ay dapat gawin ang ``SchemaType``
na katangian. Ito ay gawa na para sa lahat ng klase ng base types at iba pang mga types (see `list of types implementing the SchemaType`_).
Para sa karamihan ng iba pang mga kaso, maaari din itong awtomatiko na makamit, gamit ang
``#[derive(SchemaType)]``::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

Ang pagsasagawa ng ``SchemaType`` trait ng mano-mano ay nangangailangan lamang ng pagtukoy ng isang
function, at ayun ay ang getter para sa ``schema::Type``, na mahalagang naglalarawan
kung paano ang uri na ito ay kinakatawan bilang mga byte at kung paano ito kinakatawan.

.. todo::

   Gumawa ng halimbawa kung paanong mano-manong isinasagawa ang ``SchemaType`` at i-link
   ito mula dito.

Pagsasama ng contract state
---------------------------

Upang makabuo at magsama ng schema para sa contract state, isinalaysay namin ang uri
kasama ang ``#[contract_state(contract = ...)]`` macro::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

O para mas simple kung ang contract state ay tipo ng naisagawa na ``SchemaType``, e.g., u32::

   #[contract_state(contract = "my_contract")]
   type State = u32;

Pagsasama ng function parameters
--------------------------------

Upang makabuo at isama ang schema para sa mga parameter para sa init at
receive functions, itinakda namin ang opsyonal ``parameter`` katangian para sa
``#[init(..)]``- and ``#[receive(..)]``-macro::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }

Pagbuo ng schema
================

Ngayon, tayo ay handa na bumuo ng akwal na schema gamit ang ``cargo-concordium``, at tayo ay
merong pagpipilian na i-embed ang schema at/o isulat ang schema sa isang file.

.. seealso::

   Para sa marami pang pagpipilian, tignan ang
   :ref:`here<contract-schema-which-to-choose>`.

Ang pag-embed sa schema
-----------------------

Para ma-embed natin ang schema sa smart contract module, magdadagdag tayo ng
``--schema-embed`` para buoin ang command

.. code-block:: console

   $cargo concordium build --schema-embed

Kung matagumpay ang kinalabasan ng command, sasabihan ka nito ng kabuoang laki ng
schema sa bytes.

Pagpapalabas sa schema file
---------------------------

Para ipalabas ang schema sa isang file, pwede nating gamitin ang ``--schema-out=FILE``
kung saan ang ``FILE`` ay isang path na kung saan gagawa:

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
