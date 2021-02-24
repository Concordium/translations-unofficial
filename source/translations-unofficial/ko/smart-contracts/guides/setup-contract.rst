.. highlight:: toml

.. _setup-contract-ko:

===================================
스마트 계약 프로젝트를 설정
===================================

Rust의 스마트 계약은 일반 Rust 라이브러리 상자로 작성됩니다.
그런 다음 이 라이브러리는 Rust 대상인 ``wasm32-nowknown-nown-nowknown`` 을 사용하여 Wasm으로 컴파일됩니다.
우리는 의존성 관리를 위해 Cargo_를 사용할 수 있습니다.

새 스마트 계약 프로젝트를 설정하려면 먼저 프로젝트 디렉토리를 만듭니다.
프로젝트 디렉토리 내에서 터미널에서 다음을 실행하십시오:

.. code-block:: console

   $cargo init --lib

이것은 몇 개의 파일과 디렉토리를 생성하여 기본 Rust 라이브러리 프로젝트를 설정합니다.
이제 디렉토리에 ``Cargo.toml`` 파일과 ``src`` 디렉토리 및 일부 숨겨진 파일이 포함됩니다.

Wasm을 만들려면화물에 올바른 ``크레이트 유형`` 을 알려야합니다.
``Cargo.toml`` 파일에 다음을 추가하면됩니다:

   [lib]
   crate-type = ["cdylib", "rlib"]

스마트 계약 표준 라이브러리 추가
==========================================

다음 단계는 ``concordium-std`` 를 종속성으로 추가하는 것입니다.
작고 효율적인 스마트 계약을 작성하기위한 절차 적 매크로와 함수를 포함하는 Rust 용 라이브러리입니다.

라이브러리는 ``Cargo.toml`` 을 열고 ``concordium-std= "*"`` 줄을 추가하여 추가됩니다 (바람직하게는`*` 를 `concordium-std`_ 의 최신 버전으로 대체)
``[dependencies]``섹션 ::

   [dependencies]
   concordium-std = "0.4"

상자 문서는 docs.rs_에서 찾을 수 있습니다.

.. note::

   이 상자의 수정 된 버전을 사용하려면 ``concordium-std`` 로 저장소를 복제하고 대신 디렉토리에 종속성 지점을 가져야합니다.
   다음을 ``Cargo.toml`` 에 추가하여 ::

      [dependencies]
      concordium-std = { path = "./path/to/concordium-std" }

.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _rustup: https://rustup.rs/
.. _repository: https://gitlab.com/Concordium/concordium-std
.. _docs.rs: https://docs.rs/crate/concordium-std/
.. _`concordium-std`: https://docs.rs/crate/concordium-std/

그게 다야! 이제 자신 만의 스마트 계약을 개발할 준비가되었습니다.