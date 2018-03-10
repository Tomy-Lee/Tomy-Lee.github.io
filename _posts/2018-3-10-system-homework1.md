---
layout:     post
title:      "Introduction of SE & OOAD"
subtitle:   "系统分析与设计作业1"
date:       2018-03-10
author:     "tomylee"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - 系统分析与设计
---
### 一、简述题
#### 1.软件工程的定义
软件工程：(1)将系统化、规范化、可度量的方法应用于软件的开发、运行和维护的过程，即将工程化应用于软件中。(2)对(1)中所述方法的研究。 ---IEEE[IEE93]

软件工程是指导计算机软件开发和维护的学科。采用工程的概念、原理、技术和方法来开发和维护软件，把经过实践考验而证明正确的管理技术和当前能够得到的最好的技术方法结合起来，这就是软件工程。
#### 2.阅读经典名著“人月神话”等资料，解释 software crisis、COCOMO 模型。

软件危机(software crisis)是指在软件开发及维护的过程中所遇到的一系列严重问题，这些问题皆可能导致软件产品的寿命缩短甚至夭折。20世纪60年代以前，计算机刚刚产生，软件设计往往有一个特定的目的，软件的规模较小，一般也没有文档资料，缺少系统化的开发方法。随着计算机的发展，过去开发软件的方式已经无法满足大型软件的开发，软件危机出现。 软件危机的根源是软件的大量需求与软件生产力效率之间的矛盾和软件系统的复杂性与软件开发方法之间的矛盾。软件危机的原因主要是用户需求不明确、软件开发过程缺乏正确的理论指导、软件开发的规模越来越大且软件开发的复杂度越来越高。软件工程的表现形式有项目运行超出预算、项目运行超过时间、软件质量低落、软件通常不匹配需求和项目无法管理且代码难以维护。软件危机的解决途径主要是正确认识计算机软件的内涵、充分认识到软件开发是一种组织良好、管理严密、协同配合的工程活动、采用成熟的软件开发技术和方法、开发和使用适当的软件工具。

COCOMO模型：COCOMO模型是一种在软件项中估算工作量、成本以及时间表的模型。由三个不断深入和详细的层次组成。第一层，“基本COCOMO”，适用对软件开发进行快速、早期地对重要的方面进行粗略的成本估计，但因其缺少不同的项目属性（“成本驱动者”）的因素，所以准确性有一定的局限性。“中级COCOMO”中考虑进了这些成本驱动者。“详细COCOMO”加入了对不同软件开发阶段影响的考量。 COCOMO是基于模型的成本估算方法，是一种代码行分析方法，通过基础的COCOMO的计算可以得出每个开发者需要投入的时间，整个项目的开发时间及开发人数。  

#### 3.软件生命周期。

软件生命周期又称为软件生存周期或系统开发生命周期，是软件的产生直到报废的生命周期，周期内有问题定义、可行性分析、总体描述、系统设计、编码、调试和测试、验收与运行、维护升级到废弃等阶段，这种按时间分程的思想方法是软件工程中的一种思想原则，即按部就班、逐步推进，每个阶段都要有定义、工作、审查、形成文档以供交流或备查，以提高软件的质量。但随着新的面向对象的设计方法和技术的成熟，软件生命周期设计方法的指导意义正在逐步减少。 生命周期的每一个周期都有确定的任务，并产生一定规格的文档(资料)，提交给下一个周期作为继续工作的依据。按照软件的生命周期，软件的开发不再只单单强调"编码"，而是概括了软件开发的全过程。软件工程要求每一周期工作的开始只能必须是建立在前一个周期结果"正确"前提上的延续;因此，每一周期都是按"活动 ── 结果 ── 审核 ── 再活动 ── 直至结果正确"循环往复进展的。

#### 4.按照 SWEBok 的 KA 划分，本课程关注哪些 KA 或 知识领域？

SWWEBok的10个知识域（KnowledgeAreas，KA）如下：

|中文|英文|
|---|---|
| 软件需求 | Software Requirements |
|软件设计|Software Design|
|软件构造|Software Construction|
|软件测试|Software Testing|
|软件维护|Software Maintenance|
|软件配置管理|Software Configuration Management|
|软件工程管理|Software Engineering Management|
|软件工程过程|Software Engineering Process|
|软件工程工具和方法|Software Engineering Tools and Methods|
|软件质量|Software Quality|

本课程关注的KA有软件需求、软件设计、软件建构、软件测试、软件构型管理、软件开发工程、软件质量等。 

#### 5.解释 CMMI 的五个级别。例如：Level 1 - Initial：无序，自发生产模式。

CMMI的全称为Capability Maturity Model Integration，即能力成熟度模型集成，能增强软件企业的开发与改进能力，帮助企业对软件工程过程进行管理和改进，从而能按时地、不超预算地开发出高质量的软件。

第一级别：初始级。首先，软件的过程有序性程度不高，甚至会出现混乱的情况，软件是否能够成功主要取决于研发人员本身的实力和努力，项目可能会成功，但是任务的完成中存在很大的偶然性。也就是说企业不能担保在实施同一类项目的时候还能按时按期完成任务，研发人员是这一个级别最关键的因素，企业占到的作用不甚明显。

