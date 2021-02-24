.. Should answer:
    - Why write a smart contract using rust?
    - What are the pieces needed to write a smart contract in rust?
        - State
            - Serialized
            - Schema
        - Init
        - Receive
    - What sort of testing is possible
    - Best practices?
        - Ensure 0 amount
        - Don't panic
        - Avoid heavy calculations

.. _writing-smart-contracts-ko:

====================================
Rust에서 스마트 계약 개발
====================================

Concordium 블록 체인에서 스마트 계약은 Wasm 모듈로 배포됩니다.
그러나 Wasm은 주로 컴파일 대상으로 설계되었으며 손으로 작성하기에는 편리하지 않습니다.
대신 우리는 Wasm으로의 컴파일을 잘 지원하는 Rust_ 프로그래밍 언어로 스마트 계약을 작성할 수 있습니다.

스마트 계약은 Rust로 작성할 필요가 없습니다. 이것은 우리가 제공하는 첫 번째 SDK입니다.
수동으로 작성된 Wasm 또는 C, C ++, AssemblyScript_ 등에서 컴파일 된
:ref:`Wasm은 우리가 <wasm-limitations-ko>` 를 부과하는 Wasm 제한을 준수하는 한 체인에서 동일하게 유효합니다.

.. seealso::

   아래에 설명 된 함수에 대한 자세한 내용은 Rust에서 Concordium 블록 체인에 스마트 계약을 작성하기위한 concordium_std_API를 참조하세요.

.. seealso::

   스마트 계약 모듈에 대한 자세한 내용은 :ref:`contract-module` 을 참조하십시오.

스마트 계약 모듈은 Rust에서 라이브러리 상자로 개발 된 다음 Wasm으로 컴파일됩니다.
올바른 내보내기를 얻으려면 매니페스트 파일에서 `crate-type` 속성을``[ "cdylib", "rlib"]``로 설정해야합니다:

.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...

``concordium_std`` 를 사용하여 스마트 계약 작성
======================================================

``concordium_std`` 크레이트를 사용하는 것이 좋습니다. 이는 스마트 계약 모듈을 개발하고 호스트 함수를 호출하는 데 좀 더 Rust와 유사한 경험을 제공합니다.

크레이트를 사용하면 각각 ``#[init(...)]`` 및 ``#[receive (...)]`` 주석이 달린 간단한 Rust 함수로 초기화 및 수신 함수를 작성할 수 있습니다.

다음은 카운터를 구현하는 스마트 계약의 예입니다:

.. code-block:: rust

   use concordium_std::*;

   type State = u32;

   #[init(contract = "counter")]
   fn counter_init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let state = 0;
       Ok(state)
   }

   #[receive(contract = "counter", name = "increment")]
   fn contract_receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       ensure!(ctx.sender().matches_account(&ctx.owner()); // Only the owner can increment
       *state += 1;
       Ok(A::accept())
   }

주의 사항 여러 가지가 있습니다 :

.. todo::

   - 읽기 쉬운 방법으로 요구 사항을 작성합니다 (예 : 단락을 하위 글 머리 기호로 분할).
   - 이러한 요구 사항은이 예제의 일부가 아닌 어딘가에 작성된 사양의 일부 여야합니다.

- 기능의 유형 :

  * 초기화 함수는 ``&impl HasInitContext -> InitResult<MyState>`` 유형이어야합니다.
    여기서 ``MyState`` 는 ``Serialize`` 특성을 구현하는 유형입니다.
  * 수신 함수는 ``A: HasActions`` 유형 매개 변수,``&impl HasReceiveContext`` 및
    ``&mut MyState`` 매개 변수를 취하고 ``ReceiveResult<A>`` 를 반환해야합니다.

- 주석 ``#[init(contract = "counter")]`` 는 ``counter`` 라는 계약의 init 함수로 적용되는 함수를 표시합니다.
  구체적으로 말하자면,이 매크로는이면에서이 매크로가 필요한 서명과 ``init_counter`` 라는 이름으로 내 보낸 함수를 생성한다는 것을 의미합니다.

- ``#[receive(contract = "counter", name = "increment")]`` 는 직접 조작 할 상태를 역 직렬화하고 제공합니다.
  이면에서이 주석은 필요한 서명이있는 ``counter.increment`` 라는 이름으로 내 보낸 함수를 생성합니다,
  그리고 상태를 필수 유형 ``State`` 로 역 직렬화하는 모든 상용구를 수행합니다.

.. note::

   그 직렬화 복원 비용이없는 것은 아니다 참고, 경우에 사용자는 호스트 기능의 사용을보다 세밀하게 제어 할 수 있습니다.
   이러한 사용 사례에서 주석은 오버 헤드가 적지 만 사용자에게 더 많은 것을 요구하는``low_level`` 옵션을 지원합니다.

.. todo::

   - 저수준 설명
   - 위의 노트에서 호스트 기능을 사용하기 전에 개념을 소개하십시오.


직렬화 가능한 상태 및 매개 변수
-------------------------------------------

