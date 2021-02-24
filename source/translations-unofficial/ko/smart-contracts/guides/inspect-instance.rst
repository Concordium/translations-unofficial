.. _inspect-instance-ko:

=====================================
스마트 계약 인스턴스를 검사합니다.
=====================================

이 가이드는 스마트 계약 인스턴스를 검사하는 방법을 보여줍니다.
인스턴스를 검사하면 이름, 소유자, 모듈 참조, 잔액, 상태 및 수신 기능이 표시됩니다.

준비
===========

최신 :ref:`Concordium 소프트웨어 <downloads>` 를 사용하여 :ref:`노드 실행 <run-a-node>` 를 수행하고 있고 검사 할 스마트 계약 인스턴스가 체인에 있는지 확인합니다.

.. seealso::
   스마트 계약 모듈을 배포하는 방법은 :ref:`deploy-module-ko` 및 인스턴스를 만드는 방법 :ref:`initialize-contract-ko` 를 참조하십시오.

점검
==========

주소 인덱스가 ``0`` 인 스마트 계약 인스턴스에 대한 정보를 검사하거나 표시하려면 다음 명령을 실행하십시오:

.. code-block:: console

   $concordium-client contract show 0

출력은 다음과 유사해야합니다:

.. code-block:: console

   Contract:        my_contract
   Owner:           '4Lh8CPhbL2XEn55RMjKii2XCXngdAC7wRLL2CNjq33EG9TiWxj' (default)
   ModuleReference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'
   Balance:         0.000000 GTU
   State:
       {
           "first_field": 0,
           "second_field": 42
       }
   Functions:
    - receive_one
    - receive_two

.. seealso::

   계약 인스턴스 주소에 대한 자세한 내용은 :ref:`references-on-chain` 을 참조하세요.

검사의 세부 수준은 ``show`` 명령이 :ref:`contract schema <contract-schema-ko>` 에 액세스 할 수 있는지 여부에 따라 다릅니다.
스키마가 포함 된 경우 암시 적으로 사용됩니다. 그렇지 않으면 ``--schema /path/to/schema.bin`` 매개 변수를 사용하여 스키마를 제공 할 수 있습니다.

.. note::

   ``--schema`` 매개 변수를 사용하여 제공된 스키마 파일은 포함 된 스키마보다 우선합니다.

.. seealso::

   :ref:`스마트 계약 스키마를 사용하는 이유와 방법에 대해 자세히 알아보십시오 <contract-schema-ko>`.
