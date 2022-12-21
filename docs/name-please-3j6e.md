# 请问姓名？

> 原文：<https://dev.to/burdettelamar/name-please-3j6e>

最近我发表了一篇文章，文章中的例子是用 Ruby 交互式 Shell`irb`完成的。

将片段从`irb`窗口复制并粘贴到帖子编辑器中是一件烦人又乏味的事情。

所以我决定自动化。(修卡！).

我正在建立一个程序，它将:

*   阅读包含用于`irb`的片段的 markdown。
*   通过`irb`运行每个这样的片段，捕获会话。
*   用该输出替换原来的代码片段。
*   将更改的段落标记为 Ruby 代码块。

问题是，该怎么称呼它？

*   开始思考`irb2md`，但是尽管输出是降价的，输入不仅仅是简单的`irb`。
*   现在是`irb_filter`，但这也不能说明一个准确的故事。
*   倾向于`irb_in_md`，但对此并不高兴。

建议？