.. _setup-tools:

=============================
安装开发工具
=============================

在开始开发智能合约之前，我们需要设置环境。

Rust和Cargo
==============

首先， `安装 rustup`_ ，它将在您的计算机上同时安装 Rust_ 和 Cargo_ 。然后使用rustup来安装用于编译环节的Wasm：

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium是为Concordium区块链开发智能合约的工具。它可用于 :ref:`compiling<compile-module>` 和 :ref:`testing<unit-test-contract>` 智能合约，并启用诸如： :ref:`building contract schemas<build-schema>` 之类的功能。

.. todo::

   添加用于测试和架构的链接。

Cargo Concordium作为 :ref:`Concordium software<downloads>` 软件包的一部分分发。建议将该工具放在您操作系统的PATH中。

有关如何使用Cargo Concordium的说明：

.. code-block:: console

   $cargo concordium --help

Concordium 软件
===================

部署智能合约并与之交互的工具是 :ref:`concordium-client<concordium_client>` 。它作为 :ref:`Concordium software<downloads>` 软件包的一部分发布 。

.. note::

   要部署智能合约模块并与链进行交互，请确保您正在 :ref:`运行一个节点<run-a-node>`.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _install rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
