# 学习有什么反应？现在就让它可以访问

> 原文：<https://dev.to/yoshimii/learning-react-make-it-accessible-now-1bb7>

你好德夫斯🤠！

对于没有时间阅读所有内容的编程新手来说，这是一系列文章中的第一篇。这里的想法是，我阅读一个主题(这些天来的反应)并分享我发现简单并适用于我现在工作的项目的观点。

1️⃣第一件事第一:标题页

使用屏幕阅读器的用户首先听到的是他们登陆的页面名称。让名字有意义。给用户一些上下文！亚达亚达，我们知道。对吗？但是，如何命名单个页面应用程序的不同组件路径呢？很高兴你问了！**输入:**

## [反应过来文档标题](https://github.com/gaearon/react-document-title)

一个简单的安装`npm install --save react-document-title`，你就有了 Yousef 和你自己的一个 NPM 软件包，它有一个“在单页应用程序中指定`document.title`的声明方式”

2️⃣:第二件事让我大吃一惊:情态动词

Modal 是一个我从来不想了解的词。这不是我们经常听到的时髦词汇，比如——咳咳，敏捷或者，咳咳香草 JavaScript。但它们实际上无处不在，而且众所周知并不友好。

就是那些烦人的盒子挡住了你真正想看的东西的视线。他们会说:‘今天就加入...、'或'第一个知道...'而他们真正的意思是“现在就给我们你的电子邮件地址。”想象一下无法逃离那个地狱。

**现在，我们可以通过**帮助其他人脱离模态边缘

## [react-aria-modal](https://github.com/davidtheclark/react-aria-modal)

另一个简单的 npm 包可以帮助人们在网上更快乐。

react-aria-modal 使用 aria(可访问的富互联网应用程序)属性来实现完全可访问的体验。

只需`npm install react-aria-modal`用户就可以在模态中的不同字段间切换，也可以按 escape 键关闭它。

我喜欢有人花时间来建立这一点，因为这个帖子看起来他们正在寻找共同维护者！所以，这也是一个为开源项目做贡献的好机会。嘣。一石二鸟。

披露:这最后一件事不是 React specific，所以请放心继续，但它非常容易应用和记住去做。保证。

3️⃣的第三件事也是经常被忽视的是:对比🖤

可以说设计是新项目中最有趣的部分。虽然有些人希望它已经完成了，但其他人希望他们有更多的时间来使事情像素完美。不是每个人都有如此固执己见的权利。

色觉缺陷影响了全世界数百万的网络用户。即使是那些没有它的人也有一些将显示器或电视设置得恰到好处的经验。

当你在谷歌上搜索面包食谱时，你能想象没有这种选择吗？那就不是黑麦了...t 。事实上，这将是彻头彻尾的酸。小麦不能这样🍞。我不能决定选择正确的面包。不抱歉。**反正这里要帮忙的是:**

自 1999 年以来，WebAIM 一直在帮助改善这种体验。真的！

他们的对比检查测试你的颜色，看看他们是否得到了 WCAG(网页内容可访问性指南)的批准。通过对比前景色和背景色，您可以轻松检查对比度是否合格。你需要至少 4.5:1 的正常大小的文本才能通过 AA 级评级。我幻想着有一天我能向招聘人员强调我的投资组合的可及性🌠。

作为互联网产品的制造者，我们有责任为每个人制造所有的东西。这些都是让人们的生活变得更好的快速方法。

这是我的第一篇帖子，所以这里有一个简短的说明:

我发现自己可以奢侈地把时间 100%花在学习 web 开发上。(我在 Lambda 学校上学。问我这件事。所以，我有一个致力于全职学习的时间表，即使我没有足够的时间来阅读我一天中打开的每一个标签。我希望我现在能给你提供一些有用的知识。