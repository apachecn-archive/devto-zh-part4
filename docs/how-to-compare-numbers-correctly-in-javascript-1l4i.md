# 如何在 JavaScript 中正确比较数字

> 原文：<https://dev.to/alldanielscott/how-to-compare-numbers-correctly-in-javascript-1l4i>

这篇文章中的建议与 JavaScript 有关，因为 JavaScript 中的所有数字(目前)都是 IEEE-754 双精度浮点数。然而，这里的一切同样适用于任何具有浮点类型的语言。

> 简而言之:不要使用语言提供的等式测试，也不要使用语言提供的“ε”常数作为你对错误的“容忍度”。而是选择自己的承受力。

现在是长版本(我最初是为了回应我在网上发现的关于如何在 JavaScript 中比较数字的一些有缺陷的建议而写的)。

## 问题，以及解决问题的有缺陷的方法

以这段(“坏的”)代码为例，它解决了经典的浮点问题`(0.1 + 0.2) == 0.3`返回 false:

```
let f1 = 0.1 + 0.2;
let f2 = 0.3;
console.log(Math.abs(f1 - f2) < Number.EPSILON); // 'True - Yippeee!!!' 
```

好的，到目前为止还不错。但是其他输入失败:

```
let f1 = 1000000.1 + 0.2;
let f2 = 1000000.3;
console.log(Math.abs(f1 - f2) < Number.EPSILON); // '!!!!!! false !!!!!!!' 
```

使用的基本模式是合理的:避免直接的相等比较，并检查您的两个数字是否在可容忍的差异范围内。然而，所用的公差选择不当。

## 为什么会数。上面第二个例子失败了？

用数字其实是很危险的。ε作为数字比较的“公差”。

其他语言也有类似的结构。网络语言都有 double。例如ε)。如果您查看任何关于此类常数的声音文档，它们往往会附带一个警告，不要使用“浮点 epsilon”进行比较。

语言提供的“epsilon”只是您可以用特定浮点类型表示的最小可能“增量”。对于 IEEE 双精度数，该数(number。ε)很小！

使用它进行比较的问题是，浮点数的实现类似于科学记数法，其中有少量的有效数字和一个向左或向右移动小数点的指数(可能向左或向右移动 1000000)。

双精度浮点数(JavaScript 中使用的)大约有 15 个有效(十进制)数字。这意味着，如果你想保存一个像 1，000，000，000 (10 位有效数字)这样的数字，那么你只能保存一个小数到大约 5 或 6 位。双精度浮点数 3，000，000，000.00001 和 3，000，000，000.000011 将被视为相等。(注意，因为浮点数是以二进制形式存储的，所以并不是一直都有*精确的* 15 个有效的十进制数字——信息是以 2 的幂而不是 10 的幂丢失的)。

号码。ε是 waaaaay 小于 0 . 00001，所以第一个例子的“容差”是数字。ε(因为被比较的数字都小于 1.0)，第二个例子就破了。

## 没有放之四海而皆准的“ε”可供比较

如果你去网上搜索，会有很多关于如何选择合适的ε(或容差)来进行比较的讨论。经过所有的讨论，以及一些非常聪明的代码，有很好的机会计算出一个“动态计算的通用ε”(基于被比较的最大数字)，它最终总是归结为:

> 您需要选择对您的应用有意义的公差！

动态计算的容差(基于被比较数字的规模)不是通用解决方案的原因是，当被比较数字的集合在大小上变化很大时，很容易出现打破最重要的平等规则之一的情况:“平等必须是可传递的”。即

> 如果 a == b，并且 b == c，那么 a == c 的值也必须为 TRUE！

在程序中使用随每个等式测试而变化的容差是一个非常好的方法。= c 当你有理由认为 a 和 c 相等的时候。你也可以保证这将发生在恼人的“随机”时间。这是去虫岛的路:如果你敢进来，愿上帝宽恕你的灵魂...arrrrrrrr**！！！

** *其实...“啊啊啊啊啊！！!"更合适的是*

## 为您的应用选择公差

那么，如何为你的程序选择一个合适的公差呢？很高兴你问了！...

让我们假设你有一个以毫米为单位的建筑尺寸(一个 20 米长的建筑就是 20，000 毫米)。当你比较的时候，你真的关心这个尺寸和其他尺寸是否在 0.000000001 毫米以内吗？-可能不会！

在这种情况下，合理的ε(或容差)可能是. 01 或. 001**。请将其插入到`Math.abs(f1 - f2) < tolerance`表达式中。

绝对不要让**而**在这个应用程序中使用`Number.EPSILON`，因为*可能*在某个地方得到一个 200 米长的建筑(200，000 毫米),这可能无法与使用 JavaScript 的`Number.EPSILON`得到的另一个 200 米长的建筑进行正确的比较。

** *如果你使用可以用二进制精确表示的公差，事情会变得更简单。一些不错的简单选项是 2 的幂。例如 0.5 ( 2^-1)、0.25 ( 2^-2)、0.125 ( 2^-3)、0.0625 ( 2^-4)等。*

## 尽可能避免浮点数

**即使在不可避免的 JavaScript 中**

顺便说一句，如果你不在乎前一个例子中的测量值之间的距离是否小于 1 毫米，那么你可能只需要使用整数类型就可以了。

如果你在用 JavaScript 工作，那么你[目前**]会被浮点数所困。JavaScript 提供的唯一真正的替代方法是将数字存储为字符串。对于只需要测试相等性而不需要对其执行数值运算的大整数(如数据库主键)，这实际上是一种明智的方法。当你得到的整数大到足以包含超过 15-16 位的数字时，还有更多的“浮点陷阱”在等着你！(具体来说，任何大于 9，007，199，254，740，991 的数字)

同样(仍然是上面的“建筑模型”示例)，如果您只关心您的测量值是否在 0.1 毫米之内，那么您可以使用“十进制”类型(如果您的语言支持的话)，或者只是将您的所有测量值存储为表示十分之一毫米的整数(例如，20 米的建筑= 200，000“十分之一毫米”的内部值)

浮点数对于它们的设计目的(真实世界测量或坐标的复杂建模)来说是很棒的，但它们将怪异引入了涉及金钱的计算，或其他我们期望“美好而均匀”的事情。

** *截至 2019 年年中，一直在谈论将“BigInt”类型引入 JavaScript(提供浮点数的替代方案)，但它在许多实现中仍不受支持，也尚未成为最终的 ECMAScript 规范。谷歌的 JavaScript 实现似乎是与 Mozilla 一起的早期采用者，所以你现在应该能够在 Chrome、Firefox 和其他 V8 衍生平台的当前版本中使用它。*

## 浮点数为什么这么怪异？

如果你还不熟悉旧的 0.1+0.2！= 0.3，那么我已经快速整理了一份关于浮点数工作方式的入门书，这将有助于理解这种疯狂。

[为什么浮点数如此怪异> >](https://dev.to/alldanielscott/why-floating-point-numbers-are-so-weird-e03)

## 一个互动的玩物:去打破东西

如果你想玩玩 Javascript 中的浮点比较，看看数字变大后精度如何下降，那么我有一个 js fiddle:[https://jsfiddle.net/r0begv7a/3/](https://jsfiddle.net/r0begv7a/3/)

[https://jsfiddle.net/r0begv7a/3//embedded//dark](https://jsfiddle.net/r0begv7a/3//embedded//dark)