# 🎉宣布闸门:React 的“加载更多”组件🌊

> 原文：<https://dev.to/geoff/announcing-floodgate-a-load-more-component-for-react-3llp>

我很高兴地宣布，经过 2 年多的开发， [**React 水闸**](https://github.com/geoffdavis92/react-floodgate) 已经发布了 1.0.0 版本！

Floodgate 是 [React.js](https://reactjs.org) 的“加载更多”组件，它将管理和更新排队数据的复杂性转化为直观的体验。它采用了一种极简的配置方法:只需要一个道具，并使用[渲染道具模式](https://reactjs.org/docs/render-props.html)，开发人员可以精确控制渲染多少可用数据，以及渲染的方式。

要快速了解 Floodgate 的基本实现，请查看下面的示例:

```
import React from "react";
import Floodgate from "react-floodgate";

export default function LoadMore() {
  const albums = ["For Emma, Forever Ago", "Bon Iver, Bon Iver", "22 a million", "i,i"];
  return (
    <Floodgate data={albums} initial={1} increment={1}>
      {({ items, loadNext, loadComplete }) => (
        <React.Fragment>
          <h1>Bon Iver Albums</h1>
          <ol>
            {items.map(album => <li>{album}</li>)}
          </ol>
          <button onClick={loadNext} disabled={loadComplete}>Add Album</button>
        </React.Fragment>
      )}
    </Floodgate>
  );
} 
```

Enter fullscreen mode Exit fullscreen mode

这就是创建一个可工作的“加载更多”组件所需要的全部内容！

要立即开始使用 Floodgate，请将其安装到您的 React 项目中:

```
# using npm
npm i react-floodgate

# using yarn
yarn add react-floodgate 
```

Enter fullscreen mode Exit fullscreen mode

### 特性

虽然 Floodgate 很简单，但它可能非常强大，这取决于它的特性如何与其他组件和模式相结合。下面简单介绍一下 Floodgate 可以做什么；查看[自述文件](https://github.com/geoffdavis92/react-floodgate/blob/master/README.md)以获得更深入的理解和技术细节。

#### 📊消费任何类型的数据

Floodgate 的`data` prop 只要求传递一个数组给它；数组可以是任何东西，包括空的！字符串、解析的 JSON 对象、React 组件，甚至 JavaScript 函数都可以传入；请记住，Floodgate 将这些项目的呈现和实现留给了开发人员。

像`data={["hello", "world"]}`、`data={[<li>Eggs</li>, <li>cereal</li>, <li>paper towels</li>]}`、`data={[]}`这样的值都是有效的，可以传递给水闸。注意，虽然 Floodgate 不关心什么类型的项目组成数组，但建议确保所有数组元素的类型是一致的。

#### 🔢确定要渲染的项目数量

告诉 Floodgate 应该用`initial`道具在初始渲染中加载多少来自`data`数组的项目。`increment`道具处理在渲染道具函数中对`loadNext()`的后续调用中加载多少项目。

#### 🎛从父组件管理道具

通过利用 React 的生命周期方法和自定义回调属性，Floodgate 的属性可以完全由父组件的状态管理，允许实例的数据异步更新或最终用户对加载多少项有更多的控制。我称之为[控制闸门](https://github.com/geoffdavis92/react-floodgate#controlled-floodgate)模式。

#### ☎️用事件驱动的回调道具处理事件

render prop 函数公开了许多由其子组件调用的 Floodgate 方法；即`loadNext`、`loadAll`、`reset`和`exportState`。当这些方法被调用时，Floodgate 调用提供给`on[MethodName]`道具的函数，如果提供的话。

#### 🔮利用上下文 API

从 v0.6.0 开始，Floodgate 利用 React 的[上下文 API](https://reactjs.org/docs/context.html) ,避免了开发人员使用`FloodgateContext`导出将渲染道具中公开的方法传递到任何需要它们的地方。

#### 🛠用打字稿建成

Floodgate 是用 Typescript 构建的，并分发了一个[类型定义文件](https://github.com/geoffdavis92/react-floodgate/blob/master/src/types.d.ts)，以增强开发人员的体验。

### 例子

为了更好地理解 Floodgate 如何工作，请查看这些 [Codesandbox.io 示例](https://codesandbox.io/search?query=&page=1&configure%5BhitsPerPage%5D=12&refinementList%5Btags%5D%5B0%5D=react-floodgate-examples)。您可以看到代码的设置方式，以及最终用户将与之交互的应用程序。

我个人网站的[写作页面](https://geoffdavis.info/writing/)上就有一个现实生活中的例子。

### 路线图

这个项目非常环保，但是除了解决问题和满足 Floodgate 用户的迫切需求之外，我还计划在不久的将来实现一些功能:

*   误差边界
*   挂钩支架(`useFloodgate`)
*   文档网站
*   改进自述文件，尤其是“贡献者”部分

### 探索 GitHub

Floodgate 可在 GitHub 的[geoffdavis 92/react-flood gate](https://github.com/geoffdavis92/react-floodgate)获得。在那里，您可以查看组件的自述文件，检查源文件，提交问题，查看打开的项目，并做所有常见的 GitHub repo 事情。

### 你怎么看？

你喜欢水闸吗？你的 app 急需这个组件吗？请通过[tweet 告诉我](https://twitter.com/intent/tweet?text=%40gdavis92%20%5Byour%20thoughts%20here%5D%20%23reactfloodgate)这件事，或者在下面留下评论！

🎉快乐发展！🎉