.. _interact-instance:

=======================================
与智能合约实例互动
=======================================

本指南将向您展示如何与智能合约实例进行交互，这意味着触发可能会更新实例状态的接收功能。

制备
===========

确保您正在使用最新的  :ref:`Concordium software<downloads>`  运行一个节点 :ref:`running a node<run-a-node>` ，并且要在链上检查智能合约实例。

.. 也可以看看：：
   有关如何部署智能合约模块的信息，请参见：:ref:`deploy-module`  。
   如何创建实例：:ref:`initialize-contract`.

由于与智能合约的交互是交易，因此，您还应确保已 ``concordium-client`` 建立一个具有足够 GTU 的帐户来支付交易费用。

.. 注意::

   此事务的成本取决于发送给接收函数的参数的大小以及函数本身的复杂性。

相互作用
===========

要0使用无参数接收功能使用地址索引更新实例， ``my_receive`` 同时允许使用多达1000个能量，请运行以下命令：

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

如果成功，则输出应类似于以下内容：

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

以JSON格式传递参数
---------------------------------

如果提供了 :ref:`smart contract schema <contract-schema>` 作为文件或嵌入在模块中，则可以传递 JSON格式的参数。该模式用于将 JSON序列化为二进制。

.. 也可以看看：：

   :ref:`Read more about why and how to use smart contract schemas
   <contract-schema>`.

要 0 使用 ``my_parameter_receive`` 具有 ``my_parameter.json`` 格式参数文件的 receive 函数 ，通过地址索引更新实例，请运行以下命令：

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json

如果成功，则输出应类似于以下内容：

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

否则，将显示描述问题的错误。下一节将介绍常见错误。

..另

   请参见

   ：有关合同实例地址的更多信息，请参阅 :ref:`references-on-chain`.

   
.. 注意::

  如果以JSON格式提供的参数不符合架构中指定的类型，则将显示错误消息。例如：

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. 注意::

   如果给定的模块不包含嵌入式模式，则可以使用 ``--schema /path/to/schema.bin`` 参数提供它。
   
.. 注意::

   在更新期间，也可以使用 ``--amount AMOUNT`` 参数将GTU转移到合同中 。

以二进制格式传递参数
-----------------------------------

当以二进制格式传递参数时， 不需要 :ref:`contract schema <contract-schema>`。

要 0 使用 ``my_parameter_receive`` 带有 ``my_parameter.bin`` 二进制格式的参数文件的接收函数 ，通过地址索引更新实例，请运行以下命令：

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

如果成功，则输出应类似于以下内容：

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. 也可以看看：：

   有关如何在智能合约中使用参数的信息，请参阅
   :ref:`working-with-parameters`.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
