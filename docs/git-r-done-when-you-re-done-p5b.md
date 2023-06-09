# 🌮你做完了就完事了。

> 原文：<https://dev.to/chiangs/git-r-done-when-you-re-done-p5b>

*我再次等待我的应用程序运行测试和构建，所以我想我应该再写一个 quickie。*

 *之前我展示了如何为紧急情况编写一个 git 函数，我需要 [GTFO](https://dev.to/chiangs/in-case-of-fire-gtfo-30g4) 。

今天，我想向你展示一个更常见的过程，如果我们独自或在小团队中工作，我们都可能会这样做。

当我完成一个功能分支时，通常会经历以下步骤:

*(假设我在我的目录的顶层，并且我过去做过`git push origin head -u`)*

```
> {feature-branch} git add .
> {feature-branch} git commit -m "Awesome commit msg"
> {feature-branch} git push
> {feature-branch} git checkout develop
> {develop} git merge -
> {develop} git push 
```

Enter fullscreen mode Exit fullscreen mode

如果我想在本地和远程清理合并的分支，我也必须这样做:

```
> {develop} git branch -d feature-branch
> {develop} git push origin :feature-branch 
```

Enter fullscreen mode Exit fullscreen mode

***那可是好多步骤。*T3】**

下面是我如何让`git done`和`git donep`变得更简单，我实际上为`git`设置了一个别名`g`，所以从现在开始我将使用它:

我将进入我的`.gitconfig`并创建两个新的别名，如下所示:

```
[alias]
  done = ""
  donep = "" 
```

Enter fullscreen mode Exit fullscreen mode

使用与我的`g tfo`函数相同的语法，我将首先创建`done`，它执行以下操作:

1.  合并分支
2.  删除本地分支
3.  删除远程分支

那个函数看起来是这样的:

```
done = "!f() { git merge $1 && git branch -d $1 && git push origin :$1; }; f" 
```

Enter fullscreen mode Exit fullscreen mode

你会注意到`$1`。这是对一个变量的引用，当调用函数时，这个变量将被传入，而这个函数恰好是分支名称。现在，我已经尝试使用了`-`和`@{-1}`，但是删除函数的远程分支部分不会接受它们作为对最后使用的分支的引用。因此，除非这里的读者有什么想法，否则我只能具体地传入特性分支名称，这在某种程度上也使它更易于重用，但是如果分支名称很复杂或者很难记住，这就有点麻烦了。但至少，你仍然只需要输入一次！

🤷‍♂️ ***那么我们如何使用它呢？【T2***

```
> {develop} g done feature-branch 
```

Enter fullscreen mode Exit fullscreen mode

🤯嘣。就是这样。

但是对于这一个，我们没有推动开发远程的改变。这就是`donep`的用武之地。

```
donep = "!f() { git merge $1 && git branch -d $1 && git push origin :$1 && git push; }; f" 
```

Enter fullscreen mode Exit fullscreen mode

```
> {develop} g donep feature-branch 
```

Enter fullscreen mode Exit fullscreen mode

***注意:*** 这个功能也假设你已经做了`git push origin head -u`并设置了上游。如果还没有也不想要，可以将功能修改为:

```
donep = "!f() { git merge $1 && git branch -d $1 && git push origin :$1 && git push origin $2; }; f" 
```

Enter fullscreen mode Exit fullscreen mode

然后像这样运行:

```
> {develop} g donep feature-branch develop 
```

Enter fullscreen mode Exit fullscreen mode

🐱‍👤 ***Git r' done！*T3】**

如果你觉得这很有价值，请留下评论并关注我的[dev . to @ Chiang](https://dev.to/chiangs)和 [Twitter @chiangse](https://twitter.com/chiangse) ，🍻干杯！*