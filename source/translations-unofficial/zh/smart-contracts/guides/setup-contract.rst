.. highlight:: toml

.. _setup-contract:

===================================
创建一个智能合约工程
===================================

Rust中的智能合约被编写为普通的Rust库crate, 然后通过设置Rust的 ``wasm32-unknown-unknown`` 目标将该库编译为Wasm. 由于智能合约只是一个Rust库，因此我们可以使用 Cargo_ 进行依赖管理。

要创建新的智能合约工程，请首先创建一个工程目录。在工程目录中，在终端中运行以下命令：

.. code-block:: console

   $cargo init --lib

这将通过创建一些文件和目录来设置默认的Rust库工程。您的目录现在应该包含一个 ``Cargo.toml`` 文件和一个 ``src`` 目录以及一些隐藏文件。

为了建造Wasm，我们需要告诉Cargo正确的 ``crate-type`` 。这是通过在文件 ``Cargo.toml`` 中添加以下内容来完成的:

   [lib]
   crate-type = ["cdylib", "rlib"]

添加智能合约标准库
==========================================

下一步是添加 ``concordium-std`` 为依赖项。它是一个用于Rust的库，其中包含程序宏和用于编写小型高效的智能合约的函数。

 ``concordium-std`` 库的添加可以通过这种方式：打开 ``Cargo.toml`` 并在 ``[dependencies]`` 部分中添加一行 ``concordium-std = "*"`` （最好将*替换为最新版本的 `concordium-std`_ ）：

   [dependencies]
   concordium-std = "0.4"

这个crate的文档可在 docs.rs_ 上找到。

.. 注意::

   如果您想使用此crate的修改版，则必须通过将以下内容添加到 ``Cargo.toml`` 来克隆这个依赖 ``concordium-std`` 的库，并令依赖指向此目录:
   
      [dependencies]
      concordium-std = { path = "./path/to/concordium-std" }

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rustup: https://rustup.rs/
.. _repository: https://gitlab.com/Concordium/concordium-std
.. _docs.rs: https://docs.rs/crate/concordium-std/
.. _`concordium-std`: https://docs.rs/crate/concordium-std/

这就对了！现在您可以开发自己的智能合约了。
