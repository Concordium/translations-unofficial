.. _unit-test-contract:

============================
对Rust合约进行单元测试
============================

本指南将向您展示如何为用Rust编写的智能合约编写单元测试。有关测试智能合约Wasm模块的信息，请参阅: :ref:`local-simulate` 。

用Rust写的智能合约是一个Rust库，我们可以通过为合约函数注释 #[test] 属性，从而能像库一样对其进行单元测试。

.. code-block:: rust

    // contract code
    ...

    #[cfg(test)]
    mod test {

        #[test]
        fn some_test() { ... }

        #[test]
        fn another_test() { ... }
    }

可以使用cargo的以下命令来完成测试任务：

.. code-block:: console

   $cargo test

默认情况下，此命令将合同和单元测试编译为本地机器码（最有可能是x86_64），然后运行它们。这种测试对于初始开发和功能正确性测试很有用。对于全面测试，重要的是要包含目标平台特征, 即Wasm32。平台之间存在许多细微的差异，这些差异可以改变合约的原始逻辑。其中一个区别就是指针的大小，其中Wasm32使用四个字节，而不是八个字节，这在大多数平台上都很常见。

编写单元测试
==================

单元测试通常遵循三部分结构，您可以在其中进行以下操作：设置某些状态，运行某些代码单元，以及对代码的状态和输出进行断言。

如果合同功能是使用#[init(..)]或 编写的#[receive(..)]，我们可以直接在单元测试中测试这些功能。

.. code-block:: rust

   use concordium_std::*;

   #[init(contract = "my_contract", payable, enable_logger)]
   fn contract_init(
      ctx: &impl HasInitContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
   ) -> InitResult<State> { ... }

   #[receive(contract = "my_contract", name = "my_receive", payable, enable_logger)]
   fn contract_receive<A: HasActions>(
      ctx: &impl HasReceiveContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
      state: &mut State,
   ) -> ReceiveResult<A> { ... }

在concordium-std名为的子模块(test_infrastructure)中可以找到功能参数的测试存根.

.. 另

   请参见：:有关更多信息和示例，请参见
   concordium-std的包装箱文档。

.. todo::

   显示更多关于如何编写单元测试的信息

在Wasm中运行测试
=====================

在大多数情况下，将测试编译为本机代码就足够了，但是也可以将测试编译为Wasm并使用节点使用的解释器运行它们。这使测试环境更接近于链上的运行环境，并且在某些情况下可能会捕获更多错误。

开发工具 ``cargo-concordium`` 包括一个用于编译成Wasm的测试运行程序，它使用与Concordium节点中附带的Wasm解释相同的Wasm解释程序。

.. 另::

   有关如何安装 ``congo -concordium`` 的指南，请参阅 :ref:`setup-tools` 。

单元测试必须使用 ``#[concordium_test]``代替 ``#[test]``，并且我们使用 ``#[concordium_cfg_test]`` 代替 ``#[cfg(test)]`` ：

.. code-block:: rust

   // contract code
   ...

   #[concordium_cfg_test]
   mod test {

       #[concordium_test]
       fn some_test() { ... }

       #[concordium_test]
       fn another_test() { ... }
   }

当 ``concordium-std`` 中包含了 ``wasm-test`` , ``#[concordium_test]`` 宏会创建以WASM形式运行的测试，否则将回退到像使用 ``#[test]`` 那样，这意味着我们仍然可以使用 ``cargo test`` 将单元测试编译成本地机器码。

类似地，在构建具备 ``wasm-test`` 的 ``concordium-std`` 时，宏 ``#[concordium_cfg_test]`` 会包含我们的模块，否则功能类似于使用 ``#[test]`` ，从而使我们能够控制何时在构建中包含测试。

现在可以使用以下命令构建和运行测试：

.. code-block:: console

   $cargo concordium test

该命令行会通过 ``wasm-test`` 功能( ``concordium-std`` 已启用)将单元测试编译成Wasm, 并使用 ``cargo-concordium`` 的单元测试执行器。

.. 警告::

   编译为Wasm时，不会显示 panic！的错误消息，以及 assert！的不同变体。

   相反，我们得在合约的单元测试中使用 ``fail!`` 和 ``claim!`` 来做断言，因为它们会在测试失败退出前将错误信息报告给测试运行器。 ``fail!`` 和 ``claim!`` 都内置在 ``concordium-std`` 中。
   
.. todo::

    Use link concordium-std: docs.rs/concordium-std when crate is published.
