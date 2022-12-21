# 想知道对此最好的 AWS 服务是什么

> 原文：<https://dev.to/nmaxcom/wondering-what-s-the-best-aws-service-for-this-2275>

编辑:通过更多的研究和来自这个社区的宝贵意见，我找到了一个解决方案。我选 EBS 和 DocumentDB。谢谢大家！！

你好。我一直在用自动气象站，尤其是 EC2，Route 53...作为一个 VPS，服务几个网站(我已经出了年度免费层)和玩 devOps 来练习。

我可以帮助选择什么样的 AWS 服务。我有一个相对简单的应用:这是一个 **NodeJS + ExpressJS + MongoDB** 慈善网站，我估计每月访问量不超过 5k。我将为新闻和帖子使用一个无头节点 CMS，并使用 ExpressJS 连接前端(没什么特别的，只是可能是 Jade/Pug)。该网站将增加几个部分和几项服务(如筹资者网络支持等)，所以我对一个简单的解决方案感兴趣，我可以在本地工作后 git push，并在网上部署它而不打嗝。

我立即想到使用 **Docker** 来获得一个良好的开发环境，并且没有版本/依赖问题。从 AWS 我想有一些**容易设置**和**监视器**，**管理**，**自动缩放**，**低维护**，如果**负担得起**就更好了！:)我想过微服务，但如果我可以不增加复杂性，我更喜欢:容器中的一切，包括 MongoDB。

这就是怀疑的地方。我读过的大部分内容(AWS 文档、教程、youtube、帖子...)只说 ECS，我觉得对我来说是矫枉过正。一个集群中的多个 EC2 具有不同的可用性区域，使用 ELB 法盖特 ECR...我的意思是，我当然不希望网站关闭，但对于这样的网站，难道没有更简单的解决方案吗？
看了 AWS 关于 Fargate，Elastic BeanStalk，ECS 等的文档。我比以前更迷茫了。使用容器听起来很舒服，但我并不锁定它，如果有更好的选择，我洗耳恭听！

有人能帮我解决这个问题吗？

谢谢大家！