---
layout:     post
title: 毕业设计过程阶段总结及研究生入学前科研记录
subtitle:  论文，代码，相关问题
date:       2019-1-5
author:     "tomylee"
header-img: "img/t01c7d4d9a67918cf19.jpg"
tags:
    - 论文阅读
    - 科研
---

### 一、记录
#### 从考研结束后就开始了跟着师兄开始了毕业设计过程，最近阅读了几篇论文：
#### 下面是我在阅读论文时的部分思维导图，用以记录自己的阅读过程，相信大家也看不清图片。仅仅用来记录。

1.MiniSat
![minisat](/img/report/MiniSat_withMarginNotes.png)

2.MWCP
![maximum clique](/img/report/89C04C12-D34B-4817-931B-9452BDA36082.png)

3.s-plex
![s-plex](/img/report/3CE2DDD8-61EA-4631-85AA-53E5DF64C895.png)

### 二、论文阅读

```
1. 《组合测试:原理与方法》 严俊, 张健   Journal of Software, Vol.20, No.6, June 2009, pp.1393−1405 
2. TCA: An Efﬁcient Two-Mode Meta-Heuristic Algorithm for Combinatorial Test Generation.  2015 30th IEEE/ACM International Conference on Automated Software Engineering
3. An Extensible SAT-solver.  http://minisat.se/downloads/MiniSat.pdf
4. Frequency-driven tabu search for the maximum s -plex problem.  Computers and Operations Research 86 (2017) 65–78
5. Two Efficient Local Search Algorithms for Maximum Weight Clique Problem. Proceedings of the Thirtieth AAAI Conference on Artificial Intelligence (AAAI-16)

```

---
### 三、毕业设计及研究生前夕科研时间轴（ing）
```
 2018.12.25     小组会议，初步选定课题方向
      12.26     NP问题，SAT问题，图染色问题，局部搜索问题  
                论文1：《组合测试:原理与方法》
                论文2：TCA: An Efﬁcient Two-Mode Meta-Heuristic Algorithm for Combinatorial Test Generation
      12.27-29  源代码 ing
      12.30     小组会议，提交generateModel接口代码
      12.31     开始阅读MiniSat代码
 2019.1.3       论文第一遍概读完成，准备第二遍
      1.7       开始阅读新论文及代码
      1.9       论文4：Two Efficient Local Search Algorithms for Maximum Weight Clique Problem
                Conﬁguration Checking(CC)，最大团问题，禁忌搜索，s-plex
      1.10      论文5:Frequency-driven tabu search for the maximum s -plex problem
      1.13      小组讨论，下一步设计方法改进CC应用于s-plex
      1.19      研究师兄对于s-plex的改进（DTCC）
      1.21      讨论DTCC属性值修改规则及潜在的提升
      1.22      分析小组内kplex-DTCC-RL(Reinforcement Learning)代码；
                调节基于扰动的强化学习模型DTCC-RL中vpush的概率参数
      1.23      分析Multi-armed bandit problem（多臂赌博机问题），提升实验结果在MANN上的鲁棒性。
                Benchmark针对数据集mann_a81.clq     mann_a45.clq两个情况。
                查阅：《基于多臂赌博机的信道选择》2018, Vol. 39, No. 4，COMPUTER ENGINEERING & SOFTWARE 
      1.25      讨论perturb取值时exploit-explore比例，计划对于ε取值线性下降比较结果差异。
      1.27      到家后进行ε比对实验，绘制时间t-ε取值-结果avg、max趋势图，讨论ε下降函数对于结果的影响
      1.28      线性下降，上下波动监测25次随机seed结果
      1.29      绘制结果比较图表
      1.30      brock800_4测例k=2/3/4实验测试，分析perturb策略，针对不同图学习不同搜索策略
      1.31      分析实验结果，线上讨论CC与DTCC差距策略
      2.14      分析当前结果，准备调节M1参数，预计30小时
      2.16      导师见面，探讨之后发展方向
                使用36号服务器进行大图测试
      2.17      对于新的强化学习策略进行测试，预计33小时
      2.19      优化代码，进行最终实验
      2.22      LaTex绘制论文数据表格
      2.25      整理实验数据，编制latex格式结果表格
      2.26      使用新数据绘制realworld结果表格，修正论文
      3.2       制作论文介绍PPT
      3.3       开始回到第一个论文课题：TCA 
      3.7       组内讨论，商定下一步计划。开始分析TCA代码结构。
                论文1:[casa]Evaluating improvements to a meta-heuristic search for constrained interaction testing
      3.10-4.17 毕业论文
      4.18      IJCAI19论文rebuttal，新添加不使用强化学习的BLP对照实验。
      4.20      协助师兄进行ASE19论文实验处理。
                论文1：2016-Goal-conflict detection based on temporal satisfiability checking
                论文2：2018-A genetic algorithm for goal-conflict identification
      4.21      复现Tableaux实验
      4.23      Tableaux-aalta实验服务器运行
      4.25      Tableaux-pltl实验服务器运行
      5.1-5.6   LOGION和GA等13个算法部署实验，收集分析数据
      5.7       LTLmodel counter复现
      5.8       General计算实现
      5.9       对比LOGION和conflict-detection-timeout的general表现
      5.10      varify13个算法的BC，并汇总数据
      5.11-16   截稿前实验
      6.13-6.19 看完Shaowei Cai老师16篇论文，看完"Stochastic Local Search"第1-5章                       
                1.EWLS: A New Local Search for Minimum Vertex Cover
                2.Local search with edge weighting and configuration checking heuristics for minimum vertex cover
                3.Two New Local Search Strategies for Minimum Vertex Cover
                4.Configuration Checking with Aspiration in Local Search for SAT
                5.NuMVC: An Efficient Local Search Algorithm for Minimum Vertex Cover
                6.Double Configuration Checking in Stochastic Local Search for Satisfiability
                7.Two Weighting Local Search for Minimum Vertex Cover.
                8.Improving WalkSAT By Effective Tie-Breaking and Efficient Implementation
                9.Fast Solving Maximum Weight Clique Problem in Massive Graphs
                10.Two Efficient Local Search Algorithms for Maximum Weight Clique Problem
                11.Local Search for Minimum Weight Dominating Set with Two-Level Configuration Checking and Frequency Based Scoring Function
                12.Finding A Small Vertex Cover in Massive Sparse Graphs: Construct, Local Search, and Preprocess
                13.A Fast Local Search Algorithm for Minimum Weight Dominating Set Problem on Massive Graphs
                14.Improving Local Search for Minimum Weight Vertex Cover by Dynamic Strategies
                15.NuMWVC: A Novel Local Search for Minimum Weighted Vertex Cover Problem
                16.Towards faster local search for minimum weight vertex cover on massive graphs
      6.20-6.24 看完三篇机器学习与运筹优化论文，针对最大k-plex问题提出重启策略的变体；
                寻找研究方向新问题作为研究生方向挑战；
                1.Machine Learning for Combinatorial Optimization_ a Methodological Tour d'Horizon
                2.Machine Learning-Based Restart Policy for CDCL SAT Solvers
                3.An Empirical Study of Branching Heuristics Through the Lens of Global Learning Rate 
```
