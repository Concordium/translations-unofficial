
.. _Discord: https://discord.gg/xWmQ5tp

.. _testnet-get-started:

=======================================
Concordium ID：开始使用该应用
=======================================

.. contents::
   :local:
   :backlinks: none

在按照本指南进行操作之前，您应该已经完成​​Concordium ID的安装，如 上一章<testnet-get-the-app> 中所述  。

设置密码和生物识别
================================

首次打开Concordium ID应用程序时，您会受到一个流程的欢迎，该流程将帮助您设置密码和生物特征认证，创建一个 :ref:`glossary-initial-account`,，它还将引导您完成操作。得到一个： :ref:`glossary-identity` 。初始帐户是一种特殊类型的帐户，在创建身份后由  :ref:`the previous chapter<testnet-get-the-app>` 提交给链。您可以使用初始帐户进行的交易与使用常规帐户进行的交易相同，但是身份提供者将知道初始帐户的所有者。创建您的身份后，您将可以自己向该链提交帐户，身份提供者将不知道这些帐户。您可以在 :ref:`Identities
and accounts<reference-id-accounts>` 页面上了解有关帐户的更多信息。

打开Concordium ID时遇到的第一个屏幕是这个屏幕。它只会说明您必须经历此过程才能开始。

如果您准备好继续，可以按 **是，开始！**  下一个屏幕将要求您输入六位数的密码。如果您希望使用包含字母的完整密码，也可以在此处选择使用。

.. image:: images/concordium-id/int1.png
      :width: 32%
.. image:: images/concordium-id/int2.png
      :width: 32%

.. todo::

   编写指令以使两个或更多图像并排居中


选择密码或完整密码后，如果您的手机支持面部识别或指纹识别，您将可以选择也使用生物识别。如果可以选择，我们建议使用生物识别。

.. image:: images/concordium-id/int3.png
      :width: 32%
      :align: center

要求您的初始帐户和身份
=========================================

接下来，您将可以选择一个新的初始帐户和身份，或者导入一个已经存在的集合。假设这是您第一次使用Concordium ID，则可以选择" **我要创建我的初始帐户** "以继续。

.. image:: images/concordium-id/int4.png
      :width: 32%
      :align: center


在下一个屏幕上，您将看到有关初始帐户的描述以及获得该帐户必须完成的三个步骤以及您的身份。简而言之，初始帐户是您所选择的身份提供者提交给链的帐户，这意味着他们将知道您是该帐户的所有者。之后，您将能够自己将帐户提交到链中，这意味着这些帐户的所有者只有您自己知道。

.. image:: images/concordium-id/int5.png
      :width: 32%
      :align: center

上面提到的三个步骤是：

1. 初始帐户的命名
2. 命名您的身份
3. 从您选择的 :ref:`glossary-identity-provider` 请求初始帐户和身份

您将在下一页遇到第一步，提示您输入初始帐户的名称。按下继续将带您进入下一页，您必须在其上命名您的身份。这两个名称只会由您自己知道，因此您可以根据自己的喜好或多或少地给它们命名（可以使用的字母和符号有一些限制）。

在下面的示例中，我们选择将初始帐户称为*示例帐户1*，并将其称为*示例身份*。如前所述，您可以选择所需的名称。

.. image:: images/concordium-id/int6.png
      :width: 32%
.. image:: images/concordium-id/int7.png
      :width: 32%

按 **继续身份提供商**, 您将被带到一个页面，您必须在身份提供商之间进行选择。身份提供者是一个外部实体，它将在返回要在链上使用的身份对象之前，验证您的身份。目前，您可以选择以下选项：

* *您将被带到一个页面，您必须在身份提供商之间进行选择。身份提供者是一个外部实体，它将在返回要在链上使用的身份对象之前，验证您的身份。目前，您可以选择以下选项：
* *Notabene，通过它可以验证您的真实身份。

.. image:: images/concordium-id/int8.png
      :width: 32%
      :align: center

通过选择Notebene Development，您将获得一个测试身份，而无需再费力。如果选择Notabene，则将转到其外部身份发布流程，它将引导您完成为身份对象进行验证的过程。完成此流程后，将带您回到Concordium ID。

完成任何一个身份发放流程之后，将出现以下屏幕。它将向您显示您的身份和初始帐户的概述。

.. image:: images/concordium-id/int9.png
      :width: 32%
      :align: center

根据您选择的身份提供商，身份证的布局可能会略有不同。您可以看到示例帐户1由身份示例身份持有。在此过程中创建的帐户将 在应用程序中标记为（Initial），因此您知道哪个帐户是身份提供者提交给链的初始帐户。

按" **完成** "，您将进入*“帐户”*屏幕。在此屏幕上，您将能够看到您新创建的初始帐户。它可能显示Pending图标，这意味着身份提供者仍在致力于提交和创建您的初始帐户和身份。您也可以通过单击显示屏底部的" **身份** "来导航到 *“身份”* 屏幕。在此屏幕上，您可以看到您新创建的身份，如果身份提供者尚未完成身份验证，则该身份可能仍处于待处理状态。您现在所要做的就是等待它们完成。

.. image:: images/concordium-id/int10.png
      :width: 32%
.. image:: images/concordium-id/int11.png
      :width: 32%


支持与反馈
==================

如果您遇到任何问题或建议，请在  `Discord`_ 上发布您的问题或反馈，或通过  testnet@concordium.com 与我们联系。
