# VS 代码 VS Atom——如何抉择？

> 原文：<https://dev.to/areknawo/vs-code-vs-atom-how-to-decide-25b7>

**这篇文章摘自[我的博客](https://areknawo.com)，所以请务必查看更多最新内容。**

我想我们都同意每个程序员都需要一样东西，不，我不是在说电脑——这太明显了。我说的是软件- **文字编辑器**。无论是基于 GUI 还是基于终端，IDE 与否等等。永远是必需品！

就我而言，由于我主要从事 web 开发，我不需要任何特别的东西。在 JS 生态系统中，所有工具都足够直观和易于使用，因此我不需要任何特殊的 IDE 或任何其他抽象层。现在，当谈到舒适，这是一个完整的另一个故事。也许我不需要 IDE，但这并没有阻止我尝试 10 多种不同的文本编辑器——简单的、IDE 的、类似 VIM 的等等...——就是为了找一个我用起来最舒服的。还有男人！-我花了很长时间...

长话短说，我选择了[**VS**](https://code.visualstudio.com/)，但我仍然安装了另外 3 个我最喜欢的代码编辑器，它们是 [WebStorm](https://www.jetbrains.com/webstorm/) 、 [Sublime Text](https://www.sublimetext.com/) 和 [**Atom**](https://atom.io/) 。由于前两者是付费的，并且为可扩展的编辑器(如 VS Code 和 Atom)提供了类似的功能，大多数人会在这两者之间做出选择。

# 一些背景

因为你正在阅读这篇文章，我敢打赌你至少已经了解了一些关于 Atom 和 VS 代码的知识。对于那些不知道的人，这里有一些信息。

VS 代码和 Atom 都是**开源**代码编辑器，最初分别由微软和 GitHub 创建。每一个编辑器都基于 [**电子**](https://electronjs.org/)——一个用 web 技术创建*【原生】*桌面应用的框架——HTML、CSS、JS——加上 Node.js。我说的*【原生】*是指预捆绑 Chromium——Chromium 越多越好，对吗？这些编辑器被认为适合用于不同的编程语言，但是毫无疑问它们最适合 web 开发，尤其是考虑到它们的构建方式。

编辑器还具有现代外观的用户界面、高级语法突出显示、开发良好的扩展和主题化系统。我们一会儿会谈到，来比较两者，但是首先...

# 一点历史

从历史的角度来看，原子是第一位的。Atom 于 2014 年发布，是第一个用 Electron 构建的软件(当时被命名为 *Atom Shell* )，也是 Electron 最初创建的软件。它过去是，现在也是作为*“21 世纪的黑客编辑器”*进行营销的。没过多久，编辑器就开始流行起来，和电子版一起成为了完全开源的**(最初只有部分代码)。**

 **开源电子导致了许多不同应用程序的产生，现在每天都有许多人在使用。其中一个应用是——你猜对了——VS 代码。一个旨在*【重新定义】*整体开发体验的代码编辑器。

尽管这两个编辑器在本质上非常相似，但事实证明 VS Code 更胜一筹，尤其是在考虑性能的时候。随着时间的推移，越来越多的用户选择 VS 代码而不是 Atom。它变得越来越受欢迎，因此发展了更大的社区和用户基础。

从最近的历史来看，在最近的 2018 年的[栈溢出开发者调查中，VS 代码被认为是*所有类别中最受欢迎的开发工具*。尽管如此，Atom 仍然被广泛使用。但是，随着微软](https://insights.stackoverflow.com/survey/2018#technology-_-most-popular-development-environments)[收购 GitHub](https://blogs.microsoft.com/blog/2018/10/26/microsoft-completes-github-acquisition/) ，Atom 编辑器的前景并不乐观。虽然这是一个由其社区开发的开源项目，但来自 GitHub 的创作者的贡献无疑构成了整个代码库的重要部分。即使微软最近成为一个更加开放和支持自由/开源软件的公司，当一个比另一个更好的时候，维护两个非常相似的工具也没有多大意义...或者是？嗯，还有待观察。目前，Atom 仍在运行，我们将对这两个编辑器进行简单的比较。

# 设计

让我们从最*【争议】*的品类——设计开始比较吧。现在，我知道**的个人偏好**可能会在这一类别中有所不同，但我会尝试至少提供我自己的观点。

因此，Atom 和 VS 代码开箱后看起来都不错。使用下面有 **CSS** 的 HTML 很容易完成。它们都有漂亮的或暗或亮的简约设计，以及丰富的主题选择。再次感谢 CSS，您可以轻松地为两个编辑器创建自己的自定义主题，或者使用扩展注册表中提供的主题——有很多！

在我看来，设计良好的用户界面不会分散你的注意力，也不会让你想一会儿。这就是为什么，无论我使用 Atom 还是 VS 代码，我总是使用类似的主题，基于**谷歌的材料设计**。找到一个并不困难，因为两个编辑器的最佳选择都是最受欢迎的，并且下载量最高。

说了这么多，我总有一种感觉，Atom 的 [**Atom 材质 UI**](https://atom.io/themes/atom-material-ui) 比 VS Code 的 [**材质主题**](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme) 在我看来更好看。结合 Atom 的通用 UI 布局，它看起来更简洁，即使与仍然很棒的 VS 代码相比也是如此。此外，虽然它更适用于主题而不是代码编辑器本身，但 Atom Material UI 开发人员在使用**对比**方面做得更好，尤其是考虑到我经常使用的轻量级版本。尽管如此，我还是要提醒你，这是个人喜好的问题，你完全不必同意我的观点。无论如何，在这里，重点是原子。

# 表现

关于电子应用的性能，不同的人有不同的说法。它们很慢，消耗大量内存等等。而且，虽然我可以否认这种说法有些道理，但不同的应用程序不一定会有与其他应用程序相同的性能问题。当然，如果你只关心原始性能，我的建议是使用付费的 **Sublime Text** ，这是目前整个市场上最快的代码编辑器之一，但是，由于我们只比较 Atom 和 VS 代码，你可以把它作为一个参考点。一般来说，这两者在性能谱上都介于 Sublime Text 和 **WebStorm** (一个成熟的 IDE)之间。但是具体在哪里呢？

即使 Atom 是第一个，每个使用过这两个编辑器的人，至少在某种程度上，都必须同意——VS 代码只是**快**。与之相关的一切都是快速、流畅和高性能的。那么，电子怎么会比...电子？！嗯，答案以**优化**的形式出现...和扩展。两个编辑器都有大量的**第三方扩展**，主题和可用工具，可以帮助你根据自己的需要定制它们。但是，由于这些扩展通常被大量使用，它们肯定会对应用程序的整体性能产生一些影响。最好的例子是 Atom 的扩展中心的一个小标签，指示给定的扩展增加到加载时间的时间量。VS 代码背后的人已经做了更好的工作，使得所有这些扩展(或者更确切地说是它们背后的**架构**)对性能要求更低。

当然，Atom 的开发人员认识到了这个问题，并在不断改进他们的产品。最近他们正在开发 Atom 的**文件浏览器**，取得了一些令人印象深刻的成果！在基于 Rust 的 [regex 库](https://github.com/BurntSushi/ripgrep)(很可能还有一些 WASM)的帮助下，他们大幅提高了搜索速度(尤其是在大型项目中)。所以，我只希望这些**的性能提升**能继续下去，最终赶上 VS Code 的产品...

# 生态系统

现在，由于 VS 代码和 Atom 都有自己的**扩展**和**主题化**系统，让我们也对它们做一个简单的概述吧！如果原始数据是你所关心的，那么我想你会很高兴听到这两个编辑器都发布了大约 **11K - 12K** 的扩展和主题，而 VS 代码只多一点点。

两个扩展注册表都可以通过编辑器或网站获得——分别针对 [Atom](https://atom.io/packages) 和 [VS Code](https://marketplace.visualstudio.com/) 。在两个编辑器中，只需点击一个按钮就可以轻松安装一个扩展，根本不需要重新加载(只是最近才添加到 VS 代码中)。移除过程也是如此。

如您所见，从用户的角度来看，扩展系统尽可能简单。那么扩展开发者呢？两个官方文档( [Atom](https://atom.io/docs) 、 [VS Code](https://code.visualstudio.com/api) )都有详细深入的**指南**和 **API 参考**。当比较两者时，我认为 Atom 的比 VS Code 的更适合初学者。可能是因为 VS Code 在扩展方面的架构更复杂。但是，总的来说，如果你想创建你的第一个扩展，也许在开始 VS 代码之前，先用 Atom 来看看它到底是什么...

# 配置

大量的扩展和定制带来了大量的配置。而且，不管你是否同意我的观点，它代表了编辑器整体用户体验的一个重要部分。

VS 代码的配置只涉及到一个简单的 JSON 文件...直到最近。现在，只要有可能，一个 **GUI 界面**是可用的。这相当简单，但它完成了它的工作，而且做得很好。这只是对我们已经拥有的 JSON 和基于 TS 的自动完成功能的一个小小的抽象。

在原子方面，情况看起来只有一点不同。你不再需要编辑一个 JSON 文件，而是到处都有 GUI。编辑器设置本身与扩展完全分离，因为每个扩展都有自己的专用页面。我认为这是一个很好的方法。可悲的是，在配置过程中，一些用户报告了滞后和其他**性能问题**。也许是因为他们安装了太多的扩展？就我个人而言，我没有遇到过类似的问题，可能是因为我安装的扩展数量相当少，但谁知道呢？

# 经历

现在，让我们再一次分析已经说过的内容，加上一些补充，以确定使用给定代码编辑器的**一般经验**是什么。

虽然 Atom 的极简 UI 看起来很华丽(至少对我来说)，但 VS 代码的性能是好的设计无法掩盖的。另外，VS 代码看起来还是很酷的！这两个编辑器都有大量的扩展可供选择，并且配置它们的方式非常简单。

我还没有谈到的是整体的*“开发舒适度”*。我的意思是，在给定的编辑器中编写代码是多么的**愉快。由于我不能谈论 JavaScript 之外的语言，这里是我的 web 开发定制意见- **VS 代码钉钉！我认为这对任何人来说都不足为奇。作为同一个公司——微软——创造了惊人的代码编辑器和生产力增强的 JavaScript 子集类型脚本——这些产品注定要很好地一起工作！它们确实配合得很好，TS 提供了自动补全和所有所谓的*“智能感知”*功能，没有 TS 你只能梦想。Atom 仍然有自己的扩展来完成这项工作，但是他们仍然有很长的路要走，才能达到 VS Code 目前提供的结果。****

但是，Atom 也有一些锦囊妙计。我认为最大的一个是它的 GitHub 集成。因为 Atom 以前是 GitHub 的附属公司，这个特性有点...可预见的。到目前为止，VS Code 有相当数量的 GitHub 相关扩展，但是没有一个能够复制 Atom 所提供的。然而，由于最近微软的收购，事情可能会开始改变...

# 决策过程

以上就是我对这两位编辑的主观和客观的看法。现在，是你**选择**的时候了！特别是对于初学者来说，这可能是一个非常耗时的过程，所以我建议你两者都尝试一下，最终选择**的那个**。即使这样，记住你的选择不一定要保持不变。如果你已经下定决心，那就太好了！考虑在评论中让我知道，*你的选择编辑是什么*？

我希望这篇文章至少给你提供了一些有趣和有用的信息。如果确实如此，考虑一下通过在 Twitter 上关注我的**[**在我的脸书页面**](http://facebook.com/areknawoblog) 或者只是通过查看我的个人博客 来表达你的支持。再次，我希望你喜欢阅读，并祝你有一个美好的一天！****