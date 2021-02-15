.. _setup-toolstr:

=========================================
Geliştirme için gerekli araçlar yükleme
=========================================

Akıllı sözleşmeler geliştirmeye başlamadan geliştirme önce ortamını
kurmamız gerekiyor.

Rust ve Cargo
==============

İlk olarak, hem Rust_ hem de Cargo_'yu makinenize kuracak olan `install rustup`_ ‘ı kurun.
Ardından, derleme için kullanılan Wasm hedefini kurmak için ``rustup`` kullanın:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium, Concordium blok zinciri için akıllı sözleşmeler geliştirmek
aracıdır.
Akıllı sözleşmeleri :ref:`derleme<compile-moduletr>`,  :ref:`test etme<unit-test-contracttr>`
ve :ref:`sözleşme şemalarını yapılandırma <build-schematr>` gibi özellikleri aktif etmek
için kullanılabilir..

.. todo::

   Add links for testing and schemas.

Cargo Concordium :ref:`Concordium software<downloads>` paketinin bir parçası olarak
dağıtılır ve PATH'inize yerleştirilmelidir.

Cargo Concordium nasıl kullanılacağına ilişkin açıklama için alttaki komutu çalıştırın:

.. code-block:: console

   $cargo concordium --help

Concordium software
===================

Akıllı sözleşmeleri devreye alma ve bunlarla etkileşim kurma aracı
:ref:`concordium-client<concordium_client>`'dır.
:ref:`Concordium software<downloads>` paketinin bir parçası olarak dağıtılır.

.. note::

   Akıllı sözleşme modüllerini dağıtmak ve zincirle etkileşimde bulunmak için,
   Bir :ref:`Node<run-a-node>` çalıştırdığınızdan emin olun.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _install rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
