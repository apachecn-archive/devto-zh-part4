# PSA: Go 1.13 默认模块代理隐私

> 原文：<https://dev.to/luispa/psa-go-1-13-default-module-proxy-privacy-ckh>

原帖[此处](https://codeengineered.com/blog/2019/go-mod-proxy-psa/)。

## PSA: Go 1.13 默认模块代理隐私

Go 1.13 刚刚发布，默认情况下使用 Google 操作的代理来获取模块依赖。

Go 模块包含了在以模块形式获取依赖项时使用代理的能力。jFrog 迅速推出 [GoCenter](https://gocenter.io/) 提供高性能缓存。通常情况下，从 GoCenter 获取模块比从 GitHub 等地方获取要快得多。GoCenter 针对此用例进行了性能优化。

## 谷歌默认

随着 Go 1.13 的发布，`GOPROXY`默认为`https://proxy.golang.org,direct`。这意味着像`go get`和`go build`这样的命令将试图从 Go 代理中获取模块，该代理由谷歌操作，并受谷歌隐私政策的管辖。如果该模块不存在，Go 将尝试从源代码中获取它。

值得称赞的是，当你访问[https://proxy.golang.org/](https://proxy.golang.org/)时，你会发现的第一个链接是到[隐私政策](https://proxy.golang.org/privacy)的，在那里信息被捕获，隐私政策被记录。我很高兴他们分享了这些信息，并且很坦诚。

## 潜在泄漏

这可能会给专有软件带来问题。尤其是那些为谷歌开发有竞争力的解决方案而没有关注的人。

考虑包是公司私有的情况。也许它们托管在内部 Gitlab 或 GitHub Enterprise 上。这些是内部应用程序或专有软件。关于这些包的细节将被发送到一个代理，默认情况下是由谷歌操作的。

想象一下人们可以用这种信息拼凑出的细节。您知道一个或一组 IP 正在拉动某一组模块。有些是公开的，你知道细节，有些是私人的，但是名字会泄露一些。从这个信息中可以推测出什么？尤其是如果他们有来自其他数据源的其他数据要与此合并。

留意这种泄漏是公司管理层经常试图关注的事情。

## 更改您的配置

Go 团队意识到了这个问题，这就是为什么有像`GOPRIVATE`和`GONOPROXY`这样的环境变量可以和`GOPROXY`一起用来控制代理配置和信息泄漏。

如果你在 Go 中处理一段专有代码，你应该了解这些环境变量。

这些变量将让您控制发送给代理的内容，甚至让 glob 模式匹配。这对于更细粒度的控制很有用。

## 违约是一件大事

违约是一个大问题。大多数人大部分时间使用默认设置操作。许多人甚至不知道可以改变的设置或他们的选项。就 Go 而言，如果大多数使用 Go 的开发人员没有意识到这种变化正在发生，并且它会悄悄地对他们生效，我不会感到惊讶。

默认设置的影响并不是一个新的想法。早在 2005 年，雅各布·尼尔森就写过违约的力量。虽然这篇文章一开始谈到了搜索引擎，但它也涉及到了其他界面。在这一点上，它指出:

> 在用户界面设计的许多其他领域，用户依赖默认值。例如，他们很少利用花哨的定制功能，这使得优化默认用户体验变得很重要，因为这是大多数用户坚持的。

在这种情况下，Google 优化了默认的用户体验，向他们发送依赖信息。

原帖[此处](https://codeengineered.com/blog/2019/go-mod-proxy-psa/)。