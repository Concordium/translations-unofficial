.. _`Network Dashboard`: https://dashboard.testnet.concordium.com/
.. _Discord: https://discord.gg/xWmQ5tp

.. _run-a-node:

==========
运行节点
==========

.. contents::
   :local:
   :backlinks: none

在本指南中，您将学习如何在计算机上运行一个节点来参与Concordium网络。
这意味着您收到其他节点的块和事务，以及传播有关Concordium中的节点和块事务的信息网络。 
遵循本指南后，您将能够

-  运行一个Concordium节点
-  在网络仪表板上观察它
-  直接从您的机器查询Concordium区块链的状态。

不需要账户即可运行节点

开始之前
================

在运行Concordium节点开始之前，你需要

1. 安装和运行Docker.

   -  在 *Linux* 上, 允许Docker以非root用户运行.

2. 下载并解压缩 :ref:`concordium-node-and-client-download` 软件包。

从早期版本的Open Testnet升级
===============================================

要为Open Testnet 4升级到当前的Concordium软件，请执行以下操作：

-  请按照上述步骤 :ref:`download<downloads>`  操作，以下载最新的Concordium软件。

-  从解压缩后的文件夹中运行 ``concordium-node-reset-data`` 可执行文件。  
   
   -  对于 *Mac* 用户：第一次打开该工具时，右键单击 ``concordium-node-reset-data`` 文件，然后选择 **打开** 。将会出现一条消息，说明该软件来自一个身份不明的开发商。
      请再次选择 **打开**
   -  对于 *Windows* 用户: 第一次打开该工具时，右键单击 ``concordium-node-reset-data`` 文件， 
      将会出现一条消息，说明该软件来自一个身份不明的开发商。
      请选择 **更多信息** → **仍然运行**.

-  工具将会询问:

      *您想删除已保存的密钥吗？*

   为先前版本创建的帐户在Open Testnet 3上不再有效。
   因此，如果您存储了先前版本的帐户，我们建议输入 **y** ，这将删除所有账户密钥。

.. _running-a-node:

运行节点
==============

要开始运行将加入Open Testnet的客户端，请按照下列步骤操作：

1. 从解压缩的文件夹中打开 ``concordium-node`` 执行文件.

-  对于 *Mac* 用户：第一次打开该工具时，右键单击 ``concordium-node``  执行文件，然后选择“打开”。 
   将会出现一条消息，说明该软件来自一个身份不明的开发商。 
   再次选择“打开”。
-  对于 *Windows* 用户: 第一次打开该工具时，右键单击 ``concordium-node-reset-data`` 文件， 
   将会出现一条消息，说明该软件来自一个身份不明的开发商。
   请选择 **更多信息** → **仍然运行**.
-  当时 **重新启动** 节点时，请考虑使用 ``--no-block-state-import`` 选项。 
   这会使节点在Concordium区块链从非活动状态起下载区块，可能会加快启动过程。
   
2. 输入节点名称，此名称将显示在公共仪表板中.
3. 如果该工具已启动，则将询问您是否要在启动之前删除本地节点数据库。 点击 **y** 将删除并随后重新创建有关
   保存在您计算机上的Concordium区块链状态信息。 **注意删除本地节点数据库意味着您将花费更长的时间节点以赶上Concordium网络。**

该工具现在将下载Concordium Client映像并将其加载到Docker。
客户端将启动并开始日志记录关于节点的操作信息。

在仪表板上看到您的节点
=================================

在运行 ``concordium-node`` 后你可以

-  在 `Network Dashboard`_ 查看你的节点
-  :ref:`query<testnet-query-node>` 关于区块、交易记录、账户的信息

Network Dashboard
-----------------

客户需要一段时间才能同步Concordium区块链状态。 
例如，这涉及下载有关区块链中所有块的信息。

您还可以在 `Network Dashboard`_ 里的其他信息，了解您的节点要花多长时间才能同步完链。
您可以在仪表板顶部比较节点的 **Length** 值(你已接收到的块)和 **Chain Len** 值（网络中最长链中的块）。

启用入站连接
============================

如果您在防火墙后或家庭路由后运行节点，那么可能您只能连接到其他节点,但其他节点将无法与您的节点连接。
您的节点将完全很好地参与Concordium网络。
它将能够发送交易，并且 :ref:`如果配置<become-a-baker>`,则可以烘培并完成。

However you can also make your node an even better network participant
by enabling inbound connections. By default, ``concordium-node`` listens
on port ``8888`` for inbound connections. Depending on your network and
platform configuration you will either need to forward an external port
to ``8888`` on your router, open it in your firewall, or both. The
details of how this is done will depend on your configuration.
然而，您也可以使节点成为更好的网络参与者
通过启用入站连接。 默认情况下， ``concordium-node``  监听端口 ``8888`` 上用于入站连接。 
取决于您的网络和平台配置，您将需要转发外部端口到路由器上的 ``8888``  ，或在防火墙中将其打开，或两者同时打开。
具体操作方式取决于您的配置。

配置端口
-----------------

The node listens on four ports, which can be configured by supplying the
appropriate command line arguments when starting the node. The ports
used by the node are as follows:
节点侦听四个端口，可以通过提供以下端口来配置
启动节点时使用适当的命令行参数。 
节点使用的端口如下：

-  8888, peer-to-peer的网络端口, 设置 ``--listen-node-port`` 
-  8082, 中件间使用的端口, 设置 ``--listen-middleware-port``
-  10000, gRPC端口, 设置 ``--listen-grpc-port``

当在docker容器上方更改映射时，必须停止(:ref:`stop-a-node`)，重置并重新启动。 
要重置容器，请使用 ``concordium-node-reset-data``  或在终端运行 ``docker rm concordium-client``

我们 *强烈建议* 您的防火墙应配置为仅允许端口8888上的公共连接（peer-to-peer网络端口）。 
有权访问其他端口的人可能可以采取控制您的节点或您已保存在该节点上的帐户。

.. _stop-a-node:

停止节点
=================

通过按下 **CTRL+c** 停止节点，然后等待节点关闭

If you accidentally close the window without explicitly shutting down
the client, it will keep running in the background in Docker. In that
case, use the ``concordium-node-stop`` binary in the same way you opened
the ``concordium-node`` executable.
如果您不小心关闭了窗口而没有明确关闭客户端，它将在Docker中继续后台运行。 
这种情况下，通过运行在``concordium-node`` 文件夹里的  ``concordium-node-stop`` 可执行文件可以停止

支持与反馈
==================

您的节点的日志记录信息可以使用 ``concordium-node-retrieve-logs``  工具。 
这将使日志从运行映像保存到文件里。 此外，如果获得许可，它将检索有关当前在系统上运行的程序信息。

您可以将日志、系统信息、问题和反馈发送到testnet@concordium.com。 
您也可以通过我们的  `Discord`_ 联系，或查看我们的 :ref:`troubleshooting page<troubleshooting-and-known-issues>` 

