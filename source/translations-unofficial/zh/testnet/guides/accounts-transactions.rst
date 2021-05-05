.. _Discord: https://discord.gg/xWmQ5tp

.. _guide-account-transactions:

=========================================================
Concordium ID：开始进行账户和交易
=========================================================

.. contents::
   :local:
   :backlinks: none

在遵循本指南之前，您需要完成初始账户和身份的请求，如 :ref:`上一章<testnet-get-started>` 中所述。

创建一个新账户
====================
在介绍账户、余额和交易如何工作之前，让我们创建第二个账户。首先转到 *Accounts* 页面。在右上角，您应该看到一个 **加号** 。按下以继续。在下一个屏幕上，将要求您命名新账户。在此示例中，我们将选择名称  *Example Account 2* ，但是您可以选择所需的任何名称。

.. image:: images/concordium-id/acc1.png
      :width: 32%
.. image:: images/concordium-id/acc2.png
      :width: 32%

当按下 **Next** 时，将出现一个屏幕，您必须在该屏幕上决定使用哪个身份来打开新账户。
到目前为止，您可能只有一个，但是如果您有更多，则可以从列表中选择所需的身份。通过单击一个身份，您将进入下一个屏幕。当创建非初始账户（即不是在创建身份时所创建的账户），您可以选择显示多个属性 :ref:`glossary-attribute` 。但这是没有必要的，并且如果您没有特殊的原因，我们建议您不要公开任何内容，因为已公开的内容将在链上且无法删除。

.. image:: images/concordium-id/acc3.png
      :width: 32%
.. image:: images/concordium-id/acc4.png
      :width: 32%

如果按了 **Reveal account attributes button** 按钮，则将转到下一页。您可以勾选要显示的属性，然后按提交账户。在此页面或上一页中按 **Submit account** ，将带您进入最终账户创建页面，该页面将为您提供简短概述，并告诉您该账户已提交。

.. image:: images/concordium-id/acc5.png
      :width: 32%
.. image:: images/concordium-id/acc6.png
      :width: 32%

通过单击 **Ok, thanks** ，谢谢您在提交概述中，您将返回到账户页面。您可能会发现您的新账户仍处于待处理状态，因为可能需要几分钟才能完成最终确定。如果您尚未尝试这样做，则可以尝试点击其中一张账户卡的向下箭头，将会展开。这显示了两个新的信息，*at disposal* 和 *staked* 。可支配字段将告诉您在当前时刻有多少可用的账户余额，关于抵押金额您可以在 :ref:`managing accounts<managing_accounts>` 页面上了解更多。

.. image:: images/concordium-id/acc7.png
      :width: 32%
.. image:: images/concordium-id/acc8.png
      :width: 32%


进行交易
====================
接下来，尝试按新创建账户的 **Balance** 区域。在此屏幕上，您可以看到账户的当前余额，当时时刻，您还可以请求在Testnet上使用100 GTU。请求100 GTU是Testnet的功能，而对于Testnet 4，它将实际上将有2000 GTU转移到该账户，即使该按钮显示为100。在一个账户只可有一次获取GTU。点击它，您会注意到正在出现交易。等待一会儿，将有2000 GTU添加到您的账户中。

.. image:: images/concordium-id/acc9.png
      :width: 32%
.. image:: images/concordium-id/acc10.png
      :width: 32%

现在我们的账户中有一些GTU，让我们尝试进行交易。按 **SEND** 按钮执行此操作。在下一页上，您可以输入要转账的金额，然后选择接收人。在此示例中，我们将传输10 GTU。

.. image:: images/concordium-id/acc11.png
      :width: 32%
.. image:: images/concordium-id/acc12.png
      :width: 32%

确定金额后，我们现在将选择接收人。为此，请按 **Recipient or shield amount** 按钮。在此页面上，您可以在通讯录中搜索接收人，也可以通过扫描接收账户的二维码来添加接收人。正如您在屏幕截图中所看到的，我们仅保存了一个接收人，即 *示例账户1* 。在此之上，我们可以选择屏蔽金额，但稍后我们会再讨论。在此示例中，我们将选择 *Example Account 1* 作为我们的接收人。

.. image:: images/concordium-id/acc13.png
      :width: 32%
.. image:: images/concordium-id/acc14.png
      :width: 32%

选择金额和接收人后，我们可以按 **Send Funds** 继续。通过这样做，我们将在确认界面上看到 金额、接收人和发送账户。按 **Yes, send funds**，我们将使用密码或生物识别技术进行验证，然后将交易提交至链上。完成交易可能需要一些时间。

.. image:: images/concordium-id/acc15.png
      :width: 32%
.. image:: images/concordium-id/acc16.png
      :width: 32%

现在，我们可以看到 **Example Account 2** 的 **Transfers** 日志显示已扣除了该金额，再加上一笔 *fee* 。所有交易都将收取费用，并且根据交易类型的不同，费用可能会有所不同。按下交易将使您看到更多详细信息。

.. image:: images/concordium-id/acc17.png
      :width: 32%
.. image:: images/concordium-id/acc18.png
      :width: 32%

.. _move-an-amount-to-the-shielded-balance:

将金额移动到被屏蔽的余额
========================================
如果返回到 **Accounts** 屏幕，现在可以看到10 GTU已转移到 *Example Account 1* 的 *Balance* 中。您可能已经注意到，这些账户还具有 :ref:`glossary-shielded-balance` 。简而言之，屏蔽余额的用于在账户上保留GTU的屏蔽（加密）金额。让我们尝试在 *Example Account 2* 中添加一些受屏蔽的GTU 。首先点击账户卡的 **Shielded Balance** 区域。

