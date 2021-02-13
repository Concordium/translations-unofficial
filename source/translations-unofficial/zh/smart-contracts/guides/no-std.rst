.. _no-std:

======================
建立使用 ``no_std``
======================

本指南说明了如何启用 ``no_std``  rust 智能合约，从而有可能将生成的 Wasm 模块的大小减少几千字节。


准备
===========

``concordium-std`` 没有该 ``std`` 功能的编译需要使用 rust 每晚工具链，可以使用 ``rustup`` 以下方法安装：

.. code-block:: console

   $rustup toolchain install nightly

设置用于 ``no_std``
====================================


该 ``concordium-std`` 库公开了一项 ``std`` 功能，该功能可以使用 rust 标准库。默认情况下启用此功能。

要禁用它，必须简单地禁用 ``concordium-std`` 模块依赖项中的默认功能 。

.. code-block:: rust

   [dependencies]
   concordium-std = { version: "=0.2", default-features = false }

为了能够在有和没有 ``std`` 之间切换， ``std`` 还可以在自己的模块中添加a ，以启用以下 ``std`` 功能 ``concordium-std`` ：

.. code-block:: rust

   [features]
   std = ["concordium-std/std"]

这是智能合约示例的设置，其中 ``std`` 默认情况下启用了每个智能合约模块。

构建模块
===================

为了使用夜间工具链，请在 ``+nightly`` 之后 添加 ``cargo``：

.. code-block:: console

   $cargo +nightly concordium build

如果要禁用自己的智能合约模块的默认功能，则可以为传递额外的参数 ``cargo``：

.. code-block:: console

   $cargo +nightly concordium build -- --no-default-features
