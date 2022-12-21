# 抱歉，但是你的猫或狗 AI 正在破坏这个世界。

> 原文：<https://dev.to/juandes/sorry-but-your-cat-or-dog-ai-is-damaging-the-world-2kkh>

[![Sorry, but your cat or dog AI is damaging the world.](img/412acee790896005ade2932c702ac6a3.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--gAXFGbiL--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wanderdata.com/conteimg/2019/09/0_OZsjzssKP2jq6GiH.jpeg)

人工智能领域正在改变世界。自从它在本世纪初复苏以来，我们已经感受到了这种技术对我们日常生活、商业世界和计算全景的冲击和影响。一开始，我们将人工智能及其孩子**机器学习**与网飞和亚马逊的推荐系统或旨在预测这是否是垃圾邮件的模型联系在一起。然而，情况发生了变化。人工智能已经进化得如此之快，以至于它正滑向我们生活的其他方面。例如，无人驾驶汽车无疑是其主要成就之一。但是，随着该领域的不断发展和进步，训练和维护这些系统所需的要求和计算能力也在不断提高。虽然这些平台和服务正在塑造我们的未来，但不幸的是，它们的高功耗要求正在对环境造成损害。

2019 年 7 月，来自艾伦人工智能研究所、卡耐基梅隆大学和华盛顿大学的罗伊·施瓦茨、杰西·道奇、诺亚·史密斯和柳文欢·埃齐奥尼发布了一篇题为“ [**绿色人工智能**](https://arxiv.org/abs/1907.10597) ”的论文在出版物中，作者倡导他们命名的 _ 绿色 A_I，一种人工智能研究，以及对环境友好的**方法**和**包容的**。特别是，作者建议使用**效率**，以及**准确性**和整体性能，作为未来人工智能实施的评估指标。此外，作者认为绿色人工智能可以将这个领域变成一个更具包容性的领域。通过这样做，不仅研究人员，而且学生和其他无法接触到最先进的机器的人都将有机会为该领域做出贡献。

<figure>[![Sorry, but your cat or dog AI is damaging the world.](img/e32217c8dc7f8b09a5277d879b357f62.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Opchf7W6--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wanderdata.com/conteimg/2019/09/image-1.png) 

<figcaption>照片由 [Jono](https://unsplash.com/@xjono?utm_source=medium&utm_medium=referral) 上 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)</figcaption>

</figure>

## 红色艾

根据论文，绿色 AI 的反义词是**红色 AI** 。也就是，“*通过使用大规模计算能力，寻求在准确性(或相关衡量标准)方面获得最先进结果的人工智能研究——本质上是“购买”更强的结果。*“关于这一点，他们说，尽管研究正在产生更好、更准确的机器学习模型，但实际的收益充其量只是对数。换句话说，当我们考虑科学投入的资源数量时，这种好处并不显著。这里，资源指的是三个不同的因素。第一个是进入模型的训练数据的**数量，它影响训练阶段的长度。然后，在单个例子上有模型的**执行成本**(要么训练系统，要么预测结果)。最后，他们提到的最后一个资源是一个模型在找到一组最优超参数之前经历的**次实验**或迭代，这是一个用于调整模型的*旋钮*。以这三个分量为基础，作者开发了一个随它们线性增长的方程。看起来是这样的:**

<figure>[![Sorry, but your cat or dog AI is damaging the world.](img/8bd7c643df74fa006493109ea672c096.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--X7OSX0LO--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wanderdata.com/conteimg/2019/09/image-2.png) 

<figcaption>方程 1。人工智能的成本。摘自绿艾论文。</figcaption>

</figure>

简而言之，这意味着开发一个机器学习模型的总成本与处理单个( **E** )样本的产品成比例，乘以训练( **D** )数据集的规模乘以( **H** )超参数试验的数量。但是为什么是这些特殊的价值观而不是其他的呢？作者对此进行了补充。第一个组成部分 **E** ，与训练或维护大型神经网络模型所需的大量资源相关的费用。例如，在某个时候，著名的围棋系统 AlphaGo 的成本约为每小时 1000 美元([来源](https://www.nature.com/articles/nature16961))，而其他用于检测假新闻的 Grover model 则需要超过 25，000 美元来训练它([来源](https://arxiv.org/abs/1905.12616))。

同样，等式的第二部分， **D** ，指的是训练集的大小，解释了当前采用更大数据集的趋势增加了系统的计算成本。关于这一点，他们评论说，训练像 FAIR 的 RoBERTa 语言模型这样的人工智能实体需要大约 25000 个 GPU 小时，因为它有 400 亿个单词的大型训练语料库([来源](https://arxiv.org/abs/1907.11692))。然后，还有最后一个元素， **H** 。这个描述了一个项目在最终版本发布之前要经历的实验次数。例如，Google 的一个项目已经在超过 12000 种不同的架构上进行了训练。作者指出，这一数值通常不被报道。

<figure>[![Sorry, but your cat or dog AI is damaging the world.](img/9912ea05b87572beddfc7e67251ad10a.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--NUhHdLqp--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wanderdata.com/conteimg/2019/09/image-3.png) 

<figcaption>图片由[西蒙尼·达尔梅里](https://unsplash.com/@simone_dalmeri?utm_source=medium&utm_medium=referral)上传 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)</figcaption>

</figure>

* * *

## 绿色艾

前一节中给出的等式和概念指出了如果希望降低训练机器学习模型所需的成本和资源，可以考虑的不同措施。尽管如此，这些价值并不能说明全部，也不能准确描述人工智能的环保程度。因此，作者介绍了各种效率的绿色度量，这些度量可以用作量化人工智能环境友好性的评估指标。

这些措施中的第一个是由模型产生的直接碳足迹。然而，现实地说，这一措施是相当不稳定的，因为它是如何依赖于电力基础设施。因此，不可能用它来比较不同型号的性能。与这个分数相似的是该型号的**用电量**。与碳足迹不同，电力使用更容易获得，因为大多数 GPU 和 CPU 都报告电力消耗比率。然而，这个使用值非常依赖于硬件。因此，再一次，用它来比较不同的模型是不实际的。另一个效率指标是**培训时间**。虽然这是一个非常直接的方法，可以快速判断人工智能的效率和绿色程度，但与上一个一样，它也非常依赖于硬件。

<figure>[![Sorry, but your cat or dog AI is damaging the world.](img/50e0307878cd606504782c5b50bbe34c.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--qxT52Oe9--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wanderdata.com/conteimg/2019/09/image-4.png) 

<figcaption>照片由[莎拉·杜维勒](https://unsplash.com/@sarahdorweiler?utm_source=medium&utm_medium=referral)上传 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)</figcaption>

</figure>

另外两个更技术性的指标是**参数的数量**和**浮点运算** (FLOP)。前一个涉及到让一个算法*学会*的点点滴滴；换句话说，它们是学来的*东西*。使这种方法成为一种合适的方法是因为参数的数量与模型的内存消耗直接相关，因此，它与所需的能量有关。尽管如此，它并不完美，这是由于模型的架构设计。因此，即使两个模型有相似的一组参数，它们的形状也会决定它们的工作量。

最后，我们有作者所谓的具体措施，浮点运算。FLOP 定义了模型执行的基本算术操作的数量，给出了它所做的总工作量及其能耗的估计值。此外，FLOP 的另一个有用的特性是它完全独立于硬件，因此，它适用于跨模型进行公平的比较。尽管翻牌并不完美。例如，一个模型可以有不同的实现，因此即使两个架构是相同的，它们所做的工作量也直接与神经网络下面的框架相关联。目前是[一些](https://github.com/Swall0w/torchstat) [库](https://github.com/Lyken17/pytorch-OpCounter)计算一个系统的 FLOP，甚至还有[论文](https://arxiv.org/abs/1611.06440)报告它们的值。然而，提交人指出，这并不是一种普遍采用的做法。

## 那么，对此我们能做些什么呢？

在整篇论文中，作者提到了谷歌等知名公司生产的大型、复杂和昂贵的模型。但是我们剩下的人怎么办呢？作为一名数据从业者和机器学习工程师，我会插话说，我们也可以有所作为。

首先，当开发一个新模型时，我们应该记住论文中的等式，并考虑它的每个组成部分。与此同时，我们应该问自己，并确定开发一个 ML 系统所需的某些部分(例如，一个大规模数据集)是否对我们想要实现的性能至关重要。更极端的是，我们需要考虑我们是否应该训练这个模型。比如，这个世界*真的*需要另一个猫或者狗的分类器吗？外面有很多这样的人。抓住一个，带着它走。同样，还有像 [*转移学习*](https://en.wikipedia.org/wiki/Transfer_learning) 这样的技术，其中可以重用为一个任务训练的模型作为另一个任务的起点；这种方法可以大大减少训练时间。然后，就是很多机型的精简版。虽然它们不如它们的重型对应物精确，但它们可以非常快速和高效，例如 [MobileNet](https://arxiv.org/abs/1801.04381) 。别误会我的意思。我并不是说我们应该忽略笨重的模型，停止做“红色人工智能”绝不！有些关键案例，如疾病诊断和自动驾驶汽车，需要最先进的模型。

<figure>[![Sorry, but your cat or dog AI is damaging the world.](img/de91b10776c42ad2eb4c256faa576658.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--yTsioZti--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wanderdata.com/conteimg/2019/09/image-5.png) 

<figcaption>照片由[埃文·丹尼斯](https://unsplash.com/@evan__bray?utm_source=medium&utm_medium=referral)上传 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)</figcaption>

</figure>

作为总结，我想说的是，我们，数据社区，应该更加意识到我们正在对环境产生的影响。为了进一步传播声音，并鼓励其他人，我们可以开始采取一些小步骤，例如用上述指标来衡量模型，并记录结果。没有必要说人工智能已经存在了。它的模式和产品会越来越好，越来越强，越来越饥渴。所以，我真的相信我们只是在抓这个红绿人工智能现象的表面。

感谢阅读。

> 菠萝图片由[菠萝供应公司](https://unsplash.com/@pineapple?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄。