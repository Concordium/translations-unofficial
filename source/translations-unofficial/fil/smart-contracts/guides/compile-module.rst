.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rust-analyzer: https://github.com/rust-analyzer/rust-analyzer

.. _compile-module:

=========================================
Mag-compile ng Rust smart contract module
=========================================

Itong guide na ito ay ipapakita sa iyo kung paano mag-compile ng smart contract module na isinulat sa Rust papunta sa 
Wasm module.

Paghahanda
==========

Siguraduhing merong Rust at Cargo na naka-install at ang ``wasm32-unknown-unknown``
target, kasama ang ``cargo-concordium`` at ang Rust source code para sa smart
contract module, na gusto mong i-compile.

.. seealso::

  Para sa mga tagubilin sa kung paano i-install ang mga tool ng developer tingnan ang
   :ref:`setup-tools`.

Pagco-compile sa Wasm
=====================

Para makatulong makabuo ng smart contract modules at upang samantalahin ang mga features
tulad ng :ref:`contract schemas <contract-schema>`, irinirekumenda namin na gumamit ng
``cargo-concordium`` tool para sa pagbuo ng Rust_ smart contracts.

Para makabuo ng smart contract, patakbuhin ang:

.. code-block:: console

   $cargo concordium build

Ito ay gumagamit ng Cargo_ para sa pagbuo, pero nagpapatakbo rin ng mga optimizations sa mga resulta.

.. seealso::

   Para sa pagbuo ng schema para sa isang smart contract module, ang ilang :ref:`further
   preparation is required <build-schema>`.

.. note::

   Posible ring mag-compile gamit ang Cargo_ direkta sa pamamagitan ng pagpapatakbo ng:

   .. code-block:: console

      $cargo build --target=wasm32-unknown-unknown [--release]

   Tandaan na kahit merong ``--release`` set, ang maproproduce na Wasm module ay may kasamang
   impormasyon sa debug.

Removing host information from build
====================================

The compiled Wasm module can contain information from the host machine building
the binary; information such as the absolute path of the ``.cargo`` directory.

For most people this is not sensitive information, but it is important to be
aware of it.

On Linux the paths can be inspected by running:

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: The solution

The ideal solution would be to remove this path entirely, but that is
unfortunately not a trivial task in general.

It is possible to work around the issue by using the ``--remap-path-prefix``
flag when compiling the contract.
On Unix-like systems the flag can be passed directly to the ``cargo concordium``
invocation using the ``RUSTFLAGS`` environment variable:

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

Which will replace the users home path with the empty string. Other paths could
be mapped in a similar way. In general using ``--remap-path-prefix=from=to``
will map ``from`` to ``to`` at the beginning of any embedded path.

The flag can also be set permanently in the ``.cargo/config`` file in your
crate, under the build section:

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

where `<user>` should be replaced with the user building the wasm module.

Caveats
-------

The above will likely not fix the issue if the ``rust-src`` component is
installed for the Rust toolchain. This component is needed by some Rust tools
such as the rust-analyzer_.

.. seealso::

   An issue reporting the problem with ``--remap-path-prefix`` and ``rust-src``
   https://github.com/rust-lang/rust/issues/73167
