.. _initialize-contract-zh:

====================================
初始化智能合约实例
====================================

本指南将向您展示如何使用 JSON 或 二进制格式的参数从已部署的智能合约模块初始化智能合约。此外，它还将说明如何命名实例。

准备
===========

确保您正在使用最新的 :ref:`Concordium software<downloads>` 下载并运行节点 :ref:`running a node<run-a-node>` ，并确保您在部署了一个智能合约模块 :ref:`deployed <deploy-module-zh>` 。

由于初始化智能合约是一项交易，因此您还应确保已通过 ``concordium-client`` 建立一个具有足够 GTU 的账户来支付交易费用。

.. note::

   此交易的成本取决于发送给init函数的参数的大小以及函数本身的复杂性。

初始化
==============

要从已部署的模块(模块编号(module reference)为: ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` )中初始化无参数智能合约 ``my_contract`` 的实例，同时本交易只允许使用最多 1000 NRG，请运行以下命令：

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

如果成功，则输出应类似于以下内容：

.. todo::

   Update address format to ``<i, s>`` from ``{"index": i, "subindex": s}``
   (throughout the documentation).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

看到此输出，即意味着已使用上述显示的地址创建了一个新的链上合约实例。

.. 另请参阅：
   想更深入地了解合约初始化，请参见：:ref:`contract-instances-init-on-chain-zh` .

   有关模块引用和实例地址的更多信息，请参见 :ref:`references-on-chain` .

   直接使用模块引用可能很不方便，如要为它们命名，请参阅：:ref:`naming-a-module` .

.. _init-passing-parameter-json-zh:

传递JSON格式的参数
---------------------------------

如果提供了 :ref:`smart contract schema <contract-schema-zh>` (无论是schema文件还是schema嵌入在模块中)，则可以传递JSON格式的参数。该schema可用于将JSON序列化为二进制。

.. seealso::

   :ref:`Read more about why and how to use smart contract schemas <contract-schema-zh>`.

   :ref:`Parameters can be also passed in binary format <init-passing-parameter-bin-zh>`.

要使用JSON格式的参数文件 ``my_parameter.json`` 初始化 *模块(引用号: ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` )* 的合约 ``my_parameter_contract`` 的实例，请运行以下命令：

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

如果成功，则输出应类似于以下内容：

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

否则，将显示有关问题描述的错误。下一节将介绍常见错误。

.. note::

   如果以JSON格式提供的参数不符合schema中指定的类型，则将显示错误消息。例如：

.. code-block:: console

   Error: Could not decode parameters from file 'my_parameter.json' as JSON:
   Expected value of type "UInt64", but got: "hello".
   In field 'first_field'.
   In {
       "first_field": "hello",
       "second_field": 42
   }.

.. note::

   如果给定的模块不包含嵌入式schema，则可以使用 ``--schema /path/to/schema.bin`` 参数提供它。

.. note::

   GTU也可以在初始化合约实例时使用 ``--amount AMOUNT`` 参数转移到合同实例。


.. _init-passing-parameter-bin-zh:

以二进制格式传递参数
-----------------------------------

当以二进制格式传递参数时，就不需要合约schema :ref:`contract schema <contract-schema-zh>` 。

要使用二进制格式的参数文件 ``my_parameter.bin`` 来初始化 *模块(引用编号:9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2)中的合约my_parameter_contract* 的实例，请运行以下命令：

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


如果成功，则输出应类似于以下内容：

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. 另

   请参见
   ：有关如何在智能合约中使用参数的信息，请参阅 :ref:`working-with-parameters` .

.. _naming-an-instance-zh:

命名合约实例
==========================

可以为智能合约实例指定本地别名或 *name* ，这使得引用起来更容易。该名称仅由本地存储 ``concordium-client`` ，在链上不可见。

.. 另请参见：

   有关名称和其他本地设置的存储方式和位置的说明，请参见 :ref:`local-settings`.

要在初始化期间添加名称，请使用--name参数。

在这里，我们从模块(引用号: ``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` )中初始化合约 ``my_contract`` 并将实例命名为 ``my_named_contract`` ：

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


如果成功，则输出应类似于以下内容：

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

合同实例也可以使用以下 *name* 命令命名。要将地址索引号为0的实例命名为 ``my_named_contract`` ，请运行下面的命令：


.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

如果成功，则输出应类似于以下内容：

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. 另

   请参见
   ：有关合同实例地址的更多信息，请参阅 :ref:`references-on-chain`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
