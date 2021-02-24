.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rust-analyzer: https://github.com/rust-analyzer/rust-analyzer

.. _compile-module-ko:

====================================
Rust 스마트 계약 모듈 컴파일
====================================

이 가이드는 Rust로 작성된 스마트 계약 모듈을 Wasm 모듈로 컴파일하는 방법을 보여줍니다.

준비
===========

Rust와 Cargo가 설치되어 있고``wasm32-unknown-unknown ''타겟이 있는지 확인하세요,
``cargo-concordium ''및 스마트 계약 모듈 용 Rust 소스 코드와 함께 컴파일하고 싶습니다.

.. seealso::

   개발자 도구를 설치하는 방법에 대한 지침은 :ref:`setup-tools-ko` 를 참조하십시오.

Wasm에 컴파일
=================

스마트 계약 모듈 구축을 돕고 :ref:`계약 스키마 <contract-schema-ko>`와 같은 기능을 활용하려면,
Rust_ 스마트 계약을 구축하기 위해``cargo-concordium ''도구를 사용하는 것이 좋습니다.

스마트 계약을 작성하려면 다음을 실행하십시오.

.. code-block:: console

   $cargo concordium build

이것은 건물에 Cargo_ 를 사용하지만 결과에 대해 추가 최적화를 실행합니다.

.. seealso::

   스마트 계약 모듈에 대한 스키마를 구축하려면 일부 :ref:`추가 준비가 필요합니다 <build-schema-ko>`.

.. note::

   다음을 실행하여 Cargo_ 를 사용하여 직접 컴파일 할 수도 있습니다.

   .. code-block:: console

      $cargo build --target=wasm32-unknown-unknown [--release]

   ``--release`` 가 설정되어 있어도 생성 된 Wasm 모듈에는 디버그 정보가 포함됩니다.

빌드에서 호스트 정보 제거
====================================

컴파일 된 Wasm 모듈은 바이너리를 빌드하는 호스트 머신의 정보를 포함 할 수 있습니다; ``.cargo`` 디렉토리의 절대 경로와 같은 정보.

대부분의 사람들에게 이것은 민감한 정보가 아니지만 알고있는 것이 중요합니다.

Linux에서는 다음을 실행하여 경로를 검사 할 수 있습니다:

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: The solution

이상적인 해결책은이 경로를 완전히 제거하는 것이지만 불행히도 일반적으로 사소한 작업은 아닙니다.

계약을 컴파일 할 때 ``--remap-path-prefix`` 플래그를 사용하여 문제를 해결할 수 있습니다.
유닉스 계열 시스템에서 플래그는 ``RUSTFLAGS`` 환경 변수를 사용하여 ``cargo concordium`` 호출에 직접 전달할 수 있습니다.

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

사용자 홈 경로를 빈 문자열로 바꿉니다. 다른 경로도 비슷한 방식으로 매핑 할 수 있습니다.
일반적으로 ``--remap-path-prefix=from=to`` 를 사용하면 포함 된 경로의 시작 부분에서 ``from`` 을 ``to`` 로 매핑합니다.

이 플래그는 빌드 섹션 아래의 상자에있는 ``.cargo/config`` 파일에서 영구적으로 설정할 수도 있습니다:

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

여기서 `<user>` 는 wasm 모듈을 빌드하는 사용자로 대체되어야합니다.

소송 절차 정지 통고
--------------

위의 방법은 Rust 도구 모음에 ``rust-src`` 구성 요소가 설치된 경우 문제를 해결하지 못할 가능성이 높습니다.
이 구성 요소는 rust-analyzer_ 와 같은 일부 Rust 도구에 필요합니다.

.. seealso::

   ``--remap-path-prefix`` 및 ``rust-src`` 문제를보고하는 문제
   https://github.com/rust-lang/rust/issues/73167
