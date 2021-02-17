.. _initialize-contract-fil:

=========================================
Pag-initialize ng smart contract instance
=========================================

Ang guide na ito ay ipapakita sa inyo kung paano mag-initialize ng isang smart contract mula sa isang na-deploy na na
smart contract module na may mga parameters sa JSON o sa binary format.
Karagdagan, ipapakita rin dito kung paano papangalanan ang isang instance.

Paghahanda
==========

Siguraduhin na ikaw ay :ref:`running a node<run-a-node>` gamit ang pinakabagong :ref:`Concordium software<downloads>` at meron ka ng smart
contract :ref:`deployed <deploy-module>` sa ibang module na on-chain.

Dahil ang pagdedeploy ng isang smart contract module ay isinasagawa sa pamamaraan ng transaksyon,
kakailanganin mo rin na magkaroon ng ``concordium-client`` setup ng isang account na may
sapat na GTU para bayaran ang transaksyon.

.. note::

   Ang gastos ng trasaksyon ay nakadepende sa laki ng the smart contract
   module. ``concordium-client`` ay ipinapakita ang gastos at nagtatanong para sa kompirmasyon
   bago pa nito gawin ang ano mang uri ng transaksyon.

Pag-initialize
==============

Upang ma-initialize ang isang instance ng parameterless smart contract ``my_contract``
mula sa na-deloy na na module na may reference na
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` habang
pinapayagan hanggang 1000 NRG na magamit, ipatakbo ang
sumusunod na command:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

Kapag matagumpay, ang kinalabasan ay dapat katulad ng sumusunod:

.. todo::

   Update address format to ``<i, s>`` from ``{"index": i, "subindex": s}``
   (throughout the documentation).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Pag nakita mo ang mensahe na ito ay ibig sabihin ay may bago ka ng on-chain contract instance na nagawa
na pinapakita ang address.

.. seealso::

   Para magkaroon ng mas malalim pa na pagkakaintindi patungkol sa contract initialization, tignan ang
   :ref:`contract-instances-init-on-chain`.

   Para sa mas marami pang inpormasyon patungkol sa module references at mga instance addresses,
   tignan ang :ref:`references-on-chain`.

   Gamit ang mga module references ng direkta ay madali lang; para pangalanan sila, tignan ang
   :ref:`naming-a-module`.

.. _init-passing-parameter-json-fil:

Pag-pasa ng mga parameters sa JSON format
-----------------------------------------

Ang isang parameter sa JSON format ay pwedeng pumasa kung ang isang :ref:`smart contract schema
<contract-schema>` ay sinuplayan, pwedeng file o pwedeng naka-embed sa module.
Ang schema ay ginagamit para i-serialize ang JSON sa binary.

.. seealso::

   :ref:`Read more about why and how to use smart contract schemas <contract-schema>`.

   :ref:`Parameters can be also passed in binary format <init-passing-parameter-bin>`.

Para mag-initialize ng isang instance ng contract ``my_parameter_contract`` mula sa
module na may reference
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` na may
parameter file ``my_parameter.json`` sa JSON format, patakbuhin ang sumusunod na command:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

Kapag matagumpay, ang kinalabasan ay dapat katulad nito:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Kung hindi naman, isang error na nagpapaliwanag ng problema ang makikita.
Ang mga karaniwang pagkakamali ay ipapaliwanag sa susunod na seksyon.

.. note::

   Kung ang parameter na ibinigay sa JSON format ay hindi sumangayon sa tipo na
   tinutukoy sa schema, isang error message ang makikita. Halimbawa:

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   Kung ang binigay na module ay hindi naglalaman ng isang embedded schema, pwede itong suplayan
   gamit ang ``--schema /path/to/schema.bin`` parameter.

.. note::

   Ang GTU ay pwedeng mailipat sa isang contract instance habang nag-initialize pa
   gamit ang ``--amount AMOUNT`` parameter.


.. _init-passing-parameter-bin-fil:

Pagpasa ng mga parameter sa binary format
-----------------------------------------

Kapag ipapasa ang parameters sa binary format, ang :ref:`contract schema
<contract-schema>` hindi na kinakailangan.

Para ma-initialize ang instance ng contract ``my_parameter_contract`` mula sa
module na may reference
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` na may
parameter file ``my_parameter.bin`` sa binary format, patakbuhin ang sumusunod na command:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


Kapag matagumpay, ang kinalabasan ay dapat katulad nito:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. seealso::

   For information on how to work with parameters in smart contracts, see
   :ref:`working-with-parameters`.

.. _naming-an-instance-fil:

Pagpapangalan sa contract instance
==================================

Ang isang module ay pwedeng bigyan ng isang lokal na alyas, o *name*, para makilala ito ng mas
madali.
Ang pangalan ay nakalagay lang sa lokal sa pamamagitan ng ``concordium-client``, at hindi ito
makikita on-chain.

.. seealso::

   Para sa paliwanag kung paano at saan ang mga pangalan at iba pang mga lokal na settings
   nakalagay, tignan ang :ref:`local-settings`.

Para magdagdag ng pangalan habang nagde-deploy, ang ``--name`` parameter ay ginagamit.

Dito, tayo ay nag-initialize ng contract ``my_contract`` mula sa deployed module
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` at papangalanan
ito na ``my_named_contract``:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


Kapag matagumpay, ang kinalabasan ay dapat katulad nito:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

Ang mga contract instances ay pweder ing pangalanan gamit ang ``name`` command.
Para pangalanan ang isang instance na may address index ``0`` bilang ``my_named_contract``, patakbuhin ang
sumusunod na command:

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

Kapag matagumpay, ang kinalabasan ay dapat katulad nito:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. seealso::

   Para sa mas maraming inpormasyon patungkol sa mga contract instance addresses, tignan ang
   :ref:`references-on-chain`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
