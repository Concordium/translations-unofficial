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

Pagtatanggal ng host information mula sa build
==============================================

Ang na-compile na Wasm module ay naglalaman ng inpormasyon mula sa host machine na bumuo
ng binary; inpormasyon tulad ng absolute path ng ``.cargo`` directory.

Para sa maraming tao ito ay hindi sensitibong inpormasyon, pero importante rin na maging
alam mo ang patungkol dito.

Sa Linux ang mga paths ay pwedeng ma-inspeksyon sa pamamagitan ng pagpapatakbo ng:

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: The solution

Ang ideyal na solusyon ay tanggalin ang buong path na ito, pero sa
kasamaang palad di ito madaling gawin.

Pwedeng magkaroon ng remedyo sa isyu sa paggamit ng ``--remap-path-prefix``
i-flag ito kapag nagco-compile na ng contract.
Sa mga Unix-like na mga sistema ang flag ay pwedeng ipasa ng deretso sa ``cargo concordium``
invocation gamit ang ``RUSTFLAGS`` environment variable:

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

Na kung saan ay papalitan nito ang home path ng users ng empty string. Ang ibang mga paths ay pwedeng
ma-mapa sa parehong pamamaraan. Sa pangkalahatang gamit ``--remap-path-prefix=from=to``
ay mama-mapa ``from`` to ``to`` sa simula ng kahit anong embedded path.

Ang flag ay pwedeng i-set ng permanente sa ``.cargo/config`` file sa iyong
crate, sa ilalim ng build section:

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

kung saan `<user>` ay dapat palitan ng user na bumubuo ng wasm module.

mga pag-iingat
--------------

Ang nakasaad sa itaas ay hindi mareresolba ang isyu kung ang ``rust-src`` na component ay
naka-install para sa Rust toolchain. Ang component na ito ay kailangan ng ibang Rust tools
tulad ng rust-analyzer_.

.. seealso::

   Isang isyu na inirereport ang problema kasama ang ``--remap-path-prefix`` and ``rust-src``
   https://github.com/rust-lang/rust/issues/73167
