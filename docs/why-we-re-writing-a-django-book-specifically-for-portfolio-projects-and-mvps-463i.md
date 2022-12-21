# 为什么我们要专门为投资组合项目和 MVP 写一本 Django 书

> 原文：<https://dev.to/jamestimmins/why-we-re-writing-a-django-book-specifically-for-portfolio-projects-and-mvps-463i>

TLDR:构建 web 应用程序最困难的部分是很少使用框架本身，甚至是“业务逻辑”。它干净地将你的应用集成到现代网络应用中的所有其他工具中。所以我们正在写一本专门关于构建和部署一个生产就绪的 Django API 的书，你可以引以为豪。

我记得当我第一次学习 web 编程时，我觉得自己很愚蠢。我选择 Django 是因为我喜欢 Python，虽然网上有很多很棒的资源，但我从来不觉得自己对这个项目有足够的了解，可以把我引以为豪的项目放到 Github 或 Hacker News 上。

大多数文档和教程只涉及构建一些你可以在自己的计算机上安全运行的东西，或者它们对我来说太专业了。每当我试图集成一个新的工具或服务时，总会有新的难题和问题拖慢我的进度。我花了几年时间才自信满满地展示自己开发的应用程序。

我不想构建玩具应用程序。我想构建足够安全和可扩展的生产应用程序，以发布到开放的 web 上。

当我和我的合著者 Bryan 开始计划[full stack Django API](https://www.fullstack.io/fullstack-django)时，我们问 Django 生态系统中存在什么问题，特别是 Django Rest 框架。我们可以写一本关于 Django 的大型参考书，但那似乎是浪费时间。Django 已经有了广泛的、成熟的在线文档。像《Django 的两勺》和《Django 的测试驱动开发》这样的书是它们各自领域的黄金标准。我们想创造一些补充现有材料的东西，而不是试图与之竞争。

所以我们列出了困惑的地方。其中包括:

*   如何部署实时生产应用程序？
*   我的业务逻辑放在哪里？
*   如何以安全的方式存储配置和凭据？
*   我如何处理认证？
*   我应该使用什么类型的数据库，如何在我自己的计算机上连接到它，而不是一个实时的生产数据库。
*   姜戈的所有部分是如何组合在一起的？
*   什么是任务队列，什么时候应该使用它们？
*   缓存还是不缓存？

最终变得清晰的是，这些困惑的领域不仅仅是我们独有的。许多 Djangonauts 新手对制作和部署酷项目的可能性感到兴奋，但却被将他们的 API 集成到更广泛的开发环境中的困难所困扰。

这就是我们正在写的书要帮助的人。我们的目标是让初级和中级开发人员通过 Fullstack Django APIs 工作，并带着制作生产就绪 API 所需的信心和技能离开。他们可以向招聘人员或客户展示的东西，并且可以轻松地用来构建存储真实用户数据的实时 web 服务。

如果我们成功了，这是每个使用 Django 的科技公司都会给新员工的东西，这样他们就可以准备好写产品代码了。我们知道这是一个雄心勃勃的目标，但我们对在这一领域取得的进展感到兴奋。随着发布日期的临近，我会提供更多具体的细节，但在此之前，我很高兴在 Dev.to 上分享更多关于这个过程的信息。

如果你对这个项目感兴趣，并且希望在它发布后能看到第一章，你可以在这里注册[来了解最新消息。](https://www.fullstack.io/fullstack-django)