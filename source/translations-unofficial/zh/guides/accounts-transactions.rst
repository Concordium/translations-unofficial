.. _Discord: https://discord.gg/xWmQ5tp

.. _guide-account-transactions:

=========================================================
Concordium ID：开始进行帐户和交易
=========================================================

.. contents::
   :local:
   :backlinks: none


· 在遵循本指南之前，您应该已经完成了请求初始帐户和身份的请求，如上一章 **<testnet-get-started>** 中所述。

创建一个新账户
====================
在介绍帐户，余额和交易如何工作之前，让我们创建第二个帐户。首先转到 **帐户** 页面。在右上角，您应该看到一个 **加号** 。按下以继续。在下一个屏幕上，将要求您命名新帐户。在此示例中，我们将选择名称  **Example Account 2** ，但是您可以选择所需的任何名称。

.. image:: images/concordium-id/acc1.png
      :width: 32%
.. image:: images/concordium-id/acc2.png
      :width: 32%

当按下Next时，将出现一个屏幕，您必须在该屏幕上决定使用哪个身份来打开新帐户。到目前为止，您可能只有一个，但是如果您有更多，则可以从列表中选择所需的身份。通过单击一个身份，您将进入下一个屏幕。创建非初始帐户（即在创建身份时未创建的帐户）时，您可以选择显示一些： :ref:`glossary-attribute`.。这是没有必要的，并且如果您没有特定的原因，我们建议您不要公开任何内容，因为已公开的属性将在链上且无法删除。

.. image:: images/concordium-id/acc3.png
      :width: 32%
.. image:: images/concordium-id/acc4.png
      :width: 32%

如果确实按了 **“显示帐户属性”**  按钮，则将转到下一页。您可以勾选要显示的属性，然后按 **提交帐户** 。在此页面或上一页中按**“提交帐户”**，将带您进入最终帐户创建页面，该页面将为您提供简短概述并告诉您该帐户已提交。

.. image:: images/concordium-id/acc5.png
      :width: 32%
.. image:: images/concordium-id/acc6.png
      :width: 32%

通过单击 **确定，谢谢您** 在提交概述中，您将返回到帐户页面。您可能会发现您的新帐户仍处于待处理状态，因为这可能需要几分钟才能最终确定。如果您尚未尝试这样做，则可以尝试按其中一张帐户卡上的向下箭头，以查看它会折叠到该卡上。这揭示了两个新的信息，可供使用和抵押。**“可支配”** 字段将告诉您在给定的时刻有多少可用的帐户余额，以及您可以在 :ref:`managing accounts<managing_accounts>` 页面上详细了解的抵押金额。

.. image:: images/concordium-id/acc7.png
      :width: 32%
.. image:: images/concordium-id/acc8.png
      :width: 32%


进行交易
====================
接下来，尝试按新创建帐户的 **“余额”** 区域。在此屏幕上，您可以看到帐户的当前余额，并且在这一点上，您还可以请求在Testnet上使用100 GTU。对于100 GTU的请求是Testnet的功能，对于Testnet 4，它将实际上将2000 GTU转移到该帐户，即使该按钮显示为100。 GTU 删除仅在帐户上可用一次。按下它，您会注意到正在出现交易。这将待一会儿，不久后会将2000 GTU添加到您的帐户中。

.. image:: images/concordium-id/acc9.png
      :width: 32%
.. image:: images/concordium-id/acc10.png
      :width: 32%

现在我们的帐户中有一些GTU，让我们尝试进行交易。按发送按钮执行此操作。在下一页上，您可以输入要转账的金额，然后选择收件人。在此示例中，我们将传输10 GTU。

.. image:: images/concordium-id/acc11.png
      :width: 32%
.. image:: images/concordium-id/acc12.png
      :width: 32%

确定金额后，我们现在将选择收件人。为此，请按 **“选择收件人”** 或**“屏蔽数量”** 按钮。在此页面上，您可以在通讯簿中搜索收件人，也可以通过扫描接收帐户的 QR码 来添加收件人。正如您在屏幕截图中所看到的，我们仅保存了一个收件人，即 *示例帐户1* 。在此之上，我们可以选择屏蔽金额，但稍后我们会再讨论。在此示例中，我们将选择 *示例帐户1* 作为我们的收件人。

.. image:: images/concordium-id/acc13.png
      :width: 32%
.. image:: images/concordium-id/acc14.png
      :width: 32%

选择 金额 和 收件人 后，我们可以按 **“发送资金”** 继续。通过这样做，我们将在确认屏幕上看到我们可以在其中确认金额，收件人和发送帐户。按 **是，发送资金** ，我们将使用密码或生物识别技术进行自我验证，然后将交易提交给链。完成交易可能需要一些时间。

.. image:: images/concordium-id/acc15.png
      :width: 32%
.. image:: images/concordium-id/acc16.png
      :width: 32%

现在，我们可以看到 **“示例帐户2 ”** 的 **“转帐”** 日志显示已扣除了该金额，再加上一笔费用。所有交易都将收取费用，并且根据交易类型的不同，费用可能会有所不同。按下交易将使您看到更多详细信息。

.. image:: images/concordium-id/acc17.png
      :width: 32%
.. image:: images/concordium-id/acc18.png
      :width: 32%

.. _move-an-amount-to-the-shielded-balance:

将金额移动到被保护的余额
========================================
如果返回到 **“帐户”** 屏幕，现在可以看到 10 GTU 已转移到 **示例帐户1** 的余额中。您可能已经注意到，这些帐户还具有： :ref:`glossary-shielded-balance` 。简而言之，屏蔽余额用于在帐户上保留GTU的屏蔽（加密）金额。让我们尝试在 **“示例帐户2 ”** 中添加一些受屏蔽的GTU 。首先按下帐户卡的 **“屏蔽余额”** 区域。

