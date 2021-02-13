.. _inspect-instance:

=================================
检查智能合约实例
=================================

本指南将向您展示如何检查智能合约实例。检查实例将向您显示其名称，所有者，模块引用，余额，状态和接收功能：

制备
===========

确保您正在使用最新的 :ref:`Concordium software<downloads>` 运行一个节点 :ref:`running a node<run-a-node>`，并且要在链上检查智能合约实例。

.. 另
   请参阅：有关如何部署智能合约模块的信息，请参见：部署模块，以及
   如何创建实例的初始化协议。

检查
==========

要检查或显示有关具有地址索引的智能合约实例的信息0，请运行以下命令：

.. code-block:: console

   $concordium-client contract show 0

输出应类似于以下内容：

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

.. 另

   请参见
   ：有关合同实例地址的更多信息，请参阅 :ref:`references-on-chain` 。


检查的详细程度取决于 ``show`` 命令是否可以访问 :ref:`contract schema <contract-schema>` 。如果架构是嵌入式的，则将隐式使用它。否则，可以使用 ``--schema /path/to/schema.bin``  参数提供架构。

.. 注意::

   使用 ``--schema`` 参数提供的模式文件将优先于嵌入式模式。

.. 另::

   :ref:`Read more about why and how to use smart contract schemas <contract-schema>` .
