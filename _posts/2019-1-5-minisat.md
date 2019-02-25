---
layout:     post
title: 毕业设计过程之阶段小总结
subtitle:  论文，代码，相关问题
date:       2019-1-5
author:     "tomylee"
header-img: "img/t01c7d4d9a67918cf19.jpg"
tags:
    - 论文阅读
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
### 三、毕业设计时间轴（ing）
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
      1.27      到家后进行ε比对实验，绘制时间t-ε取值-结果avg、max趋势图，讨论ε下降函数对于结果的影响。
      1.28      线性下降，上下波动监测25次随机seed结果。
      1.29      绘制结果比较图表
      1.30      brock800_4测例k=2/3/4实验测试，分析perturb策略，针对不同图学习不同搜索策略
      1.31      分析实验结果，线上讨论CC与DTCC差距策略
      2.14      分析当前结果，准备调节M1参数，预计30小时
      2.16      导师见面，探讨之后发展方向。
                使用36号服务器进行大图测试。
      2.17      对于新的强化学习策略进行测试，预计33小时。
      2.19          优化代码，进行最终实验。
      2.22          LaTex绘制论文数据表格
      2.25          整理实验数据，编制latex格式结果表格
```
