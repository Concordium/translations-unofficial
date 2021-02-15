
.. _networkDashboardLink: https://dashboard.testnet.concordium.com/
.. _node-dashboard: http://localhost:8099
.. _Discord: https://discord.com/invite/xWmQ5tp

.. _become-a-baker-zh:

==================================
成为baker（创建块）
==================================

.. contents::
   :local:
   :backlinks: none


本节说明 烘焙师baker(即矿工) 是什么，baker 在网络中的角色以及如何成为baker 。

通过阅读本节，您将学到：

- 什么是 baker 及其相关概念。
- 如何升级您的节点成为 baker 。

成为 baker 的过程可以概括为以下步骤：

   1. 获取一个帐户和一些GTU。
   2. 获取一组 baker 密钥。
   3. 将 baker 密钥注册到该帐户。
   4. 使用 baker 密钥 启动节点。

完成这些步骤后，baker 节点将烘烤(bake)区块(即挖出区块)。如果烘焙区块被添加到链中，则该节点的 baker 将获得奖励。

.. note::

   在本节中，我们将使用名称 `bakerAccount` 作为将用于注册和管理 baker 的帐户的名称。

定义
===========

Baker
-----

积极参与网络并创建新区块被添加到链中的节点就是 *Baker（或正在烘烤）*。Baker收集，排序和验证区块中包含的交易，以维护区块链的完整性。Baker 对自己烘烤出的每个区块进行签名，以便网络的其他参与者可以检查和执行该块。

Baker 密钥
----------

每个 Baker 都有一组称为 *Baker* 密钥的加密密钥。节点使用这些密钥对它烘烤的块进行签名。为了烘焙出由特定 Baker 签名的块，该节点必须在加载其Baker密钥集合的情况下运行。

Baker 账户
-------------

每个帐户可以使用一组 Baker 密钥来注册 Baker。

每当 Baker 烘烤有效的区块（包含在链中）时，一段时间后关联的帐户就会收到网络的奖励。

Stake and lottery
-----------------

.. todo::

   - Link to release schedule.

该帐户可以将其GTU余额的一部分抵押到 *baker stake* 中，然后可以手动释放所有或部分抵押额。在 Baker 释放之前，质押的金额不能移动或转移。

.. note::

   如果某个帐户拥有部分来自"Release Schedule"类型转帐的金额，则即使该部分金额尚未释放，也可以质押。

为了被网络选择来烘烤一个区块，Baker必须参加一场全网 *lottery(抽奖活动)* ，在该抽奖活动中，中奖彩票(即被选中出块)的概率与所质押的代币数量大致成比例。

在计算和决定某个Baker是否可以进入Finalization Committee中时，同样需要质押代币。请参阅 **Finalization** 。

.. _epochs-and-slots-zh:

Epochs and slots
----------------

在Concordium区块链中，时间细分为slot(插槽)。插槽的持续时间固定在“创世纪”块(Genesis Block)中。在任何给定的分支上，每个插槽最多可以有一个块，但是可以在同一插槽中产生不同分支上的多个块。

.. todo::

   让我们添加一张图片。

在考虑奖励和其他与烘焙相关的概念时，我们使用 *epoch* 的概念作为时间单位，它定义了一个时间段，在这个时间段里当前Bakers和stakes固定不变。 *Epochs* 的时长早已经固定在 **“创世块”** 中。在测试网中， *epochs* 的持续时间均为 **1小时**。

开始烘烤
============

管理账户
-----------------

本节简要介绍了导入帐户的相关步骤。有关完整的说明，请参见： :ref:`managing_accounts`。

使用 :ref:`concordium_id` 中的app创建帐户。成功创建帐户后，导航至 **“更多”** 选项卡并选择 **“导出”** 即可获取包含帐户信息的JSON文件。

要将帐户导入工具链运行：