第二级别：可管理级。公司在管理上已经具备了一定的能力，能够建立比较基础的项目管理规范，对于项目的实施能够列出相应的计划和流程，并且随着流程的进行可以对此实施监控和控制。从一级到二级，最大的差别就是企业的管理有了相当程度的进步，利用管理手段排除了企业在第一级别完成任务时候的随机性与不确定性，保证企业的项目都能得到成功。

第三级别：已定义级。在第三级别的情况下，企业不仅能够把软件管理和工程管理两个过程都实现标准化和文档化，而且软件产品的整个生产过程，都是可见可控的。也就是说，企业根据自身的情况以及自己的流程能够建立一套规范制度的管理体系与流程，从而保证在同类或者是不同类的项目上都能够得到成功的实施。在这一阶段，企业的科学管理已经形成企业文化，更是企业内涵。

第四级别：量化管理级。是第三级别的升级版，在第四级别的时候，企业的项目管理首先是已经形成了完善的制度，而且根据名称，可以平判处，这一级别最关键的两个字就是“量化”。对项目流程的管理做到量化、数字化、具体化，对软件过程和产品精度都有定量的控制，实现管理更加细致化，精细化，项目的质量也能因此保证相对的高质量和稳定性。

第五级别：优化管理级。第五级别优化管理级是软件企业项目管理目前来说的最高境界。企业能够非常主动的来对流程进行一定程度的改善，将更加先进的技术运用到其中，让流程优化上升到一个更高的层次。在第四级别的基础上，还能够利用当前的信息资料来对项目过程中出现的问题进行预防，让每一个项目都能有非常高的质量。

#### 6.用自己语言简述 SWEBok 或 CMMI （约200字）

CMMI全称是Capability Maturity Model Integration,就是能力成熟度集成模型，它是由美国国防部与卡内基-梅隆大学和美国国防工业协会共同开发和研制的。CMMI里面的要求都是来自于成功企业的最佳实践的，先进性不必怀疑。说到CMMI就会提到另外3个字母SEI(Software Engineering Institute)，也就是软件工程学院，他们提出了CMMI标准。CMMI有两种表述方式：连续式与阶段式，两种方式只是从不同的角度来阐述CMMI，其实质上表达的内容是一致的。连续式其实更加能反应过程改进的本质，并且能更好地引导企业把过程改进做到实处，但连续式比较难以理解。阶段式是直接继承CMM的，大家都比较容易理解，而且阶段式有一个级别，在商业上更好宣传，但很容易导致企业为了过级而过级。连续式和阶段式同时也是评估的两个不同角度，用连续式评估，企业会得到很多个PA的Level，用阶段式评估，企业会得到一个整体的Level。CMMI是为企业的商业目标服务的，既不是纯粹提高质量，也不是光增加公司的成本而不提高效益，CMMI是为了提高企业的生产力。

### 二、解释 PSP 各项指标及技能要求：
1. 说明项目的大小, 一般用代码行数 (Line Of Code, LOC) 来表示；也可以用功能点 (function point). 一个重要的指标是: 你在实际产品中写了多少代码，不包括空行/注释行/单字符行。
2. 花费时间，可以用小时, 天，月，年来表示。一组人所花费的时间可以用 (人数\*时间) 来表示，例如某项目花费了10个人·月。 
3. 质量如何，交付的代码中有多少缺陷?  交付有两个定义, 在 Code Complete “代码完成” 的时候, 交付给测试人员;交付到顾客那里去 (在软件交付的时候)。可以用缺陷的数量来除以项目的大小。例如 5 bugs / KLOC，意味着每千行程序有5个缺陷。
4. 是否按时交付，以标准方差来看，稳定的交付时间更为重要。

#### 阅读《现代软件工程》的 [PSP: Personal Software Process](http://www.cnblogs.com/xinz/archive/2011/11/27/2265425.html) 章节。按表格 PSP 2.1， 了解一个软件工程师在接到一个任务之后要做什么，需要哪些技能，解释你打算如何统计每项数据？ （期末考核，每人按开发阶段提交这个表）
```
计划阶段，要估计这个任务所需时间
开发阶段，进行分析需求、生成设计文档、设计复审 (和同事审核设计文档)、代码规范 (为目前的开发制定合适的规范)、具体设计、具体编码、代码复审、测试（包括自我测试，修改代码，提交修改）
记录时间花费
测试报告
计算工作量
事后总结
提出过程改进计划
```
关于PSP2.1的数据统计，可以小范围针对一个班级的课程项目进行采集，也可以针对一个公司的项目组进行数据分析，甚至可以扩大范围到具有相同特征的群体，比如原博客中一群平均工作时间在3年左右，平均毕业学位为硕士的职业软件工程师的匿名调查。然后对收集到的数据进行统计和记录并进行分析。根据原博客结果，可以看到SDE 在“需求分析”和“测试” 这两方面明显地要花更多的时间（多60% 以上）；但是在具体编码上, SDE 要少花1/3 强的时间。