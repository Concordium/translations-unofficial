.. _inspect-instance-fil:

==================================
Suriin ang smart contract instance
==================================

Itong gabay ay magpapakita kung paano suriin ang smart contract instance.
Sa pagsuri ng instance ay makikita ang pangalan, may-ari, module reference, balanse,
estado, at receive-functions:

Paghahanda
==========

Siguraduhin na ikaw ay :ref:`running a node<run-a-node>` gamit ang pinakabagong :ref:`Concordium software<downloads>` at mayroon kang
smart-contract instance on-chain para magsuri.

.. seealso::
   Para sa kung paano mag-deploy ng smart contract module tignan ang :ref:`deploy-module` at para sa
   kung paano gumawa ng instance :ref:`initialize-contract`.

Inspeksyon
==========

Para mag-inspeksyon o ipakit ang impormasyon tungkol sa smart contract instance na mayroong
address index na ``0``, patakbuhin ang sumusunod na command:

.. code-block:: console

   $concordium-client contract show 0

The output should be similar to the following:

.. code-block:: console

   Contract:        my_contract
   Owner:           '4Lh8CPhbL2XEn55RMjKii2XCXngdAC7wRLL2CNjq33EG9TiWxj' (default)
   ModuleReference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'
   Balance:         0.000000 GTU
   State:
       {
           "first_field": 0,
           "second_field": 42
       }
   Functions:
    - receive_one
    - receive_two

.. seealso::

   Para sa karagradang impormasyon tungkol sa contract instance address, tignan ang
   :ref:`references-on-chain`.

Ang lebel ng detalye ng inspeksyon ay nakadepende kung ang ``show`` commnad ay may
access sa :ref:`contract schema <contract-schema>`.
Kung ang schema ay naka-embed, ito ay magagamit ng hindi direkta.
Kung hindi man, ang schema ay maaring maibigay gamit ang ``--schema /path/to/schema.bin``
parameter.

.. note::

   Ang schema file na binigay gamit ang ``--schema`` parameter ay may karapatan sa pangunguna
   sa embedded schema.

.. seealso::

   :ref:`Read more about why and how to use smart contract schemas <contract-schema>`.
