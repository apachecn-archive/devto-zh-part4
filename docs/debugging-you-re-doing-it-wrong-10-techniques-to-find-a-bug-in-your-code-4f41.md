# 调试——你做错了。在代码中发现 bug 的 10 种技巧

> 原文：<https://dev.to/nikpoltoratsky/debugging-you-re-doing-it-wrong-10-techniques-to-find-a-bug-in-your-code-4f41>

你还记得那些花在调试上的很长很长的时间吗？当你盯着一个代码库却不知道到底哪里出错了？你不是一个人！我认为所有的开发人员都会时不时地在调试中挣扎。这就是为什么在这篇文章中，我将告诉你我最喜欢的在代码中查找 bug 的方法。

## 目录

*   [谷歌错误信息](#google-error-message)
*   [控制台记录一切](#console-log-everything)
*   [使用调试器](#use-debugger)
*   [问题定位](#problem-localization)
*   [创建一些测试](#create-a-few-tests)
*   [分析日志](#analyze-logs)
*   [问朋友](#ask-a-friend)
*   git bisect
*   [和一只橡皮鸭说话](#talk-to-a-rubber-duck)
*   [手鼓舞蹈](#tambourine-dancing)

* * *

## 谷歌错误信息

我不打算按照有用性对这些提示进行分类，但是谷歌错误信息排在第一位是有原因的。我想，如果你连一个错误信息都看不懂，最简单的解决方法就是**谷歌错误信息**。

在大多数情况下，你所面对的错误信息是被其他人搜索过的。我们有很多美丽的地方，比如 StackOverflow 和 T2 GitHub issues，人们在这里互相帮助。这就是为什么你不仅能找到答案，还能找到避免这种错误的步骤，或者为什么这个错误首先会发生的动机。那么，为什么不去谷歌一下这个错误呢？这是这里最简单的方法。

* * *

## 控制台记录一切

我认为这是在代码库中找到 bug 的最流行的方法之一——在代码库中添加十分之一的`console.log(...)`语句，重新运行应用程序，并尝试找出哪里出错了。

我喜欢它，从我的角度来看，它是简单问题的正确解决方案，这些问题已经局限于几个类。但是，当你不知道这里发生了什么，也不知道那个令人毛骨悚然的虫子住在哪里的时候，用`console.log(...)`开始寻找总是一个坏主意。因为在这种情况下，您可能会错过一些没有记录的重要内容，您将不得不添加控制台日志并多次重新运行应用程序，直到您找到失败的原因。

* * *

## 使用调试器

我想我在这里没有太多要说的。所有开发人员都知道调试器是什么以及为什么要使用它。相反，我要告诉你一个前几天在办公室和我的一个队友发生的小讨论。

前几天我在和我的一个队友讨论不同的调试方式(这也是这篇文章出现的原因)。我说过找到问题的最可靠的方法之一是使用调试器。就这么简单——设置断点，然后执行一些操作，一步一步地检查代码库，观察应用程序的状态如何变化。还有什么比这更简单的呢？

而我的队友介绍了一个令人兴奋的想法——当你使用调试器时，你不是在训练你的分析技能和批判性思维。原因是使用调试器，你只是盯着有变量的窗口，等待不正确的行为。而当你在没有调试器的情况下挖掘代码库时，你会更加专注，并试图理解正在发生的事情，并在每次寻找 bug 时学习新的东西。

你对调试器和手工调查有什么看法？请在评论中给我你的想法。

* * *

## 问题定位

**问题定位**方法的关键思想是一步一步地注释/删除代码，直到你发现哪里出了问题。当您长时间没有编译和执行应用程序而编写一个算法或一些业务逻辑时，它特别有用。

在这种情况下，我总是做错一些事情，对我来说最简单的方法是部分注释代码，并试图拦截消失的错误。然后，重复它，直到所有的错误将被发现和修改。

在某些情况下，当您注释一半代码(或删除一半文件)时，使用二分搜索法线方法可能是个好主意——然后检查错误是否仍然存在。如果是，用这一半的一半重复同样的动作，如果是现在，用另一半重复同样的动作。

当然，这只有在特殊情况下才起作用，当你能够注释/删除代码部分仍然保持它的工作。

* * *

## 创建几个测试

好吧，这是一个疯狂的想法，然而它在某些特殊情况下可能是有用的。在我看来，当你有一些算法工作不正确时，它工作得很好，并且太复杂而不能用调试器写`console.log`或通过它。

在这种情况下，为它编写几个测试可能是有用的。测试可以帮助你定位算法中的问题。

之后，当错误被本地化，并且您知道在哪里搜索时，您可以使用另一种方法来最终修复它。

* * *

## 分析日志

是的，我知道你讨厌用日志分析所有这些 10mb 的文本文件。但是通常它可以为您节省几个小时的调试时间。当然，首先必须充分配置日志记录。收集的日志文件必须保留适当的时间。但是如果所有的条件都满足了——你就很幸运了。这就像控制台日志记录一样，但是您已经将所有的`console.log`都放在了它们的位置上，并且可以通读在您的系统上执行的操作。

但不幸的是，这通常是一个梦...我们并不总是对伐木给予足够的关注。

* * *

## 问朋友

这很明显，但我们并不经常这样做。我认为我们所有人在办公室里都有更有经验的队友。不然我们可以跨网找专家。别害怕，好吗？人们随时准备帮助你！

酪你需要满足一个要求——在询问某人之前，检查所有可用的信息来源。如果你在寻求一些简单的帮助，这些帮助写在文档的第二页上，那么。好吧。准备逃跑吧！

* * *

## Git bisect

Git 不仅帮助我们保存应用程序的修订历史，还为我们提供了一些调试工具。其中一个工具是 **git 平分**——这个工具用于在你的 git 历史上执行二分搜索法。如果您有一段时间没有使用代码库，并且自从您上次干预以来，已经添加了几百个提交，那么这是非常有用的。而现在，你遇到了一个 bug，却不知道它是如何以及何时出现的。但是你记得你在版本`2.0.15`中没有它，例如。

在那种情况下 **git 平分**会帮助你。这个想法很简单，你用`git bisect start`开始调试过程，然后，我们需要将当前版本标记为*坏*版本，因为我们这里有一个 bug-`git bisect bad`。之后，我们必须告诉 git 一个*好的*工作版本:`git bisect good 2.0.15`。在这个阶段，设置已经完成，我们可以开始搜索。

**git 平分**在*坏-好*范围的中间选择提交并检查它。然后，我们必须检查我们是否在这个版本中有错误？如果是-运行`git bisect bad`，如果不是-运行`git bisect good`。然后，git 将在原来的*坏-好*范围内选择一个新的提交，我们必须重复这个过程，直到找到一个有 bug 的提交。

**git 平分**是一个非常强大的工具，这就是为什么完整的描述会超出文章的格式，但是[这里有一个链接，里面有一个很好的解释](https://git-scm.com/docs/git-bisect)。

* * *

## 和一只橡皮鸭说话

这是理解代码中发生的事情的最有效的方法之一。主要的想法是，你需要找到一只橡皮鸭，把它放在你面前，然后向它解释你的系统，从一般概念开始，然后一行一行地解释。你可以在这里了解更多信息。我喜欢这个想法，并经常使用它，所以，让我告诉你一个小故事，我是如何第一次使用它，甚至没有注意到它。

当我刚刚开始我的程序员生涯时，我正在构建一个包含相当复杂的手写动画的 Android 应用程序。我小心翼翼地，一步一步地做了一个动画，一切都很好，直到我坚持下来。

我有一个不超过 150 行的动画代码。我找不到为什么它不工作的问题。我检查了所有的算法多次，但仍然一无所获。盯着屏幕看了几个小时后，我决定向一个队友求助。

我已经开始详细解释这个动画是如何工作的，很快，我就找到了问题所在！其中一个方法隐式地将`float`转换为`int`🤦‍♂️🤦‍♂️🤦‍♂️🤦‍♂️🤦‍♂️.当我试图向我的队友详细解释时，我花了一分钟才明白。

那时候我甚至不知道这是橡皮鸭的一种方法。后来我意识到。但我已经明白它有多强大。

* * *

## 手鼓舞蹈

这是我最喜欢的用软件解决问题的方式。你只需要铃鼓。你需要围着你的工作站跳舞，敲着铃鼓。如果你有一个带有你正在研究的技术标志的铃鼓，这将是一个很大的优势。例如，我有一个手鼓可以帮助我解决微服务的问题:

[![Java Tambourine](img/7991ecf926af03f78f269ff7105c8320.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--1XiZtUAW--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://i.ibb.co/j6mydhm/java-tambourine.jpg)

但是对于前端应用程序来说完全没有用。

* * *

## 结论

感谢您阅读本文！希望你能从这篇文章中学到一些新的东西。我想在这里向那些阅读材料到最后的人透露的最后一件事是，处理代码库中的 bug 和问题的最有力的方法是编写没有 bug 的代码。去问你的经理。😅

无论如何，要集中精力，始终如一。一步一步地做你的调查，使用上面提到的技巧。你就能有效地解决所有的问题。

[在 Twitter 上关注我，保持关注](https://twitter.com/nikpoltoratsky),如果你有什么特别想听的话题，请告诉我！