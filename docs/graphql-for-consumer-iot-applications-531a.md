# 面向消费者物联网应用的 GraphQL

> 原文：<https://dev.to/stewartjarod/graphql-for-consumer-iot-applications-531a>

在 Yonomi，我们必须满足客户的各种要求。我们精通基于事件的架构、发布/订阅协议，并且我们在硬件的限制内工作:内存、电池和互联网可用性。我们还必须快速迭代，并在无数设备上扩展功能。

在过去的一年中，我们了解了许多关于 GraphQL 的知识，并发现了一些可能有利于物联网行业，特别是我们的消费者物联网客户的伟大功能。

## 输入 GraphQL

### 强大的图式

GraphQL 有一个强大的内置模式和一个强类型系统，作为 API 的基本文档。该模式由 GraphQL 模式定义语言(SDL)提供，它允许我们定义设备的类型。你可以在这里了解更多关于 SDL 的信息。一旦定义好，客户机和服务器就有了一个关于参数和值的契约。

例如，灯光设备类型可能需要定义最终用户称为其灯光的设备名称、灯光是开还是关，以及灯泡当前设置的亮度。

```
type Light {
  id: ID!
  name: String!
  power: Boolean!
  brightness: Int
} 
```

Enter fullscreen mode Exit fullscreen mode

使用自定义模式指令，我们可以更进一步，为设备类型定义哪些属性是有状态的。在这种情况下，我们可以发出一个突变请求来设置设备上该属性的状态。当设备接受并执行该请求时，它可以用该属性的新状态进行响应。

自定义模式指令让客户端和服务器知道哪些属性由设备有状态地管理。用户可以请求设置这些属性的状态，但是最终，设备管理它所处的实际状态。

```
type Light {
  id: ID!
  name: String!
  power: Boolean! @state
  brightness: Int @state
} 
```

Enter fullscreen mode Exit fullscreen mode

自文档化 API 使 GraphQL 成为需要控制设备属性和状态的消费者物联网的理想选择。模式的作用相当于一个契约，在公开设备如何工作时，客户机和服务器都必须履行这个契约。在上面的灯光类型的例子中，灯光的名称必须是一个字符串，并且功率和亮度属性是有状态的，只有设备可以向云报告其状态。用户可以告诉设备设置一个状态，但最终，设备是请求是否被接受的真实来源。

### 数据关系

我们最初考虑 GraphQL 是因为我们的客户希望对他们请求的数据有更多的控制权。他们还希望通过尽可能少的移动应用程序请求来访问相关数据。GraphQL 是这方面的自然选择，因为它能够通过定义对象/模型模式的类型定义来引用相关数据。

一个很好的用例是将设备与位置相关联。在消费物联网中，我们将设备与房屋内的房间联系起来，以获得许多房间都需要的产品，如灯泡。在这种情况下，我们的移动应用程序将列出与单个房屋位置相关的房间中的设备。当用户想确保在度假时灯是关着的，他们可以检查家里所有灯的状态，并且只发出一个 API 请求。

```
Query {
  Location(id: "my_house") {
    devices {
      id
      ... on Light {
        power
        brightness
      }
    }
  }
} 
```

Enter fullscreen mode Exit fullscreen mode

这个问题通常被称为上取/下取问题。在 RESTful 架构中，通常至少要发出两次请求才能获得所有这些数据(欠取)，并且会获得比真正需要的更多的信息(过取)。对于无法处理未知属性或内存有限的设备来说，过度获取数据可能极其有害。通过过量提取数据，设备容易发生缓冲区溢出，这种溢出会超出缓冲区的边界并覆盖相邻的内存位置。让设备控制它们想要处理的数据，而不必担心接收的数据多于请求的数据，这完全缓解了这个问题。

移动开发者也天生意识到数据消耗，并努力尽可能少地使用数据。GraphQL 支持通过模式中定义的数据关系最小化数据使用，并允许客户端在查询中有选择性。

### 订阅

最重要的是，Apollo 服务器支持开箱即用的订阅。订阅是观察从 Apollo 服务器发出的事件的 GraphQL 操作。本机 Apollo 服务器支持 GraphQL 订阅，无需额外配置。

GraphQL 客户端可以在设备状态发生变化时获得近乎实时的信息。无需轮询设备的当前状态，只需订阅并在发生变化时收听。这对于希望在设备状态变化可用时立即显示的应用程序客户端来说非常有用。

## GraphQL 陷阱

尽管 GraphQL 的这些特性很好，但在为生产环境选择相对较新的软件时，还是有一些注意事项。

### 工装

Apollo 和 Prisma 已经为 GraphQL 提供了大量的工具，并且在社区的支持下，情况越来越好。然而，有一些事情仍然很难做到，每个人都在以不同的方式做事。即验收测试。

在 Yonomi，我们正在开发自己的验收测试工具，希望有一天能与社区分享。

### 200 OK

在 GraphQL 中，一切都是 200 OK。

[![](img/01ffb0c862cd2251729cdcb26ea767ac.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--AlWjwgen--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/146wewlafjhfuj2qvft2.png)

是不是做错了查询或者变异？200 好吧。

内部服务器错误？200 好吧。

不允许访问这些数据？200 好吧。

只要你点击 GraphQL 服务器，你就会收到一个 200 OK。作为云专家，我们已经学会依靠 HTTP 状态来提供有用的标准信息。GraphQL 显然忽略了 HTTP 的一些黄金标准。

### 为空性

在 GraphQL 中，默认情况下每个字段都是可空的。当与 n 个数据库、服务等交互时，有许多方法会出错。执行单个 GraphQL 请求。因此，如果其中任何一个失败，GraphQL 将简单地为该属性或父属性返回 null。虽然这是一个设计选择，但在使用 GraphQL 时值得注意。如果您的客户依赖空值来表示不仅仅是 API 没有返回的值，那么您可能会遇到问题。阿波罗公司的优秀员工在这里提供了合理的建议。保持一致和深思熟虑，尽可能使用非空值。

## graph QL 为消费者物联网带来的诸多优势

GraphQL 能够使用其模式进行自我文档化，可以将 API 交互简化为一个请求，允许对特定字段的请求，并提供简单易用的订阅来访问实时信息。尽管 GraphQL 在考虑可用工具时有一些弱点，并认为一切都很好，但它为物联网特别需要的用例提供了很多功能。在 Yonomi，我们将继续学习和探索 GraphQL 如何更好地满足客户现在和未来的需求。

*原载@Yonomi [面向消费级物联网应用的 graph QL](https://www.yonomi.co/blog/2018/12/7/graphql-for-consumer-iot-applications)*