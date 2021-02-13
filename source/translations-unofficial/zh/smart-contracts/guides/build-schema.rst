.. _list of types implementing the SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schema:

=======================
建立合约模式
=======================

本指南将向您展示如何使用构建智能合约模式，如何将其导出到文件 和 / 或将模式嵌入到智能合约模块中 ``cargo-concordium``。

制备
===========

首先，请确保您已 ``cargo-concordium`` 安装，如果没有，安装指南 :ref:`setup-tools`  将为您提供帮助。

我们还需要您希望为其构建架构的智能合约的 Rust 源代码。

为架构设置合同
===============================

为了构建合同模式，我们首先必须准备用于构建模式的智能合同。

我们可以选择将智能合约的哪些部分包括在架构中。选项包括 合同状态 和 /或初始化和接收函数的每个参数的模式。

我们要包含在模式中的每种类型都必须实现 ``SchemaType`` 特征。对于所有基本类型和某些其他类型，已经完成了此操作（请参阅 `list of types implementing the SchemaType`_）。对于大多数其他情况，也可以使用 ``#[derive(SchemaType)]`` 以下方法自动实现 ：

``#[derive(SchemaType)]``::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

 ``SchemaType`` 手动实现特征仅需要指定一个函数，该函数是 a 的获取器 ``schema::Type`` ，它实质上描述了如何将这种类型表示为字节以及如何表示它。

.. 去做：：

   创建一个示例来展示如何手动实现 ``SchemaType`` 和链接
   从这里开始。

包括合同状态
------------------------

为了生成并包括合同状态的模式，我们用 ``#[contract_state(contract = ...)]`` 宏注释类型： ::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

如果合同状态是已经实现的类型，甚至更简单SchemaType，例如u32： ::

   #[contract_state(contract = "my_contract")]
   type State = u32;

包括功能参数
-----------------------------

要生成并包含用于初始化和接收函数的参数的模式，我们为 ``#[init(..)]`` -和 ``#[receive(..)]`` -宏设置了可选的 ``parameter`` 属性： ::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }


建立架构
===================

现在，我们准备使用来构建实际的模式 ``cargo-concordium`` ，并且可以选择嵌入模式和/或将模式写入文件。

.. 另::

   有关更多选择的信息，请参见
   :ref:`here<contract-schema-which-to-choose>`.

嵌入架构
--------------------

为了将模式嵌入到智能合约模块中，我们 ``--schema-embed`` 在build命令中添加 了

.. code-block:: console

   $cargo concordium build --schema-embed

如果成功，命令的输出将告诉您模式的总大小（以字节为单位）。

输出模式文件
------------------------

要将模式输出到文件中，我们可以使用 ``--schema-out=FILE`` where  ``FILE`` 是要创建的文件路径：

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
