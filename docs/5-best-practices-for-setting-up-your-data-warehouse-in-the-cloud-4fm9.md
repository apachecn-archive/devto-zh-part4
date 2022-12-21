# 在云中建立数据仓库的 5 个最佳实践

> 原文：<https://dev.to/eddiesegal/5-best-practices-for-setting-up-your-data-warehouse-in-the-cloud-4fm9>

在内部维护数据仓库可能是一项昂贵而复杂的工作。硬件和软件的初始资本成本可能高达数百万美元，花费在设计架构、设计计划和实现数据仓库解决方案上的时间和精力也是如此。

虽然大型企业能够承担费用，但中小型组织通常无法在内部创建和维护数据仓库。幸运的是，云计算的出现使得在云中托管数据仓库成为可能，消除了与本地模型相关的成本。

## 云中的数据仓储

考虑到成本和时间的影响，一些组织正在将其数据仓库迁移到基于云的解决方案。这有助于降低成本和提高性能水平。也许基于云的解决方案最大的好处是它提供的灵活性和可扩展性，以及在存储或处理能力方面快速数据增长的可能性。

基于云的解决方案包含大量工具、技术和托管服务，有助于降低与维护本地数据仓库相关的开销成本和复杂性。基于云的数据仓库节省了您的时间，降低了您的成本，因此您可以将资源集中在业务的其他方面。
在建立基于云的数据仓库之前，请确保创建一个符合行业标准的云迁移策略。下面，您将看到关于在云中建立数据仓库的五个基本最佳实践的综述。

## 1。了解迁移挑战

当您迁移到基于云的企业数据仓库(EDW)平台时，您需要考虑关键的迁移和设计问题。为了确保停机时间保持在最低限度，为您要迁移到云的任何 EDW 功能设计一个无缝集成策略。这样，您可以将数据迁移到新环境，而不会中断任何剩余的本地流程。

下面，您将看到对每个云迁移策略应该涵盖的主要方面的回顾。

### 将数据迁移到云端

将大量数据从一个存储库迁移到另一个存储库通常非常耗时。尝试迁移 EDW 时，在过程早期准确定义数据集和卷至关重要。这种技术可以实现数据移动的最佳连接。准确的数据迁移时间表也很重要。

### 数据集成和访问

要让数据从本地存储库流向云，需要一个 [ETL 流程](https://www.guru99.com/etl-extract-load-process.html)。需要验证不同的 ETL 工具，以确保正确的云操作和支持，以及与云原生 EDW 技术集成所需的功能。确保在您的迁移策略中涵盖这些方面。

### 数据中转费用

如果数据移动效率低下，迁移数据的成本会迅速增加。由于云服务提供商提供经济高效的数据存储价格，您不想因为不得不再次迁移而浪费这个机会。在传输之前最大限度地压缩数据也是一个好主意。

### 开发者体验

基于云的 edw 具有一定的灵活性和功能，这通常是内部解决方案所不具备的。对于需要跟上新功能步伐并不断学习云服务提供商添加的新功能的开发人员来说，这些通常是具有挑战性的。

寻找能够提供用户友好的文档和培训机会的服务提供商。包含采用云 EDW 的示例、培训和相关资源的强大知识库有助于简化未来的任务。

诚然，任何 it 服务的迁移都可能是一个令人望而生畏的前景，但 edw 也带来了额外的风险，因为它可能会中断业务连续性。迁移到云 EDW 时，需要在迁移前分析和规划一些关键因素:

1.  **在本地 EDW 实施中使用专有功能** -在迁移之前，需要充分检查专有功能的所有使用案例，以确定在云中提供类似功能的最佳方式。

2.  **数据访问和开发人员体验** -开发人员需要选择将其内部环境转移到基于云的 EDW。这有助于抑制在云中部署新工作负载或进行平台间集成时的生产力损失。

3.  **查询成本** -某些云服务根据被查询的数据量收费。虽然这被证明对许多工作负载是有利的，但需要提前与开发人员沟通，以确保任务得到有效执行。在迁移之前，应充分了解添加新报告功能的成本。

## 2。使用正确的迁移策略

在迁移过程中，数据将从现有解决方案迁移到云中。不同的迁移策略支持不同的数据传输，包括只将数据管道的一部分迁移到云。

您可以将 CSV 或 Excel 格式的非结构化数据直接注入像亚马逊 S3 这样的对象存储服务，而不是在本地维护文件服务器。然后，这些数据通过云中的数据管道进行转换和处理，以更低的成本和管理开销提高性能。

此外，根据数据量和运营需求，迁移策略可能需要将数据移动到云仓库。在数据量较低且不需要连续操作的情况下，您可以设置一个单步迁移系统，将相关数据提取并导入到基于云的数据仓库中。

也就是说，如果您正在处理具有持续操作需求的大量数据，这种方法就不切实际了——因为迁移的数据量非常大，而且每个迁移时间段都会添加新数据。

要解决这个问题，你可以把这个过程分成两步。在第一阶段，关键是在非高峰时间提取数据，以便将影响降至最低。接下来，通过与提取的数据卷的格式相匹配的方法将数据迁移到云中。

### 3。持续创建快照和备份

大多数云服务提供商在您的数据仓库集群中复制您的所有数据，并且您的所有数据通常都备份到一个对象存储服务中。尽管如此，企业应该认真考虑在分布式位置创建夜间备份，以防止由于负面事件而丢失数据。负面事件可以是任何事情，从网络攻击到设备故障和自然灾害。

默认情况下，Redshift 会尝试维护至少三份数据副本——原始数据、计算节点上的副本和亚马逊 S3 的备份。如果您想进一步自定义副本的数量或自动化快照创建过程，您可以使用第三方供应商提供的 AWS 灾难恢复服务。默认保留期为 1，您可以将其配置为 35 天。

### 4。使用 ELT Over ETL 进行批处理

在云中设置数据仓库解决方案时，有机会优化数据管道。将非结构化或半结构化数据移动到数据仓库时，通常使用[提取、转换和加载(ETL)](https://www.webopedia.com/TERM/E/ETL.html) 。ETL 主要是一个连续的过程，提取的数据被清洗、转换、丰富并装载到仓库中。

当谈到像 Amazon Redshift 这样的云中的数据仓库解决方案时，您可以将提取的数据直接迁移到仓库——在仓库中执行数据转换的效率足够高。这将修改数据管道的顺序，以提取加载和转换(ELT)。

### 5。将托管服务用于仓储工作流

组织可以选择使用云作为基础架构即服务(IaaS)。他们可以安装用于数据仓库的软件，并通过云服务器实施 ETL 流程。然而，当迁移到云服务时，这提供了有限的价值。

云服务的主要优势是完全管理或半管理的数据仓库服务以及 ETL / ELT 解决方案。这有助于降低组织的总体拥有成本。例如，亚马逊红移为客户提供了[Pb 级的数据仓库解决方案](https://thenextweb.com/media/2012/11/28/amazon-web-services-launches-petabyte-scale-redshift-data-warehouse-with-nasa-netflix-and-flipboard/)。这允许数据存储和分析查询。

## 结论

将您的数据仓库迁移到云中不一定是一项昂贵的工作。通过详细的规划和正确的策略，您可以设计出适合项目规模和您的组织的迁移过程。一旦您在云中设置好一切，您将能够消除与维护本地数据仓库相关的成本和复杂性。