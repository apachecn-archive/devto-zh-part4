# C#函数式编程

> 原文：<https://dev.to/gabrielschade/c-functional-programming-4f7o>

让我们从一个问题开始:

> 值得吗？

像软件开发中的几乎所有事情一样，这取决于…我真的很喜欢函数式编程范例，那么，可能这里有一些偏见。但是让我们试着讲道理，对吗？

第一大点是:为什么要用 C#？—如果你想使用函数式编程，那么在. NET 环境中， **F#** 很有可能比 C#更适合。

这里有一个小问题，很多。NET 开发人员不知道什么是 F#，我真诚地认为这是一个巨大的错误，但这是另一篇文章的主题。所以，让我们把它保持在 C#上，这就把我们带到了下一个问题:

> 你喜欢系统吗？Linq？

答案大概会是: ***是的！*T3】**

因为制度。Linq 很棒，你猜怎么着，它使用了一个核心函数概念:*高阶函数*。还有很多与系统相关的东西。Linq，像扩展方法，IEnumerables 等等。

回到最初的问题，C#包含了一些与函数范式相关的固有特性，所以，公平地说，使用它你可以做很多事情。

让我们编写一个简单的例子，我们想要迭代一个`IEnumerable`来显示它的每一个元素: