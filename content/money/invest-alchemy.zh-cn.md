---
title: "我的投资助手"
date: 2022-09-12
draft: false
tags: ["投资组合", "交易策略", "资金策略", "风险评估", "交易评测"]
keywords: "我的投资助手"
description: "我的投资助手系统支持多种投资交易策略，可评估用户的投资交易水平，自动计算用户投资组合的净值，从而提升用户的投资交易水平。"
isCJKLanguage: true
---

## 背后的故事

在2016年的时候，我曾开发过一款App名为<q>[交易日记](/money/build-trade-system/)</q>，这个App是根据我自身对投资交易的需求而打造，之所以有这些需求是因为我经历了黑暗的A股2015年股灾。当时我的投资仓位出现了很大的亏损，甚至参与创业的公司也因此而倒闭，所以花费了累计半年的时间学习[投资交易原理相关的书籍](/money/passive-income-protfolio/202007/)。

之后有了交易日记App的前身，是一个Excel表格：

![](https://img.bmpi.dev/815c5166-a61f-1625-401f-80c695979bb7.png)

这个Excel是由VBA开发的，甚至对接了东方财富Choice软件的接口去获取股价等数据。当时我的注意力还放在股票投资交易上。但经过半年的实操后，发觉股票投资并不适合业余投资交易的人。就算能盈利也需要耗费非常多的心力，所以之后我逐渐将注意力放在了指数基金上。在18年开始，受到一本书的启发，我决定做一个长期的投资组合，名为[被动收入投资组合](/money/passive-income-protfolio/202006/)。

被动收入投资组合是一个完全由指数基金组成的投资组合，它类似<q>FoF</q>基金。我为此开发了另外一套更简单的Excel模版来监控整个投资组合的收益与风险指标。

![](https://img.bmpi.dev/7ea206be-409c-d2a2-bd39-7b618e3ba345.png)

在长达几年的实操中，我逐渐青睐技术分析中的[双均线投资策略](/money/passive-income-protfolio/202008/)，为此开发了一个策略提醒的小工具，这个工具每天会自动将投资交易的买卖信号通过邮件的方式去提醒交易者。

![](https://img.bmpi.dev/ed73c1ad-0a1c-cd37-7b10-250ae3cdd56b.png)

在这个工具持续运行的时间里，我一直在思考它的演进方向。直到今年才有了一些想法：

![](https://img.bmpi.dev/179d4fd0-018a-9fe0-a432-8dc8cf5d7fdd.png)

最终这些想法变成具体的一些任务及代码：

![](https://img.bmpi.dev/898d87a6-f801-e6e5-8510-c8791a3b9da5.png)

在肝了近百小时后，这个系统上线了：

[我的投资助手](https://www.myinvestpilot.com/)

## [我的投资助手](https://www.myinvestpilot.com/)

从功能上来讲，我的投资助手可以做下面的事情：

- 策略：策略提醒功能，能支持多种交易策略的扩展。
  - Double MA：双均线策略，已经上线。
  - Turtle：海龟交易策略。
  - Backtrace MA250：回踩年线策略。
  - Breakthrough Platform：平台突破策略。
  - 其他。
- 市场
  - 市场热度监控：比如全市场历史PE/PB的区间，开户人数的变化等，这些数据的变化和趋势本身也可以作为交易策略的输入。
  - 全球指数的历史数据：这些数据主要给投资组合表现评测做对比，也可以生成投资组合与全球主流指数对比的走势图。
- 投资组合：以基金计算净值的方式衡量投资组合的表现。
  - 通过历史交易、资金流水、历史持仓台账的数据计算出组合的净值。当前可支持A股ETF/LOF等场内基金、股票等投资标的，场外公募基金未来会支持的。
- 交易
  - 用户：支持用户的投资组合净值的计算。
  - 机器人：机器人可以自动追踪某个交易策略，根据资金策略自动维护某个投资组合。这比简单的回测要更系统和真实，可以看到每天的交易记录和持仓记录，还有资金的变化记录等。
- Web界面
  - 自动展示某个投资组合的收益走势图及全球主流指数的对比，还可以看到投资组合的风险状况，及交易、持仓和资金记录。
- 通知
  - 邮件通知
  - Telegram Channel通知

一些系统界面截图：

![](https://img.bmpi.dev/7360956a-1c30-77a2-e9a2-fd4db6f03683.png)

目前我的投资助手只上线了三款投资组合，前两款都是机器人跟随双均线交易策略的投资组合，第三个是被动收入投资组合。

![](https://img.bmpi.dev/e7eda067-5c2a-4fa3-05aa-2df0ca94ad45.png)

## 未来的规划

我的投资助手的主体功能大概完成了八成左右，剩下的就是围绕此做的一些修补，最多的还是如何开发不同的投资策略及机器人，用来方便的追踪不同交易策略对投资组合收益与风险的影响。

当然也可以把真实用户的投资交易记录用这个系统生成投资组合并自动维护，也可以方便的知道自己的交易水平与全球主流指数相比到底相差几何，我相信这种可视化的风险监控方式对我们长期投资来说是有益处的。

最后这套系统的代码是[开源](https://github.com/bmpi-dev/invest-alchemy)的，这意味着你可以按照自己的想法去开发，也可以免费的自用。当然如果你不想花时间折腾这些，但是又想用这种方式去量化理解自己的投资组合，那也可以[与我联系](/about/)。
