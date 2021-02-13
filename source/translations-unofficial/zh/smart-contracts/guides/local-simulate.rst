.. _local-simulate:

===================================
本地模拟合约功能
===================================

本指南介绍如何在给定的上下文 和 状态下本地模拟某些初始化或从Wasm智能合约模块接收功能。该仿真对于检查智能合约和特定情况下的结果很有用。

.. 也可以看看：：

   有关自动单元测试的指南，请参阅： :ref:`unit-test-contract`.

准备
===========

 ``cargo-concordium`` 如果没有安装，请确保已安装指南 :ref:`setup-tools` 。您还将需要Wasm中的智能合约模块来进行仿真。

.. todo::

   Write the rest, when the schema stuff is in place.
   
模拟实例化
========================

要使用来模拟智能合约实例的实例 ``cargo-concordium`` ，请运行以下命令：

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` （与 ``--context`` 参数一起使用）是一个文件，其中包含上下文信息，例如链的当前状态，事务的发送者以及哪个帐户调用了此功能。这种情况的一个示例可能是：

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

.. 也可以看看：：

   有关上下文的参考，请参见 :ref:`simulate-context`.


模拟更新
==================

要使用来模拟对合同智能合约实例的更新 ``cargo-concordium`` ，请运行：

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin

``receive-context.json`` （与 ``--context`` 参数一起使用）是一个文件，其中包含上下文信息，例如链的当前状态，事务的发送者，调用此功能的帐户以及发送当前消息的帐户或地址。这种情况的一个示例可能是：

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

.. 也可以看看：：

   有关上下文的参考，请参见  :ref:`simulate-context`.
