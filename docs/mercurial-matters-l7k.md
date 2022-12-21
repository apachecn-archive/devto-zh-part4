# 变化无常的事情

> 原文：<https://dev.to/dealeron/mercurial-matters-l7k>

你想听另一个故事吗？一个装满 DevOps 和 DevOps-ey 的东西？哈！我有一个故事给你。这是一个关于钥匙库、自动化和完成工作的故事。

## 金库

它从一个[天蓝色的钥匙金库](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)开始。这是一个存储应用程序机密的本机服务。传统上，分别用于服务或网站的`application.config`或`web.config`包含代码运行所需的连接字符串。这很好，但是这样做相当于把你的密码写在贴在键盘下面的便利贴上(我正看着你呢，杰瑞！).如果有人走到你的办公桌前，他们会知道在哪里找它，一旦他们抓住你的连接字符串，游戏就结束了。

为了应对这种情况，人们通常会对其进行加密。然而这种方法的缺点是加密密钥通常在机器上，这并没有好到哪里去。如果 Nuget 包或者你在运行时用来解密它的任何机制没有保持更新，你的应用程序很可能会崩溃，因为它不能读取配置文件，并且在你的文件中留下看起来像[矩阵代码](https://en.wikipedia.org/wiki/Matrix_digital_rain)的东西(这已经发生在我身上，非常可怕)。那么，最好的去处是 Azure Key vault(AKV ),将它们存储在一个特别加密的外部数据库中，该数据库不仅允许基于凭据的身份验证，还允许使用唯一的 SSL 证书进行身份验证，以避免必须将密码写在某个地方并记住它。

## 赋权水星

我们的一个开发团队(Mercury)最近转而使用 AKV 开发他们正在创造的新东西。他们需要给他们的新金库增加很多秘密，老实说，这非常乏味。门户一般允许你一次上传 1 个，而且是一圈一圈的点击。因为您可能会复制粘贴所有的值，所以很容易复制错误的值。所以我给了他们一个我在以前的项目中用过的解决方法: