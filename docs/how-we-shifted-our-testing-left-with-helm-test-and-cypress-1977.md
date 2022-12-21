# 我们如何用头盔测试和赛普拉斯测试来改变我们的测试

> 原文：<https://dev.to/steffendev/how-we-shifted-our-testing-left-with-helm-test-and-cypress-1977>

我们遇到了一个问题。我们的开发严重超过了我们的测试速度，我们的测试人员没有时间做真正好的回归。这当然导致了 bug 溜出门外，影响了我们的生产用户。我们需要改变，所以我们决定拿出一些开发能力，用它来创建自动化测试，以减轻我们 QA 的负担。

这就是赛普拉斯的用武之地。如果你不熟悉 [Cypress](https://www.cypress.io/) 是一个绝对令人惊叹的测试工具，它让开发人员用 javascript 编写测试，而没有像 Selenium 或 Ranorex 这样令人头疼的现有工具。我不会在这篇文章中讨论它的优点，但是在试用了 Cypress 之后，我们决定用它重写我们的整个测试套件。在大约一个 sprint 之后，我们实现了一个核心回归测试(我们的应用不是超级大的，所以我们的核心回归也不是)。我们最终在不到 2 分钟的时间内完成了大约 30 个测试。

这是事情开始变得有趣的地方。Cypress 发布了 [base docker 图片](https://github.com/cypress-io/cypress-docker-images)，我们开始在它们的基础上进行 cypress 测试。我们在与主应用程序相同的 Jenkin 管道中进行容器化，以确保我们的应用程序及其测试保持同步。然后，测试和应用程序映像都被推送到我们的工厂，并用版本号进行标记。

我们使用 kubernetes 来编排我们的环境，并使用 Helm 来管理 Kubernetes 配置。Helm 有一个很棒的功能叫做 [Helm Tests](https://helm.sh/docs/developing_charts/#a-breakdown-of-the-helm-test-hooks) ，它允许为一个图表运行一个测试图像，并报告它的状态。我们配置了我们的 helm 图表来使用测试图像，并且我们已经准备好使用 helm test 命令来运行我们的 cypress 测试。

所有这一切的最终结果是一组自动化的回归测试，在每个环境的每个版本上运行。我们头盔测试中的任何失败都会被报告为部署失败。这给了我们的 QA 团队信心，在他们开始特性测试之前，新图像已经通过了基线回归检查。我们有一句谚语“如果你不打破，你就没有建设”。错误是不可避免的，但现在我们能够在 QA 花费任何精力之前发现它们，他们可以专注于功能测试，这是我们为用户提供价值的地方。

展望未来，我们计划扩展我们的测试套件，以包括更多的边缘案例。当我们的 QA 手动发现错误时，我们希望将它们合并，这样它们就不会再次被遗漏。此外，我们还考虑定期对生产环境运行测试套件，作为一种健康检查，让我们在用户发现问题之前发现问题。