.. code-block:: console

   $concordium-client config account import <path/to/exported/file> --name bakerAccount

 ``concordium-client`` 将要求你输入密码以解密导出的文件然后会导入所有帐户(accounts)。用于 *交易签名* 的密钥和用于 *被屏蔽的转账(shielded transfers)* 的密钥，都会用同一个密码加密。

为 baker 创建密钥并注册
--------------------------------------------

.. note::

   对于此过程，该帐户需要拥有一些GTU，因此请确保在移动应用程序中请求该帐户的100 GTU空投。

每个帐户都有一个唯一的baker ID，该ID在注册baker时使用。该ID必须由网络提供，并且无法预先计算。必须将ID保存在赋予到节点的baker密钥文件中，以便它可以使用baker密钥创建块。 ``concordium-client`` 执行以下操作时，会自动填充此字段。

要创建一组新的密钥，请运行：

.. code-block:: console

  $concordium-client baker generate-keys <keys-file>.json

这里您可以为密钥文件选择一个任意名称。要在网络中注册密钥，您需要运行节点 :ref:`running a node <running-a-node-zh>` 并发送一笔 ``baker add`` 交易到网络：

.. code-block:: console

   $concordium-client baker add <keys-file>.json --sender bakerAccount --stake <amountToStake> --out <concordium-data-dir>/baker-credentials.json

命令中的占位符解释如下：

- 将 ``<amountToStake>`` 替换成面包师baker初始质押的GTU代币数量
- 将 ``<concordium-data-dir>`` 替换成下面的数据目录：
  * 在Linux 和 MacOS 上： ``~/.local/share/concordium``
  * 在 Windows 上： ``%LOCALAPPDATA%\\concordium`` 。

（输出文件名应保留 ``baker-credentials.json`` ）。

这里concordium-client提供了一个 ``--no-restake`` 命令指示符，指定了它就可以避免自动将奖励添加到 baker 的抵押金额上。这指示符会在下文的 **Restaking the earnings** 部分中描述。

为了使用这些 baker 密钥启动节点并开始生成块，您首先需要关闭当前正在运行的节点（在运行该节点的终端上 通过 按 ``Ctrl + C`` 或 使用 ``concordium-node-stop`` 可执行文件）。

将文件放置在适当的目录中之后（已在上一个命令中指定输出文件时中完成），然后使用 ``concordium-node`` 再次启动节点。当该baker被包含在当前epoch时段的网络baker集合中时，该节点将自动开始烘焙(挖矿出块)。

此更改将立即执行，并且在 *添加baker的那笔交易被打包入块中的那个epoch* 之后的那个epoch结束时生效。

.. table:: Timeline: adding a baker

   +-------------------------------------------+-----------------------------------------+-----------------+
   |                                           | When transaction is included in a block | After 2 epochs  |
   +===========================================+=========================================+=================+
   | Change is visible by querying the node    |  ✓                                      |                 |
   +-------------------------------------------+-----------------------------------------+-----------------+
   | Baker is included in the baking committee |                                         | ✓               |
   +-------------------------------------------+-----------------------------------------+-----------------+

.. note::

  如果在epoch E的某个区块中包含添加 baker 的事务，则在epoch (E + 2) 开始时，该 baker 将被视为烘焙委员会的一部分。

管理baker
==================

检查 baker 的状态及其lottery(中奖，即被选中出块)能力
------------------------------------------------------

为了查看节点是否正在烘焙，您可以查看显示信息中提供的不同精确度的各种来源。

- 在 **网络仪表板** 中，您的节点将在 ``Baker`` 列中显示其BakerID 。
- 使用 ``concordium-client`` 您可以检查当前Baker的列表以及他们持有的相关代币质押数量，即他们的“中奖”能力。代币质押量越高(低)，对应的Baker被选中来烘烤一个块的可能性越高(低)。

  .. code-block:: console

     $concordium-client consensus show-parameters --include-bakers
     Election nonce:      07fe0e6c73d1fff4ec8ea910ffd42eb58d5a8ecd58d9f871d8f7c71e60faf0b0
     Election difficulty: 4.0e-2
     Bakers:
                                  Account                       Lottery power
             ----------------------------------------------------------------
         ...
         34: 4p2n8QQn5akq3XqAAJt2a5CsnGhDvUon6HExd2szrfkZCTD4FX   <0.0001
         ...

