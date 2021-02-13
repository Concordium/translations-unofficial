.. _contract-instances:

========================
智能合约实例
========================

.. todo::

  - 阐明与智能合约相关的实例如何与模块相关（例如，现在它说一个实例是模块+状态，但是下面的图片显示的实例为contracts+state）。
  - 决定我们应该如何准确地定义智能合约模块（作为它自己的概念或者作为Wasm模块），以及我们是否应该讨论它们。
  - 决定我们是否应该有一个具体的代码示例，以及它是否应该处于Wasm、Rust或伪代码中。
  - 考虑用一幅图来解释模块和实例之间的关系。

**智能合约实例** 本质是保存着特定状态和GTU令牌量的智能合约模块。我们可以基于同一模块创建多个智能合约实例。例如，对于 :ref:`auction<auction>` 合同，可以有多个实例，每个实例都专门用于对特定项目进行投标，并有各自的参与者。

可以通过调用智能合约模块中请求的函数的 *init* 交易部署的 :ref:`Smart contract module<contract module>` 创建智能合约实例。此函数可以接受一个参数。它的最终结果必须是实例的初始智能合约状态。

.. note::

   智能合约实例通常简称为*实例*。

.. graphviz::
   :align: center
   :caption: Example of smart contract module containing two smart contracts:
             Escrow and Crowdfunding. Each contract has two instances.

   digraph G {
       rankdir="BT"

       subgraph cluster_0 {
           label = "Module";
           labelloc=b;
           node [fillcolor=white, shape=note]
           "Crowdfunding";
           "Escrow";
       }

       subgraph cluster_1 {
           label = "Instances";
           style=dotted;
           node [shape=box, style=rounded]
           House;
           Car;
           Gadget;
           Boardgame;
       }

       House:n -> Escrow;
       Car:n -> Escrow;
       Gadget:n -> Crowdfunding;
       Boardgame:n -> Crowdfunding;
   }
   
智能合约实例的状态
==================================

智能合约实例的状态由两部分组成，用户定义的状态和合约持有的GTU数量，即其余额。当提到状态时，我们通常只指用户定义的状态，而没包括GTU金额的原因是，GTU只能根据网络规则使用和接收，例如，智能合约不能创建或销毁GTU令牌。

.. _contract-instances-init-on-chain:

实例化一个链上合约
=======================================

每个智能合约都必须包含一个用于创建智能合约实例的函数。这样的函数称为init函数。

要创建智能合约实例，帐户将发送一个特殊交易，其中包含对已部署智能合约模块的引用和用于实例化的init函数的名称。

该交易还可以包括GTU的金额，该金额被添加到智能合约实例的余额中。函数的参数以字节数组的形式作为交易的一部分提供。

总而言之，交易包括：

- 智能合约模块的引用
- init函数的名称
- init函数的参数
- 实例的GTU数量

init函数也可以表明它不希望使用这些参数创建新实例。如果init函数接受这些参数，它将设置实例的初始状态及其余额。实例在链上被赋予一个地址，发送交易的帐户成为实例的所有者。如果函数拒绝这些参数，则不会创建任何实例，并且只有这笔尝试创建实例的交易在链上可见。

.. seealso::

   See :ref:`initialize-contract` guide for how to initialize a
   contract in practice.

合约实例的状态
==============

每个智能合约实例都有自己的状态，在链上表现为字节数组。合约实例使用主机环境提供的函数来读取、写入和调整状态大小。

.. seealso::

   See :ref:`host-functions-state` for a reference of these functions.

智能合约状态的大小是有限的。目前智能合约状态的限制是16KiB。

.. seealso::

   Check out :ref:`resource-accounting` for more on this.

和一个合约实例交互
============================

智能合约可以暴露出零个或多个与实例交互的函数，称为 *receive* 函数。

与init函数一样，receive函数是通过交易触发的，交易包含合约的一些GTU和字节形式的函数参数。

总之，一个智能合约交互的交易包括：

- 智能合约实例的地址
- receive函数的名称
- receive函数的参数
- 实例的GTU数量

.. _contract-instance-actions:

日志记录事件
==============

.. todo::

   Explain what events are and why they are useful.
   Rephrase/clarify "monitor for events".

可以在执行智能合约功能期间记录事件。init和receive函数都是这样。日志是为链外使用而设计的，因此链外的参与者可以监视事件并对其作出反应。智能合约或链上的任何其他参与者都无法访问日志。可以使用主机环境提供的函数记录事件。

.. seealso::

   See :ref:`host-functions-log` for the reference of this function.

这些事件日志由bakers保留并包含在交易摘要中。

记录事件有一个相关的成本，类似于写入合约状态的成本。在大多数情况下，只有记录几个字节才能降低成本。

.. _action-descriptions:

操作描述
===================

receive函数返回要由链上的主机环境执行的操作的描述。

合同可能产生的行为有：

- **Accept** 是一个总是成功的原始操作
- GTU从实例到指定帐户的 **Simple transfer** 
- **Send** ：调用指定智能合约实例的receive函数，可以选择将一些GTU从发送实例转移到接收实例。

如果某个操作未能执行，则会还原receive函数，使实例的状态和余额保持不变。然而，
- 触发（不成功的）receive函数的交易仍然添加到链中，
- 并且交易成本，包括执行失败操作的成本，还是会从发送帐户中扣除。

处理多个操作描述
---------------------------------------

可以使用 *和* 组合器来链接操作描述。动作描述序列 ``A`` **和** ``B`` 

1) 执行 ``A``
2) 如果 ``A`` 成功，则执行 ``B`` 
3) 如果 ``B`` 失败，则整个操作序列失败（并且 ``A`` 的结果被还原）

处理错误
---------------

如果前一个操作失败，请使用 ``或`` 组合器执行操作。动作描述 ``A`` 或 ``B`` 

1) 执行 ``A`` 
2) 如果成功，则停止执行
3) 如果 ``A`` 失败，则执行 ``B`` 

.. graphviz::
   :align: center
   :caption: Example of an action description, which tries to transfer to Alice
             and then Bob, if any of these fails, it will try to transfer to
             Charlie instead.

   digraph G {
       node [color=transparent]
       or1 [label = "Or"];
       and1 [label = "And"];
       transA [label = "Transfer x to Alice"];
       transB [label = "Transfer y to Bob"];
       transC [label = "Transfer z to Charlie"];

       or1 -> and1;
       and1 -> transA;
       and1 -> transB;
       or1 -> transC;
   }

.. seealso::

   See :ref:`host-functions-actions` for a reference of how to create the
   actions.

整个操作树是 **原子** 执行的，要么导致所有相关实例和帐户的更新，要么在拒绝的情况下导致执行付款，但没有其他更改。发送发起交易的帐户支付整个树的执行费用。

