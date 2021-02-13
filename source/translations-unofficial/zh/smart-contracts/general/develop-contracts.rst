.. Should answer:
    - Why write a smart contract using rust?
    - What are the pieces needed to write a smart contract in rust?
        - State
            - Serialized
            - Schema
        - Init
        - Receive
    - What sort of testing is possible
    - Best practices?
        - Ensure 0 amount
        - Don't panic
        - Avoid heavy calculations

.. _writing-smart-contracts:

==================================
用Rust开发智能合约
==================================

在concordium区块链上，智能合约部署为Wasm模块，但Wasm是合约的编译后的指令，不方便手工编写。相反，我们可以用 Rust_ 编程语言编写智能合约，它能很好的支持编译成Wasm。

智能合约不必用Rust写。这只是我们提供的第一个SDK。手动编写的WASM，或者从C、C++、AssemblyScript等中编译的WASM，在链上同样有效，只要它遵循 :REF: `WASM限制，我们强加<wasm-limitations>` .

.. seealso::

   有关下面描述的函数的更多信息，请参阅concordium_std_ 用于在Rust中的Concordium区块链上编写智能合约的API。

.. seealso::

   See :ref:`contract-module` for more information about smart contract modules.

用Rust开发了一个智能合约模块作为一个库crate，然后编译成Wasm。要获得正确的导出，必须在manifest文件中将 `crate-type` 属性设置为 ``["cdylib"，"rlib"]`` :

.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...

Writing a smart contract using ``concordium_std``
=================================================

建议使用 ``concordium_std`` crate，用该crate为开发智能合约模块和调用主机函数就像开发Rust一样。

crate可将init和receive函数编写为简单的Rust函数，分别使用 ``#[init(...)]`` 和 ``#[receive(...)]`` 进行注释。

这是实现计数器的智能合约的示例：

.. code-block:: rust

   use concordium_std::*;

   type State = u32;

   #[init(contract = "counter")]
   fn counter_init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let state = 0;
       Ok(state)
   }

   #[receive(contract = "counter", name = "increment")]
   fn contract_receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       ensure!(ctx.sender().matches_account(&ctx.owner()); // Only the owner can increment
       *state += 1;
       Ok(A::accept())
   }

这里有很多注意事项：

.. todo::

   - Write up the requirements in an easier to read way (e.g., split up paragraphs into sub-bullets).
   - These requirements should be part of a specification that is written up somewhere,
     i.e., not just as part of this example.

- 函数类型:

  * 一个init函数必须是一个 ``&impl HasInitContext -> InitResult<MyState>`` 类型，这里的 ``MyState`` 是一个实现了 ``Serialize`` 的类型.
  * A receive function must take a ``A: HasActions`` type parameter,
    a ``&impl HasReceiveContext`` and a ``&mut MyState`` parameter, and return
    a ``ReceiveResult<A>``.
    receive 函数必须接收一个 ``A: HasActions`` 类型的参数, 一个 ``&impl HasReceiveContext`` 和 ``&mut MyState`` 参数, 以及返回一个 ``ReceiveResult<A>`` 参数.

- The annotation ``#[init(contract = "counter")]`` marks the function it is
  applied to as the init function of the contract named ``counter``.
  Concretely, this means that behind the scenes this macro generates an exported
  function with the required signature and name ``init_counter``.
  

- ``#[receive(contract = "counter", name = "increment")]`` deserializes and
  supplies the state to be manipulated directly.
  Behind the scenes this annotation also generates an exported function with name
  ``counter.increment`` that has the required signature, and does all of the
  boilerplate of deserializing the state into the required type ``State``.

.. note::

   请注意，反序列化并不是没有代价的，在某些情况下用户可能希望对主机函数的使用进行更细粒度的控制。对于这样的场景，注解支持一个　``low_level`` 的选项，它有较少的开销，但是需要用户提供更多的资源。 

.. todo::

   - Describe low-level
   - Introduce the concept of host functions before using them in the note above


Serializable state and parameters
---------------------------------

.. todo:: Clarify what it means that the state is exposed similarly to ``File``;
   preferably, without referring to ``File``.

在链上，实例的状态表示为字节数组，并在以Rust标准库的 ``File`` 接口类似的接口中暴露出来。

这可以使用包含（反）序列化函数的 ``Serialize`` trait来完成。

``concordium_std`` crate 包含了这个特性，并且实现了Rust标准库中的大多数类型。它还包括用于为用户定义的结构和枚举派生特征的宏。

.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

参数初始化和接收函数的必要条件相同。

.. 注意::

   严格来说，我们只需要将字节反序列化为我们的参数类型，但是在编写单元测试时能够序列化类型很方便。

.. _working-with-parameters:

Working with parameters
-----------------------

init和receive函数的参数与实例状态类似，表示为字节数组。虽然字节数组可以直接使用，但它们也可以反序列化为结构化数据。

反序列化参数的最简单方法是通过 `get`_ trait的 `get()`_ 函数。

例如，请参见以下协定，其中参数 ``ReceiveParameter`` 在突出显示的行上反序列化：

.. code-block:: rust
   :emphasize-lines: 24

   use concordium_std::*;

   type State = u32;

   #[derive(Serialize)]
   struct ReceiveParameter{
       should_add: bool,
       value: u32,
   }

   #[init(contract = "parameter_example")]
   fn init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let initial_state = 0;
       Ok(initial_state)
   }

   #[receive(contract = "parameter_example", name = "receive")]
   fn receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let parameter: ReceiveParameter = ctx.parameter_cursor().get()?;
       if parameter.should_add {
           *state += parameter.value;
       }
       Ok(A::accept())
   }

上面的receive函数效率低下，因为它甚至在不需要值的时候反序列化该值，即当 should_add为false。

为了获得更多的控制，在这种情况下，更高效，我们可以使用 `Read`_ trait反序列化参数：

.. code-block:: rust
   :emphasize-lines: 7, 10

   #[receive(contract = "parameter_example", name = "receive_optimized")]
   fn receive_optimized<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let mut cursor = ctx.parameter_cursor();
       let should_add: bool = cursor.read_u8()? != 0;
       if should_add {
           // Only decode the value if it is needed.
           let value: u32 = cursor.read_u32()?;
           *state += value;
       }
       Ok(A::accept())
   }

请注意，只有当 ``should_add`` 为 ``true`` 时，才会反序列化该值。虽然在这个例子中，效率的提高是最小的，但对于更复杂的例子，它可能会产生实质性的影响。

用 ``cargo-concordium`` 构建一个智能合约模块
==========================================================

Rust编译器很好地支持使用 ``wasm32-unknown-unknown`` 目标编译到Wasm。但是，即使在使用 ``--release`` 进行编译时，生成的构建也会在自定义部分中包含大量调试信息，这是没有用的

为了优化构建并允许嵌入模式等新功能，我们建议使用 ``cargo-concordium`` 来构建智能合约。

.. seealso::

   关于使用 ``cargo-concordium`` 构建的说明请参考 :ref:`compile-module`

测试智能合约
=======================

使用stubs进行单元测试
---------------------

模拟合约调用
-----------------------

最佳实践
==============

不要担心
-----------

.. todo::

   Use trap instead.

避免创建黑洞
--------------------------

智能合约不需要使用发送给它的GTU数量，默认情况下，智能合约不定义清空实例余额的任何行为，以防有人发送一些GTU。这些GTU将永远 *失去* ，也没有办法找回它们。

因此，对于不处理GTU的智能合约来说，最好确保GTU的发送量为零，并拒绝任何不处理GTU的调用。

将繁重的计算任务移到链下
---------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Get: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Read: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/

