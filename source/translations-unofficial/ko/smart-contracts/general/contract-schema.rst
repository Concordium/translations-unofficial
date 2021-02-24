.. Should answer:
..
.. - Why should I use a schema?
.. - What is a schema?
.. - Where to use a schema?
.. - How is a schema embedded?
.. - Should I embed or write to file?
..

.. _`custom section`: https://webassembly.github.io/spec/core/appendix/custom.html
.. _`implementation in Rust`: https://github.com/Concordium/concordium-contracts-common/blob/main/src/schema.rs

.. _contract-schema-ko:

======================
스마트 계약 스키마
======================

스마트 계약 스키마는 더 구조화 된 표현으로 바이트 표현하는 방법에 대한 설명입니다.
스마트 계약 인스턴스의 상태를 표시하고 JSON과 같은 구조화 된 표현을 사용하여 매개 변수를 지정할 때 외부 도구에서 사용할 수 있습니다.

.. seealso::

   Rust에서 스마트 계약 모듈 용 스키마를 빌드하는 방법에 대한 지침은 :ref:`build-schema-ko` 를 참조하세요.

왜 계약 스키마를 사용
=========================

인스턴스 상태 및 초기화 및 수신 함수에 전달 된 매개 변수와 같은 블록 체인의 데이터는 일련의 바이트로 직렬화됩니다.
직렬화는 사람의 가독성보다는 효율성을 위해 최적화되었습니다.

.. todo::

   이해하기가 다소 어려울 수있는이 하위 섹션을 다시 작성; 특히, 아마도 그냥 편의를 위해 말
   사용자는 데이터를 직렬화 해제하는 방법을 설명하는 스키마도 제공하는 한 직렬화되지 않은 데이터를 함수에 전달할 수 있습니다.

일반적으로 이러한 바이트에는 구조가 있으며이 구조는 계약 기능의 일부로 스마트 계약에 알려져 있습니다,
그러나 이러한 기능 외에는 바이트를 이해하는 것이 어려울 수 있습니다.
특히 컨트랙트 인스턴스의 복잡한 상태를 검사하거나 스마트 컨트랙트 함수에 복잡한 매개 변수를 전달할 때 그렇습니다.
후자의 경우 바이트는 구조화 된 데이터에서 직렬화되거나 수동으로 작성되어야합니다.

바이트의 수동 구문 분석을 방지하기위한 솔루션은 바이트에서 구조를 만드는 방법을 설명하고 외부 도구에서 사용할 수있는 *스마트 계약 스키마* 에이 정보를 캡처하는 것입니다.

.. note::

   ``concordium-client`` 도구는 스키마를 사용하여 :ref:`JSON 매개 변수를 직렬화<init-passing-parameter-json-ko>`하고 계약 인스턴스의 상태를 JSON으로 역 직렬화 할 수 있습니다.

그런 다음 스키마는 체인에 배포되는 스마트 계약 모듈에 포함되거나 파일에 기록되고 오프 체인을 통해 전달됩니다.

파일을 포함하거나 작성해야합니까?
====================================

계약 스키마를 포함해야하는지 또는 파일에 기록해야하는지 여부는 상황에 따라 다릅니다.

스키마를 스마트 계약 모듈에 임베딩하면 계약과 함께 스키마를 배포하여 올바른 스키마가 사용되고 있는지 확인하고 누구나 직접 사용할 수 있습니다.
단점은 스마트 계약 모듈의 크기가 커져 배포 비용이 더 많이 든다는 것입니다.
그러나 스마트 계약이 상태와 매개 변수에 대해 매우 복잡한 유형을 사용하지 않는 한, 스키마의 크기는 스마트 계약 자체의 크기에 비해 무시할 수있을 것입니다.

별도의 파일에 스키마가 있으면 배포 할 때 추가 바이트 비용을 지불하지 않고도 스키마를 가질 수 있습니다.
단점은 대신 다른 채널을 통해 스키마 파일을 배포 및 계약 사용자가 스마트 계약에 올바른 파일을 사용하고 있는지 확인해야한다는 것입니다.

스키마 형식입니다.
=======================

.. todo::

   사용자가 구현할 수있는 *모든* 추상 스키마 또는 Concordium에서 제공하는 특정 스키마에 대해 이야기하는지 명확히하십시오.
   그런 다음 둘 중 하나에 대해서만 이야기하거나 적어도 그 논의를 명확하게 분리하십시오.

스키마는 포함 할 수 있습니다

- 스마트 계약 모듈에 대한 구조 정보
- 스마트 계약 상태에 대한 설명
- 스마트 계약의 초기화 및 수신 기능을위한 매개 변수.

이러한 설명의 각각은 *스키마 유형* 라고합니다. 스키마 유형은 항상 스키마에 포함 할 선택 사항입니다.

현재 지원되는 스키마 유형은 Rust 프로그래밍 언어에서 일반적으로 사용되는 것에서 영감을 받았습니다:

.. code-block:: rust

   enum Type {
       Unit,
       Bool,
       U8,
       U16,
       U32,
       U64,
       I8,
       I16,
       I32,
       I64,
       Amount,
       AccountAddress,
       ContractAddress,
       Timestamp,
       Duration,
       Pair(Type, Type),
       List(SizeLength, Type),
       Set(SizeLength, Type),
       Map(SizeLength, Type, Type),
       Array(u32, Type),
       Struct(Fields),
       Enum(List (String, Fields)),
   }

   enum Fields {
       Named(List (String, Type)),
       Unnamed(List Type),
       Empty,
   }


여기서 ``SizeLength`` 는 ``List`` 와 같은 가변 길이 유형의 길이를 설명하는 데 사용되는 바이트 수를 설명합니다.

.. code-block:: rust

   enum SizeLength {
       One,
       Two,
       Four,
       Eight,
   }

스키마 유형이 바이트로 직렬화되는 방법에 대한 참조를 위해 독자는 `Implementation in Rust`_ 를 참조합니다.

.. _contract-schema-which-to-choose-ko:

쇄에 스키마를 임베딩
=================================

스키마는 Wasm 모듈의 `custom section`_ 기능을 사용하여 스마트 계약 모듈에 포함됩니다.
이것은 Wasm 모듈이 Wasm 모듈을 실행하는 의미에 영향을주지 않는 명명 된 바이트 섹션을 포함 할 수 있도록합니다.

모든 스키마는 ``concordium-schema-v1`` 이라는 하나의 사용자 지정 섹션에서 수집 및 추가됩니다.
이 컬렉션은 UTF-8로 인코딩 된 계약 이름과 계약 스키마 바이트를 포함하는 쌍 목록입니다.
