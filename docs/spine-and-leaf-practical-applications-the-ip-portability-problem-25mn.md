# 书脊和树叶的实际应用，IP 的可移植性问题

> 原文：<https://dev.to/ngschmidt/spine-and-leaf-practical-applications-the-ip-portability-problem-25mn>

## 到目前为止，这一切都很棒，但它缺少了一些东西...

网络是无用的，除非你用它做点什么。在大多数情况下，设备(在这种情况下，我们将使用服务器)需要通过冗余链路连接到公共的第 2 层网段。

## 这是为什么？

嗯，大多数服务器都不能**动态路由**。相反，服务器(就转发而言，它是一个完全有能力的路由器)只是有一个静态路由(**默认网关**)，用于所有第 3 层转发。对于 Clos 结构来说，这并不是一个真正的障碍——有几种方法可以解决这个问题——其中几种方法可以很好地混合在一起:

## VMWare 方式

这可能是最有可能实现的。这不是真正的 Clos 结构，由于一些缺陷(ESXi 还没有 BGP)可能会在某个时候得到解决，但它已经足够接近实现我们的目标。让我们回顾一下

*   使变更无摩擦且低风险，这样网络变更可以按需进行(变更问题)
*   确保网络利用所有链路，并保持一致的转发(环路问题)

第 3 层叶主干(Clos 实施)的主要价值主张是利用一致的 3 阶段(叶、主干、叶)转发拓扑，其中所有链路具有完全相同的延迟和链路速度。这与其他一些特性(ECMP 支持是最大的一个)一起，允许 N 倍扩展的叶到叶通信——您可以拥有 1，2，..网络中的 64 根刺。

