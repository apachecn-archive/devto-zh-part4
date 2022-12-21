# 你被一个包裹卡住了，你会怎么做？

> 原文：<https://dev.to/bertheyman/help-i-m-stuck-with-a-package-49ad>

所以，你正在努力解决一个复杂的问题，并找到了完美的解决方案。直到你发现一个突然打嗝或错误。

# 根源问题是什么？

试着找到你需要解决的潜在问题。也许这个包对你的用例并不完美，或者你的方法需要一些调整。

*   有没有**简单的变通方法**？通常，很多路线会把你带到罗马。
*   这个套餐真的是您需要的定制解决方案吗？或者只有 90%，你最后会黑掉剩下的部分吗？
*   包**是否得到良好的维护和记录**还是被废弃？一个使用良好的软件包可能比一个缺乏活动的软件包覆盖更多的情况和语言版本。

# 检查已知问题的包

如果这个包适合您的用例，但是您仍然会遇到问题，那么其他人可能也在同一条船上。搜索 Github 或 Gitlab 问题以寻找答案。

提示:搜索时，问题也包括已关闭的问题，因为这些问题通常包含已回答的问题。

还没有吗？考虑开一个新的问题。
尽可能多的添加上下文(当前语言版本，之前尝试过什么，...)，这样有助于快速得到答案。

# 去做&解决它/拉请求

对于较小的 bug，您可以尝试直接在您的供应商文件夹中编辑这个包——然后用一个 pull 请求来应用更改。
对于更复杂的情况，下拉软件包的一个自己的分支(参见[这是马特·斯托弗的优秀指南](https://mattstauffer.com/blog/how-to-contribute-to-an-open-source-github-project-using-your-own-fork/)马特·斯托弗的】)并将 composer 链接到您的本地版本。不确定该怎么做？[这里有一个阅读](https://johannespichler.com/developing-composer-packages-locally/)。

pull 请求实际上是对包维护者的请求，将您的更改拉入项目中——不要与您从远程拉代码相混淆。

> 同样，添加尽可能多的上下文:您对这个拉取请求到底做了什么？为什么用某种方式解决？维护人员必须处理大量的拉请求，所以这些信息对处理它们会有很大的帮助。

不要认为你的解决方案不够好。维护人员将为您提供清晰的反馈，并在需要时请求更改。事实上，我的第一个拉请求被拒绝了，因为它引起了另一个我没有想到的问题——我得到了一些很好的反馈，并从中学习了很多。

*记住:99.9%的时间包所有者都是志愿者，所以给他们一些时间来回顾你的改变。*

接受了？恭喜你，你刚刚完成了你的第一个开源贡献！根据不同的情况，少数/数千人会从你的工作中受益。牛逼吧？