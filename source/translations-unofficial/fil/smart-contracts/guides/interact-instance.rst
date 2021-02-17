.. _interact-instance-fil:

=======================================
Pag-interact sa smart contract instance
=======================================

Itong gabay ay magpapakita kung paano mag-interact sa smart contract instance, na
ibig sabihin ang pag-trigger ng receive function na maaring mag-update ang estado ng
instance.

Paghahanda
==========

Siguraduhin na ikaw nagawa mo ito :ref:`running a node<run-a-node>` gamit ang pinakabagong :ref:`Concordium software<downloads>` at mayroon kang
smart-contract instance on-chain para suriin.

.. seealso::
   Para sa kung paano mag-deploy ng smart contract module tignan ang :ref:`deploy-module` at para sa
   kung paano mag-create ng instance :ref:`initialize-contract`.

Dahil sa ang interaksyon sa smart contract ay mga transaksyon, dapat siguraduhin
mayroon kang ``concordium-client`` na naka-setup kasama ang account na may sapat na GTU na ipangbabayad
para sa mga transakyon.

.. note::

   Ang gastusin para sa transaksyon na ito ay nakadepende sa laki ng parameters na pinadala sa
   receive function at ang kumplikado ng sariling function.

Interaksyon
===========

Para mag-update ng instance na may address index na ``0`` gamit ang paramaterless
receive function ``my_receive`` habang pinapahintulutan ang higit sa 1000 na enerhiya na maaring magamit,
para patakbuhin ang sumusunod na command:

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

If successful, the output should be similar to the following:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

Pagpasa ng parameters sa JSON format
------------------------------------

Ang parameter sa JSON format ay maaring mapasa kung ang :ref:`smart contract schema
<contract-schema>` ay binigay, bilang file or embedded sa loob ng module.
Ang schema ay ginagamit para mag-serialize ng JSON sa binary.

.. seealso::

   :ref:`Read more about why and how to use smart contract schemas
   <contract-schema>`.

Para mag-update ng instance na may address index ``0`` gamit ang receive function
``my_parameter_receive`` na may parameter file na ``my_parameter.json`` sa JSON
format, patakbuhin ang sumusunod na command:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json

Kung gumana, ang kakalabasan dapat ay katulad ng sumusunod:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

Kung hindi, may error na pinapakita ang problema.
Madalas na errors ay ang nakadetalye sa susunod na seksyon.

.. seealso::

   Para sa karagdagang impormasyon tungkol sa contract instance addresses, tignan ang
   :ref:`references-on-chain`.

.. note::

   Kung ang parameter na binigay sa JSON format ay hindi sumunod sa uri
   ng sinasabing schema, ang mensahe ng error ay mapapakita. Halimbawa:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Kung ang binigay na module ay walang embedded schema, maari itong makuha
   gamit ang ``--schema /path/to/schema.bin`` parameter.

.. note::

   Ang GTU ay maari ding ilipat sa contract habang updates gamit ang
   ``--amount AMOUNT`` parameter.

Pagpasa ng parameters sa binaray format
---------------------------------------

Kapag magpapasa ng parameters sa binary formay, ang
:ref:`contract schema <contract-schema>` ay hindi kinakailangan.

Para mag-update ng instance na may address index ``0`` gamit ang receive function
``my_parameter_receive`` na may parameter file ``my_parameter.bin`` sa binary
format, patakbuhin ang sumusunod na command:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

Kung gumana, ang kakalabasan dapat ay parehas sa sumusunod:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. seealso::

   Para sa karagdagang impormasyon kung paano ito gumanaga na may parameters sa smart contract, tignan ang
   :ref:`working-with-parameters`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
