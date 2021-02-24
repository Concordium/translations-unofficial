.. _SchemaType을 구현하는 유형 목록: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. _build-schema-ko:

=======================
계약 스키마 빌드
=======================

이 가이드는 ``cargo-concordium`` 을 사용하여 스마트 계약 스키마를 빌드하는 방법, 파일로 내보내는 방법 및/또는 스키마를 스마트 계약 모듈에 포함하는 방법을 보여줍니다.

준비
===========

먼저 ``cargo-concordium`` 이 설치되어 있는지 확인하고 그렇지 않은 경우 :ref:`setup-tools-ko` 가 도움이 될 것입니다.

또한 스키마를 구축하려는 스마트 계약의 Rust 소스 코드가 필요합니다.

스키마에 대한 계약 설정
===============================

계약 스키마를 구축하기 위해, 우리는 먼저 스키마를 구축하기위한 우리의 스마트 계약을 준비해야합니다.

스키마에 포함 할 스마트 계약 부분을 선택할 수 있습니다.
옵션은 계약 상태 및/또는 초기화 및 수신 기능의 각 매개 변수에 대한 스키마를 포함하는 것입니다.

스키마에 포함시키고자 하는 모든 유형은 ``SchemaType`` 특성을 구현해야 합니다.
이는 모든 기본 유형과 일부 다른 유형에 대해 이미 수행되었습니다 (`SchemaType을 구현하는 유형 목록`_ 참조).
대부분의 다른 경우에는 ``#[derive(SchemaType)]`` 을 사용하여 자동으로 수행 할 수도 있습니다::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

``SchemaType`` 트레이 트를 수동으로 구현하려면 하나의 함수 만 지정하면됩니다.
이것은 ``schemaType`` 에 대한 게터로, 기본적으로이 유형이 바이트로 표시되는 방법과이를 나타내는 방법을 설명합니다.

.. todo::

   수동으로 여기에서에 ``SchemaType`` 및 링크를 구현하는 방법을 보여주는 예제를 만듭니다.

계약상태포함
------------------------

생성 및 계약 상태에 대한 스키마를 포함하려면, 우리는 ``#[contract_state(contract = ...)]`` 매크로 ::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

또는 계약 상태가 이미 ``SchemaType`` 을 구현하는 유형 인 경우 더 간단합니다 (예: u32)::

   #[contract_state(contract = "my_contract")]
   type State = u32;

함수 매개 변수 포함
-----------------------------

초기화 및 수신 함수의 매개 변수에 대한 스키마를 생성하고 포함하기 위해 ``#[init(..)]``- 및 ``#[receive(..)]`` 에 대한 선택적 ``parameter`` 속성을 설정합니다. -매크로 ::

   #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }

스키마를 구축
===================

이제, 우리는 ``cargo-concordium`` 를 사용하여 실제 스키마를 구축 할 준비가되어, 우리는 포함 스키마에 대한 옵션이 있습니다 및/또는 파일에 스키마를 작성합니다.

.. seealso::

   선택할 항목에 대한 자세한 내용은 :ref:`여기 <contract-schema-which-to-choose-ko>` 를 참조하세요.

임베딩 스키마
--------------------

스키마를 스마트 계약 모듈에 임베드하기 위해 빌드 명령에 ``--schema-embed`` 를 추가합니다.

.. code-block:: console

   $cargo concordium build --schema-embed

성공하면 명령 출력에 스키마의 총 크기 (바이트)가 표시됩니다.

스키마 파일 출력
------------------------

스키마를 파일로 출력하려면 ``--schema-out=FILE`` 을 사용할 수 있습니다. 여기서 ``FILE`` 은 생성 할 파일의 경로입니다:
.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"
