# 现成解决方案的诱惑

> 原文：<https://dev.to/kohloth/the-seduction-of-off-the-shelf-solutions-28g0>

*最初发布于 2019 年 2 月 10 日[socket-two.com](http://www.socket-two.com/main/resource/off-the-shelf-software)。*

# 【乐美食家】

想象一下下面的场景。

现在是周一早上 09:53。你坐在办公桌前工作。阳光透过开放式办公空间的百叶窗，落在质朴的木地板上。办公室狗抓挠它的头的一侧，制造出一团粒子，不知何故既威严又有点令人不快。你被一种安静的氛围所包围，一种对即将到来的一周的热切热情和对刚刚过去的另一个周末的哀叹。

你已经整理了自上周五离开后收件箱里堆积的所有电子邮件，并回忆起你与一位时髦的新客户进行的最后讨论。“Le Gastronomer”-一家高科技的城市酒吧和餐厅，有新颖的菜单和与之相配的时髦建筑，位于中心位置。

他们委托你的组织为他们制作一个网络应用，作为他们自己的品牌食品博客，一个自动杂货库存管理和订购系统，当然，还有一个允许潜在顾客查看菜单的公共场所。您还记得，客户也非常希望能够高度控制促销横幅，您已经同意，他们可以配置在网站的几个区域出现。横幅行为还取决于许多变量，包括一年中的时间、一天中的时间、环境温度、储藏室库存水平、菜肴受欢迎程度以及网站管理员的人工输入。

10:00，您与开发人员、制作人和项目经理开会，讨论如何构建应用程序。客户已经同意为软件支付合理的费用，但是他们没有足够的钱，所以他们的资金不允许你和你的团队悠闲地工作。没有犯错误的余地。但是你已经喝过咖啡了，这是一个新的、未被破坏的项目和关系(目前)。

时钟滴答作响，会议开始了。您与团队一起浏览需求列表。开发人员坐在那里，弓着背，认真地听着，眼睛明亮，有一种静止的、冷漠的紧张感，让你想起一群等待一碗食物落地的狗。你一说出你的想法，就有大量的实现建议，但是经过一点讨论，站点博客的关键需求已经成为焦点，使用现成的解决方案 Wordpress 的想法已经扎根。

"不再需要开发一个文章管理系统将会节省很多时间."

"这听起来是个好主意，我们不想多此一举。"

Wordpress 有一个丰富的扩展生态系统——有几个横幅管理器可供选择，可以满足这个需求。

"这是一个非常受欢迎的系统——代码库是健壮的，并且经过了良好的测试."

"客户喜欢它-界面简单."

这些争论看起来相当有说服力，因此当团队集体吐气时，这种猜测逐渐得到满足，其中一名开发人员俯身帮助一名实习生下载并安装 Wordpress 源代码到他的开发环境中——向他演示。每个人似乎都很满足。

但这时一个斜斜的身影，一直坐在角落里蠕动，终于管道了。

“我不确定这是否是个好主意”。

# 关于“现成”软件的概念。

每当您参加如上所述的应用程序设计会议时，您可以做出的最重要的决定是关于包含第三方代码的决定。特别是，正确识别软件的哪些部分应该被委托(如果有的话),以及当出现多个选项时，您将使用哪个供应商的包，这是非常重要的。

想要通过引入第三方软件作为现成的拼图来减少开发时间是一件非常合理的事情，尤其是在时间紧迫的商业环境中。从远处看，这种逻辑是合理的，如果处理得当，寻找零件的做法会非常有效。但在现实中，团队经常如此热衷于节省时间，以至于他们没有意识到他们正在破坏项目，并且通过将一个方钉塞在一个圆孔中，使事情变得更加困难和更加耗时。这可能是灾难性的，并可能导致各种各样的疾病，我将在下面讨论。

# 潘多拉的第三方包

反对在应用程序中使用第三方代码的理由。

## 定制

*   **可能无法定制**当项目中使用第三方代码时，通常认为将它与项目开发人员编写的应用程序代码分开是一种良好的做法。这通常意味着将它存储在一个单独的目录中，从版本控制中忽略它，有时，允许它自我更新。因此，对于项目开发人员来说，更改第三方代码通常被认为是不好的做法。如果第三方代码被更新，项目开发人员可能传递的任何更改都将丢失。考虑到这一点，第三方代码被认为是不可变的。它不能被调整或定制——至少不能通过在文本编辑器中打开第三方代码并直接进行修改来调整或定制——这是一种提供最大控制的方法。一个编写良好的第三方库将包含允许以其他方式定制它的机制，例如接受一个配置对象，或者允许基类被扩展。然而，至关重要的是，经常会出现这样的情况，你需要实现的行为改变不能被很好地实现。在这种情况下，你有两个丑陋的选择。1 -告诉客户你不能提供他们要求的功能，或者 2 -写一些可怕的代码...这通常会以某种方式对应用程序产生负面影响。
*   **定制需要更长的时间**上一点提到的“不可直接修改的代码”应该说明供应商代码定制有时会更耗时。
*   定制它可能会有行为上的副作用第一点也应该对此提供一些解释。一个粗糙的定制会有一些负面影响，其中之一就是引入了 bug。例如，假设客户认为视频缩略图中出现的巨大的“播放”图标看起来很讨厌，他们希望将其删除。这个库没有隐藏按钮的配置参数，而且 HTML 是不可变的，所以开发人员发布了一个 hacky 补丁，用 CSS 隐藏按钮。客户端很高兴，因为播放按钮消失了，他们仍然可以通过单击常规缩略图区域来播放视频。然而，由于播放按钮包含 aria-label 属性，并且现在被隐藏，使用屏幕阅读器访问网站的用户不再能够感知视频。这是另一个例子。考虑一个在线商店的情况，客户希望“商品已添加到您的购物车”消息在 5 秒钟后消失，而不是无限期地徘徊。同样，如果没有支持这种行为的内向配置选项，开发人员可能会发布一个 hacky 修复程序，强制整个页面在 5 秒钟后重新加载。购物车消息现在消失了，但是站点访问者可能已经注意到他们的屏幕出现了短暂的空白，并且他们的视窗可能已经滚回顶部。这类事情本身可能是相当不合理的，但是随着这些漏洞的增加，不久整个用户体验就会变成数字垃圾。如果开发者拥有完全的控制权，修复将会像尼亚加拉瀑布的水一样自然流畅。
*   它可能会损害代码库的完整性有时，黑客式的修复不会直接导致错误，但它们确实为其他错误的滋生创造了肥沃的土壤。通常，从已建立的不合逻辑的代码库中产生的 bug 更为险恶，因为它们往往会被忽视。他们可能会悄悄地阻止十分之一的顾客完成结账过程，或者可能会不断暴露敏感信息。这种现象的机制相当简单:不合理的修复是不合逻辑的修复。当一个开发人员在一个不合逻辑的代码库上工作时，他们很容易不经意地写出逻辑代码，与先前建立的不合逻辑的代码(孤立地看，这是无伤大雅的)交互不良。这会导致新的、不合逻辑的错误，而公司可能不知道。

## 适合目的...？

*   **它是为通用目的而设计的**任何声称能帮你完成大量工作的现成软件都不是为你的项目而设计的完美组合。几乎从定义上来说，它被设计成对许多项目来说都是相对令人满意的。因此，在功能和定制方面，你*必然*会发现该解决方案在两个方面都不理想。1 -不够灵活，无法满足您的需求(见上文)。或者，2 -它包含了太多的铃声、哨声和定制，这不是最佳的。这就引出了下面的两点。
*   **计算效率低下**臃肿的软件可能会反应迟钝，运行缓慢。这种计算效率低下可能表现为页面加载持续时间过长、界面元素无响应、滚动条粘滞、崩溃和主机托管成本增加。
*   **复杂性——开发效率低下**为一个苗条的、专门构建的应用程序创建一个定制模块，可能比为一个复杂的系统创建一个定制模块要简单得多，因为复杂的系统需要很多排场和仪式。在复杂系统的情况下，模块可能需要用绝对的术语来描述其功能，并在更广泛的(通常是冗余的)上下文中声明其参数。例如，为 Joomla CMS 创建一个“Hello world”模块，开发者需要写大约 [100 行代码，分在 5 个文件](https://docs.joomla.org/J3.x:Creating_a_simple_module/Developing_a_Basic_Module)中。然而，要在一个瘦系统中创建相同的模块，这个模块可以作为一个文件中的一行来编写。
*   **可移植性稍差**许多包含第三方代码的项目使用某种自动化工具来管理供应商代码的获取和更新。Nuget、npm 和 Composer 就是这样的包管理器的例子。Npm 是一种特定类型的包管理器，称为依赖性管理器。这意味着，当您安装一个第三方软件时，npm 将自动安装您指定的软件所依赖的所有第三方软件。它还安装它们的依赖项，以及它们的依赖项，等等。这有时会导致非常深的嵌套目录结构和大量的文件集合。当需要将您的应用程序移动到不同的环境时，移动这些文件可能需要相当长的时间。通过从传输过程中省略供应商文件，然后在传输完成后使用 npm 命令行实用程序安装软件包，可以加快传输过程，但是在这种情况下，您现在需要确保命令行实用程序安装在目标设备上。对于大多数开发人员来说，这并不是一个大问题，但是对于需要方便移植的非常小的项目来说，这足以令人烦恼，以至于需要保证完全避免包管理的依赖性。

## 所有权

值得警惕的是，所有存在于这个“所有权”否定组中的否定事物(除了第一个)都是使用供应商软件的项目所独有的。如果不使用供应商软件，所有这些障碍都可以完全避免。

*   **可能难以理解**在选择使用特定的现成解决方案之前，确保开发人员可以轻松地学习如何使用它。全面的文档应该可用。如果没有，至少代码库应该是逻辑的和可读的。如果软件是晦涩的，并且在软件开发世界中通常是不为人知的，这一点尤其重要。使用一个第三方的解决方案是一个可靠的方法来启动一个从一开始就注定要失败的项目，这个第三方的解决方案没有被很好地记录，很少被使用，并且通常难以理解。值得记住的是，在某些情况下，构建好的软件并编写好文档肯定比试图借用软件并破译糟糕的文档要快。
*   **不受你控制**如果你不能编辑代码，你就没有对代码的完全控制权。您可能会受到编写解决方案的第三方开发人员的摆布。
*   **它的使用受到限制**从法律角度来看，你开发的软件归你或你的公司所有，因此，管理其使用的条款非常简单明了。在选择使用第三方软件时，您可能同意各种限制。例如，它可以用于哪种类型的应用程序，是否允许您从中获利，以及有多少开发人员可以在获得著作权时利用它。有时，能够消除这种隐忧就足以保证摆脱对第三方的依赖。令人沮丧的是，软件使用条款中的一些条款相当模糊不清。流行的 React 框架的使用条款让一些潜在的皈依者犹豫不决，因为这意味着该库不能用于支持任何与脸书竞争的软件。2002 年，道格拉斯·克洛克福特发布了他的“JSMin”软件，附带一个许可证，声明它只能“用于正义，而不是邪恶”，这引起了一些组织的足够焦虑，要求删除该条款。可笑的是，许可证被修改为允许“IBM、其客户、合作伙伴和爪牙”允许“使用 JSLint 作恶”。
*   **可能会很贵**第三方开发者可能会向你收取使用他们软件的费用。一般来说，这可以降低采用率，减少社区的规模，以及在出现问题时人们可以帮助你的可能性。此外，作为一种商业资产，商业代码可能会被混淆，或者以其他方式变得不可读，以保护收入。这也可能使修改或检查变得更加困难。当然，昂贵的软件会让你赔钱。
*   卖主有时会关闭商店，丢弃他们的包裹。他们可能决定软件已经被一个更好的发明淘汰了，他们可能因为其他的承诺而不再有时间，或者他们可能只是失去了兴趣。这可能是个问题。比依赖你无法控制的软件更糟糕的事情是依赖没人控制的软件。相反，有时软件发布活动非常活跃，软件开发人员以很短的时间间隔发布一个又一个版本的软件包。这通常是一件好事，因为你可以相信软件是最新的，并且相对来说可以很好地防范突发错误和安全缺陷。然而，这一点的另一面是，它偶尔会导致上面讨论的不可理解性。当你能找到的唯一能帮助你解决问题的指南是用不同版本的软件编写的时候，经历一个供应商软件的错误是非常令人沮丧的。我经常依赖指南和文档，而*甚至没有说明它们是针对*的软件版本。对于社区创建的指南，比如博客帖子，这是一个更常见的问题，但是在很多情况下，我发现文档也是如此。同样，这种挫败感与控制或缺乏控制的概念非常吻合。

## 互通性

如上所述，一些第三方软件可能很难定制，这通常可以追溯到第三方开发人员(可以理解)不了解或不熟悉他们解决方案的所有可能的使用案例和改编。出于类似的原因，很大一部分第三方软件也很难与其他系统集成。当使用由大量第三方解决方案组成的软件时，通常会出现过河问题:狐狸和谷物可以同时在船上或岸上，但狐狸和鸡不能。鸡可以在谷物附近，但只有当农民也在场时。随着越来越多的第三方元素被引入到等式中，您就越有可能发现您的一些包在它们之间不能很好地工作。因此，您必须想出的让它们共存的解决方案变得越来越复杂、难以维护和荒谬。仅仅这个事实就可以导致项目开发缓慢，并对交付时间表产生比内部解决方案大几个数量级的负面影响。

# 值得吗？

如果当你开始阅读这篇文章时，你对使用第三方软件来节省时间的前景完全乐观，我希望现在你所有的狂热都已经平息了。然而，我希望我没有完全扼杀这种热情。

已经观察到许多项目由于欠考虑的代码借用而降级，并且较少数量的项目真正受益于明智的借用，我的意图不是助长对预制解决方案的不灵活的恐惧。我的意图是给那些认为现成的解决方案是一种简单、保守、无风险的方法来更高效地开发定制软件的开发者或生产者灌输一种谨慎的意识。关于使用第三方软件的决策非常重要，需要谨慎对待。他们需要对竞争包有一个清晰的理解，并仔细检查这些包与项目需求的交叉程度，以及它们之间的交叉程度。但是最重要的是，记住——当开发定制的软件时，有时候从头开始真的更有效率！

# 重访“美食家”建筑

已经确定了现成解决方案的合法性是项目性质的问题，让我们再来看看假设的“Le Gastronomer”项目，并讨论 Wordpress 方案的优点和缺陷。

在我们开始之前，我应该承认“美食者”这个场景有点不现实。在一个大约 4 - 8 人的项目团队中，就像我们在这里一样，如果除了一个人之外所有的开发人员都理解了“Le Gastronomer”项目的需求，并且提倡 Wordpress 的实现，那么这个团队就不是一个非常有眼光的团队。由于它的低学习曲线，Wordpress 在构建网站时确实容易被过度使用和滥用，但专门的开发团队通常不会为此受到责备。

不管怎样，让我们从检查开发团队的评论开始。每个说法都有一定的道理，但每个说法也有缺陷。没有一个陈述完全符合项目需求的真实性质，或者手头的场景。

“不再需要开发一个文章管理系统将会节省很多时间。”

确实，Wordpress 会给你一个健壮的、功能丰富的基于博客文章的 CMS 功能。使用 Wordpress 将避免为管理后端、用户认证、文章管理用户界面以及数据库创建、读取、更新和删除操作编写代码的需要。然而，从更宏观的角度来看，在这个项目的背景下，这并不能节省很多时间。这种特性的开发是许多开发人员的饭碗，可以在不到一周的时间内从头开始编写。在这个网站上使用 Wordpress 所节省的任何时间都将被浪费掉——可能会浪费好几倍——去解决那些试图支持一个轻量级的、相当封装的、与库存管理系统集成的平台所产生的问题，以及繁重的定制需求。

这听起来是个好主意，我们不想重新发明轮子

这是一个不可靠的类比。从零开始构建一个文章管理系统，与其说是重新发明轮子，不如说是选择构建一个光滑、高效的轻量级机箱，而不是从垃圾堆中抓取一个次优的机箱。(无意冒犯 Wordpress——它确实有有效的用例 <sup>1</sup> 。)

Wordpress 有一个丰富的扩展生态系统——有几个横幅管理器可供选择，可以满足这个需求

这是最有缺陷的陈述。诚然，Wordpress 有丰富的扩展生态系统，无疑也有一些管理横幅的好的。然而，客户对横幅的行为有非常具体的要求。还有一些已经描述过的非常高级的功能，其中控制横幅的数据必须从各种各样的子系统中传递过来，包括天气 API 和库存水平管理器。找到一个现有的 Wordpress 横幅广告管理器来做这些事情的可能性是零。与简单地从头开始创建相比，找到一个能做部分工作的程序，然后对它进行黑客攻击来完成所有工作，这是一个糟糕的想法。

“这是一个非常受欢迎的系统——代码库是健壮的，并且经过了良好的测试。”

这句话暗示了一个事实，流行的系统往往非常稳定。逻辑是这样的。使用这个系统的人越多，集体努力保持它滴水不漏的程度就越大，它就越不可渗透，一般来说就没有错误。然而，另一方面，可以说使用该系统的人越多，在黑客圈子中发现可利用弱点的集体努力就越大。虽然我承认这一点，但我是在唱反调。流行软件中建立的对策往往比黑客试图破解大东西的效果更重要。

“客户喜欢它——界面简单。”

由于 Wordpress 简单的管理员界面，许多客户倾向于使用它而不是其他内容管理系统。这是一个名副其实的观点，但是如果使用 Wordpress 会导致软件架构自身崩溃的话，这就不是特别相关了。

# 负责任地使用第三方软件

这是一篇哲学文章，所以让我们全力以赴，用一些类比来结束。把软件开发人员的角色想象成音乐家、插图画家、作家或机械师。所有这些角色都需要作者进行创作。在一个创造性的过程中，很多因素决定着成功，因此，描述这种努力的最好的词之一就是“艺术性”。

一个好的工匠会为自己的工作感到自豪。他们工作认真而有条不紊。他们不会做出草率的决定，也不会冲动行事。在做出架构决策时，请记住这些品质，您应该有一定程度的保护措施来抵御逆境。

最后，让我们把软件的创造想象成一辆汽车的创造:

*   如果你想建造一艘可以居住的船，不要带着一个小救生筏，堆在马桶、沙发、四柱床、写字台、交叉训练器、桑拿浴室上...它会下沉。
*   如果你想制造一辆即使在最恶劣的环境下也能生存的坚固坦克，不要用金钱能买到的最厚的装甲板，只用唾液和蜂蜜把它们粘在一起。它会很脆弱。
*   如果你想制造一辆会飞的、能配送快餐的交通工具，不要只是把一辆汉堡车切成两半，然后把一架飞机的尾部焊接在上面。这将是不优雅的，很难随后修改。
*   如果你想造出一辆跑起来很平稳的车，不要去废品堆里收集一个丰田变速箱，一个沃尔沃发动机，一个三菱离合器，一个斯柯达车轴，还有各种各样的车轮——它不会跑起来很平稳。而~~如果~~当它分解的时候，就会变乱。 <sup>2</sup>

相反，如果你想制造任何类型的汽车，考虑从头开始制造，以完全满足你的要求。经过一些调查后，你可能会发现，从零开始建造它不会比你通过采购现有零件“领先一步”花费更多的时间。

如果经过深思熟虑，你坚信使用备件可以节省时间，那么在开始组装之前，一定要了解所有部件的作用，以及它们是如何组合在一起的。仔细选择各种*兼容零件*，为您的车辆提供您想要的功能。如果你在废品堆上找不到合适的部分，不要愤怒地抓住错误的部分说“见鬼去吧”。从头开始做那个零件——当你的引擎发出咕噜咕噜的声音时，你不会后悔的。一旦你有了你所有的部分，每一个必要的组成部分一个(T3)，不要太匆忙地工作，带着自豪和爱小心地把它们组装起来。

如果工头告诉你工作中没有时间去爱...找别的地方工作吧——否则你最终只会成为一个无用的、无私的雇佣兵！升华的爱是软件强大的原因！

# 脚注

请不要把我对 Wordpress 和 Joomla 的评论误解为对每一个软件本身的批评。这两个系统都有有效的用例，我个人是 Joomla 的粉丝，对 Joomla 项目非常钦佩。请理解，我只是批评使用这些软件的决定，当它们不是手头项目的正确选择时。

请不要将这一类比误解为对这些特定汽车制造商的攻击。我只是在描述混合不同来源的部分是如何导致不满意和不规则的整体。