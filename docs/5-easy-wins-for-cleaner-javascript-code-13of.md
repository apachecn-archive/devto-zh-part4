# 更干净的 Javascript 代码🧹的 5 个简单胜利

> 原文：<https://dev.to/mlevkov/5-easy-wins-for-cleaner-javascript-code-13of>

想象一个空的干净的厨房水槽。它是如此闪亮，你可以看到你的倒影。如果你有一个脏盘子，你可能会觉得把它扔进水池里会很难受，对吗？你会把它清理干净，然后收起来。现在，如果你的水槽已经满到边缘，一堆恶心的食物颗粒漂浮在肮脏的水中，该怎么办？在这种情况下，你会把你的盘子扔进去，因为，好吧，多吃一盘也无妨。不幸的是，这也是我们对待代码库的方式。我们并没有整理我们的代码库，只是有时会加入越来越多的代码味道。下面是你可以做的 5 件事，现在就开始整理你的代码库🚀。

出于某种原因，gists 有时会以一种非常奇怪的顺序呈现。如果代码与我写的内容不一致，刷新页面似乎可以解决问题。很抱歉。😕

## 1。使用 let 和 const，忘记 var

您不应该再使用 var，因为它很容易引入变量隐藏，并可能导致许多混乱。如果你需要一个不变的值，使用 const。如果你需要一个不变的变量，但你将在构造函数中初始化它，使用 readonly。如果你需要一个值不变的变量，用 let。