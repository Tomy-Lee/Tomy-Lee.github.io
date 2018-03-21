---
layout:     post
title:      "软件团队管理基础"
subtitle:   "系统分析与设计作业2"
date:       2018-03-18
author:     "tomylee"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - 系统分析与设计
---
### 一、简答题

#### 1.简述瀑布模型、增量模型、螺旋模型（含原型方法）的优缺点。

瀑布模型：将软件生命周期划分为制定计划、需求分析、软件设计、程序编写、软件测试和运行维护等六个基本活动，并且规定了它们自上而下、相互衔接的固定次序，如同瀑布流水，逐级下落。按工序将问题化简，将功能的实现与设计分开，便于分工协作，即采用结构化的分析与设计方法将逻辑实现与物理实现分开。 

优点：
```
为项目提供了按阶段划分的检查点；
当前一阶段完成后，只需关注后续阶段；
可在迭代模型中应用瀑布模型。 
```

缺点:
```
在项目各个阶段之间极少有反馈，只有在项目生命周期的后期才能看到结果；
通过过多的强制完成日期和里程碑来跟踪各个项目阶段；
不适应用户需求的变化。 
```


增量模型：融合了瀑布模型的基本成分(重复应用)和原型实现的迭代特征，该模型采用随着日程时间的进展而交错的线性序列，每一个线性序列产生软件的一个可发布的“增量”。 

优点：
```
人员分配灵活，刚开始不用投入大量人力资源；
如果核心产品很受欢迎，则可增加人力实现下一个增量；
当配备的人员不能在设定的期限内完成产品时，它提供了一种先推出核心产品的途径；
增量能够有计划地管理技术风险；
```

缺点：
```
由于各个构件是逐渐并入已有的软件体系结构中的，所以加入构件必须不破坏已构造好的系统部分，这需要软件具备开放式的体系结构；
在开发过程中，需求的变化是不可避免的；
增量模型的灵活性可以使其适应这种变化的能力大大优于瀑布模型和快速原型模型，但也很容易退化为边做边改模型，从而是软件过程的控制失去整体性；
```

螺旋模型：螺旋模型是一种演化软件开发过程模型，它兼顾了快速原型的迭代的特征以及瀑布模型的系统化与严格监控。螺旋模型最大的特点在于引入了其他模型不具备的风险分析，使软件在无法排除重大风险时有机会停止，以减小损失。 

优点：
```
设计上的灵活性,可以在项目的各个阶段进行变更；
以小的分段来构建大型系统,使成本计算变得简单容易；
客户始终参与每个阶段的开发,保证了项目不偏离正确方向以及项目的可控性；
随着项目推进,客户始终掌握项目的最新信息,从而他或她能够和管理层有效地交互；
```

缺点：
```
很难让用户确信这种演化方法的结果是可以控制的；
建设周期长；
无法满足当前用户需求；
```

#### 2.简述 UP 的三大特点，其中哪些内容体现了用户驱动的开发，哪些内容体现风险驱动的开发？

(1)用例驱动

(2)以架构为中心的

(3)受控的迭代式增量开发

其中，1、3体现用户驱动开发，2体现风险驱动开发。

#### 3.UP 四个阶段的划分准则是什么？关键的里程碑是什么？

1.初始阶段 里程碑：是生命周期目标(Lifecycle Objective) 里程碑，包括一些重要的文档，如：项目构想(vision)、原始用例模型、原始业务风险评估、一个或者多个原型、原始业务案例等。需要对这些文档进行评审，以确定正确理解用例需求、项目风险评估合理、 阶段计划可行等。 


2.精化阶段/细化阶段 里程碑：生命周期体系结构(Lifecycle Architecture) 里程碑。包括风险分析文档、软件体系结构基线、项目计划、可执行的进化原型、初始版本的用户手册等。通过评审确定软件体系结构已经稳定、高风险的业务需求和技术机制已经解决、修订的项目计划可行等。 

3.构建阶段/构造阶段 里程碑：初始运行能力(Initial Operational Capability) 里程碑。包括可以运行的软件产品、用户手册等，它决定了产品是否可以在测试环境中进行部署。此刻，要确定软件、环境、用户是否可以开始系统的运行。 

4.产品化阶段/移交阶段 里程碑：产品发布(Product Release)里程碑。此时，要确定终目标是否实现，是否应该开始产品下一个版本的另一个开发周期。在一些情况下这个里程碑可能与下一个周期的初始阶段的相重合。

#### 4.IT 项目管理中，“工期、质量、范围/内容” 三个元素中，在合同固定条件下，为什么说“范围/内容”是项目团队是易于控制的？

工期是在合同里面确定好的，项目的每一个阶段都有规定的完成时间，不能随意更改。

而客户在合同中也规定好了项目的验收条件，质量也是不由团队控制的。

范围/内容是由团队控制的，因为只有由团队来控制，项目才能够顺利完成。

#### 5.为什么说，UP 为企业按固定节奏生产、固定周期发布软件产品提供了依据？

UP中的每个阶段可以进一步分解为迭代。一个迭代是一个完整的开发循环，产生一个可执行的产品版本，是最终产品的一个子集，它增量式地发展，从一个迭代过程到另一个迭代过程到成为最终的系统。

迭代过程具有以下优点：降低了在一个增量上的开支风险。如果开发人员重复某个迭代，那么损失只是这一个开发有误的迭代的花费。降低了产品无法按照既定进度进入市场的风险。通过在开发早期就确定风险，可以尽早来解决而不至于在开发后期匆匆忙忙。加快了整个开发工作的进度。因为开发人员清楚问题的焦点所在，他们的工作会更有效率。由于用户的需求并不能在一开始就作出完全的界定，它们通常是在后续阶段中不断细化的。因此，迭代过程这种模式使适应需求的变化会更容易些。

UP中，软件开发生命周期根据时间（固定周期发布）和RUP的核心工作流（固定节奏生产）划分为二维空间。时间维从组织管理的角度描述整个软件开发生命周期，是RUP的动态组成部分，核心工作流从技术角度描述RUP的静态组成部分。


### 二、项目管理使用

#### 使用截图工具（png格式输出），展现你团队的任务 Kanban，请注意以下要求：
```
每个人的任务是明确的。即一周后可以看到具体成果
每个人的任务是1-2项。
至少包含一个团队活动任务
```