- 你可以使用 ``concordium-client`` 检查帐户是否已注册baker以及该baker当前已抵押的金额。

  .. code-block:: console

     $./concordium-client account show bakerAccount
     ...

     Baker: #22
      - Staked amount: 10.000000 GTU
      - Restake earnings: yes
     ...

- 如果账户的代币质押量足够大，并且节点加载了该账户的baker密钥，则该baker最终将产生区块，您可以在移动钱包中看到该帐户正在收到烘烤奖励，如下图所示：

  .. image:: images/bab-reward.png
     :align: center
     :width: 250px


更新抵押金额
--------------------------

要更新baker的质押请运行：

.. code-block:: console

   $concordium-client baker update-stake --stake <newAmount> --sender bakerAccount

要注意的是，修改质押代币量将改变选择baker烘烤块的概率。

当baker **第一次质押代币或增加质押时**，该更改将在链上执行，并在该交易被打包入区块中（可以通过看到 ``concordium-client account show bakerAccount``）后立即可见，并在此之后的2个epochs后生效。
.. table:: 时间轴: 增加质押

   +----------------------------------------+-----------------------------------------+----------------+
   |                                        | 当交易包含在区块中                      | 2个时期后      |
   +========================================+=========================================+================+
   | 通过查询节点可以看到更改               | ✓                                       |                |
   +----------------------------------------+-----------------------------------------+----------------+
   | Baker 使用新股份                       |                                         | ✓              |
   +----------------------------------------+-----------------------------------------+----------------+

当baker **减少质押代币量时** ，更改将需要 *2 + bakerCooldownEpochs* 个epochs后才能生效。一旦将该交易打包入块中，就可以在链上看到更改，可以通过以下方式进行查询 ``concordium-client account show bakerAccount`` ：

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU to be updated to 20.000000 GTU at epoch 261  (2020-12-24 12:56:26 UTC)
    - Restake earnings: yes

   ...

.. table:: 时间线：减少质押量

   +----------------------------------------+-----------------------------------------+----------------------------------------+
   |                                        |当交易包含在区块中                       | 2 +baker后冷却史时代                   |
   +========================================+=========================================+========================================+
   | 通过查询节点可以看到更改               | ✓                                       |                                        |
   +----------------------------------------+-----------------------------------------+----------------------------------------+
   | Baker使用新股份                        |                                         | ✓                                      |
   +----------------------------------------+-----------------------------------------+----------------------------------------+
   | 质押可以再次减少或Baker可以去除        |                                         |                                        |
   +----------------------------------------+-----------------------------------------+----------------------------------------+

.. note::

  在测试网中， ``bakerCooldownEpochs`` 最初设置为168个epoch。可以按以下方式检查此值：

   .. code-block:: console

      $concordium-client raw GetBlockSummary
      ...
              "bakerCooldownEpochs": 168
      ...

.. warning::

  如 `定义`_ 部分所述，账户中质押的那部分代币已锁定，即无法转账或用于付款。这意味着你应该仅仅质押这个账户中短期内不需要的代币数量。特别地，当要注销Baker或更改抵押金额，您的账户中还需要拥有一些未抵押的GTU来支付交易费用。

重新质押出块收益
----------------------

当以 baker 的身份参加网络和烘焙区块时，该帐户将在每个烘焙块上获得奖励。默认情况下，这些奖励会自动添加到质押金额中。

您可以选择修改此行为，令区块奖励回到帐户余额中而不是自动追加到质押量里。我们可以通过 ``concordium-client`` 以下方式更改此开关：

