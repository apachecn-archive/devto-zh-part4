# 为什么我要编写一个自定义引擎

> 原文：<https://dev.to/thebuzzsaw/why-i-am-writing-a-custom-engine-5b38>

这场辩论已经被讨论过一千次了。我能做的最好的事情就是表达我做这件事的理由。我不能声称对你或你的情况有答案。

我将把这个分成两部分:我从头开始创建引擎的原因和我选择 C#而不是 C、C++或 Rust 的原因。

## 为什么要定制引擎？

让我首先指出我已经编程很长时间了。我从小就在 QuickBASIC 4.5 中学习代码:QBASIC 的专业版，可以编译独立的`.exe`文件！我花了我的青春试图在我父亲的 ZEOS 386SX 上克隆主机 DOS 游戏，如 ZZT 和 Kroz 王国 II。当我了解到 C++(“真正的程序员”使用的语言)这个神话般的东西时，我改变了方向，再也没有回头。我总是很乐意在一个游戏的整个技术栈上工作。

到了上大学的时候，这对我来说是一件很容易的事:我去了，并获得了计算机科学的学士学位。我爱它的每一分钟！但最好的部分是学校推出了一门关于 3D 图形的选修课。我对 3D 游戏编程一无所知，所以我抓住了这个机会。那时我了解了强大的 4x4 矩阵。

那么，我为什么要自己做发动机呢？简而言之，我从这样做中获得了巨大的个人满足感。为了专注于游戏设计，我不认为底层技术是我想跳过的“实现细节”;底层技术*对我来说是*最好的部分。我想控制体验的每一个方面。我想控制我的存储格式。我想控制我的游戏选择加载资产的方式和时间。我想控制我的游戏窗口在调整大小或移动时的行为。

我正在为 Windows，macOS 和 Linux 制作一个 2D sidescroller 沙盒 ARPG。那完全在我的技术能力范围之内。我想要尽可能多的源代码所有权；就利润和技术寿命而言，我不想受制于任何发动机制造商。我并不反对任何使用统一或虚幻引擎的人；大部分游戏工作室都是商家第一，工匠第二。这些成本节约不容忽视。我喜欢认为(就目前而言)我的工作室显然是反过来的:工匠第一，商业第二。

明确地说，我正在编写自己的引擎，但是利用了现有的平台中间件。我使用 SDL 进行窗口和事件处理。在我的生活中有一段时间，我不害怕编写自己的中间件；我学习了 win32、Cocoa 和 Xlib 来了解一切是如何工作的，但我已经向前看了。像 SDL 这样的图书馆绰绰有余。

## 为什么是 C#？

这就是事情变得有趣的地方。:)

我绝对相信 C++很多很多年了。尤其是 C++11 的发布对我来说是一个游戏改变者。move 语义的引入解决了我认为的最后一个主要的 C++瓶颈。允许对象移动结束了像引用计数或写时复制机制这样的大的变通方法。我可以简单地将一个对象的内容转移到另一个对象，而不需要分配空间。

C#并不是我真正考虑的可行选择。我在工作的时候写 C#，但是它有太多的问题阻碍它成为真正的游戏开发语言。我对编码对抗的想法感到不舒服。然后尝试让所有东西在其他平台上都能以 Mono 的方式运行。另外，凭借我丰富的 C++背景，我非常了解 C#及其为每个小操作分配缓冲区*的决定。*

微软和。NET 基金会知道。NET 生态系统没有真正的未来，所以他们采取了激烈的措施。NET 和 C#进入了现代:他们抛弃了所有特定于 Windows 的 API 并创建了。NET Core，一个恰当的跨平台生态系统。此外，通过打破向后兼容性。NET Foundation 能够大幅提升性能，并解决超额分配的问题。

然而，真正的奇迹出现了。网芯 2.1 版:`Span<T>`。[这个...正是我所期待的。这个新类型解决了我对 C#和它的大部分 API 最大的不满。它允许我们传递对数组段的引用，而无需复制数组！你知道另一个最棒的部分是什么吗？它可以引用托管或非托管内存。所以，如果我创建一个方法`void Sort(Span<int> items)`，它将同时作用于来自其他 C#代码的`int[]`或者来自对本地 C 库的调用的`int*`。**太神奇了。**](https://msdn.microsoft.com/en-us/magazine/mt814808.aspx)

自从`Span<T>`发布后，我对 C++的忠诚迅速燃烧。我只是厌倦了处理`#include`排序(前向声明)、`Makefile`诡计、故障调试器和内存安全。我是运行`dotnet build`的*超级*粉丝，希望它能在任何桌面平台上获得成功。正如我上面提到的，我的目标硬件是台式机，所以我不需要从每一寸代码中挤出最后一个周期。我会在重要的地方进行优化。(面向数据的设计！)

实际上，我有一个用 C++编写的引擎的基本原型，为了支持我的新 C# one，我把它扔掉了。

## 结论

哇，谢谢你读完这些！我想在更多的方面进行详细的描述，我会在以后的文章中这样做。我只是想描述一下我现在的整个旅程，不想让太多人睡着。

在有人问之前，Java 从来就不是一个选项。它没有值类型，标准库也不好。`Span<T>`万岁。

问我任何事情。