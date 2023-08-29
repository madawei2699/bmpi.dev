---
title: "Google软件工程之文化篇"
date: 2022-07-20
draft: false
tags: ["软件工程", "Tech Lead", "Pair Programming"]
keywords: ""
description: "本文是《Software Engineering at Google》的读书笔记，同时会穿插分享我对软件工程的理解。本文主要介绍软件工程中人的因素，也就是文化部分。"
isCJKLanguage: true
og_image: "https://img.bmpi.dev/fb352645-8a44-1b52-f54c-50c0b6e651a0.png"
categories: [
    "软件工程"
]
markmap:
  enabled: true
  id: "software-engineering-at-google-culture"
---

- [编程与软件工程](#编程与软件工程)
- [软件工程发展史](#软件工程发展史)
- [Google软件工程文化](#google软件工程文化)
  - [团队协作的基石](#团队协作的基石)
  - [打造知识共享文化](#打造知识共享文化)
  - [领导团队（Tech Lead）](#领导团队tech-lead)
  - [工程效率测量](#工程效率测量)
- [总结](#总结)

在软件工程的概念被提出之前，IT行业经历了[软件危机](https://zh.m.wikipedia.org/zh-hans/软件危机)。当时IT行业开发的软件正在经历从小规模到大规模的过程，而没有系统化的方法论指导大规模软件的开发过程，导致软件工程师之间的协作非常的低效且质量难以得到保证。

直到IT行业意识到软件需要一种工程化的方法论来指导开发过程。

> 软件工程是将系统化的、规范的、可度量的方法用于软件的开发、运行和维护的过程，即将工程化应用于软件开发中（IEEE，1993）。

## 编程与软件工程

编程是利用某个编程语言实现某个算法或技术方案从而来解决某类问题，但这类问题规模一般很小，也不太会随着时间产生变化，或者其生命周期也很短，不需要考虑长期维护的问题。但软件工程与编程的区别在于前者需考虑时间与规模带来变化的影响。

时间的因素使软件工程需要考虑质量的问题，糟糕的质量会产生很多认知负荷，最终难以长期维护。规模的因素使软件工程需要考虑协作的问题，低效的协作可能与沟通方式有关，最终拖垮交付进度。

## 软件工程发展史

早期的软件工程借鉴了项目管理的流程，而传统的项目管理关注生产过程大过于人。

```markmap
# 传统软件工程
## 软件开发过程
### 需求
### 设计
### 开发
### 测试
### 集成
### 部署
### 维护
## 软件项目管理
### 范围管理
### 成本管理
### 时间管理
### 风险管理
### 干系人管理
### 沟通管理
### 人力资源管理
### 质量管理
```

以过程为中心的软件工程过程方法论主要有瀑布式与统一软件开发过程。这种软件开发过程需要产生大量的正式文档，通过严格的流程管理控制软件的开发过程。但软件开发过程却是一个知识密集型的生产过程，过于关注流程而忽略人的因素导致这类方法论很难适应需求变化，花费很多时间开发出的产品很难满足变化迅速的市场，最终逐渐被淘汰。

以人为中心的软件开发过程是为适应需求变化的代码编写和团队组织方法论。这类逐渐产生以极限编程与敏捷过程两种方法论。

```markmap
# 软件开发过程方法论
### 以过程为中心
- 瀑布式（Waterfall）
- 统一软件开发过程（RUP）
### 以人为中心
- 极限编程（XP）
- 敏捷（Agile）
```

这里以敏捷过程为例，敏捷开发至今已经成为很主流的软件开发过程。

```markmap
# 敏捷过程
## 价值观
- 个体和互动高于流程和工具
- 工作的软件高于详尽的文档
- 客户合作高于合同谈判
- 响应变化高于遵循计划
## 框架
### Scrum
### Kanban
## 实践
- 沟通
  - 每日站会
  - 迭代计划会议（IPM）
  - 成果展示（Showcase）
  - 回顾（Retrospective）
  - 反馈（Feedback）
- 进度
  - Sprint
  - Backlog
  - Story
    - User Story
    - Story Point
    - Story Kickoff
    - Story Desk Check
- 质量
  - 测试驱动开发（TDD）
  - 持续集成交付（CI/CD）
  - 代码评审（Code Diff）
  - 结对编程（Pair Programming）
  - 知识分享（Session）
  - 工作坊（Workshop）
```

敏捷开发倡导信任人而非死板的流程与计划管理，通过一系列如上所示的实践来营造一个信任为主的文化土壤，从而打造一个高效的研发团队，最终开发出高质量的软件成品。

当然本文不是介绍敏捷开发过程的，而是介绍Google公司的软件工程实践。

Google是一家伟大的互联网公司，同时研发出来了大量知名的软件产品。Google是怎么做到的？在2020年Google的三位工程师[@TitusWinters](https://twitter.com/TitusWinters) [@manshreck](https://twitter.com/manshreck) [@hyrumwright](https://twitter.com/hyrumwright)出版了《[Software Engineering at Google](https://abseil.io/resources/swe-book)》这本书来介绍Google的软件工程实践，这给了我们一个机会一窥究竟。

本文是Google软件工程系列的上篇之文化篇，来介绍软件工程最重要的一个基石：文化要素。

## Google软件工程文化

> 以下是《Software Engineering at Google》一书第二部分文化篇的思维导图，由于此部分占全书近20%，所以本文不会详细的介绍其中的概念，想详细了解的读者建议阅读原书。本文会结合此书这部分内容分享作者的个人理解及相关经验。

```markmap
# Google软件工程
## 文化
### 团队协作的基石
- 谦虚（Humility）
- 尊重（Respect）
- 信任（Trust）
### 打造知识共享文化
- 专业知识共享的挑战
  - 缺乏心理安全
  - 知识孤岛
  - 知识单点故障
  - 团队知识断层
  - 鹦鹉行为
  - 知识禁区
- 塑造团队学习文化
  - 营造心理安全的环境
    - 导师制度
    - 群组交流模式
  - 个人学习成长
    - 从遗留知识中学习
    - 问好问题
      - 群组
      - 邮件列表
      - 内部社区
    - 以教促学
      - 技术演讲
      - 技术文档
      - 代码
        - 注释
        - 评审
  - 团队知识沉淀
    - 知识库（Wiki）
    - 社区（Community）
    - 代码库（Codebase）
    - 简报（Newsletter）
### 领导团队（Tech Lead）
- 核心能力
  - 领导力
    - 职权（Authority）
    - 非职权
      - 团队成员
      - 项目干系人
  - 开发
  - 架构
- 反面模式
  - 招聘听话的团队成员
  - 忽视表现差的团队成员
  - 忽视人性问题
  - 做老好人
  - 打破招聘门槛
  - 把团队成员当孩子看待
- 正面模式
  - 不自我
    - 谦虚不自大
    - 信任团队成员
    - 接受团队成员反馈
    - 当犯错时及时道歉
  - 做一个禅师
    - 始终保持冷静
    - 通过提问帮助成员解决问题
  - 做团队催化剂
    - 构建团队共识
  - 扫清团队绊脚石
    - 技术问题
    - 非技术问题
    - 委托对的人解决问题
  - 成为团队老师和导师
    - 给团队成员成长机会
    - 量体裁衣式指导团队成员
  - 给团队设定清晰目标
  - 真诚
    - 友善富有同理心的提出反馈
    - 正面提出负面反馈
  - 追踪团队成员幸福感
    - 心情曲线
    - 匿名收集反馈
    - 一对一与成员沟通
    - 与团队成员沟通职业规划
  - 其它建议
    - 委托任务但也与团队成员一块解决问题
    - 培养可替换自己的第二梯队成员
    - 在问题发酵膨胀前解决掉它
    - 保护团队成员免受无关因素干扰
    - 让团队成员知道当他们做的好时
    - 在可控范围内给团队成员提供试错的机会
- 大规模团队领导力
  - 总是在做决策
    - 识别盲点
    - 权衡利弊
    - 决策，然后迭代
  - 总是不在场
    - 建立自驱团队
    - 划分问题空间
  - 总是在扩大规模
    - 通过成功的循环模式解决难题
    - 解决重要的委托，委托紧急的问题
    - 学会放弃
    - 保护好精力
### 工程效率测量
- 是否值得测量
  - 期望的结果是什么？为什么？
  - 如果数据符合预期，会采取什么行动？
  - 如果结果与预期相反，会采取什么行动？
  - 谁在何时决定对结果采取行动？
- GSM框架
  - 目标（Goals）
    - 代码质量（Quality of the code）
    - 工程师注意力（Attention from engineers）
    - 认知复杂度（Intellectual complexity）
    - 节奏和速度（Tempo and velocity）
    - 满意度（Satisfaction）
  - 信号（Signals）
  - 指标（Metrics）
    - 定量指标（Quantitative）
    - 定性指标（Qualitative）
## 过程
## 工具
```

文化为何是软件工程很重要的一个要素？我在刚入IT行业时对软件开发的理解只是学习某个编程语言来写代码。之后经过多年多个项目的历练，我逐渐对整个软件开发的全貌有了一定的理解。但还是停留在对软件开发过程的了解及相关工具的使用，直到读完这本书的文化篇后才意识到了文化的重要性。

文化虽然是和技术没有直接关系的一些软性的东西，但文化就像水一样，利用好它的力量，可以让软件开发变得更高效、高质。

### 团队协作的基石

软件开发早已不是单兵作战的时代了，虽然人们喜欢听黑客奇迹般的故事，但现在的中大型软件都是团队的集体智慧产出。

![](https://img.bmpi.dev/63d3f7fb-9e20-7d21-6ea1-d7601259b3f5.png)

![](https://img.bmpi.dev/414a14e0-e5b5-84c2-5a15-b17526d84e00.png)

如上是两大顶级开源基金会 [@TheASF](https://twitter.com/TheASF) 与 [@linuxfoundation](https://twitter.com/linuxfoundation) 的知名项目，成千上万的软件工程师的集体协作开发了达上亿的代码行数最终才诞生了这些伟大的项目。

而团队协作最重要的是成员间的谦虚、尊重与信任。如果没有这些最基本的条件，集体协作将会困难重重，团队成员无法形成稳固的合作关系，时间和精力很容易被内耗完。

### 打造知识共享文化

软件工程与传统工程的区别在于其是一项知识密集型的活动，其中涉及到了大量的知识管理工作。[知识管理](/tags/知识管理/)很容易做差，最终产生了以下的一些问题：

- 缺乏心理安全：当一个新成员进入组织的时候，会因为缺乏心理安全感而不敢暴露自己不懂的知识，这会阻碍该成员的个人成长，最终也会传导到团队的开发效率上来。
- 知识孤岛：知识没有分享的文化土壤，每个团队都闷头搞自己的项目，会逐渐产生很多重复造轮子的现象。
- 知识单点故障：团队内的重要信息只有一个人知道，而这个人又没有分享给其他人，一旦这个人不在场，团队将无法正常运转。
- 团队知识断层：团队内的技术专家并没有意愿将个人的经验传授给团队其它成员，导致团队的运转离不开一小部分技术专家，一旦这些人离开，整个团队将无法正常运转。
- 鹦鹉行为：这种很容易出现在团队新成员，由于经验限制或背景上下文了解有限，而在不了解其原理的情况下复制一些代码。虽然系统可以运行，但潜在的埋下了一些问题。
- 知识禁区：团队成员由于经验限制或不了解背景上下文，而不敢修改代码库的某些遗留代码。

这些都是组织学习文化中会遇到的一些挑战。Google在打造知识共享文化上做了很多尝试，比如营造安全的学习环境，鼓励新人通过提问与分享来提高个人的技术。这些实践在敏捷过程中也可以看到，比如：

- 鼓励个人反馈的文化（Feedback）。对事不对人，根据事实对成员提供建设性的反馈，帮助成员变的更好。
- 鼓励团队成员定期做分享（Session）。比如每周某个成员可以做某个主题的Session，这种实践能让分享者对分享的主题有更深的了解。
- 每日站会同步工作进度。不仅可以让其它成员了解你所工作的范围，还能告知其它成员潜在的交付风险。
- 定期回顾（Retro）。团队定期举行迭代回顾的会议，回顾最近工作中好的、不好的的方面及相关建议，最终制定出一些行动来提升团队的工作效率、质量与幸福感。
- 团队Wiki工作区。团队知识沉淀的体现，新人可以通过查阅这些资料迅速了解项目背景上下文。
- 代码评审（Code Diff）。代码评审在Google的软件工程中是一项全员必须执行的实践。代码评审可以提高团队成员的编码水平，发掘代码潜在Bug，识别代码坏味道，形成统一的编码风格，共享业务与技术知识，消减团队知识断层的影响，甚至可以解决鹦鹉行为与知识禁区的问题。比如我所在的项目上团队所有开发成员每天会拿出一小时做集体的Code Diff，虽然成本不低，但其收益也很高。
- 结对编程（Pair Programming）。结对编程看起来是两个人在做一个人的活，但一般也有两种模式：乒乓模式（Ping-Pong）与驾驶员观察者模式（Driver-Observer），前者适合以TDD的方式开发，后者适合老带新。结对编程是成本高但非常高效的知识共享的实践，需结合项目实际的带宽决定当前是否采用此模式开发。比如我们项目会在交付时间不紧张时采用此开发模式。

以上的一些实践需结合组织或项目的实际带宽来决定是否使用，比如Code Diff一般是建议或者强制的，而结对编程是可选的，分享（Session）和回顾可能是定期或不定期的。

### 领导团队（Tech Lead）

当做了一段时间独立贡献者（Individual Contributor）后，可能有机会做Tech Lead了。Tech Lead是一个团队必备的角色，具有一定的职权影响力，当然更多的是采用非职权影响力去带领团队。

如何领导团队是个复杂的工作，对技术人员来说，从技术到管理是很难的事情。

```markmap
# 领导团队（Tech Lead）
- 核心能力
  - 领导力
    - 职权（Authority）
    - 非职权
      - 团队成员
      - 项目干系人
  - 开发
  - 架构
- 反面模式
  - 招聘听话的团队成员
  - 忽视表现差的团队成员
  - 忽视人性问题
  - 做老好人
  - 打破招聘门槛
  - 把团队成员当孩子看待
- 正面模式
  - 不自我
    - 谦虚不自大
    - 信任团队成员
    - 接受团队成员反馈
    - 当犯错时及时道歉
  - 做一个禅师
    - 始终保持冷静
    - 通过提问帮助成员解决问题
  - 做团队催化剂
    - 构建团队共识
  - 扫清团队绊脚石
    - 技术问题
    - 非技术问题
    - 委托对的人解决问题
  - 成为团队老师和导师
    - 给团队成员成长机会
    - 量体裁衣式指导团队成员
  - 给团队设定清晰目标
  - 真诚
    - 友善富有同理心的提出反馈
    - 正面提出负面反馈
  - 追踪团队成员幸福感
    - 心情曲线
    - 匿名收集反馈
    - 一对一与成员沟通
    - 与团队成员沟通职业规划
  - 其它建议
    - 委托任务但也与团队成员一块解决问题
    - 培养可替换自己的第二梯队成员
    - 在问题发酵膨胀前解决掉它
    - 保护团队成员免受无关因素干扰
    - 让团队成员知道当他们做的好时
    - 在可控范围内给团队成员提供试错的机会
- 大规模团队领导力
  - 总是在做决策
    - 识别盲点
    - 权衡利弊
    - 决策，然后迭代
  - 总是不在场
    - 建立自驱团队
    - 划分问题空间
  - 总是在扩大规模
    - 通过成功的循环模式解决难题
    - 解决重要的委托，委托紧急的问题
    - 学会放弃
    - 保护好精力
```

对于此我的个人经验也不是很丰富，所以想了解此部分的推荐阅读此书原文。当然下面这些文章也不错：

- [The Definition of a Tech Lead](https://www.patkua.com/blog/the-definition-of-a-tech-lead/)
- [What Does a Software Tech Lead Do?](http://allyouneedisbackend.com/blog/2018/08/03/what-does-a-tech-lead-do/)
- [Tech Lead](https://icodebook.com/tags/tl/)
- [如何应对团队协作的五大障碍](https://insights.thoughtworks.cn/how-to-solve-five-dysfunctions-of-team/)
- [技术决策与团队认知负载](https://insights.thoughtworks.cn/monolith-microservice-architectural-tradeoff-cognitive-load/)

### 工程效率测量

<q>没有测量就没有优化</q>。只有测量到团队的工程效率后，我们才有可能制定提升效率的行动。Google设计出了GSM框架来测量工程效率。

```markmap
- GSM框架
  - 目标（Goals）
    - 代码质量（Quality of the code）
    - 工程师注意力（Attention from engineers）
    - 认知复杂度（Intellectual complexity）
    - 节奏和速度（Tempo and velocity）
    - 满意度（Satisfaction）
  - 信号（Signals）
  - 指标（Metrics）
    - 定量指标（Quantitative）
    - 定性指标（Qualitative）
```

工程效率的测量一般发生在大规模团队或组织级别，我所经历的项目上并没有此实践。对小型团队来说，可以通过简单的一些问卷调查这类定性的方式来收集团队成员的反馈，当然也可以通过一些量化的指标如流水线构建速度、迭代开发速率、代码静态分析结果、测试覆盖率等指标测量团队的工程效率。

推荐进一步阅读的文章：

- [敏捷交付的工程效能治理](https://insights.thoughtworks.cn/engineering-productivity-governance-in-agile-delivery/)

## 总结

软件工程是一项复杂的知识工程，这让其区别于传统的项目管理。Google的软件工程文化与以人为中心的敏捷过程所倡导的理念有很多相似之处。

但反观国内很多软件公司虽然也用上了敏捷过程的方法论，但底层的文化土壤还是以过程为中心的方法论，对团队成员的不信任，没有分享文化，团队领导一言堂还是存在的。希望这种现象能随着国内IT行业的逐渐成熟越来越少吧。