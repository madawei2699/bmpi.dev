---
title: "如何学习一门技术"
date: 2021-11-20
draft: false
tags: ["如何学习", "技术方法论", "WebRTC"]
keywords: ""
description: "本文分享笔者在学习一门新技术时的一些经验。"
isCJKLanguage: true
og_image: "https://img.bmpi.dev/feafeb84-b5f7-3b81-3b07-3bc04fa4375d.png"
---

- [要学否](#要学否)
- [怎么学](#怎么学)
  - [主动搜索](#主动搜索)
  - [技术标准](#技术标准)
  - [技术历史](#技术历史)
  - [做好笔记](#做好笔记)
  - [学习计划](#学习计划)
  - [学习技巧](#学习技巧)
  - [寻求帮助](#寻求帮助)
  - [心理建设](#心理建设)
- [怎么用](#怎么用)

作为一个 [终身学习](/self/build-personal-knowledge-system/) 的实践者，我经常有学习一些新技术的需求。如何学习这些新技术不同人有不同的做法，早前我也写过一篇 [如何快速学习一项新技能？](/self/learn-skill/) 的文章分享我学习的理论框架，但这篇文章我会以我学习 WebRTC 这个技术为例分享我在学习新技术时用的一些方法。

如何学习某个知识，在我看来，主要矛盾在于解决这三个问题。

## 要学否

在学习一门新技术前需要解决的第一个问题是**要不要投资时间去学这门技术**。就像买书最大的成本并不是买书的价格，而是看书的时间。花费大量的时间去看一本没有价值的书，无异于浪费生命。正是方向搞错了，越努力越尴尬。

怎么确定一门技术的价值，可以从以下两个方面来考虑：

- **从知识体系出发**：某门技术经常不是孤立存在的，而是一个积木般搭建的大厦的一部分。要学习顶部的技术，就需要掌握一定的底部技术。如果一个技术很基础很底层，被很多高层的技术所依赖，那学习这门技术就很有价值。
- **从应用前景出发**：如果一门技术很有市场“钱景”，或者有潜在的市场需求，那学习这门技术就很有价值。毕竟我们学习目的很大的一部分在于赚钱解决自己的生活问题。

不过这里的难点在于从我们已知的信息来分析，很难判断某门技术的市场前景。如果分析判断错误，很可能会导致我们学习这门技术的时间被浪费。那最佳的选择的就是尽可能让自己所学的技术都满足这两点，哪怕最后没有市场前景，但如能成为我们知识体系的基础，也值得投入时间去学习。

基于这两方面的考虑，我开始学习了分布式系统的一些底层知识：

[![](https://img.bmpi.dev/578bc683-a3ba-f6f8-7c6a-965b95181c58.png)](https://twitter.com/madawei2699/status/1451837146097020928)

之后通过搜索间接找到了基于 WebRTC 技术的语音聊天网站 [speakrandom](https://speakrandom.com/)，在分析这个网站技术栈的时候找到了 [pion/webrtc](https://github.com/pion/webrtc) 这个框架，最终决定从这个框架入手开始学习 WebRTC。

## 怎么学

在制定了学习目标之后，剩下的问题是怎么怎么学？学习方法千万条，重要的是找到适合自己的学习方法。

我的方法是**善用搜索，找到对的资料和对的人**。学习本身不应是一件复杂的事情，因为它不是做研究，不是探索未知的东西，只是站在巨人的肩膀上把已经被解决的问题学习一遍。

但这里的难点在于资料千万份，一不小心就找到错误的资料，让本来简单的学习变得复杂，这就像天龙八部鸠摩智学了段誉给的错误的六脉神剑剑诀，很容易学的走火入魔。

### 主动搜索

[主动获取资料](/self/my-info-input-channel/#主动方式) 方式的要点在于从错误少的信息库筛选、交叉对比选择要看的资料。由于很多技术资料都是用英文写的，用 Google 英文搜索更容易获取高质量的资料。另外使用 Google 图片关键词搜索可以快速获得架构方面的资料，方便从高层次理解这个技术。

一般我会从 Google、YouTube 和 GitHub 上搜索某个技术相关的资料、视频教程和开源库。以搜索切入，找到合适的开源项目或者技术标准，然后制定学习计划。很容易通过 `webrtc` 关键词在这些平台上搜索得到这些资料和教程：

- GitHub
  - https://github.com/pion/webrtc
- YouTube
  - [WebRTC Crash Course](https://www.youtube.com/watch?v=FExZvpVvYxA)
- Google
  - [Build the backend services needed for a WebRTC app: STUN, TURN, and signaling - HTML5 Rocks](https://www.html5rocks.com/en/tutorials/webrtc/infrastructure/)

从 pion/webrtc 这个库上了解到作者是 [@Sean DuBois](https://github.com/Sean-Der)，GitHub 关注一波然后去 YouTube 搜索下他的演讲，又收获了一波高质量的教程：

![](https://img.bmpi.dev/e0c41270-6329-edcb-815e-a83fc2ede51e.png)

### 技术标准

另外一个高质量的资料是协议标准，比如 IETF RFC 文档。搜索一番后找到 WebRTC 相关的标准：

- https://www.w3.org/TR/webrtc/

从这个 W3C 制定的标准里又可以看到很多 IETF RFC 的资料。了解这些技术标准有助于我从高层次理解这个技术的一些特性。当然这些标准的细节我暂时不会去看，等到需要了解细节的时候再去看。

另外还可以从标准中梳理出这个技术的一些历史背景知识。

### 技术历史

复杂的技术不是横空出世的，而是从简单的技术逐渐根据需求而演变来的。很多时候一个技术的复杂是因为其有很多历史性而导致的，比如 Java 的范型之所以使用复杂并具备很多限制性是因为其为兼容老的库而妥协设计出的产物。了解这个技术的历史背景有助于降低理解这个技术的复杂度。

### 做好笔记

搜索而来的资料如果不做整理和记录的话，时间久了就全忘了。我把这些资料整理到了 [Logseq](/self/okr-gtd-note-logseq/) 这个双链笔记中。

![](https://img.bmpi.dev/0cc2b35b-e70c-547c-f0fa-26a54178da87.png)

从下面这个笔记拓扑图中可以看出我记录的分布式知识（Distributed System）和 WebRTC 间的关联关系。众所周知，学习在大脑的体现就是神经元突触之间建立新的连接，笔记间的知识通过这种方式也能帮助我们快速建立知识间的联系。

![](https://img.bmpi.dev/feafeb84-b5f7-3b81-3b07-3bc04fa4375d.png)

### 学习计划

记录完笔记后，我要做的就是规划时间把整理得来的资料学习消化。在这个环节可用 [时间管理](/tags/时间管理/) 的方法制定该项技术的学习计划。

### 学习技巧

学习技巧千万条，但有一条是我觉得很重要的，那就是把你所学的**说给别人听**，从别人的反馈中了解自己对该知识掌握薄弱的点。很多时候大脑在学习的过程中会有很多模糊不清的点，如果不说出来的话，这些不清楚的点会被忽略掉，但如果要让别人听得懂，那需要我们懂的更多才行。

![](https://img.bmpi.dev/e62f6b6b-637f-b8bc-0210-60578ba8664c.png)

写文章其实也是说给别人听，只不过比单纯的说要更为系统。所以我一般在学习某个技术的时候会去写文章分享。一方面让自己的知识梳理的更清晰，另外一方面可以与读者交流，掌握更多的知识，这也可以解决掉那个经典的**我不知道我不知道的知识**，当我写出来时，会有看到的人帮我发现我不知道的知识。

当然也可以在社交网站上分享一些学习中梳理的知识点，之后方便整合成文章：

![](https://img.bmpi.dev/7b4629f4-e36c-2af1-4385-a6f41a0f72fb.png)

### 寻求帮助

找对的人解决学习中的困惑无异于能加速整个学习过程。这方面很多开源项目都有自己的讨论区，比如我在理解 WebRTC SFU 的过程中就有很多困惑甚至错误的理解，在社区中与作者沟通后才得到了正确的答案：

![](https://img.bmpi.dev/e6a7869e-5cfc-6ecc-bfa5-5ebcbc7e11df.png)

当然我们还可以在论坛、GitHub Issue、邮件组或交流群等地方中寻求帮助。

### 心理建设

学习里的一大难点可能是不好意思说出自己不懂的点，尤其是工作多年后，要承认自己不懂是件困难的事情。但如果你以终身学习为目标，那么这方面就没什么障碍。不懂就去学，不懂就去问。无知并不可怕，年龄大不懂也不可怕，可怕的是不懂却隐藏这一点。

## 怎么用

在掌握了技术的理论后，可以通过技术的实践来提高自己的技术水平，比如做一个开源项目。对技术的应用有两种方式：

- 从零开始，一步步实现自己的系统。这种方式的问题在于，刚开始我们对技术的应用不是很熟悉，完全自己做可能无法应用一些最佳实践，摸着石头过河的成本比较高。
- 从现有的应用中改造。开源项目有很多好的应用，可以直接用来学习并改造。一方面可以加速应用的开发，另外还可以学习别人成熟的经验。

通过一番搜索，我找到了两个不错的学习项目：

- [webtrc-voice-chat](https://github.com/fletcherist/webtrc-voice-chat)
- [kraken](https://github.com/MixinNetwork/kraken)

这两个都是基于 pion/webrtc 库开发的语音聊天网站。基于这两个开源项目，我可以逐渐学习并开发自己的开源项目。

在学习完这两个开源项目（读完源码）后，我制定了基于 WebRTC 的应用开发目标：做一个隐私与本地优先的语音聊天的网站 [Free for Chat](https://www.free4.chat/)。

![](https://img.bmpi.dev/fa10713d-7e8c-c8ce-1282-fff0ed51c1b8.png)

这个目标有点大，我会把这个业余项目作为技术试验田，把需要学习应用的技术都应用到这里。

学以致用，是学习的最终目的。只有真正去用这个技术，才能真正掌握它。否则花费时间去学习，不用的话很快就忘了。

最后，能看到这里的话，希望这篇文章里提到的一些方法能让你更快速的学习某个领域的技术。