.. image:: images/concordium-id/acc19.png
      :width: 32%
.. image:: images/concordium-id/acc20.png
      :width: 32%

接下来，再次按 **SEND** 按钮并输入要 *shield* 的GTU数量，这是在 *Shielded Balance** 中添加一些GTU的动作。完成此操作后，让我们再次按 **Select Recipient or shield amount** 。这次我们没有选择接收者，而是按 **屏蔽金额** 。

.. image:: images/concordium-id/acc21.png
      :width: 32%
.. image:: images/concordium-id/acc22.png
      :width: 32%

现在，我们可以像常规转帐之前一样继续并确认交易。该交易可能需要一点时间才能在链上完成。

.. image:: images/concordium-id/acc23.png
      :width: 32%
.. image:: images/concordium-id/acc24.png
      :width: 32%

通过返回 **Accounts** 页面，现在可以看到 *Example Account 2* 的受保护余额中有10 GTU 。如果按下了账户卡的 **Shielded
Balance** 区域，则可以看到在 **Shielded amount** 日志中有一个 **Shielded
Balance** 交易。进行保护交易也将收取一定费用，但该费用将从账户的常规余额中扣除。尝试返回并查看常规 *Balance* 的转移日志。

.. image:: images/concordium-id/acc25.png
      :width: 32%
.. image:: images/concordium-id/acc26.png
      :width: 32%

进行屏蔽转账
========================
有了一些屏蔽的GTU可用，我们现在可以尝试进行 *Shielded transfer* ，这意味着我们可以用加密的方式进行传输
GTU。第一步是浏览到包含屏蔽GTU的账户的 *屏蔽余额* 页面。然后按下 **SEND** 按钮。您现在可以输入金额并选择接收人。在这个例子中，我们选择了
2 GTU转移。当按下 **Select Recipient or unshield amount** 按钮，您将能够选择一个收件人。此例中我们将选择 *Example Account 2* 。

.. image:: images/concordium-id/acc27.png
      :width: 32%
.. image:: images/concordium-id/acc28.png
      :width: 32%

有了金额和接收人之后，您现在可以继续。就像其他交易一样，您现在会看到一个确认屏幕，
然后继续操作，您将能够通过密码或生物识别技术验证，然后提交屏蔽交易
到链上。同样，在链上完成交易可能需要一些时间。

.. image:: images/concordium-id/acc29.png
      :width: 32%
.. image:: images/concordium-id/acc30.png
      :width: 32%

现在，如果您返回 *Accounts* 页面，您应该可以看到在金额旁边有一个小小的盾牌。
接收账户的 *Shielded Balance* 。 这表明在受保护的余额上有新接收到的受保护的交易。
尝试按屏蔽的天平，请注意，您必须输入密码或使用生物识别验证。
发生这种情况是因为您需要先解密收到的受屏蔽交易，然后才能看到金额。

.. image:: images/concordium-id/acc31.png
      :width: 32%
.. image:: images/concordium-id/acc32.png
      :width: 32%

解屏蔽金额
==================
解密后，现在可以在 *shielded balance* 和 *Accounts* 屏幕上的账户卡上看到该金额。现在，如果我们
想要将一些GTU从屏蔽式余额转移到常规余额？让我们尝试通过以下操作将2GTU移至常规余额 *Unshielding* 数量。为此，请按屏蔽天平中的“发送”按钮，输入2作为金额，然后按 **Select Recipient
or unshield amount** 。 **hoose Unshield amount** 。

.. image:: images/concordium-id/acc33.png
      :width: 32%
.. image:: images/concordium-id/acc34.png
      :width: 32%

现在，像完成其他交易一样完成交易，然后尝试浏览至账户的常规余额以查看解除屏蔽状态。
如果交易已经在链上完成，您现在应该可以看到常规余额中有 *Unshielded amount* 。
请注意，即使您刚屏蔽的金额为2，它也不是2GTU。这是因为进行任何交易（包括
屏蔽交易将从负责交易的账户的正常余额中扣除。

.. image:: images/concordium-id/acc35.png
      :width: 32%
.. image:: images/concordium-id/acc36.png
      :width: 32%

分享您的账户地址
==========================
如果您想分享您的账户地址，可以通过按 *Address* 按钮轻松完成。这将带您进入页面，
您可以在其中选择多个分享帐户地址的选项。尝试按 *Share* 按钮，然后与某人分享您的地址。

.. image:: images/concordium-id/acc37.png
      :width: 32%
.. image:: images/concordium-id/acc38.png
      :width: 32%

检查发布时间表
==========================
在Concordium区块链上，可以进行一种以随着时间的流逝释放所要转移的金额。这叫做 *transfer with a schedule* 。目前，我们不会介绍如何通过Concordium ID进行此类转移，
但让我们看看如何检查发布时间表。如果您收到包含发布时间表的转移信息，则可以按
余额屏幕右上角的 **burger menu** 。这将使您可以按 *Release schedule* ，然后执行此操作
将被看到一个屏幕，其中包含有关释放多少GTU以及何时发布的信息。如果您想了解更多有关如何
进行带有发布时间表的转移，您可以查看 :ref:`concordium_client` 和 :ref:`transactions` 页面。

.. image:: images/concordium-id/rel1.png
      :width: 32%
.. image:: images/concordium-id/rel2.png
      :width: 32%
.. image:: images/concordium-id/rel3.png
      :width: 32%

支持与反馈
==================

如果您遇到任何问题或建议，请在 `Discord`_ 上发布您的问题或反馈，或通过testnet@concordium.com与我们联系。
