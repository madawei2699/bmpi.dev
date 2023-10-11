---
title: "我的AI阅读助手"
date: 2023-03-26
draft: false
tags: ["chatGPT", "信息输入", "自我提升", "知识管理"]
description: "基于 chatGPT 的聊天机器人，帮助你高效阅读。"
isCJKLanguage: true
---

## AI 降临

今年你一定或多或少被 chatGPT 等 AI 模型的消息轰炸过。早在去年年底的时候我就被 chatGPT 的实力震撼过，为此还专门写过一篇文章：[AI降临](/self/ai-arrival/)，虽说 AI 模型是被人工训练出来的，但许多测试都表明它或多或少都有一些与人相似的智能，根据算力的演进速度，它的成长可能比很多人想象中的要快，所以所今年是 AI 降临年也不为过。如果你经常混迹推特，可能对此会更有体会，每天有数不尽的 AI 研究被公布，新的模型被发布，甚至 GPT-4 也被 OpenAI 发布了，而 chatGPT 只是 GPT-3.5 的一个调优版！

虽然 2023 年是糟糕的一年，很多公司在裁员，经济比大家预期的要差。但 AI 技术的突破性成果让这一切看起来像是 iPhone 发布的初期，iPhone 在 2008 年发布，当年正是全球次贷危机爆发的一年，而且 PC 的发展在当时看也走到了尽头，是 iPhone 带领我们走向了移动互联网蓬勃发展的十年，谁也没能预料到 iPhone 给我们带来生活上的多大改变。所以乐观点看，也许 2023 年未来十年不错的开端，就算你此刻的处境也很糟糕，希望能坚持向前看，相信未来会更好。

关注我的朋友都知道，我投入很多的精力在[知识管理](/tags/知识管理/)上，也写了一些和信息获取的文章如[构建高质量的信息输入渠道](/self/my-info-input-channel/)与[构建自己的信息简报](/self/use-rss-email-read/)。这段时间我深深的意识到 AI 将会对我们未来的学习产生很深远的影响，从 chatGPT 能自由的切换语言到瞬间整理总结知识，这一切都看起来是如此的震撼。我为此专门测试了一些让 chatGPT 帮助我阅读总结整理书籍、文章、代码仓库，甚至与它结对编写代码。

![](https://img.bmpi.dev/a7714959-4d86-c666-37b7-970496409b4e.png)

## myGPTReader

在做完这些测试后，我意识到是时候革新我的知识管理流程和工具了，第一步就是从获取信息的阅读作为切入点，所以我花了十几天时间与 chatGPT 合作开发了我的个人 AI 阅读助手：[myGPTReader](https://github.com/madawei2699/myGPTReader)，它是一个在 slack 聊天软件里的 bot，可以读取任何网页、电子书与文档，并根据与问题相关的内容做总结与分析处理，无论你给它的是什么语言的版本，它都能按你需要的语言去整理，这对平时阅读大量英文文章的我来说是一个效率的提升，当然这个 bot 背后还是 chatGPT，所以可以询问它任何问题。

它能做什么？

### 功能特性

1. 在线读取任意网页内容包括视频（YouTube），并根据这些内容回答你提出的相关问题或总结相关内容。
   ![](https://img.bmpi.dev/my-gpt-reader-read-web-page-1.gif)
   ![](https://img.bmpi.dev/my-gpt-reader-read-web-page-2.gif)
2. 支持读取电子书与文档（支持PDF、EPUB、DOCX、Markdown、TXT），并根据这些内容回答你提出的相关问题或总结相关内容。
   ![](https://img.bmpi.dev/my-gpt-reader-read-pdf-1.gif)
   ![](https://img.bmpi.dev/my-gpt-reader-read-epub-1.gif)
3. 定时发送每日热榜新闻，无论新闻是中文还是其他语言，它都能使用 chatGPT 用中文自动总结新闻的内容，方便快速获取热点新闻信息。
   ![](https://img.bmpi.dev/my-gpt-reader-read-hot-news-1.gif)
4. 支持 `prompt` 模版，能根据消息历史记录的上下文回答你的问题，甚至能和你玩游戏。
   ![](https://img.bmpi.dev/my-gpt-reader-prompt-template-1.gif)
5. 支持多国语音交互（英文、中文、德语与日语），它会根据你的语言使用相关语言的声音来回答你的问题，从而帮助你训练外语能力，可以理解为它是你的私人外教。
   ![](https://img.bmpi.dev/my-gpt-reader-voice-1.gif)
   ![](https://img.bmpi.dev/my-gpt-reader-voice-2.gif)

这是一个开源软件，意味着你可以私有部署它到你的 workspace 中，当然也可以加入我的 [slack](https://slack-redirect.i365.tech/) 频道来免费体验它的强大。

## AI 时代的学习范式

当 AlphaGo 战胜了人类顶级棋手李世石那一刻，人类认为最复杂的棋类被 AI 淘汰了，当 Stable Diffusion 能够在一秒内画出人类画师一天的作品时，人类认为最复杂的艺术领域开始被 AI 攻克，当 chatGPT 能够在一秒内回答人类的任何问题时，人类认为最复杂的知识领域开始被 AI 攻克。

如果 AI 能够迅速掌握很多我们人类需要数年掌握的知识，那学习的意义何在？

如果把现今人类的知识领域看作冷兵器时代，那 chaGPT 的出现就如枪的诞生一样。一个带枪的人与冷兵器时代的人，谁更有优势？这个带枪的人就是<q>超级个体</q>，他能快速去实现自己的想法，而不需要去学习那么多细节性的知识，参加那么多无效的沟通会议，他能在短时间内实现以前一个团队才能实现的事情。

如果我们的很多技能注定要被 AI 淘汰的话，最好的就是提前去与它共舞。当第一辆汽车问世的时候，很多马车夫不以为然，但也有人主动去学习它，去迎接一个新时代的来临。