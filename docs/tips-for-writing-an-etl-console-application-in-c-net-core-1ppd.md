# 用 C#编写 ETL 控制台应用程序的技巧。网络核心

> 原文：<https://dev.to/iamtravisw/tips-for-writing-an-etl-console-application-in-c-net-core-1ppd>

[![Tips for Writing an ETL Console Application in C# .NET Core](img/89117fcadcd9fc59fe1867826f45cd75.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--OfbYiA37--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/http://iamtravisw.com/conteimg/2019/08/pexels-photo-577585.jpeg)

首先，这不是一份指南，而是我经验的概述。

我最近在工作中被分配了编写 ETL 过程的任务，这是我非常喜欢做的事情。目标是从一个 [Mark Logic](https://www.marklogic.com/) 数据库中取出 2000 万或更多的文档，并将它们转移到一个 [Postgres](https://www.postgresql.org/) 数据库中。很简单。但是需要注意的是，Mark Logic 中的所有数据都是 XML 格式的，而 Postgres 中所需的数据格式是 Json。不过，多亏了 Json.NET[这并不是一个太难的问题，只需要几行代码。我将在下面介绍我所经历的过程。](https://www.newtonsoft.com/json)

## 什么是 ETL？

ETL 过程是从一个数据库向另一个数据库提取、转换和加载数据的过程。根据您的数据集，这可能是一项简单的任务，也可能是一个非常耗时的过程。在我的例子中，我很幸运，我们的首席工程师已经在我们的主要应用程序中设置了类，我可以重用这些类来轻松地从 Mark Logic 中读取 XML 数据。这是一个很好的例子，说明了为什么编写干净的、可重用的代码很重要。

## 策划申请

在这个过程的计划阶段，我知道我将会处理大量的数据。加载 500 万条记录可能需要 N 个小时，如果应用程序在到达终点之前崩溃了怎么办？我们不想再次加载相同的 500 万条记录。我需要写一个方法来跟踪已经插入的记录。除此之外，我需要一个可配置的文件，让我们在运行后操作它的内容。该文件需要能够在任何环境中执行，这本身就提出了挑战。这是我最关心的两个问题，在这一点上，数据的实际提取、转换和加载是最重要的。

### 配置文件

所以你要做的第一件事就是创建一个配置文件。在创建配置时有很多选择，我选择了使用 json 文件。下面是 Gary Woodfine 关于在中设置配置文件的更详细的[指南](https://garywoodfine.com/configuration-api-net-core-console-application/?fbclid=IwAR2F9Y3oEFGCQN3ZYKKOcbY5Py3N0TFXvxuTmRlroah3h2HlNTihuRWj7M0)。控制台应用程序的 Net 核心。通过为您的配置使用 Json 文件，您可以使用名称-值对，这是使用您的设置的一种非常简单和直接的方式。

我的 appsettings.json 文件旨在保存三种主要信息。首先是我想一次移动的文件数量的批量大小。在下面的下一节中将会有更多的内容。其次，我将连接到 MarkLogic 数据库并从中获取数据的连接细节。最后，我将插入数据的 Postgres 数据库连接信息。根据控制台应用程序的复杂程度，您的结果可能会有所不同。这是引入附加设置的好地方。

### 提取

提取过程应该相对简单。我们的目标是获取数据，然后将其分解成您可以更好地控制的数据块。例如，如果您正在为 100，000，000 行数据编写一个 ETL 过程，您可能不会一次完成所有工作。如果过程中途失败了怎么办？运行这个过程需要多少内存？你的程序很可能会因为这样或那样的原因而崩溃。因此，相反，你想这样做的增量。

在我的情况下，我从原始数据库中获取所有的 id(在编写本文时大约有 2000 万个),然后将这些 id 存储在 10，000 个块中。然后，对于一次 10，000 个文档，我使用它们的 id 并从 MarkLogic 中获取实际的文档。这是我的 appsettings.json 文件中的一个设置，可以根据性能或需求进行调整。

### 变换

通过 id 获取 10，000 个文档后，我需要将数据从 XML 转换成 Json。这就是 Json.NET 图书馆发挥作用的地方。只需几行简单的代码，Json.NET 就能把大部分东西翻译成 Json，然而根据你的需要，你的结果可能会很不一样。注意这里的转换部分很短，因为 Json.NET 库为我处理了大部分繁重的工作。

### 装载

从使用配置文件中的信息连接到数据库，然后传输数据的意义上来说，加载应该非常类似于提取。在我的例子中，我连接到 Postgres 并为每个文档插入一行。这一行包含惟一的文件 ID、Jsonb 文档、创建和更新日期以及最后一个跨国 ID，以帮助防止在基本加载完成后添加重复数据。

最初，我使用 insert 语句将数据导入 Postgres，但是经过一些研究后，我发现使用 INSERT 语句并不是最有效的处理方式。Postgres 支持 [COPY](https://www.postgresql.org/docs/9.2/sql-copy.html) ，可以二进制格式在文件或文本与表格之间传输数据。我发现使用复制而不是插入在加载过程中为我节省了大约 50%的时间，正如你可以想象的那样，随着数据集的增长，这更有帮助。我强烈建议您在编写 ETL 过程时研究一下复制是否是正确的选择。

### 测试

测试将是你过程中的重要一步。您不希望只是尝试在生产中运行您的应用程序，这不是发现您在转换逻辑或加载逻辑中犯了错误的好地方。我强烈建议用 Postgres 建立一个 docker 容器，并首先在本地连接。您可以在 appsettings.json 文件中将它设置为一个选项，这样您就可以轻松地在不同环境之间切换。为 Postgres 设置 Docker 容器，这里有一篇有用的[文章](https://hackernoon.com/dont-install-postgres-docker-pull-postgres-bee20e200198)。一旦您测试了您的应用程序，您就可以传输真实的数据了。

## 任务成功

在我的公司，这个控制台应用程序被频繁使用，它每天都在运行，不断地将数据从一个数据库转移到另一个数据库。一如既往，有优化的空间，但是对于我们的需求，这个简单的控制台应用程序非常适合。由于在控制台应用程序中使用了异步任务，我可以为我想要的文档类型创建一个订单，然后运行应用程序，让它为我完成所有繁重的工作。该应用程序做了它被创建来做的事情，并得到我们需要的结果，所以我可以称之为成功。

### TL；DR 提示总结

1.  如果适用，使用现有的代码和库。
2.  异步编程可能是您想要在这里使用的东西。
3.  创建一个配置文件，这将有助于减少重复代码。
4.  如果你正在处理大量的数据，把它分成几块。
5.  由于库可以将数据从一种格式转换成另一种格式(比如从 XML 转换成 Json)，转换和加载可以紧密地结合在一起。这不是一件坏事。
6.  尝试优化代码时，探索所有选项。例如，当使用 Postgres 处理大块数据时，COPY 比 INSERT 快得多。
7.  创建一个恢复过程。您不希望移动 19，000，000/ 20，000，000 行数据，然后让您的应用程序崩溃，最后不得不重新处理前 19，000，000 条记录。
8.  首先在本地测试，Docker 非常适合这种事情。