.. image:: images/concordium-id/acc19.png
      :width: 32%
.. image:: images/concordium-id/acc20.png
      :width: 32%

接下来，再次按 **SEND** 按钮并输入要屏蔽的GTU量，这是在 **Shielded Balance** 中添加一些GTU的动作。完成此操作后，让我们再次按 **“选择收件人”** 或 **“屏蔽金额”** 。这次我们没有选择接收者，而是按 **Shield amount** 。

.. image:: images/concordium-id/acc21.png
      :width: 32%
.. image:: images/concordium-id/acc22.png
      :width: 32%

现在，我们可以像常规转帐之前一样继续并确认交易。该交易可能需要一点时间才能在链上完成。

.. image:: images/concordium-id/acc23.png
      :width: 32%
.. image:: images/concordium-id/acc24.png
      :width: 32%

通过返回 **“帐户”** 页面，现在可以看到 **示例帐户2** 的受保护余额中有10 GTU 。如果按了帐户卡的 **“保护余额”** 区域，则可以看到在 **“保护余额转账”** 日志中有一个 **“保护金额”** 交易。进行屏蔽交易也将收取一定费用，但该费用将从帐户的常规余额中扣除。尝试返回并查看常规余额的转移日志。

.. image:: images/concordium-id/acc25.png
      :width: 32%
.. image:: images/concordium-id/acc26.png
      :width: 32%

进行屏蔽传输
========================
有了一些可用的屏蔽GTU，我们现在可以尝试进行屏蔽传输，这意味着我们可以使用加密量的GTU进行传输。第一步是浏览到包含受保护GTU的帐户的受保护余额页面（如果您还没有的话）。然后按发送按钮。现在，您将能够输入金额并选择收款人。在此示例中，我们选择了传输 2 GTU。按 **“选择收件人”** 或 **“取消屏蔽金额”** 按钮时，您将能够选择收件人。在此示例中，我们将选择 **“ 示例帐户2”** 。

.. image:: images/concordium-id/acc27.png
      :width: 32%
.. image:: images/concordium-id/acc28.png
      :width: 32%

有了金额和收件人之后，您现在可以继续。就像其他交易一样，您现在将看到一个确认屏幕，然后继续进行操作，就可以使用密码或生物识别技术来验证自己，然后将被屏蔽的交易提交到链中。同样，该交易可能需要一些时间才能最终确定。

.. image:: images/concordium-id/acc29.png
      :width: 32%
.. image:: images/concordium-id/acc30.png
      :width: 32%


现在，如果您返回到 **“帐户”** 屏幕，您应该能够看到收款账户的 **“受保护的余额”** 中的金额旁边出现了一个小的保护 。这表明在受保护的余额上有新接收到的受保护的交易。尝试按屏蔽的天平，请注意，您必须输入密码或使用生物识别技术输入密码。发生这种情况是因为您需要先解密收到的受屏蔽交易，然后才能看到金额。
.. image:: images/concordium-id/acc31.png
      :width: 32%
.. image:: images/concordium-id/acc32.png
      :width: 32%

揭开金额
==================
解密后，该金额现在在受保护的余额和 **“帐户”** 屏幕上的帐户卡上可见。现在，如果我们想将一些 GTU 从屏蔽余额转移到常规余额，该怎么办？让我们尝试通过取消屏蔽金额的动作将 2 GTU移至常规余额 。为此，请按屏蔽天平中的 **“发送”** 按钮。输入2作为金额，然后按 **选择收件人** 或 **“取消屏蔽金额”** 。 **选择取消屏蔽金额** 。

.. image:: images/concordium-id/acc33.png
      :width: 32%
.. image:: images/concordium-id/acc34.png
      :width: 32%

现在，像完成其他交易一样完成交易，然后尝试浏览至帐户的常规余额以查看解除屏蔽状态。如果交易已经按链完成，您现在应该可以看到常规余额中已显示 **未屏蔽金额** 。请注意，即使您刚刚屏蔽的金额是2 ，它也不是2 GTU。这是因为进行任何事务（包括屏蔽）的费用将从负责该事务的帐户的常规余额中扣除。

.. image:: images/concordium-id/acc35.png
      :width: 32%
.. image:: images/concordium-id/acc36.png
      :width: 32%

分享您的帐户地址
==========================
如果要共享您的帐户地址，可以通过按 **“地址”** 按钮轻松完成。这将带您到一个页面，您可以在其中共享帐户地址的多个选项。尝试按 **“共享”** 按钮，然后与某人共享您的地址。

.. image:: images/concordium-id/acc37.png
      :width: 32%
.. image:: images/concordium-id/acc38.png
      :width: 32%

检查发布时间表
==========================
在Concordium 区块链上，可以进行交易，以随着时间的流逝释放所转移的金额。这称为*带计划*的 *转移*。现在，我们将不讨论如何从 Concordium ID 进行转移，但是我们来看看如何检查 发布时间表 。如果您收到包含下达时间表的转帐，则可以按余额屏幕右上角的 **汉堡菜单**。这将允许您按下**发布时间表** ，然后执行此操作，您将进入一个屏幕，其中包含有关释放多少 GTU 以及何时发布的信息。如果您想了解更多有关如何使用发布时间表进行转移的信息，您可以看一下： :ref:`concordium_client` 和 :ref:`transactions` 页面。

.. image:: images/concordium-id/rel1.png
      :width: 32%
.. image:: images/concordium-id/rel2.png
      :width: 32%
.. image:: images/concordium-id/rel3.png
      :width: 32%

支持与反馈
==================

如果您遇到任何问题或建议，请在 `Discord`_ 上发布您的问题或反馈，或通过  testnet@concordium.com 与我们联系。
