.. _deploy-module-zh:

==============================
部署智能合约模块(module)
==============================

本指南将向您展示如何 *在链上* 部署智能合约模块及其命名方式。

准备
===========

确保您正在使用最新的 :ref:`Concordium software<downloads>` 下载并运行节点 :ref:`running a node<run-a-node-zh>` ，并且准备好需要部署的 :ref:`smart-contract module<setup-tools-zh>` 。

由于部署智能合约模块是以交易的形式完成的，因此您还需要使用 ``concordium-client`` 设置一个具有足够GTU的帐户来支付交易费用。

.. note::

   交易成本取决于智能合约模块的大小。 concordium-client 会显示费用并在执行任何交易之前要求你确认。

部署
==========

要使用名称为account_name的帐户部署智能合约模块 ``my_module.wasm`` ，请运行以下命令：

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. note::

   如果要使用帐户 “default” ，则可以省略 --sender 选项。为简便起见，我们将在下面进行说明。

如果部署成功，则输出内容应类似于以下：

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'.

记下这个模块参考号(module reference),创建智能合约实例时需要使用它。

.. 请参阅
   ：有关如何从已部署的模块初始化智能合约的指南，请参见：:ref:`initialize-contract-zh` .

   有关模块引用的更多信息，请参见 :ref:`references-on-chain` .


.. _naming-a-module-zh:

给合约模块命名
===============

可以给模块一个本地别名 或 *名称*，这使得引用起来更容易。该名称仅由本地 ``concordium-client`` 存储，在链上不可见。

.. 另请参见：

   有关名称和其他本地设置的存储方式和位置的说明，请参见 :ref:`local-settings` .

要在部署期间添加名称，请使用 ``--name`` 参数。在这里，我们为模块命名 ``my_deployed_module`` ：

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

如果成功，则输出类似于以下内容：

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

也可以使用 ``name`` 命令来命名模块。要将引用的已部署模块命名 ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` 为 ``some_deployed_module`` ，请运行以下命令：

.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

输出应类似于以下内容：

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module' .
