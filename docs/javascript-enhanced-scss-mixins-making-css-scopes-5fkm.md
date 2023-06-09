# 🎩JavaScript 增强的 SCSS 混合！🎩用 CSS 变量制作 CSS 作用域。

> 原文：<https://dev.to/adam_cyclones/javascript-enhanced-scss-mixins-making-css-scopes-5fkm>

*因代码改进而重新发布*

这是 css 作用域的一个非常小的实现，这种方法是独一无二的，因为它通过 scss 中的 mixin 来编译作用域，这是通过一个非常小的 IFFE 函数与 JavaScript 共享的。scss 完成了大部分繁重的工作。

### 怎么用？

1 在你的代码库中包含 15 行 mixin 代码
2 包含 43 行 JavaScript 代码
3 这样做:

```
.some-stuff {
  @include scoped {
    background: red;
  };
} 
```

Enter fullscreen mode Exit fullscreen mode

因此，用 scss 和 JavaScript 填补空白看起来很不错。

### 它是如何工作的？

mixin 捕获在其中使用它的选择器，然后它为每次编译和 mixin 调用的每个实例生成一个 GUID。
在这个 GUID 中发生冲突的概率大约是 32429858953958 中的 *1，我会接受这个赔率，坦白地说你更有可能中彩票。
mixin 然后使用`@at-root :root`从它的块中转义，并将生成的 css 变量赋给`:root`，这主要是因为我们将知道唯一变量的位置，变量名包含 guid 类和 captured elements 类。*

现在输入 JavaScript，我们刚刚设置的变量现在对 JavaScript 可用了，我想要一种即插即用的感觉，所以这个函数是 IFFE。
这是一个难题，因为在撰写本文时，没有办法直接获得元素上的变量列表，所以在不知道键的情况下，我们不得不求助于从与该网页具有相同来源的任何样式表中功能性地抓取变量。在大多数情况下，这相当于 1 个样式表，事实上这相当快。现在我们有了一个可爱的 css 变量数组`['--guid-12345', '--guid-98765']`。如果`getPropertyValue`在一个循环中，我们现在可以获得要分配的 guid 类和被捕获的目标元素。剩下唯一要做的就是分配职业，就像`--guid-12345: .scope-12345,.target-selector`一样。

-明白了，由于没有重新编译 scss，因此 guid 不匹配，livereload 不能很好地工作，但如果您喜欢像过去那样刷新页面，您会发现在 prod 中一切都可以工作-

[https://codepen.io/acronamy/embed/oKzzax?height=600&default-tab=result&embed-version=2](https://codepen.io/acronamy/embed/oKzzax?height=600&default-tab=result&embed-version=2)

### 我们还能用这种技术做什么？

[![adam_cyclones](img/31516d4e57cb048579f963723ed1de7b.png)](/adam_cyclones) [## 🎩JavaScript 增强的 Scss mixins！🎩css 变量的智能可访问性

### 亚当·克罗克特 7 月 26 日 191 分钟阅读

#codepen #javascript #scss #css](/adam_cyclones/javascript-enhanced-scss-mixins-series-intelligent-accessibility-with-css-variables-374c)