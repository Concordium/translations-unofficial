.. highlight:: toml

.. _setup-contract-fil:

=====================================
Pagse-setup ng smart contract project
=====================================

Ang smart contract sa Rust ay nakasulat sa ordinaryong Rust library crate.
Ang library ay kino-compiled sa Wasm gamit ang Rust target
``wasm32-unknown-unknown`` at, dahil ito ay isa lamang Rust library, pwede nating gamitin ang
Cargo_ para sa dependency management.

Para i-setup ang bagong new smart contract project, una gumawa ka muna ng project directory. Sa loob
ng project directory ay patakbuhin ang sumusunod sa isang terminal:

.. code-block:: console

   $cargo init --lib

Ito ay magse-setup ng isang default Rust library project sa pamamagitan ng paggawa ng ilang mga files at
directories.
Ang iyong directory ay dapat naglalaman ng ``Cargo.toml`` file at ng ``src``
directory at iba pang mga nakatagong files.

Para maka-build sa Wasm kailangan nating sabihan ang cargo ng tamang ``crate-type``.
Ito ay nagagawa sa pamamagitan ng pagdagdag ng sumusunod na file sa ``Cargo.toml``::

   [lib]
   crate-type = ["cdylib", "rlib"]

Pagdagdag ng smart contract standard library
============================================

Ang susunod na step ay magdagdag ng ``concordium-std`` bilang dependency.
Ito ay isang library para sa Rust na naglalaman ng procedural macros at functions para sa
pagsusulat ng maliliit at mahuhusay na mga smart contracts.

Ang library ay dinadagdag sa pagbubukas ng ``Cargo.toml`` at pag-dagdag sa linya
``concordium-std = "*"`` (hanggang maari, palitan ang `*` kasama ng pinakabagong bersyon ng `concordium-std`_) sa
``[dependencies]`` seksyon::

   [dependencies]
   concordium-std = "0.4"

Ang crate documentation ay makikita sa docs.rs_.

.. note::

   Kung gusto mong mabago ang bersyon ng crate na ito, kailangan mong i-clone
   ang repository kasama ng ``concordium-std`` at magkaroon ng dependency point sa
   directory, sa pamamagitan ng pagdagdag ng mga sumusunod ``Cargo.toml``::

      [dependencies]
      concordium-std = { path = "./path/to/concordium-std" }

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rustup: https://rustup.rs/
.. _repository: https://gitlab.com/Concordium/concordium-std
.. _docs.rs: https://docs.rs/crate/concordium-std/
.. _`concordium-std`: https://docs.rs/crate/concordium-std/

Ganun lang! Ikaw ay nakahanda na para mag-debelop ng sarili mong smart contract.
