.. _no-std-ko:

======================
``no_std`` 를 사용하여 빌드
======================

이 가이드는 Rust 스마트 계약에 대해 ``no_std `` 를 활성화하여 결과 Wasm 모듈의 크기를 잠재적으로 수 킬로바이트로 줄이는 방법을 보여줍니다.

준비
===========

``std`` 기능없이 ``concordium-std`` 를 컴파일하려면 ``rustup`` 을 사용하여 설치할 수있는 rust nightly 툴체인을 사용해야합니다:

.. code-block:: console

   $rustup toolchain install nightly

``no_std`` 에 대한 모듈 설정
====================================

``concordium-std`` 라이브러리는 rust 표준 라이브러리를 사용할 수있는 ``std`` 기능을 제공합니다.
이 기능은 기본적으로 활성화되어 있습니다.

이를 비활성화하려면 모듈 종속성에서 ``concordium-std`` 의 기본 기능을 비활성화해야합니다.

.. code-block:: rust

   [dependencies]
   concordium-std = { version: "=0.2", default-features = false }

std를 사용하거나 사용하지 않고 전환 할 수 있으려면 ``concordium-std`` 의 ``std`` 기능을 활성화하는 ``std`` 를 자신의 모듈에 추가하십시오:

.. code-block:: rust

   [features]
   std = ["concordium-std/std"]

이것은 각 스마트 계약 모듈에 대한 ``std`` 가 기본적으로 활성화 된 스마트 계약 예제의 설정입니다.

모듈을 빌드합니다.
===================

야간 툴체인을 사용하려면 ``cargo`` 바로 뒤에 ``+nightly`` 를 추가합니다.

.. code-block:: console

   $cargo +nightly concordium build

자체 스마트 계약 모듈의 기본 기능을 비활성화하려면 ``cargo`` 에 대한 추가 인수를 전달할 수 있습니다:

.. code-block:: console

   $cargo +nightly concordium build -- --no-default-features