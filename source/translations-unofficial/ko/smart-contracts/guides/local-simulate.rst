.. _local-simulate-ko:

===================================
계약 기능을 로컬에서 시뮬레이션
===================================

이 가이드는 특정 컨텍스트 및 상태에서 일부 초기화 또는 Wasm 스마트 계약 모듈에서 함수를 수신하는 것을 로컬로 시뮬레이션하는 방법에 관한 것입니다.
이 시뮬레이션은 특정 시나리오에서 스마트 계약 및 결과를 검사하는 데 유용합니다.

.. seealso::

   자동화 된 단위 테스트에 대한 가이드는 :ref:`unit-test-contract-ko` 를 참조하세요.

준비
===========

가이드를 따르지 않으면 ``cargo-concordium`` 이 설치되어 있는지 확인하십시오 :ref:`setup-tools-ko`.
시뮬레이션을 위해 Wasm에 스마트 계약 모듈이 필요합니다.

.. todo::

   스키마 항목이 제자리에있을 때 나머지를 작성하십시오.

인스턴스화 시뮬레이션
========================

``cargo-concordium`` 을 사용하여 스마트 계약 인스턴스의 인스턴스화를 시뮬레이션하려면 다음 명령을 실행하십시오:

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` (``--context`` 매개 변수와 함께 사용)
체인의 현재 상태와 같은 컨텍스트 정보를 포함하는 파일입니다. 트랜잭션의 발신자 및이 기능을 호출 한 계정
이 컨텍스트의 예는 다음과 같습니다:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "initOrigin": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
   }

.. seealso::

   컨텍스트에 대한 참조는 :ref:`simulate-context`를 참조하십시오.


업데이트 시뮬레이션
==========================

``cargo-concordium`` 을 사용하여 계약 스마트 계약 인스턴스에 대한 업데이트를 시뮬레이션하려면 다음을 실행하십시오:

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin

``receive-context.json`` (``--context`` 매개 변수와 함께 사용됨)은 체인의 현재 상태와 같은 컨텍스트 정보를 포함하는 파일입니다.
트랜잭션의 발신자,이 함수를 호출 한 계정 및 현재 메시지를 보낸 계정 또는 주소.
이 컨텍스트의 예는 다음과 같습니다:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "invoker": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "selfAddress": {"index": 0, "subindex": 0},
       "selfBalance": "0",
       "sender": {
           "type": "account",
           "address": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
       },
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
       "owner": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
   }

.. seealso::

   컨텍스트에 대한 참조는 :ref:`simulate-context` 를 참조하십시오.
