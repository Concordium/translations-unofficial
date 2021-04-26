.. _unit-test-contract-ko:

============================
Rust에서 계약 단위 테스트
============================

이 가이드는 Rust로 작성된 스마트 계약에 대한 단위 테스트를 작성하는 방법을 보여줍니다.
스마트 계약 Wasm 모듈을 테스트하려면 :ref:`local-simulate-ko` 를 참조하십시오.

Rust의 스마트 계약은 라이브러리로 작성되며 ``#[test]`` 속성으로 함수에 주석을 달아 라이브러리처럼 단위 테스트 할 수 있습니다.

.. code-block:: rust

    // contract code
    ...

    #[cfg(test)]
    mod test {

        #[test]
        fn some_test() { ... }

        #[test]
        fn another_test() { ... }
    }

테스트 실행은 ``cargo`` 를 사용하여 수행 할 수 있습니다:

.. code-block:: console

   $cargo test

기본적으로이 명령은 계약을 컴파일하고 로컬 대상 (대부분``x86_64``)에 대한 기계어 코드로 테스트하고 실행합니다.
이러한 종류의 테스트는 초기 개발 및 기능 정확성 테스트에 유용 할 수 있습니다.
포괄적 인 테스트를 위해서는 `Wasm32` 와 같은 대상 플랫폼을 포함하는 것이 중요합니다.
계약의 동작을 변경할 수있는 플랫폼 간에는 미묘한 차이가 많이 있습니다.
한 가지 차이점은 포인터의 크기에 관한 것입니다. 여기서`Wasm32`는 대부분의 플랫폼에서 일반적으로 사용되는 8 바이트가 아닌 4 바이트를 사용합니다.

단위 테스트를 작성
==================

단위 테스트는 일반적으로 상태를 설정하고, 코드 단위를 실행하고, 코드의 상태 및 출력에 대한 어설 션을 만드는 세 부분으로 구성된 구조를 따릅니다.

계약 함수가 ``#[init (..)]`` 또는 ``#[receive (..)]`` 를 사용하여 작성된 경우 이러한 함수를 단위 테스트에서 직접 테스트 할 수 있습니다.

.. code-block:: rust

   use concordium_std::*;

   #[init(contract = "my_contract", payable, enable_logger)]
   fn contract_init(
      ctx: &impl HasInitContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
   ) -> InitResult<State> { ... }

   #[receive(contract = "my_contract", name = "my_receive", payable, enable_logger)]
   fn contract_receive<A: HasActions>(
      ctx: &impl HasReceiveContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
      state: &mut State,
   ) -> ReceiveResult<A> { ... }

함수 인수에 대한 테스트 스텁은 ``test_infrastructure`` 라고하는 ``concordium-std`` 의 하위 모듈에서 찾을 수 있습니다.

.. seealso::

   자세한 정보와 예제는 concordium-std 의 상자 문서를 참조하십시오.

.. todo::

   단위 테스트 작성 방법 자세히보기

Wasm에서 테스트 실행
=====================

대부분의 경우 테스트를 네이티브 머신 코드로 컴파일하는 것으로 충분하지만 테스트를 Wasm으로 컴파일하고 노드에서 사용하는 정확한 인터프리터를 사용하여 실행할 수도 있습니다.
이것은 테스트 환경을 온 체인 실행 환경에 더 가깝게 만들고 경우에 따라 더 많은 버그를 잡을 수 있습니다.

개발 도구 ``cargo-concordium`` 에는 Wasm 용 테스트 러너가 포함되어 있습니다,
Concordium 노드에 제공된 것과 동일한 Wasm 인터프리터를 사용합니다.

.. seealso::

   ``cargo-concordium`` 설치 방법에 대한 안내는 :ref:`setup-tools-ko` 를 참조하세요.

단위 테스트는 ``#[test]`` 대신 ``#[concordium_test]`` 로 주석을 달아야하며 ``#[cfg (test)]`` 대신 ``#[concordium_cfg_test]`` 를 사용합니다 :

.. code-block:: rust

   // contract code
   ...

   #[concordium_cfg_test]
   mod test {

       #[concordium_test]
       fn some_test() { ... }

       #[concordium_test]
       fn another_test() { ... }
   }

``#[concordium_test]`` 매크로는 Wasm에서 실행되도록 테스트를 설정합니다, ``concordium-std`` 가 ``wasm-test`` 기능으로 컴파일 될 때,
그렇지 않으면 ``#[test]`` 처럼 동작하도록 폴백됩니다, 즉,``cargo test`` 를 사용하여 네이티브 코드를 대상으로하는 단위 테스트를 실행할 수 있습니다.

마찬가지로 ``#[concordium_cfg_test]`` 매크로는 ``wasm-test`` 로 ``concordium-std`` 를 빌드 할 때 모듈을 포함합니다. 그렇지 않으면 ``#[test]`` 처럼 동작합니다.
빌드에 테스트를 포함 할시기를 제어 할 수 있습니다.

이제 다음을 사용하여 테스트를 빌드하고 실행할 수 있습니다:

.. code-block:: console

   $cargo concordium test

이 명령은``concordium-std``에 대해 활성화 된``wasm-test ''기능으로 Wasm에 대한 테스트를 컴파일하고``cargo-concordium ''의 테스트 실행기를 사용합니다.

.. warning::

   ``panic!`` 의 오류 메시지와 ``assert!`` 의 다양한 변형은 Wasm으로 컴파일 할 때 *표시되지 않습니다*.

   대신 ``fail!`` 및 ``claim!`` 변형을 사용하여 테스트 할 때 어설 션을 수행합니다. 이러한 변형은 테스트에 실패하기 *전에* 테스트 실행자에게 오류 메시지를 다시보고하기 때문입니다.
    둘 다 ``concordium-std`` 의 일부입니다.

.. todo::

   상자가 게시 될 때 concordium-std: docs.rs/concordium-std 링크를 사용하십시오.
