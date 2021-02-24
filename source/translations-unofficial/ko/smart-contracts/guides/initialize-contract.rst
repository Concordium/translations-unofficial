.. _initialize-contract-ko:

====================================
스마트 계약 인스턴스 초기화
====================================

이 가이드는 JSON 또는 바이너리 형식의 매개 변수를 사용하여 배포 된 스마트 계약 모듈에서 스마트 계약을 초기화하는 방법을 보여줍니다.
또한 인스턴스 이름을 지정하는 방법을 보여줍니다.

준비
===========

최신 :ref:`Concordium software<downloads>` 를 사용하여 :ref:`노드 실행 <run-a-node>`
이고 스마트 계약이 있는지 확인하십시오 :ref:`deployed <deploy-module- ko>` 일부 모듈 온 체인에서.

스마트 계약을 초기화하는 것은 거래이기 때문에 거래 비용을 지불하기에 충분한 GTU가있는 계정으로``concordium-client`` 를 설정해야합니다.

.. note::

   이 트랜잭션의 비용은 init 함수에 전송 된 매개 변수의 크기와 함수 자체의 복잡성에 따라 다릅니다.

초기화합니다.
==============

참조를 사용하여 배포 된 모듈에서 매개 변수없는 스마트 계약 ``my_contract`` 의 인스턴스를 초기화하려면
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` 1000까지 가능 NRG가 사용되는 반면,
다음 명령을 실행하십시오:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

성공하면 출력은 다음과 유사해야합니다:

.. todo::

   (문서 전반에 걸쳐) ``{"index": i, "subindex": s}`` 에서 ``<i, s>`` 에 대한 업데이트 주소 형식.

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

이 메시지는 표시된 주소로 새 온 체인 계약 인스턴스가 생성되었음을 의미합니다.

.. seealso::

   계약 초기화에 대해 더 깊이 이해하려면 :ref:`contract-instances-init-on-chain-ko` 를 참조하십시오.

   모듈 참조 및 인스턴스 주소에 대한 자세한 내용은 :ref:`references-on-chain` 을 참조하세요.

   모듈 참조를 직접 사용하는 것은 불편할 수 있습니다. 이름을 지정하려면 :ref:`naming-a-module` 을 참조하세요.

.. _init-passing-parameter-json-ko:

JSON 형식으로 매개 변수 전달
---------------------------------

:ref:`스마트 계약 스키마 <contract-schema-ko>` 가 파일로 제공되거나 모듈에 포함 된 경우 JSON 형식의 매개 변수를 전달할 수 있습니다.
스키마는 JSON을 바이너리로 직렬화하는 데 사용됩니다.

.. seealso::

   :ref:`스마트 계약 스키마를 사용하는 이유와 방법에 대해 자세히 알아보기 <contract-schema-ko>`.

   :ref:`매개 변수는 바이너리 형식으로도 전달할 수 있습니다. <init-passing-parameter-bin>`.

참조를 사용하여 모듈에서 계약 ``my_parameter_contract`` 의 인스턴스를 초기화하려면
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2``
JSON 형식의 매개 변수 파일 ``my_parameter.json`` 을 사용하여 다음 명령을 실행합니다:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

그렇지 않으면 문제를 설명하는 오류가 표시됩니다.
일반적인 오류는 다음 섹션에서 설명합니다.

.. note::

   JSON 형식으로 제공된 매개 변수가 스키마에 지정된 유형과 일치하지 않으면 오류 메시지가 표시됩니다.
   예를 들면 :

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

   GTU는 초기화 중에 ``--amount AMOUNT`` 매개 변수를 사용하여 계약 인스턴스로 전송할 수도 있습니다.

.. _init-passing-parameter-bin-ko:

이진 형식으로 매개 변수 전달
-----------------------------------

이진 형식으로 매개 변수를 전달할 때 :ref:`contract schema <contract-schema-ko>` 가 필요하지 않습니다.

참조를 사용하여 모듈에서 계약 ``my_parameter_contract`` 의 인스턴스를 초기화하려면
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2``
바이너리 형식의 매개 변수 파일 ``my_parameter.bin`` 을 사용하여 다음 명령을 실행합니다:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. seealso::

   스마트 계약에서 매개 변수로 작업하는 방법에 대한 정보는 :ref:`working-with-parameters` 를 참조하십시오.

.. _naming-an-instance-ko:

계약 인스턴스의 이름을 지정합니다.
==========================

계약 인스턴스에 로컬 별칭 또는 *name* 을 지정할 수 있으므로보다 쉽게 참조 할 수 있습니다.
이름은 ``concordium-client`` 에 의해 로컬로만 저장되며 온 체인에서는 보이지 않습니다.

.. seealso::

   이름 및 기타 로컬 설정이 저장되는 방법과 위치에 대한 설명은 :ref:`local-settings` 를 참조하십시오.

초기화 중에 이름을 추가하려면 ``--name`` 매개 변수가 사용됩니다.

여기에서는 배포 된 모듈에서 계약 ``my_contract`` 를 초기화합니다.
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2``
이름을 ``my_named_contract`` 로 지정합니다:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

계약 인스턴스는 ``name`` 명령을 사용하여 이름을 지정할 수도 있습니다.
주소 인덱스가 ``0`` 인 인스턴스의 이름을 ``my_named_contract`` 로 지정하려면 다음 명령을 실행합니다.

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. seealso::

   계약 인스턴스 주소에 대한 자세한 내용은 :ref:`references-on-chain` 을 참조하세요.

.. _parameter_cursor():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get(): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read(): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8
