---
title: "策引全球投资组合：美股全球"
date: 2024-04-26
draft: false
tags: ["全球投资", "投资组合"]
keywords: ""
description: "本文介绍策引全球投资组合美股全球的详细情况，包括交易策略、资金策略、标的池及历史回测表现等。"
isCJKLanguage: true
---

在投资交易中，我们有多种选择来确定交易标的、确认买卖时机和分配资金。例如，我们可以依靠个人经验、消息面分析、技术分析等方法做出投资决策，或者选择公募基金、私募基金或跟投大V等方式进行投资。

每种方式都有其优劣，但大多数并不适合长期投资。例如，个人经验和消息面分析容易受到情绪和信息的影响，并且会消耗大量时间来追踪交易或市场机会，而公募基金、私募基金和跟投大V等方式可能会受到操盘人更替或利益冲突的影响而难以持续。

因此，我们需要一种简单、省心的投资方式，帮助我们在长期投资的道路上稳健前行。这正是策引产品的初衷。我们通过历史数据回测找到了一些适合长期投资的策略，精选投资标的，帮助投资者在长期投资中，简单、省心地实现资产增值。

## 组合介绍

- 名称：[美股全球](https://www.myinvestpilot.com/portfolios/myinvestpilot_us_global)
- 标识：`myinvestpilot_us_global`
- 描述：基于双均线策略的美股全球ETF组合
- 货币：美元
- 市场：美股
- 建仓时间：2018-01-31

美股全球是一个基于双均线策略的美股全球ETF组合，投资范围覆盖全球多个关键市场，包括美国、德国、日本等，还包括一些与权益资产相关性极低的大宗商品ETF。这种全球化的配置有助于降低市场风险，提高投资组合的稳定性和收益率。

## 策略

策略分为交易策略和资金策略两部分，交易策略帮助我们确定具体某个交易标的的买卖时机，资金策略帮助我们合理分配资金，有效控制风险。

### 交易策略

交易策略主要分为趋势策略和均值回归策略两种。趋势策略适用于市场明显呈现趋势的时候，而均值回归策略则适用于市场震荡的情况下。在策引全球投资组合中，我们选择采用趋势策略，因为趋势策略能够以极低的成本捕捉市场的长期涨幅，非常适合长期投资。相比之下，均值回归策略虽然在短期内可能获得更高的收益，但由于难以判断市场拐点并且交易频繁，成本较高，因此并不适合长期投资。

美股全球采用经典的双均线交易策略，利用两条不同周期的移动平均线生成买卖信号。这种方法适用于追踪市场趋势变化，当短期线超过长期线时，会产生买入信号，反之则产生卖出信号。详细的策略介绍请看这篇[双均线交易策略](https://www.bmpi.dev/money/passive-income-protfolio/202008/)。

### 资金策略

资金策略是指如何合理分配资金，以达到最佳的风险收益比。在策引全球投资组合中，各个投资组合的资金策略不尽相同。有的组合采用固定资金分配策略，有的采用百分比资金分配策略，还有的采用动态资金分配策略。这些策略各有优劣，投资者可以根据自己的风险偏好和资金状况选择适合自己的资金策略。

美股全球采用百分比资金分配策略，这个资金分配策略的过程可以详细描述如下：

1. 当交易标的符合买入条件时，首先计算总资产的20%。
2. 将计算得到的20%与当前现金进行比较，取两者中较小的一个作为实际可用于购买标的的资金。
3. 将实际可用资金减去按比例计算的佣金，得到除去佣金后的可用资金。
4. 最后，根据除去佣金后的可用资金确定当前可买入标的的买入份额。
5. 如果最终可用资金少于总资产的1%，则不进行买入操作。

### 优化方式

通过在不同区间上回测组合表现，找到策略的一组参数，其表现中等偏上即可。这样的参数可以让组合在不同市场周期上表现良好，并且不会因为过于拟合而导致策略在不同周期上的适应性表现不佳。

## 标的池

美股全球投资组合通过整合全球优质的行业指数，实现了市场风险的有效分散。这种策略不仅覆盖了全球多个关键市场，而且通过多样化的配置，降低了依赖单一市场的风险。以下是我们选择美股全球投资标的池：

- SPY([标普500ETF](https://xueqiu.com/S/SPY))
- QQQ([纳指ETF](https://xueqiu.com/S/QQQ))
- CHAU([沪深300两倍做多](https://xueqiu.com/S/CHAU))
- INDL([印度两倍做多](https://xueqiu.com/S/INDL))
- EWJ([日本指数](https://etfdb.com/etf/EWJ/#etf-ticker-profile))
- EWG([德国指数](https://xueqiu.com/S/EWG))
- GBTC([比特币](https://etfdb.com/etf/GBTC/#etf-ticker-profile))
- GLD([黄金ETF](https://xueqiu.com/S/GLD))
- GUNR([全球上游自然资源](https://xueqiu.com/S/GUNR))
- PDBC([大宗商品最优化收益ETF](https://xueqiu.com/S/PDBC))
- ICLN([全球清洁能源](https://xueqiu.com/S/ICLN))
- XLE([能源业ETF](https://xueqiu.com/S/XLE))
- LTL([电信两倍做多](https://xueqiu.com/S/LTL))

## 回测表现

与[A股全球](/money/portfolios/myinvestpilot_cn_global/)投资组合相比，虽然使用的交易策略以及资金策略一致，但美股全球组合表现明显更好。

这是因为在美股可以做到真正的全球化配置，而A股市场的ETF种类有限，无法实现全球化配置。因此，美股全球组合更适合那些希望通过全球化配置以低风险进行投资增值的投资者。如果想要开户，请查看这篇[全球开户](https://www.bmpi.dev/money/guide-to-open-global-investment-account/)的文章。

以下是展示美股全球在过去几年的表现和市场比较的图表：

![](https://img.bmpi.dev/c0720ee0-cfd8-b5d6-5287-5ebf18f5a6cc.png)

- 顶部图表：展示了美股全球与其他主流全球指数的性能对比，通过突出显示的红色曲线，我们可以看到美股全球组合整体表现特别亮眼，大部分时间收益率高于15%年复合收益率。
- 中间图表：揭示了组合一直保持较高的仓位，无论是满仓还是半仓，几乎没有空仓期，这反映了组合对资金的高效利用。
- 底部图表：记录了自组合成立以来的每日盈亏变化，虽然存在一些波动，但总体上显示出比较平稳的增长趋势，适合那些偏好稳健投资的人。

## 对比指数

组合净值按照基金计算净值的方式来计算，同时与全球主流指数进行对比，而不仅限于本市场指数。这样做的目的是使组合能与全球所有主流市场对比，展示交易策略的全球有效性。
- `沪深300`(000300.SH)：代表沪深市场前300家规模最大且流动性好的股票。
- `中证500`(000905.SH)：包含沪深市场规模较小的500家公司，反映中型企业的整体情况。
- `创业板`(399006.SZ)：聚焦中国深圳交易所的高新技术和成长型企业。
- `恒生指数`(HSI)：包含香港交易所最大的50家公司，是评估香港市场表现的主要指标。
- `标普500指数`(SPX)：涵盖美国500家最大公司的股票表现，广泛用于评估美国股市的整体健康。
- `纳斯达克指数`(IXIC)：主要由美国科技股组成，展示科技行业的表现。
- `德国DAX指数`(GDAXI)：包括在法兰克福交易所上市的30家德国大型上市公司。
- `日经225指数`(N225)：代表东京证券交易所上市的225家主要公司的表现。
- `韩国综合指数`(KS11)：涵盖韩国首尔证券交易所主要上市公司的表现。
- `澳大利亚标普200指数`(AS51)：反映澳大利亚证券交易所前200家主要上市公司的表现。
- `印度孟买SENSEX指数`(SENSEX)：包括在孟买证券交易所上市的30家主要公司。
- `15%年复合收益率基准`：以15%的年复合收益率作为对比基准，超过此基准的组合表现被视为非常优秀，长期维持这一水平具有挑战性。

### 风险指标分析

截至2024年4月24日的数据显示，投资组合展现出了以下关键风险指标：

- 夏普比率为0.853，这个指标反映了每承担一单位总风险所获得的超额回报。换言之，它衡量了投资组合在单位风险下实现的回报率，因此，夏普比率越高，表明相对于承担的风险，投资组合获得的回报质量越高。
- 当前回撤为-1.69%，这一指标显示了从投资组合的最高点到当前的相对下降幅度。投资者可以通过当前回撤与最大回撤的比较来评估投资组合目前的风险水平。
- 最大回撤为-18.99%，这是一个重要的风险指标，能够帮助投资者了解投资组合在市场波动下可能面临的最大损失。当组合当前回撤接近最大回撤时，需要重点关注交易信号，因为这可能会提供不错的介入机会。
- 最大回撤持续时间为312天，这意味着该组合从一次重大资金损失中恢复到正常水平需要较短的时间。这个指标提示着投资者，在某些时期可能需要更多的耐心和坚定的信心，以克服市场波动带来的负面影响。
- 投资组合进行了278次交易，胜率较低，只有131次盈利交易对146次亏损交易，交易频率不高，但盈亏比较高，这也反映了趋势策略的特征：通过小幅亏损来捕捉大的趋势。

### 与买入持有策略对比

买入持有策略是一种被广泛采用的投资策略，它简单地将资金投入到市场中并长期持有，不进行主动的买卖操作。在这种情况下，我们创建了一个美股全球的基准投资组合，该组合使用买入持有策略等比例买入标的池中所有的标的，然后持有不卖出。最后，该基准投资组合的复合年增长率（CAGR）为7.33%。

而与之相比，美股全球的复合年增长率（CAGR）为19.54%，远高于买入持有策略。这表明了投资组合具有较高的年化收益率，且其策略表现要比买入持有策略要好。这也进一步说明了投资组合所采用的策略的有效性，它能够在市场中实现更优异的投资表现。

## 组合每日交易提醒邮件

即使我们拥有交易策略，在长期投资中，很容易忘记具体的交易时机，尤其是对于上班族来说，更是没有时间盯盘。为解决这一问题，策引全球投资组合提供了每日交易提醒邮件服务。这样，即使是上班族，也能及时获取交易信号，抓住买卖时机。

如果你订阅了投资组合的每日交易提醒邮件，你将会在每个交易日的开市前收到一封邮件，这个邮件的内容包括：

![](https://img.bmpi.dev/0c7a06df-600f-286d-dbb6-544f84960a8b.png)

具体的邮件投递时间见[策引策略](https://www.myinvestpilot.com/strategy)页面中重要问题【邮件提醒的时间以及非交易日的信号处理是怎样的？】的介绍，在此不再赘述。

__注意__：每次交易时，每个投资组合都会使用其配置的资金策略来确定交易份额。然而，在每日交易提醒邮件中，只会显示交易标的和交易信号，而不会包含具体的交易份额信息。用户在进行实际操作时，需要根据自身的资金状况和风险偏好来决定具体的交易份额。当然，他们也可以参考投资组合的资金配置策略，以辅助确定交易份额。

## 其他

关于组合其他重要问题请参考组合页面中的[重要问题](https://www.myinvestpilot.com/portfolios)部分，也可以添加我的微信号`improve365_cn`，与我直接联系。