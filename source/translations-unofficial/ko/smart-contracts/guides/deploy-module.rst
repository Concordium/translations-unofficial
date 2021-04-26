.. _deploy-module-ko:

==============================
스마트 계약 모듈을 배포
==============================

이 가이드는 *온 체인* 스마트 계약 모듈을 배포하는 방법과 이름을 지정하는 방법을 보여줍니다.

준비
===========

최신 :ref:`Concordium 소프트웨어 <downloads>` 를 사용하여 :ref:`노드 실행 <run-a-node>` 인지 확인하고
배포 할 준비가 된 :ref:`smart-contract 모듈 <setup-tools-ko>` 가 있습니다.

스마트 컨트랙트 모듈 구축은 거래 형태로 이루어지기 때문에,
또한 GTU가 충분한 계좌로 거래대금을 지불할 수 있는 ``cordium-clien`` 설정이 필요합니다.

.. note::

   거래 비용은 스마트 계약 모듈의 크기에 따라 다릅니다.
   ``concordium-client`` 는 거래를 실행하기 전에 비용을 표시하고 확인을 요청합니다.

전개
==========

이름이 account-name 인 계정을 사용하여 스마트 계약 모듈 ``my_module.wasm`` 을 배포하려면 다음 명령을 실행하십시오.

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. note::

   --sender 옵션은 계정 "default" 를 사용하는 경우 생략 할 수 있습니다. 간결성을 위해 다음에서 그렇게 할 것입니다.

성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'.

스마트 계약 인스턴스를 만들 때 사용되는 모듈 참조를 기록해 둡니다.

.. seealso::

   배포 된 모듈에서 스마트 계약을 초기화하는 방법에 대한 가이드는 :ref:`initialize-contract-ko` 를 참조하세요.

   모듈 참조에 대한 자세한 내용은 :ref:`references-on-chain` 을 참조하세요.

.. _naming-a-module-ko:

모듈 이름 지정
===============

모듈에 로컬 별칭 또는 *name* 을 지정할 수 있으므로 참조하기가 더 쉽습니다.
이름은 ``concordium-client`` 에 의해 로컬로만 저장되며 온 체인에서는 보이지 않습니다.

.. seealso::

   이름 및 기타 로컬 설정이 저장되는 방법과 위치에 대한 설명은 :ref:`local-settings` 를 참조하십시오.

배포 중에 이름을 추가하려면 ``--name`` 매개 변수가 사용됩니다.
여기에서 모듈 이름을 ``my_deployed_module`` 로 지정합니다;

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

성공하면 출력은 다음과 유사해야합니다:

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

모듈은 ``name`` 명령을 사용하여 이름을 지정할 수도 있습니다.
참조를 사용하여 배포 된 모듈의 이름을 지정하려면
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2`` 로서
``some_deployed_module``, 다음 명령을 실행하십시오:

.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

출력은 다음과 유사해야합니다:

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module'.
