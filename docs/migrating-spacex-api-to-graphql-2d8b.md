# 将 SpaceX API 迁移到 GraphQL🚀

> 原文：<https://dev.to/swcarlosrj/migrating-spacex-api-to-graphql-2d8b>

作为另一个物理、天文、火箭的爱好者，当我发现 SpaceX 公共 API(感谢[全栈阿波罗教程](http://apollographql.com/docs/tutorial/introduction.html)👏)，我对调查可用数据很感兴趣。

我立即访问了 [SpaceX API GitHub](https://github.com/r-spacex/SpaceX-API) 并…

[![](img/9bda726d09c8e8978b9ff9d8650496d3.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--QPncIGqZ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/3396/1%2AfrgOjBqsIBs4f8t6gBSECA.png)

*   🐵s 将是未来 Animoji 的手跟踪 UX 测试

休息？外部文档？邮递员？？😅SpaceX API 之旅开始并不顺利，但我仍然对挖掘构成 API 的资源感到兴奋，我继续我的方式去[文档](https://documenter.getpostman.com/view/2025350/RWaEzAiG)😬

和往常一样，我跳过了介绍，直接进入参考资料，寻找**圣杯**、*数据和它们各自的类型定义*，让我们回顾一下里面有什么👇

[![](img/fd03fc109ded538ba0a5f92e2c69c77d.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--V8JTShS---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2300/1%2A6Bz0dZavgjWvbqisAWSJCg.png)

✅ **终点**，😒v3？，有 v2 吗？医生在哪里？😕

✅ **类型**，我不知道什么是查询字符串，但我们有一些类型定义😍

🔲**数据**，无数据，无😭。我们继续找，数据模型应该就在附近！

作为一个习惯使用 GraphQL 的用户，这种体验并不太好，但值得一试🤞

[![](img/2b4203ec12af6df97937f40350c1ce83.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--DgruALEc--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2296/1%2A3EIsYZy7ht0BJXsZ32NUJQ.png)

尤里卡，我找到了🎉

✋等一下，original_launch_unix 看起来不太熟悉，让我们再检查一下“类型定义”，
🤔它不在那里，描述总是说“过滤器…”
😓那不是数据类型定义，
😫那时他们会在哪里呢？

* *滚动试图找到类型定义……**

* *我脑:
* *不说
不说
不说
不说
不说
不说
不说
不说
不说

* *我:** “保持冷静，让我们看看能否在提供的 API 集合中找到它们”

**跑步[邮差集合](https://app.getpostman.com/run-collection/3aeac01a548a87943749)……**

[![](img/8f8c38edf62646c5c7cf7552dcd95cb8.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Vk-awbsl--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2000/1%2Ari-tQl6i7oUMuDjAl6vblA.png)

**安装、注册、设置…**

* *我脑:
* *不说
不说
不说
不说
不说
不说
不说
不说
不说

* *我:** “用 GraphQL 还等什么？”

是的，我放弃了😓，就在那之后，我开始了 API 迁移…

## 为什么？

因为，

> # I just want to explore the data & and their types

不需要设置任何东西，因为…

👉**过度提取** : 90%的响应超过了应用程序的数据需求，我只想问我想要什么！

👉**下钻**:多个请求是一个糟糕的性能解决方案，我喜欢一次请求所有数据

👉**打字**，**文档**，**版本**，* *生态系统，** …♾，总是有些棘手

## 如何？

[![](img/ea6d7c7309d7e5ca9dc3fac0cdbfd337.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Cn7KYGSq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/4408/1%2A01W2XwVEpRyg-3Ayt4FN5g.png)

## 什么？

它不是一个框架，也不是一个库，它是一种面向 API 的* *查询语言* *，作为规范本身，它与任何编程语言都兼容💫

GraphQL:一种用于 API 的查询语言

掌握它👩‍🎓👨‍🎓伊芙·波尔切洛&亚历克斯·班克斯的《学习图表》

[![](img/43bc21a89ec6f6a295db9731dbd7be47.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--ephyM9OV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/2000/1%2Au8MDB0zH-dDLCB82SSRwCQ.png)

## 🌒🌔迁移🌖🌘

关于 GraphQL 技术人员&的工具，我能说些什么呢？简单地说，我已经部署了一个 API，能够交互地获取真实数据&探索完全更新的文档(以及更多)*，* *在几个小时内* *，非常有趣&！

## 步骤

1️⃣设置服务器和模块化模式⚙️
2️⃣将数据库&构建器包含到上下文中
3️⃣创建域的类型定义&解析器💪
4️⃣实行[琐碎的解决办法](https://graphql.org/learn/execution/#trivial-resolvers)t5】5️⃣吸取教训💡
6️⃣部署🚀它

看起来不难，对吧？，实际上这并不是因为出色的 GraphQL 社区🙌，[不要错过所做的改变](https://github.com/r-spacex/SpaceX-API/compare/master...swcarlosrj:graphql)！

## 结论

自己画……(叉开它，打破它，改进它，但特别是，享受它😄)

> # T2] Buy back [T3】 API 【T4] document & everything

[https://codesandbox.io/embed/yv004pqnq9](https://codesandbox.io/embed/yv004pqnq9)

[![](img/c5ce5801aec5c7d14617a1b50036d15b.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--3M8UvU2v--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/3840/1%2Af6IMUipNmjSrEp9iEAX4sw.jpeg)

### 不要错过基于 SpaceX GraphQL API 的 GraphQL 最佳实践&结论

> # T2] [Experience and lessons of migrating API to GraphQL](https://medium.com/@swcarlosrj/lessons-learned-migrating-apis-to-graphql-8a015d08b163)

请考虑一下🙏🏻荷兰、contribut♻️ing 和沙尔💜该死的。