.. todo:: 상태가``File`` 과 유사하게 노출된다는 것이 무엇을 의미하는지 명확히하십시오. 가급적이면 ``파일``을 참조하지 않습니다.

온 체인에서 인스턴스의 상태는 바이트 배열로 표시되고 Rust 표준 라이브러리의 ``File`` 인터페이스와 유사한 인터페이스에 노출됩니다.

이것은 (비) 직렬화 기능을 포함하는``직렬화`` 특성을 사용하여 수행 할 수 있습니다.

``concordium_std`` 크레이트에는 Rust 표준 라이브러리에있는 대부분의 유형에 대한이 특성과 구현이 포함되어 있습니다.
또한 사용자 정의 구조체 및 열거 형에 대한 특성을 파생하기위한 매크로도 포함됩니다.

.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

매개 변수가 함수를 초기화하고 수신하는데도 마찬가지입니다.

.. note::

   엄밀히 말하면 바이트를 매개 변수 유형으로 역 직렬화하기 만하면됩니다.
   그러나 단위 테스트를 작성할 때 유형을 직렬화 할 수있는 것이 편리합니다.

.. _working-with-parameters-ko:

매개 변수로 작업합니다.
-----------------------

초기화 및 수신 함수에 대한 매개 변수는 인스턴스 상태와 같이 바이트 배열로 표시됩니다.
바이트 배열을 직접 사용할 수 있지만 구조화 된 데이터로 역 직렬화 할 수도 있습니다.

매개 변수를 역 직렬화하는 가장 간단한 방법은 `Get`_ 트레이 트의 `get()`_ 함수를 사용하는 것입니다.

예를 들어, 강조 표시된 줄에서 ``ReceiveParameter`` 매개 변수가 역 직렬화되는 다음 계약을 참조하십시오:

.. code-block:: rust
   :emphasize-lines: 24

   use concordium_std::*;

   type State = u32;

   #[derive(Serialize)]
   struct ReceiveParameter{
       should_add: bool,
       value: u32,
   }

   #[init(contract = "parameter_example")]
   fn init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let initial_state = 0;
       Ok(initial_state)
   }

   #[receive(contract = "parameter_example", name = "receive")]
   fn receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let parameter: ReceiveParameter = ctx.parameter_cursor().get()?;
       if parameter.should_add {
           *state += parameter.value;
       }
       Ok(A::accept())
   }

위의 수신 함수는``value`` 이 필요하지 않은 경우, 즉 ``should_add`` 가 ``false`` 인 경우에도 역 직렬화한다는 점에서 비효율적입니다.

더 많은 제어와이 경우 효율성을 높이기 위해 `Read`_ 트레이 트를 사용하여 매개 변수를 역 직렬화 할 수 있습니다::

.. code-block:: rust
   :emphasize-lines: 7, 10

   #[receive(contract = "parameter_example", name = "receive_optimized")]
   fn receive_optimized<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let mut cursor = ctx.parameter_cursor();
       let should_add: bool = cursor.read_u8()? != 0;
       if should_add {
           // Only decode the value if it is needed.
           let value: u32 = cursor.read_u32()?;
           *state += value;
       }
       Ok(A::accept())
   }

``value`` 는 ``should_add`` 가 ``true`` 인 경우에만 역 직렬화됩니다.
이 예에서는 효율성 향상이 미미하지만 더 복잡한 예에서는 상당한 영향을 미칠 수 있습니다.

``cargo-concordium`` 으로 스마트 계약 모듈 구축
==========================================================

Rust 컴파일러는``wasm32-unknown-unknown`` 타겟을 사용하여 Wasm으로 컴파일하는 것을 잘 지원합니다.
그러나 ``--release`` 로 컴파일하는 경우에도 결과 빌드에는 사용자 지정 섹션에 많은 디버그 정보 섹션이 포함됩니다,
온 체인 스마트 계약에는 유용하지 않습니다.

빌드를 최적화하고 스키마 포함과 같은 새로운 기능을 허용하려면 ``cargo-concordium`` 을 사용하여 스마트 계약을 구축하는 것이 좋습니다.

.. seealso::

   ``cargo-concordium`` 을 사용하여 빌드하는 방법에 대한 지침은 :ref:`compile-module` 을 참조하십시오.


스마트 계약을 테스트합니다.
==============================

스텁을 사용한 단위 테스트입니다.
---------------------------------

계약 호출 시뮬레이션
--------------------------

모범 사례
==============

당황하지 마세요
-----------------------

.. todo::

   대신 트랩을 사용하십시오.

블랙홀 생성 방지
--------------------------

스마트 계약은 GTU 전송량을 사용하는 데 필요하지 않으며,
기본적으로 스마트 계약은 누군가가 GTU를 보낼 경우 인스턴스의 잔액을 비우는 동작을 정의하지 않습니다.
이 GTU는 영원히 *분실* 되며 복구 할 방법이 없습니다.

따라서 GTU를 다루지 않는 스마트 계약에 대한 좋은 관행입니다.
전송 된 GTU 양이 0인지 확인하고 그렇지 않은 호출을 거부합니다.

무거운 계산을 오프 체인으로 이동
---------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Get: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Read: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/
