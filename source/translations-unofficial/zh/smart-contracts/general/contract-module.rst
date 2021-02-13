.. _contract-module:

======================
智能合约模块
======================

智能合约部署在链上的 *智能合约模块* 里.

.. note::

   一个智能合同模块通常只是简单地称为 *模块* .

一个模块可以包含一个或多个智能合约，从而允许代码在合约之间共享，并且可以选择包含 :ref:`contract schemas <contract-schema>` .

.. graphviz::
   :align: center
   :caption: 一个包含了２个智能合约的模块.

   digraph G {
       subgraph cluster_0 {
           node [fillcolor=white, shape=note]
           label = "Module";
           "Crowdfunding";
           "Escrow";
       }
   }

模块必须是自包含的，并且只有一个允许与链交互的受限导入列表。这些由主机环境提供，通过导入名为 ``concordium`` 的模块可用于智能合约。

.. seealso::

   完整内容请参考 :ref:`host-functions` .

链上语言
=================

在Concordium区块链上，智能合约语言是　`Web Assembly`_ (简称Wasm)的一个子集，它被设计成一个可移植的编译对象，并在沙盒环境中运行。这很有用，因为智能合约将由网络中不一定信任代码的面包师们(bakers)运行。

Wasm是一种低级语言，用手写是不切实际的。相反，人们可以用更高级的语言编写智能合约，然后编译成Wasm。

.. _wasm-limitations:

Limitations
-----------

.. todo::

   Add other limitations, such as start sections...

区块链环境非常特殊，每个节点必须能够以完全相同的方式执行合同，使用完全相同的资源量。否则节点将无法就链的状态达成共识。因此，智能合约需要在Wasm的一个受限子集中编写。

浮点数
^^^^^^^^^^^^^^^^^^^^^^


虽然Wasm确实支持浮点数，但是智能合约不允许使用它们。原因是Wasm浮点数可以有一个特殊的``NaN``（“非数字”）值，它的处理会导致不确定性。

这个不支持浮点数的限制在区块链中被全局定死了，这意味着智能合约不能包含浮点类型，也不能包含任何涉及浮点值的指令。

部署
==========

将模块部署到链意味着将模块字节码作为交易提交到Concordium网络。如果 *有效*，此交易将包含在块中。与其他交易一样，此交易也有相关的成本。成本是基于字节码的大小，并收取检查模块的有效性和链上存储的费用。

部署本身并不执行智能合约。要执行，用户必须首先创建契约的 *实例* 。

.. seealso::

   See :ref:`contract-instances` for more information.

.. _smart-contracts-on-chain:

.. _smart-contracts-on-the-chain:

.. _contract-on-chain:

.. _contract-on-the-chain:

链上的智能合约
===========================

链上的智能合约是从已部署模块导出的函数集合。用于此操作的具体机制是 `WebAssembly`_ 导出部分。智能合约必须导出一个用于初始化新实例的函数，并且可以导出零个或多个用于更新实例的函数。

由于智能合约模块可以导出多个不同智能合约的函数，因此我们使用命名方案将函数关联起来：

- ``init_<contract-name>`` : 初始化智能合约的函数必须以 ``init`` 开头，后跟智能合约的名称。合同只能由ASCII字母数字或标点符号组成，不允许包含.符号。

- ``<contract-name>.<receive-function-name>`` : 用于与智能合约交互的函数以合约名称作为前缀，后跟 ``.`` 和函数的名称。与init函数相同，协定名称不允许包含 ``.`` 符号。

.. note::
   
   如果您使用Rust和concordium-std开发智能合约，则程序宏 ``#[init(...)]`` 和 ``#[init(...)]`` 会设置正确的命名方案。

.. _Web Assembly: https://webassembly.org/
