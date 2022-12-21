# 科洛列夫。网络减肥药

> 原文：<https://dev.to/fomkin/korolev-slimming-pill-for-web-549a>

大约一年前，尼基塔·普罗科波夫发表了一篇文章——宣言“软件祛魅”。从积极的反馈来看，开发者想要关心他们生产的产品的质量。也许，是时候开始行动了？

在这篇文章中，我想谈谈我的项目，在我看来，它可以解决现代网络的主要性能问题，并让用户更开心一点。它们是- JS bundles 大小，高交互时间，高 RAM 和 CPU 消耗。

在进一步阅读之前，[跟随链接](https://match3.fomkin.org)。试着打几场比赛。从桌面上玩是可取的。

## 一点历史

一开始，web 的创造者将浏览器设计成 Web 服务器的瘦客户机。浏览器显示从服务器接收的超文本页面。它简单而优雅。通常情况下，美好的想法会遭遇现实，几年后，浏览器制造商增加了对脚本语言的支持。起初，它只是用来装饰的。直到第一个十年的中期，用 JS 创建网站被认为是合适的，只是作为一种选择。

现代的网站开发方法是对用户界面交互性要求不断增长的结果。提高交互性的任务落在了模板设计者的肩上。他们通常没有能力和权力来开发一个“跨领域”的解决方案。模板设计师学了 JS，成了前端工程师。逻辑逐渐开始从服务器流向客户端。前端人员在客户端编写任何东西都很方便。对于后端人员来说，不考虑用户是很方便的。“我给你 JSON，然后我不在乎”——他们说。两年前，无服务器架构开始流行。建议是 JS 应用程序将直接处理数据库和消息队列。

目前，一个普通的网站是一个用 JS 编写的复杂应用程序和一个简单的 API 服务器。主逻辑在胖客户机上执行，服务器部分退化为数据库代理。

如果技术债务在服务器端可能不会直接影响你的用户，那么在客户端就会影响。如果你的初创公司“起飞”并开始盈利，那么随着负载的增加，情况只会变得更糟。需求会改变。代码库会膨胀，团队中的离职率会增加。页面会因为依赖而变得越来越大。网站将加载过时的 JSON。后台任务的数量会成倍增加，每个任务每秒钟运行几毫秒，一段时间后会导致一个不幸用户的 iPad 延迟和升温，以便他或她可以在上面煎鸡蛋。因为害怕破坏系统，没人敢修它。最终，疲惫不堪的前端人员会带着一个建议来找经理，放弃旧的丑陋的框架，在一个崭新的框架上从头开始重写一切。经理会拒绝，前端的家伙会开始一起使用两者。

## 科洛列夫是如何工作的

那么，如果我们回到转折点呢？到有人想出不用重新加载页面就能更新内容的想法，历史的必然性催生了 AJAX 的那一刻？如果我们把所有东西都留在服务器上，做一个瘦客户机会怎么样？最好的网站会在服务器上预先呈现页面，这样用户就可以在 JS 加载之前看到界面。考虑到当前对交互性的需求，我们可以更进一步，只将代码留在负责处理 I/O 的客户机上。对这个问题的思考让我想到了科洛列夫项目。

从客户端的角度来看，这是如何实现的？用户来到页面。服务器发送生成的 HTML 和一个小脚本(大约 6kb，没有压缩)，它通过 web 套接字连接到服务器。当用户产生一个事件时(例如，一次点击)，脚本将它发送到服务器。服务器处理一个事件生成并发送一系列命令，如“在那里添加一个新的`<div>`”，“向元素添加一个类”，“删除元素”客户端将命令列表应用于 DOM。因此，使用 HTML 不会发生——脚本直接使用 DOM，所以不要担心滚动条的形式或位置会重置。

服务器上发生了什么？当浏览器发出页面请求时，科洛列夫会创建一个新的会话。产生初始状态并存储在高速缓存中。HTML 从这个状态开始呈现，a 发送给客户端作为对请求的响应，服务器也在会话中存储“虚拟 DOM”。请求页面后，服务器接受打开 web 套接字的请求。科洛列夫将打开的 web 套接字与会话相关联。来自客户端的每个事件都可以改变与会话相关的状态(但不能直接修改 DOM)。每次状态改变都会导致对 render 函数的调用，这将创建一个新的“虚拟 DOM”来与旧版本进行比较。比较的结果是要发送给客户端的命令列表。

一个代码和一个开发者的脑袋里会发生什么？上面写的可能会让你想起 React，不同的是一切都发生在服务器上。科洛列夫也有类似的方法。因此，如果你曾经和 React 或者其他“虚拟 DOM”一起工作过，那么科洛列夫的工作风格对你来说会很熟悉。如果您不熟悉 React，请想象您在其上映射了数据模型和模板。事件处理程序改变数据，页面自己改变。

## 表现

关于科洛列夫有两个流行的问题:“如果延迟很高怎么办”和“它如何加载我的服务器。”两者都很有道理。

前端人员习惯于他或她的程序在用户的本地机器上运行。这意味着一旦 JS-machine 执行完代码，对它所做的更改就会被应用，并且浏览器开始呈现。我在开头特别举了一个例子。如果你没有停止阅读，我想你会有很好的体验。特别是如果你算上的话，那台服务器托管在莫斯科。如果你住在三藩市，理论上最小往返时间将是 62 毫秒。此外，你可以阅读关于 UX 和响应时间限制的[报告](https://www.nngroup.com/articles/response-times-3-important-limits/)。查看任何网站的平均客户端延迟。如果小于 100，延迟是极好的。我希望我消除了对滞后可能性的疑虑。

后端人员通常会问关于服务器负载的问题。变化推断引擎的工作速度非常快:在 MacBook 2013 上，对于两个 500 个节点的任意树，每秒大约有 1 万个差异。静态渲染也给出了相当好的结果:每秒高达一百万页。每个“虚拟 DOM”都在一个特殊的序列化表示中进行存储和处理，平均每个 web 页面占用 128 KB 的堆。渲染过程经过专门优化，没有内存和 GC 开销。

至于编程的速度，这里科洛列夫给出了极好的好处。不需要在数据库和服务器之间编写额外的层。不需要在客户端和服务器之间协商协议。不需要担心 JS 包的大小——客户机上 JS 的权重总是保持不变。不需要额外的工作来支持服务器事件:接受来自队列的消息并更改会话状态，科洛列夫将呈现并交付。

## 成本

但是优势是有代价的。你应该打破一些习惯，一些新的习惯必须获得。例如，你将不得不离开 JS 动画，并对 CSS 动画感到满意。如果您想为来自不同国家的用户提供高质量的服务，您必须学习如何使基础设施最初地理分布。你必须放弃 JS，转而使用 Scala。

我有点惭愧(其实我没有)，我误导了读者，没有马上说科洛列夫是用 Scala 写的。如果我在上面告诉你，你会读到这里吗？谈到科洛列夫，我必须克服两种成见。第一个是关于服务器渲染被认为是缓慢的，而不是交互式的。第二个是关于 Scala 是一个复杂的东西。第一种和第二种刻板印象都与现实无关。

而且，在 Scala 上用 React 风格编程比在 JS 上更方便。现代 JS 倾向于函数式编程，Scala 给了它开箱即用。例如，Scala 中的一个对象有一个 copy()方法，它允许你通过改变一些字段来复制一个对象。标准库中包含的不可变集合。

## 结论

科洛列夫由几个捐助者开发了三年，许多早期问题已经解决。该项目有详细的文件，并涵盖所有功能的例子。我提议开始为小型独立项目使用科洛列夫。我希望科洛列夫能让网络不那么令人沮丧。

[链接到项目](https://github.com/fomkin/korolev)

## 类似项目

1.  [Blazor 服务器端](https://docs.microsoft.com/en-us/aspnet/core/blazor/hosting-models?view=aspnetcore-3.0#server-side)
2.  [凤凰城现场直播](https://github.com/phoenixframework/phoenix_live_view)
3.  [Kweb](http://docs.kweb.io/en/latest/intro.html)