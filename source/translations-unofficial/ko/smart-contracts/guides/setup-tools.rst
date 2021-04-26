.. _setup-tools-ko:

=============================
개발 용 도구 설치
=============================

스마트 계약 개발을 시작하기 전에 환경을 설정해야합니다.

Rust 과 Cargo
==============

먼저, 컴퓨터에 Rust_ 와 Cargo_를 모두 설치하는`install rustup`_ 입니다.
그런 다음 ``rustup`` 을 사용하여 컴파일에 사용되는 Wasm 대상을 설치합니다:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium은 Concordium 블록 체인을위한 스마트 계약을 개발하기위한 도구입니다.
:ref:`컴파일 <compile-module-ko>` 및 :ref:`테스트 <unit-test-contract-ko>` 스마트 계약에 사용할 수 있으며 :ref:`빌딩 계약 스키마 <build-schema-ko>`.

.. todo::

   테스트 및 스키마에 대한 링크를 추가하십시오.

Cargo Concordium은 :ref:`Concordium software <downloads>` 패키지의 일부로 배포됩니다.
도구는 PATH에 있어야합니다.

Cargo Concordium 실행 방법에 대한 설명은 다음을 참조하십시오:

.. code-block:: console

   $cargo concordium --help

Concordium software
===================

스마트 계약을 배포하고 상호 작용하는 도구는 :ref:`concordium-client <concordium_client>` 입니다.
:ref:`Concordium software <downloads>` 패키지의 일부로 배포됩니다.

.. note::

   스마트 계약 모듈을 배포하고 체인과 상호 작용하려면 :ref:`running a node <run-a-node>` 인지 확인하십시오.

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _install rustup: https://rustup.rs/
.. _crates.io: https://crates.io/
