.. _deploy-module-fil:

==================================
I-deploy ang smart contract module
==================================

Ang gabay na ito ay ipapakita sa iyo kung paano i-deploy ang smart contract module *on-chain* at
paano pangalanan ito.

Paghahanda
==========

Siguraduhin na ikaw ay :ref:`running a node<run-a-node>` gumagamit ng pinakabagong :ref:`Concordium software<downloads>` at
meron kang :ref:`smart-contract module<setup-tools>` na nakahanda ng i-deploy.

Dahil ang pagdedeploy ng isang smart contract module ay isinasagawa sa pamamaraan ng transaksyon,
kakailanganin mo rin na magkaroon ng ``concordium-client`` setup ng isang account na may
sapat na GTU para bayaran ang transaksyon.

.. note::

   Ang gastos ng trasaksyon ay nakadepende sa laki ng the smart contract
   module. ``concordium-client`` ay ipinapakita ang gastos at nagtatanong para sa kompirmasyon
   bago pa nito gawin ang ano mang uri ng transaksyon.

Pag-deploy
==========

Para i-deploy ang isang smart contract module ``my_module.wasm`` gamit ang account
na may pangalan account-name, patakbuhin ang sumusunod na command:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. note::

   The --sender option can be omitted if the account "default" is to be used. For brevity, we will do so in the following.

Kapag tagumpay, ang kinalabasan ay dapat katulad ng sumusunod:

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'.

Dapat tandaan ang module reference dahil ginagamit ito sa paggawa ng mga smart contract
instances.

.. seealso::

   Para sa gabay kung paano mag-initialize ng smart contracts mula sa na-deploy na na module tignan ang
   :ref:`initialize-contract`.

   Para sa mas marami pang inpormasyon patungkol sa mga module references, tignan ang :ref:`references-on-chain`.

.. _naming-a-module-fil:

Pagpapangalan sa module
=======================

Ang isang module ay pwedeng bigyan ng isang lokal na alyas, o *name*, para makilala ito ng mas
madali.
Ang pangalan ay nakalagay lang sa lokal sa pamamagitan ng ``concordium-client``, at hindi ito
makikita on-chain.

.. seealso::

   Para sa paliwanag kung paano at saan ang mga pangalan at iba pang mga lokal na settings
   nakalagay, tignan ang :ref:`local-settings`.

Para magdagdag ng pangalan habang nagde-deploy, ang ``--name`` parameter ay ginagamit.
Dito, pinapangalanan natin ang module na ``my_deployed_module``:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

Kapag matagumpay, ang kinalabasan ay dapat katulad ng sumusunod:

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

Ang mga module ay pwede ring pangalanan gamit ang ``name`` command.
Para pangalanan ang na-deploy na na module na may reference
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` as
``some_deployed_module``, patakbuhin ang sumusunod na command:
.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

Ang kinalabasan ay dapat katulad ng sumusunod:

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module'.
