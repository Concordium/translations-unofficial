.. _list of types implementing the SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schema:

=======================
构建合约schema
=======================

本指南将向您展示如何构建智能合约schema，如何将其导出到文件 和 将schema嵌入到智能合约模块中, 全程都只需使用 ``cargo-concordium`` 就可以。

准备工作
===========

首先，请确保您已安装 ``cargo-concordium`` ，如果没有，请参考安装指南 :ref:`setup-tools` 。

我们还需要您准备好需要构建schema的智能合约的 Rust 源代码。

创建智能合约
===============================

为了构建schema，我们首先得准备好智能合约。

我们可以决定将智能合约的哪些部分包括在schema中，可以是 合约的state(状态) 和(/或)init(初始化)函数和recieve(接收)函数的每个参数。

包含在schema中的每种类型都必须实现 ``SchemaType`` 特征(trait)。所有基本类型和某些其他类型都已经实现了``SchemaType`` 特征(trait)（请参阅 `list of types implementing the SchemaType`_ ），而对于其他(自定义)的类型，则可以使用 ``#[derive(SchemaType)]`` 以下方法自动实现：

``#[derive(SchemaType)]``::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

手动实现 ``SchemaType`` 特征仅需要指定一个函数，该函数就是 ``schema::Type`` 的getter函数，它描述了如何将这种类型序列化成字节以及如何表示它。

.. todo：：

   创建一个示例来展示如何手动实现 ``SchemaType`` 和链接
   从这里开始。

将合约状态(state)包含在schema中
------------------------

为了将合同状态(state)包含在schema中，我们用 ``#[contract_state(contract = ...)]`` 宏注释类型： 
  ::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

如果合约状态(state)是已经实现了SchemaType的类型，如u32，则更简单： 
  ::

   #[contract_state(contract = "my_contract")]
   type State = u32;

将函数入参出参包含在schema中
-----------------------------

要生成并包含用于初始化(init)函数和接收(recieve)函数的参数的schema，我们为 ``#[init(..)]`` 和 ``#[receive(..)]`` 宏 设置了可选的 ``parameter`` 属性： ::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }


构建schema
===================

现在，我们准备使用 ``cargo-concordium`` 来构建实际的schema，并且可以选择嵌入schema和/或将schema写入文件。

.. 另::

   有关更多选择的信息，请参见 :ref:`here<contract-schema-which-to-choose>` .

将schema嵌入module中
--------------------

为了将schema嵌入到智能合约模块(module)中，我们 ``--schema-embed`` 在build命令中添加了

.. code-block:: console

   $cargo concordium build --schema-embed

如果成功，命令的输出将告诉您schema的总大小（以字节为单位）。

输出schema文件
------------------------

要将schema输出到文件中，我们可以使用 ``--schema-out=FILE`` where  ``FILE`` 是要创建的文件路径：

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
