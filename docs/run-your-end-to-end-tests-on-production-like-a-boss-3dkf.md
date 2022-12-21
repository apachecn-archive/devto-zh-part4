# 像老板一样在生产中运行端到端测试

> 原文：<https://dev.to/mariocasciaro/run-your-end-to-end-tests-on-production-like-a-boss-3dkf>

端到端测试通常在测试或试运行环境中运行，这是有充分理由的。但是有些情况下，在生产环境中运行测试并不是那么疯狂。

## 为什么不投产？

有充分的理由不在生产环境中运行您的测试。

首先，当你的代码投入生产时，通常对测试来说已经太晚了😓。你想要运行你的测试，这样你的客户就不会得到一个坏的产品，所以通常你应该在代码到达你的客户手中之前测试你的代码*。*

其次，**你不想在没有先弄乱 staging 的情况下弄乱 production** 。这实际上是上述观点的结果。如果您没有首先在一个阶段环境中运行您的测试，并且您的测试破坏了一些东西，它们将破坏生产，说得委婉一点，这是不希望的。测试是为了发现错误，而错误是不可预测的，你永远无法确定它们的程度。您最终可能会破坏您的生产环境，甚至销毁数据，谁知道呢。从积极的方面来看，最好是您的测试发现问题，而不是您的客户，至少您会得到通知😝。

不在生产环境中运行测试的另一个原因是数据噪音。想象一下，多年来每天多次运行您的测试。你将**在你的生产数据**中结束有许多噪音，这将使它难以提取有意义的信息。如今，数据是一种宝贵的资源。在测试/试运行环境中，如果需要的话，您可以经常刷新数据库，以清除测试创建的所有数据。显然，这在生产上并不那么简单。

## 那么，为什么还要在生产中运行呢？

即使在生产环境中运行您的测试最初看起来可能不是一个好主意，但它有一些重要的优势。当然，正如我们将在后面看到的，您不应该在没有适当的计划来减轻我们刚刚谈到的负面影响的情况下在生产环境中运行您的测试。也就是说，让我们看看为什么**你应该**在生产上运行一些测试。

嗯，首先，**你的分期环境不是你的客户用的**。登台环境应该尽可能地复制生产环境，但是，您的登台环境永远不会与您的生产环境完全相同。至少，您将拥有不同的 URL，但是您也可以拥有不同的 API 键用于外部服务或变量，这些外部服务或变量在生产时而不是在暂存时启用/禁用某些东西。这些是您的 QA 策略中的黑点，也只能通过测试您的生产环境来消除。

此外，您的应用程序的**良好状态不仅仅来自它的代码**，而是来自以下因素的组合:

*   它使用的数据的**完整性。**
*   任何外部系统的**状态(如数据库、存储服务等)。)看情况。**
*   它在其上运行的运行时环境的**状态(例如，服务器、PaaS 等。).**

所有上述因素在您的暂存环境和生产环境之间必然是不同的。如果您的数据在生产中由于您遗漏的一个 bug 而损坏，您的应用程序可能会变得不稳定。如果您的存储提供商出现问题，您的用户将无法上传他们的文件。如果您错误地仅在生产服务器上设置了某些文件权限，您可能无法运行应用程序的某些部分或访问它需要的某些文件。

毕竟，端到端测试**也是一种集成测试**，所以你将能够捕捉到所有上述问题——希望在你的客户注意到它们之前——在生产中运行一个适当设计的测试计划。

## 如何在生产中正确运行测试

因此，我们评估了在生产上运行一些端到端测试确实是有益的，但也有一些我们应该考虑的警告。因此，下面是在生产环境中运行一些端到端测试时要考虑的良好实践列表。

### 不要仅仅依赖“生产”来进行测试

这大概是最重要的一个方面。在进入生产环境之前，您至少应该在您的应用程序*上运行相同的测试集，在一个试运行或测试环境中。这将确保您的测试不会破坏生产，但是它也将在您的客户之前捕获任何主要的 bug。*

在理想的设置中，我们会有一个三层测试计划:

*   在代码投入生产之前，在试运行阶段运行完整的回归测试套件。
*   在每次提交和部署后，对试运行运行一组较小的测试(冒烟测试)
*   定期在生产环境中运行一组更小的测试

### 少考，常考

我们真正想做的是*监控*生产环境，而不仅仅是测试它。这意味着应该有非常有限数量的非常有意义的测试，以很短的时间间隔连续运行。这将**确保您的应用程序实际运行，其主要组件正常工作**。

测试运行的时间间隔可能会有所不同，这取决于测试运行的时间。但是一个好的值落在 15-30 分钟之间。

### 测试后的清理

您的生产数据很重要。你可以用它来改善你的产品，你的客户体验，现在甚至可以建立强大的机器学习模型。因此，重要的是不要让我们的测试产生不必要的噪音。

一个解决方案是删除我们在测试过程中创建的所有数据。我们可以使用各种策略——我们将有一个单独的博客帖子来详细讨论它们——但是最简单的方法可能是使用相同的测试自动化来执行清理。

## 姑且称之为前端监控

定期检查网站或网络应用程序是否正常运行的做法也被称为**监控**。

在最底层的实现中，我们有**正常运行时间监控**，它只是确保我们的服务正常运行。这通常是通过检查对 HTTP 端点调用的响应来完成的。T2 有许多供应商可供选择。

如果我们决定定期在产品上运行我们的端到端测试，我们实际上是在实现一些**前端监控**(有人称之为*用户流监控*)。

这两种技术的范围和目的是不同的，但在许多方面是重叠的。两者都旨在确保生产应用程序正常运行。正常运行时间监控通常运行非常基本的检查，需要瞬间运行，这就是为什么监控间隔通常非常短，在许多情况下低至 1 分钟。另一方面，为前端监控执行的检查(或测试)需要更长的运行时间，但也更加详尽，因为它们还会考虑用户界面子系统及其与后端的集成。

## 结论

测试和 T2 生产这两个词很少出现在同一个句子中，这是有充分理由的。然而，我们已经看到，通过适当的测试策略，我们可以在生产中安全地实施**前端监控**解决方案。

如果你有兴趣建立一个前端监控解决方案，[前端机器人](https://frontendrobot.com)有一个专门为这个用例设计的[测试调度器](https://frontendrobot.com/docs/scheduling-runs/)。