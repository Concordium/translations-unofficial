.. _interact-instance-ko:

=======================================
스마트 계약 인스턴스와 상호 작용
=======================================

이 가이드는 스마트 계약 인스턴스와 상호 작용하는 방법을 보여줍니다. 즉, 인스턴스 상태를 업데이트하는 수신 기능을 트리거하는 것을 의미합니다.

준비
===========

최신 :ref:`Concordium 소프트웨어 <다운로드>` 를 사용하여 :ref:`노드 실행 <run-a-node>` 를 확인하고 검사 할 스마트 계약 인스턴스가 체인에 있는지 확인합니다.

.. seealso::
   스마트 계약 모듈을 배포하는 방법은 :ref:`deploy-module-ko` 및 인스턴스를 만드는 방법 :ref:`initialize-contract-ko` 를 참조하십시오.

스마트 계약과의 상호 작용은 거래이기 때문에 거래 비용을 지불하기에 충분한 GTU가있는 계정으로 ``concordium-client`` 를 설정해야합니다.

.. note::

   이 트랜잭션의 비용은 수신 기능에 전송 된 매개 변수의 크기와 기능 자체의 복잡성에 따라 다릅니다.

상호 작용
===========

최대 1000개의 에너지를 사용할 수 있도록 허용하면서 매개 변수 없는 수신 함수 ``my_receive`` 를 사용하여 주소 색인 ``0``
으로 인스턴스를 업데이트하려면 다음 명령을 실행합니다:

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

성공한 경우 출력은 다음과 같아야 합니다:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

JSON 형식으로 매개 변수 전달
---------------------------------

:ref:`스마트 계약 스키마 <contract-schema-ko>` 가 파일로 제공되거나 모듈에 포함 된 경우 JSON 형식의 매개 변수를 전달할 수 있습니다.
스키마는 JSON을 바이너리로 직렬화하는 데 사용됩니다.

.. seealso::

   :ref:`스마트 계약 스키마를 사용하는 이유와 방법에 대해 자세히 알아보십시오 <contract-schema-ko>`.

수신 기능을 사용하여 주소 인덱스가 ``0`` 인 인스턴스를 업데이트하려면 ``my_parameter_receive``
매개 변수 파일 ``my_parameter.json`` JSON 형식으로 다음 명령을 실행합니다:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json

성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

그렇지 않으면 문제를 설명하는 오류가 표시됩니다.
일반적인 오류는 다음 섹션에서 설명합니다.

.. seealso::

   계약 인스턴스 주소에 대한 자세한 내용은 :ref:`references-on-chain` 을 참조하세요.

.. note::

   JSON 형식으로 제공된 매개 변수가 스키마에 지정된 유형과 일치하지 않으면 오류 메시지가 표시됩니다. 예를 들면 :

    .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. note::

   주어진 모듈에 포함 된 스키마가 포함되지 않은 경우 ``--schema /path/to/schema.bin`` 매개 변수를 사용하여 제공 할 수 있습니다.

.. note::

   GTU는 ``--amount AMOUNT`` 매개 변수를 사용하여 업데이트 중에 계약으로 전송할 수도 있습니다.

이진 형식으로 매개 변수 전달
-----------------------------------

이진 형식으로 매개 변수를 전달할 때 :ref:`계약 스키마 <contract-schema-ko>` 가 필요하지 않습니다.

이진 형식의 매개 변수 파일 ``my_parameter.bin`` 과 함께 수신 함수 ``my_parameter_receive`` 를 사용하여 주소 인덱스가 ``0`` 인 인스턴스를 업데이트하려면,
다음 명령을 실행하십시오:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. seealso::

   스마트 계약에서 매개 변수로 작업하는 방법에 대한 정보는 :ref:`working-with-parameters` 를 참조하십시오.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