思科确实将这一点推向了极限，发表了一篇关于参考实施的[论文](https://www.cisco.com/c/dam/en/us/td/docs/solutions/Enterprise/Data_Center/MSDC/1-0/MSDC_AAG_1.pdf)，其中利用 16+spine 实际上比使用支持 QSFP+的设备节省了资金。由于 QSFP28 的出现和更实惠，这个结论有些过时，但收获应该是一样的- BGP/IS-IS 是为了方便**不规则拓扑**中的**数万**网络节点而构建的。拥有数百台交换机的数据中心网络并不能与之相提并论，但我们可以利用这一优势。

VMWare 现在也支持这种拓扑，因为他们开始解决 NSX 的路由问题。当前发布的参考架构( [VMWare Validated Design 5.0](https://communities.vmware.com/docs/DOC-39632) 在本文发布时)采用了 4.0 版的新折衷方案——将 ToR 交换机连接成对，更像传统的交换机部署，VLANs 对着叶片以提供服务器可达性。

**这里有一个问题**——当在 ToR 对之间移动时，如何让虚拟机保持它们的 IP 地址？

这就是 NSX 的用武之地。NSX-V/T 都提供了**覆盖网络**，其中动态固定隧道适配器(如 ubersteroids 上的 GRE)在封装方法(VXLAN/GENEVE)内管理虚拟网段的成员资格，提供完全虚拟化的第 2 层网段，可在任何地方移植。这确保了唯一不可移植的是服务器，这对于现在来说已经足够好了。

VMWare 的方法并不“纯粹”(无论这意味着什么)，但当我们重新审视我们的目标(**提供一个超稳定、易于改变的数据中心网络**)时，它确实满足了我们的需求，并提供了一个分界点，就数据中心结构而言，这些改变要简单得多。如果主机上的 NSX 发生故障，您可能会丢失部分虚拟网段，或者在最坏的情况下丢失几个虚拟机。织物故障的灾难性要大得多。

优点:

*   由于分配工作，变更风险很低
*   高度灵活
*   NSX-T 可以在非 ESXi 平台上运行

缺点:

你需要模糊“网络”和“系统”之间的界限。我在许多生产环境中看到了一种模式，即组织的网络团队将管理 vSphere 分布式/标准交换机，以确保交换机和主机充分集成。如果这种模式在组织上不可行，你将很难与 NSX 相处。即使不是，你的网络/系统团队**也必须**交叉培训。

## 思科/大交换机方式

另一种选择是将覆盖网络的责任完全卸载到数据中心结构上，维护“纯”Clos 拓扑，并使用相同的覆盖技术在软件中处理 ToR 绑定。我把这保持在一个高水平，因为，老实说，我还没有在任何大到足以受益的地方工作过。

优点:

*   几乎可以在任何东西上工作
*   通常附带自动化的生命周期管理和配置平台
*   您可以将网络和系统团队分开

缺点:

*   如果这不是您的供应商预期的用例，您将无法获得所需的灵活性
*   供应商锁定基本上是有保证的
*   不能在通用硬件上运行
*   $$$

## 那个疯狂的科学家道

...只需在每台虚拟机、docker 主机或虚拟化主机上安装一个路由包。运行 DHCP 进行初始地址发布，然后运行带有动态范围的 OSPF 或 BGP，然后为您提供的服务通告一个环回地址。

其实也没那么难。如果你有一个 linux 操作系统，你只需要安装一个软件包来运行一个路由服务。如果你是一个系统的家伙，这比建立一个 LEMP 堆栈更容易！下面是一些开源、公开可用的软件的例子，它们将执行这项任务:

*   [Quagga](https://www.nongnu.org/quagga/) (所有 RPs，包括 IS-IS)
*   [BIRD](https://bird.network.cz/) (OSPF/BGP/RIP，包括 MPLS)
*   [OpenBGPD](http://www.openbgpd.org/) (BGP)我的新宠是[散场路由](https://frrouting.org/)。它在 VMWare 新版 NSX (NSX-T)积云网络和大量其他东西的掩护下。它是这个列表中最完整的功能，执行通常要支付更多费用的任务(思科仍然将 BGP 作为附加许可证)。

这可以提供的一个巧妙的东西是**任播网络服务**的概念。对于像 DHCP、DNS 等无状态服务，可以利用这些守护进程中的一个来通告一个公共地址。无需搜索正确的服务器或汇集 DNS 服务的候选列表，客户只需询问最近的 DNS 服务器即可——这是许多亿亿级 DNS 实施的工作方式，例如:

1.1.1.1(云闪)

8.8.8.8(谷歌)

9.9.9.9(第九季度)

这种方法的缺点是您没有任何正式的支持——这是一个相当大的缺点。好消息是，有一些商业上可行的主机路由产品，如 Cumulus 的主机包(白皮书[此处](https://cumulusnetworks.com/learn/web-scale-networking-resources/white-papers/routing-host/))最终，像这样的产品将作为插件在常见的虚拟机管理程序上运行。

## 结论

### 为什么以及在哪里应该尝试这个？

让我们保持简单——应用 Clos 数据中心结构将需要某种程度的解决方案设计——它不能简单地在数据中心内使用叉车，但目前有许多产品可以解决近期的问题。这些实现很少是完美的，所以设计时要反复改进，在数据中心外围为新版本留出额外的端口，等等。

### 我应该使用什么路由协议，技术？

利用你所知道的。我在这个模块中提供的实验室示例是 2002 年生产的。如果是第三层，比较熟悉，就用。我们只有一个硬性要求——快速第 3 层交换。

对于路由协议，使用你所知道的-如果一个协议是陌生的，你将很难支持它。运行 OSPF 没有任何问题(甚至 RIP！)为了这些目的。我个人最喜欢的是运行两个——要么是 IS-IS，要么是 OSPF 结合 BGP——但这是由我对未来的一些要求驱动的:

*   NSX-T 仅支持 BGP
*   BGP 是高度可扩展部署的必由之路
*   任何运营商网络工程师使用它都会感到如鱼得水

关于 Clos 网络的这一部分到此结束。后来，我甚至可能会应用它！