# 跳棋面试问题:移动棋子

> 原文：<https://dev.to/mortoray/checkers-interview-question-moving-a-piece-5430>

使用之前[模拟的棋盘](https://interview.codes/success/checkers-interview-question-data-modelling/)，我们现在可以在棋盘上移动棋子。这部分练习使用了一些基本的网格移动逻辑和错误检查。它由两部分组成:一个简单的动作和一个跳跃。我期望候选人完成这两部分。

# 移动棋子的功能

现在我们有了棋盘，就可以将棋子从一个位置移动到另一个位置。新功能的要求是:

*   一枚棋子可以水平、垂直或对角移动一格，到达 8 个相邻点中的一个
*   在源位置必须有一个片段
*   目标必须为空

我没有再次提到地点必须在黑板上。这是一个隐含的需求，因为我们正在建立一个数据模型。几个候选人不检查这一点，并编写将失败的代码。以前编写过助手函数的人通常不必担心这个问题，因为他们已经执行了检查。

许多候选人会重复检查——冗余代码是可怕的。这是一个很好的机会来看看候选人的经验和编写代码的方法。他们注意到冗余了吗？他们决定写一个新的函数来合并这个代码和旧的支票吗？

> 💭有时，谢天谢地，这种情况很少发生，我会在这一点上遇到防御性的人。当我指出多余的代码时，他们会[开始咆哮](https://interview.codes/success/dont-say-negative-things-in-an-interview/)为什么没问题。展现你积极的一面，优雅地接受批评。

函数参数也可能导致代码重复。我们反复提到了董事会的位置，所以我期待一个封装了位置的类型。不管是哪种语言，类型都是候选人应该熟悉的。即使在弱类型语言中，如 JavaScript，他们仍然可以使用元组或匿名结构。我已经看到，在优秀的编码人员中，轻松创建逻辑数据类型是很常见的。

```
def move_piece(self, from : Position, to : Position ):
    if not self.valid_location( from ) or not self.valid_location( to ):
        raise ValueError( “Bad position” ) 
```

Enter fullscreen mode Exit fullscreen mode

## 检查移动

检查移动是否在一个空格之外是解决问题的关键部分。受访者表现各异。有些人甚至没有停顿就完成了检查，有些人遇到了重大障碍，有些人甚至没有自己想出解决方案。

我经常看到创建允许移动的表格:一个数组列出了一个棋子可以移动到的所有相对位置。这与一个 for 循环相结合，该循环检查目的地是否是那些允许的片段中的任何一个。这不是一个好的解决方案，但是，这是一个解决方案。在面试中，无论你有什么想法，你都要坚持下去。这种基于表格的方法比没有方法要好。我会和候选人一起检查，指出问题，并提出替代方案。

基于增量的比较是一个简单的答案。在处理网格、地图或任何有距离的东西时，会出现很多增量值。由于基于网格的问题在面试中很常见，你应该熟悉这个概念。

下面的代码获取`x`和`y`的增量值，并检查移动是否为逻辑距离 1。

```
dx = to.x - from.x
dy = to.y - from.y

valid_move = abs(dx) <= 1 and \
    abs(dy) <= 1 and \
    dx + dy != 0 
```

Enter fullscreen mode Exit fullscreen mode

最后一点，`dx + dy != 0`是测试总距离的一个小技巧。我从未见过候选人使用它。更有可能是负面比较，`and not (dx ==0 and dy == 0)`。

# 添加一个跳转

有了基本的移动，我添加了另一个选项。有可能跳过另一个棋子，向八个方向的任何一个移动两个距离。这些要求是:

*   跳跃是从当前位置向八个方向中的任何一个方向移动两个空格
*   只有在当前位置和目标位置之间有一块时才允许
*   移动功能应该更改为允许跳转

候选人如何对待这种变化可以揭示他们的习惯。我很欣赏人们创建一个全新的功能，并说他们以后会想出如何将它与之前的功能结合起来。这使他们能够清晰地思考，并编写特定于跳转的代码。

其他人从移动代码中的增量测试开始，当检测到跳转时，编写一个全新的分支。这在逻辑上类似于一个新函数。它节省了一点开销，但增加了一些复杂性。

一群不幸的候选人试图将这些新的需求直接灌输到现有的 move 代码中。它涉及一些复杂的条件，奇怪的变量，通常还有大量的错误。采取这种方法的人情况最差。他们最不可能及时解决问题。也很难给予任何部分的信任，因为在完成之前，旧的行为和新的行为都不起作用。

找出问题是编码的关键，不仅在面试中，在日常工作中也是如此。从清晰的分离开始，然后将代码合并在一起。我就是这样写代码的。我不介意从一点冗余开始，只要我知道我要摆脱它。

我将测试有效跳转的逻辑留给您作为练习。想想三角洲运动。

## 扩展位

跳跃有额外的要求。我过去常常立即提到他们，但这增加了复杂性，而没有对候选人的能力有更多的了解。相反，我把它们钉在末端，作为一种放松练习。

*   如果一个棋子跳过了一个相同颜色的棋子，现有的棋子将保留
*   如果一个棋子跳过了另一个颜色的棋子，则另一个棋子被移走

这是一个真正的跳棋游戏的核心机制。你想拿走对方的棋子。

# 高级扩展

和我的[卡题](https://interview.codes/success/interview-question-a-two-player-card-game/)一样，我喜欢准备好高级要求。少数考生在实施跳转后，还有充裕的时间。既然他们已经通过了基本面试，我不介意问个难题。

*   在棋盘上找到一个有效的黑色移动

也就是说，编写一个函数，返回代表有效移动的源位置和目标位置。有效意味着这些值可以传递给`move`函数。

从来没有人写过这样的代码。太多了，而且有很多骗人的细节。相反，我对算法如何工作的口头描述感到满意。

到目前为止，有一个人给了我一个很好的答案。另一个人有一个好主意，虽然不太明白，但他欣赏这个挑战。

# 评估候选人

我创造了这个面试挑战来评估候选人。需求的进展让我洞察到个人如何处理他们的编码，以及他们的弱点。数据建模和错误处理的结合给了候选人很多可以考虑的东西，没有复杂的需求。

数据建模方面似乎让这个问题比我的纸牌游戏问题更难。关注数据结构，而不是逻辑流程，似乎是许多候选人的弱点。知道了这一点，这绝对是我在雇佣所有候选人之前会问他们的问题。这是编程的基本技能。

由于这种挑战，我经常保留这个问题，以便筛选更有经验的候选人。虽然一个低年级学生应该能够做到这一点，但通常需要更多的帮助，我们没有时间了。让我的问题适应候选人的期望对面试很重要，尤其是在早期阶段。

我建议您尝试编写这段代码。注意所有提到的事情。确保您可以创建自定义类型，并知道如何将它们用于参数。注意多余的代码，并考虑如何删除它们。了解基于网格的问题，因为它们在面试中经常出现。