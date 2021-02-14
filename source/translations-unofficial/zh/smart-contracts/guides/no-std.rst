.. _no-std:

======================
使用 ``no_std``
======================

本指南将会阐述如何为rust智能合约使用 ``no_std`` ，从而有可能令生成的Wasm模块的大小减少几千字节！


准备
===========

编译缺少 ``std`` 功能的 ``concordium-std`` 需要使用 rust的 *nightly* 工具链，可以使用 ``rustup`` 以下命令安装：

.. code-block:: console

   $rustup toolchain install nightly


创建 ``no_std`` 合约模块
====================================

该 ``concordium-std`` 库提供了 ``std`` 功能，该功能让我们使用 rust 标准库。此功能是默认启用的。

要禁用它，只需在模块module的依赖项中简单地禁用 ``concordium-std`` 的默认功能。

.. code-block:: rust

   [dependencies]
   concordium-std = { version: "=0.2", default-features = false }

为了能够在有 ``std`` 和没有 ``std`` 之间切换，还可以在自己的模块中添加 ``std`` ，以启用 ``concordium-std`` 的 ``std`` 功能 ：

.. code-block:: rust

   [features]
   std = ["concordium-std/std"]

这是智能合约示例的设置，其中每个智能合约模块的 ``std`` 都默认启用了。

构建合约模块(module)
=====================

为了使用 *nightly* 工具链，请在 ``+nightly`` 之后 添加 ``cargo``：

.. code-block:: console

   $cargo +nightly concordium build

如果要禁用自己的智能合约模块的默认功能，则可以为传递额外的参数 ``cargo`` , 如下：

.. code-block:: console

   $cargo +nightly concordium build -- --no-default-features
