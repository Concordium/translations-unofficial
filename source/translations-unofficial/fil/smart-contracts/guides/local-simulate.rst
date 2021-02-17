.. _local-simulate-fil:

==============================================
I-simulate ang mga contract functions sa lokal
==============================================

Ang gabay na ito ay patungkol sa kung paano mag-simulate sa local ang invocation ng ibang init o
ng receive function mula sa Wasm smart contract module sa binigay na kontexto at
estado.
Itong simulation na ito ay mahalaga sa pag-inspect ng isang smart contract at ang kinalabasan nito
sa partikular na mga pagkakataon.

.. seealso::

   Para sa gabay sa awtomatikong unit tests, tignan ang :ref:`unit-test-contract`.

Paghahanda
==========

Siguraduhin na meron kang naka-install na ``cargo-concordium``, kung wala sundan mo ang gabay na
:ref:`setup-tools`.
Kakailanganin mo rin ang isang smart contract module sa Wasm para i-simulate.

.. todo::

   Isulat ang mga sumusunod, kapag ang schema stuff ay nakapwesto na.

Pag-simulate ng instantiation
=============================

Para i-simulate ang instantiation ng isang smart contract instance gamit ang
``cargo-concordium``, patakbuhin ang sumusunod na command:

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` (used with the ``--context`` parameter) ay isang file na
naglalaman ng context information tulad ng ano ang kasalukuyang kalagayan ng chain, ang
tagapagpadala ng transaksyon, at anong account ang i-invoke para sa function na ito.
Ang halimbawa nito ay pwedeng:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "initOrigin": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
   }

.. seealso::

   Para sa reference ng context tignan ang :ref:`simulate-context`.


Pag-simulate ng mga updates
===========================

Para ma-simulate ang update sa isang contract smart contract instance using
``cargo-concordium``, run:

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin

``receive-context.json`` (ginagamit kasama ang ``--context`` parameter) ay isang file na
naglalaman ng context information tulad ng ano ang kasalukuyang kalagayan ng chain, ang 
tagapagpadala ng transaksyon, at anong account ang i-invoke para sa function na ito.
Ang halimbawa nito ay pwedeng:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "invoker": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "selfAddress": {"index": 0, "subindex": 0},
       "selfBalance": "0",
       "sender": {
           "type": "account",
           "address": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
       },
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
       "owner": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
   }

.. seealso::

   Para sa reference ng kontexto tignan ang :ref:`simulate-context`.