.. code-block:: console

   $concordium-client baker update-restake False --sender bakerAccount
   $concordium-client baker update-restake True --sender bakerAccount

restake指示符的更改将立即生效；然而，这些变化开始影响下一个epoch的烘烤概率和区块定案能力。开关的当前值可以在帐户信息中看到，可以使用 ``concordium-client`` 以下命令查询：

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker: #22
    - Staked amount: 50.000000 GTU
    - Restake earnings: yes

   ...

.. table:: Timeline: updating restake

   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   |                                               | When transaction is included in a block | 2 epochs after being rewarded |
   +===============================================+=========================================+===============================+
   | Change is visible by querying the node        | ✓                                       |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   | Earnings will [not] be restaked automatically | ✓                                       |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+
   | If restaking automatically, the gained        |                                         | ✓                             |
   | stake affects the lottery power               |                                         |                               |
   +-----------------------------------------------+-----------------------------------------+-------------------------------+

注册 baker 后，它将自动重新获取收入，但是如上所述，可以通过为命令 ``baker add`` 提供 ``--no-restake`` 标志来更改此收入，如下所示：

.. code-block:: console

   $concordium-client baker add baker-keys.json --sender bakerAccount --stake <amountToStake> --out baker-credentials.json --no-restake

定案(Finalization)
------------------

定案是指定案委员会( *finalization committee* )中的节点执行的表决过程，当委员会中有足够多的成员收到该区块并就其结果达成一致时，委员会将定案该区块。较新的块必须以最终块作为祖先，以确保链的完整性。有关此过程的更多信息，请参见：:ref:`finalization<glossary-finalization>` 部分。

定案委员会由拥有一定质押代币量的 *baker们* 组成。这意味着，要参加定稿委员会，您可能必须增加质押代币量直到指定的阈值。在测试网中，参与定稿委员会所需的赌注 **金额为现有GTU总额的0.1％** 。

参与定稿委员会的成员会在定稿的每个区块上产生奖励。奖励将在区块完成后的某个时间支付给 baker 账户。

移除baker
================

控制帐户可以选择在链上注销其 baker。为此，您必须执行 ``concordium-client`` ：

.. code-block:: console

   $concordium-client baker remove --sender bakerAccount

这会将 baker 从 baker列表中删除，并解锁 baker 上的质押代币，然后我们就可以自由转移或移动这些代币。

移除 baker 时，更改的时间与减少质押的时间相同。移除操作将需要2个以上的 **bakerCooldownEpochs** epoch才能生效。一旦将交易包含在一个区块中，该更改就会在链上可见，您可以通过 ``concordium-client`` 照常查询帐户信息来检查此移除操作何时生效：

.. code-block:: console

   $concordium-client account show bakerAccount
   ...

   Baker #22 to be removed at epoch 275 (2020-12-24 13:56:26 UTC)
    - Staked amount: 20.000000 GTU
    - Restake earnings: yes

   ...

.. table:: 时间轴: 移除baker

   +--------------------------------------------+-----------------------------------------+----------------------------------------+
   |                                            | 当交易包含在区块中                      | 2 + baker 后冷却史时代                 |
   +============================================+=========================================+========================================+
   | 通过查询节点可以看到更改                   | ✓                                       |                                        |
   +--------------------------------------------+-----------------------------------------+----------------------------------------+
   | Baker 贝克被从烘焙委员会中移除             |                                         | ✓                                      |
   +--------------------------------------------+-----------------------------------------+----------------------------------------+

.. warning::

  减少质押代币量和移除 baker 不能同时进行。在通过减少质押代币量而产生的冷却期间，无法移除 baker，反之亦然。

支持与反馈
==================

如果您遇到任何问题或建议，请在 `Discord`_ 上发布您的问题或反馈，或通过 testnet@concordium.com 与我们联系。
