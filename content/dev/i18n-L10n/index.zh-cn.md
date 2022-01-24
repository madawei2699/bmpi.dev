---
title: "国际化与本地化"
date: 2021-06-27
draft: false
tags: ["i18n", "l10n"]
keywords: ""
description: "本文分享笔者在项目中实施国际化与本地化的总结经验。"
categories: [
    "什么是X"
]
isCJKLanguage: true
og_image: "https://img.bmpi.dev/9adbc2f4-7fa4-0f67-046b-f135c4b117b7.png"
---

- [国际化（i18N）](#国际化i18n)
  - [国际化需解决的问题](#国际化需解决的问题)
  - [国际化相关标准](#国际化相关标准)
  - [文本编码](#文本编码)
  - [locale 与 language tag](#locale-与-language-tag)
  - [语言与国家代码](#语言与国家代码)
  - [gettext](#gettext)
  - [国际化流程](#国际化流程)
- [本地化（L10N）](#本地化l10n)
  - [本地化流程](#本地化流程)
  - [制定本地化策略](#制定本地化策略)
    - [地域与语言](#地域与语言)
    - [新增地域/语言/服务](#新增地域语言服务)
    - [增量本地化](#增量本地化)
    - [翻译的管理](#翻译的管理)
    - [本地化的实施方式](#本地化的实施方式)
    - [本地化多语言的实现方式](#本地化多语言的实现方式)
    - [本地化的挑战](#本地化的挑战)
    - [是否需要考虑SEO](#是否需要考虑seo)
    - [产品设计的本地化](#产品设计的本地化)
  - [微服务下的本地化](#微服务下的本地化)
    - [本地化的技术或业务标准制定](#本地化的技术或业务标准制定)
    - [开发环境与业务流程](#开发环境与业务流程)
    - [静态文本的处理](#静态文本的处理)
    - [是否存储语言与地区设置](#是否存储语言与地区设置)
    - [后端服务的本地化](#后端服务的本地化)
    - [第三方服务与资源的本地化](#第三方服务与资源的本地化)
    - [发布流程](#发布流程)
  - [微前端架构下的本地化](#微前端架构下的本地化)
  - [本地化的测试](#本地化的测试)
  - [本地化平台](#本地化平台)

一个成功的产品要走向全球需要经历很多环节，从软件开发的视角主要有国际化和本地化两个流程：

![](https://img.bmpi.dev/53875fc9-00ac-e8e2-8d91-06399755dcba.png)

语言环境是在某个国家或地域内使用特定的语言或语言变体，其决定了日期、时间、数字和货币的格式和解析方式，以及各种测量单位和时区、语言、国家与地区的翻译名称。**国际化使一个软件能够处理多个语言环境，本地化使一个软件支持一个特定的地区的语言环境**。这意味着全球化的流程是先使软件具备国际化的能力，之后做本地化实施使其能支持特定地区特定的语言环境。

> 基于他们的英文单字长度过长，常被分别简称成i18n（18意味着在“internationalization”这个单字中，i和n之间有18个字母）及L10n。使用大写的L以利区分i18n中的i和易于分辨小写l与1。([Wikipedia](https://zh.wikipedia.org/wiki/国际化与本地化))

## 国际化（i18N）

### 国际化需解决的问题

- 能够以用户本地的语言显示文本；
- 能够以用户本地的语言输入文本；
- 能够处理以特定编码的用户本地语言的文本。

### 国际化相关标准

我们知道国际化是为了解决与用户本地语言相关的文本显示与输入的问题，这个问题又与用户国家和语言相关，比如同样的英语在美国和英国就不同。在国际化标准还未出现之前，曾经有多种表示国家与语言的方法，这个 [Making Sense of Language Tags](https://www.slideserve.com/shantell/making-sense-of-language-tags) 的 Slide 就分享了这段有趣的国际化标准问世的历史。直到 IETF BCP(Best Current Practice) 47 的出现，才统一规定了国际化中语言标识（Language Tag）的定义及匹配标准。

而由于很多软件与系统早于此标准出现，就会出现一些与此标准不统一的问题。一个突出的问题就是语言标识定义中的连接线的选择，在 Linux 系列的操作系统中用 locale 来定义语言环境，如`en_US`表示美国英语，而在 BCP 47 中用`en-US`表示美国英语。前者选择了用`_`而后者采用了`-`来连接语言和国家。这种混乱有时候会带来很多意想不到的困惑，有时候你使用的某个库支持`en_US`，有的库却支持`en-US`，这不得不让国际化实现的过程中多了一些兼容性处理的工作，甚至因为语言不统一，出现很多沟通上的问题。

国际化相关的标准如下：

- IETF [RFC 6365](https://datatracker.ietf.org/doc/html/rfc6365)：统一定义了和国际化相关的术语。
- IETF [BCP47](https://www.rfc-editor.org/info/bcp47)
  - [RFC 4647](https://www.rfc-editor.org/rfc/rfc4647.txt)：制定了如何通过过滤（Filtering）和查找（Lookup）的方式匹配语言标识（Language Tag）。
  - [RFC 5646](https://www.rfc-editor.org/rfc/rfc5646.txt)：定义了语言标识（Language Tag）的组成，如使用`en-US`标识美国英语。
- ISO
  - [ISO 639](https://www.iso.org/iso-639-language-codes.html)：语言编码（Language codes）标准。
  - [ISO 3166](https://www.iso.org/iso-3166-country-codes.html)：国家编码（Country codes）标准。
  - [ISO 15924](https://unicode.org/iso15924/iso15924-codes.html)：脚本（Script codes）标准。

一个完整的语言标识（Language Tag）组成如下：

![](https://img.bmpi.dev/2d607dbd-e778-1434-1461-e03a347d06c8.png)

更详细的介绍见我这个 [i18N in Java](https://talk.bmpi.dev/2021/i18n-java) 的 Slide。

### 文本编码

不同编码有着可以表示不同字符集合的区别，比如我们无法用  ASCII 编码来表示汉字。Unicode 字符集可以用从 0 到 10FFFF （十六进制）范围的码点来显示几乎所有人类已知的字符。它的存储至少需要 21 位。文本编码系统 UTF-8 将 Unicode 码点适配到一个合理的 8 位数据流，并兼容 ASCII 数据处理系统。UTF 表示 Unicode 转换格式（Unicode Transformation Format）。

自 2009 年以来，UTF-8 一直是万维网的最主要的编码形式。截止到 2019 年 11 月， 在所有网页中，UTF-8 编码应用率高达 94.3%（其中一些仅是 ASCII 编码，因为它是 UTF-8 的子集），而在排名最高的 1000 个网页中占 96％。所以在国际化中推荐采用 UTF-8 编码。

这篇 [IT产品的国际化，绝不是“支持英文”就足够](https://mp.weixin.qq.com/s/j5hfWOBsOMYcQuMG36zqNA) 文章提到一些 GBK 编码的文本中有许多“看起来一样”的文字，其实有细微差别。但是，为了节省 Unicode 中的空间，给它们指定了同样的 Code Point。

![](https://img.bmpi.dev/274d1394-df55-c0cb-331c-635979581c65.png)

如何区分这些同样码位（用不同字形显示一个字符，即同一字位）的同位异字？这就需要 locale 的帮助了。

> 计算汉字数量时，通常是按照字形来计算的，即将一个代表相同语音语义的字的简化，繁体，异体，新字形，旧字形等等分别进行计算。这种计算方式实为是在计算变体。所以，长期以来错误地把大型字典里收入的字形数看作是汉字系统的规模。([Wikipedia](https://zh.wikipedia.org/wiki/字位))

### locale 与 language tag

locale 是软件在运行时的语言环境, 它包括语言（Language）, 地域（Territory）和字符集（Codeset）。locale 使用 language tag 标识语言国家，比如在 GNU Linux 中的定义格式为: 语言[_地域[.字符集]]，如美国英文是`en_US.UTF8`。在 Linux 中 locale 包括以下几个部分：

- LC_COLLATE：控制字符排序。
- LC_CTYPE：控制字符处理函数在处理大、小写或判断是否是字符。
- LC_MESSAGES：提示信息的格式。
- LC_MONETARY：货币的格式。
- LC_NUMERIC：数字的格式。
- LC_TIME：时间的格式。

如果你的 locale 是 en_US.UTF8，那么必须将其修改为 zh_CN.UTF8 才能正确显示中文。在 macOS 操作系统的 `/usr/share/locale` 目录中存放着全部支持的 locale：

![](https://img.bmpi.dev/b40d3f61-046a-df48-735f-b27ec188a3e8.png)

而在 BCP 47 中，language tag 的定义为`langtag = language["-" script]["-" region]*("-" variant)*("-" extension)["-" privateuse]`。

### 语言与国家代码

同一种语言在不同国家地区可能有一些细微的差异，比如美国英语和英国英语就有一些差异。同一个国家可能也有多种语言，比如中国有简体与繁体语言。在上述 locale 的介绍我们看到了使用`语言_地域`或`语言-地域`的方式来确切的表达一个国家的语言。

对于国家和语言 ISO 制定了相应的标准代码：[ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1) 与 [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1)。

浏览器使用语言代码来在 `Accept-Language` HTTP 头里发送浏览器接受的语言名。比如：it, de-at, es, pt-br 。

### gettext

GNU gettext 是 GNU 国际化与本地化（i18n）函数库，它常被用于编写多语言（multilingualization，缩写为 M17N）程序。许多编程语言如 C、C++、Python、PHP、Rust、Elixir 等都在语言内部支持了 gettext 的使用。

以下是 Java 调用 gettext 完成国际化的流程：

![](https://img.bmpi.dev/4a7179b3-ccae-ca0e-8554-1cb19a753e7d.png)

- xgettext 扫描源代码抽取出 tr()、trc() 和 trn() 这些 i18n 函数的输入字符串并创建一个包含所有源语言字符串的 pot 文件。翻译者需要工作的对象是.po文件，它是由 msginit 程序从 .pot 模板文件生成的。
- msgmerge 将字符串合并到一个包含单个语言环境翻译的 po 文件中。
- msgfmt 用于生成继承 Java `ResourceBundle` 类的 Java 类文件。

如下图是 PHP 使用 gettext 实现国际化的流程图：

![](https://img.bmpi.dev/5da7482c-d121-958e-606e-ff36aded4a1f.png)

Elixir 使用 gettext 实现 i18n 的目录结构：

```shell
priv/gettext
└─ en_US
|  └─ LC_MESSAGES
|     ├─ default.po
|     └─ errors.po
└─ it
   └─ LC_MESSAGES
      ├─ default.po
      └─ errors.po
```

### 国际化流程

gettext 的使用流程就是一个典型的使应用支持 i18n 国际化的过程：

1. 配置 i18n 框架。i18n 框架通过系统或者浏览器（如果是 Web 应用）的语言标识自动获取相关的语言文件。如 gettext 使用的是 .mo 后缀的文件，而 Javascript 一般是 .json 文件，Java 是 .properties 文件。
2. 抽取硬编码的源语言文本。在硬编码的地方调用 i18n 函数。对于这部分可以人工抽取，也可以通过程序或者插件（如 Javascript 的 i18next 国际化框架有 i18next-scanner）自动抽取。
3. 最后实施**本地化**。翻译（可通过人工或机器翻译，也有相关的翻译平台可以集成）这些抽取出来的要支持的国家语言文件。

## 本地化（L10N）

### 本地化流程

![](https://img.bmpi.dev/9adbc2f4-7fa4-0f67-046b-f135c4b117b7.png)

如上图是一个典型的本地化流程图。其中参与方有：

- 开发团队（Dev Team）：开发人员使系统具备国际化的能力，并将机器翻译版的多语言版本部署到集成环境供测试人员测试，能搭建自动化的翻译集成流水线。
- 市场团队（Market Team）：确认产品的市场和支持的语言，整理产品涉及到的术语表，并购买专业翻译服务，确定最终多语言翻译的版本。如果是大公司的话，可能有专门的全球化团队完成此工作。
- 翻译管理平台（TMS）：完成翻译语言的管理，一般都有特定的 API 接口或 SDK 开发工具包，可以集成到 CI/CD 环境，能自动化源语言和翻译语言文件的上传和下载。并具备管理界面，供翻译人员修改和确定翻译的最终版本。能提供多个机器翻译服务，也能提供人工翻译的购买或以开源协作的方式完成人工翻译。

### 制定本地化策略

#### 地域与语言

这块首先要考虑这些基本的前置问题：

- 地域的业务含义
- 用户的默认地域
- 地域的默认语言
- 不同地域是否使用同一套系统
- 是否支持用户切换地域
- 用户能否属于多个地域
- 地域与国家是否一对一关系
- 地域与语言的映射关系
- 地域与语言有无联动关系（用户是否能看到所有地域支持的所有语言）
- 语言切换是否需要保存到用户个人信息中
- 是否需要通过用户环境语言标识（操作系统或浏览器）设置用户默认语言
- **服务是否多地域部署，多地域数据是否隔离**

#### 新增地域/语言/服务

- 系统是否可以支持新增地域及新增地域的流程
- 系统是否可以支持新增语言及新增语言的流程
- 系统新增子服务时本地化的流程

#### 增量本地化

- 当实施本地化时新页面或组件出现时的本地化流程

#### 翻译的管理

- 是否需要翻译管理平台（TMS）
- 翻译管理平台的选型
- 翻译管理平台的集成
- 是否需要订购专业翻译服务
- 开发与翻译团队的协作流程

#### 本地化的实施方式

- 系统各服务是否由各自开发团队做本地化
- 是否有专门的本地化团队做本地化
- 本地化团队与各服务开发团队的协作模式
  - 是否通过代码 Open PR 的方式做本地化
  - 各服务开发团队如何做增量本地化
  - 各团队关于本地化的知识同步
- 本地化技术标准的制定与组织内部推广
  - 针对日期、时间、时区、数字和货币的特定语言格式使用行业标准库（例如 Unicode Common Locale Data Repository [CLDR](http://cldr.unicode.org/)）
  - locale 标识采用`语言_地域`或``语言-地域``格式，如`en-US`代表美国英语语言。

#### 本地化多语言的实现方式

- 通过子域名（gTLDS）或国家顶级域名（如 ccTLDs）区分多语言。如：https://en.wikipedia.org/
- 通过 URL 路径区分多语言。如：https://localizejs.com/de/
- 通过 URL 查询参数区分多语言（对 SEO 不友好）。如：https://locize.com/?lng=de
- 通过用户语言设置区分多语言。如：https://myaccount.google.com/language
- 通过浏览器本地存储区分多语言。如：https://www.instagram.com/

#### 本地化的挑战

![](https://img.bmpi.dev/f655798a-fd23-5828-1b72-b6ecd6d83b7a.png)

本地化的挑战主要有不同地域的语言、文化、书写习惯及法律方面的差异带来的问题，具体有以下类别：

- 文本编码：对于大多数西欧语言的文本，ASCII 字符编码就足够了。但是，使用非拉丁字母的语言（例如俄语、中文、印地语和韩语）需要更大的字符编码，例如 Unicode 编码。
- 单复数：不同语言有着不同的单复数形式。复数是用来表示一个“不是一”的数。单复数的变化型态在每个语言里面都不一样，最普遍的复数型态用来表示二或更大的数字。在某些语言中，也有用来表示分数、零、负数或者二。
- 图片翻译：有文字的图片需要被翻译。
- 动态数据（来自 API 的数据）：后端传给前端的被显示在界面上的数据都需要本地化。但是如何区分这些数据的来源是个难题，比如虽然数据是来源自后端的，但可能来自数据库，可能来自文件，可能来自内部其他服务，也可能来自第三方依赖的服务。
- 图标：一些在某个地区识别度很高的图标在其他地区用户看起来可能是完全不认识的或者是其他东西。
- 姓名/地址：姓和名的先后次序，地址书写的先后次序。比如中文都是先姓后名。
- 性别：有些语言比如法语很强调性别。
- 电话：不同国家的电话格式也不相同。
- 声音：不适当的声音或提示可能会引起人的反感，有些国家对声音性别很敏感。
- 颜色：颜色和色调与地域或市场有关，比如红色在美股标示下跌，在 A 股标示上涨。
- 计量单位
  - 货币：货币格式设置必须考虑货币符号、货币符号位置和负号的位置。大多数货币使用与区域性或区域设置中的数字相同的小数点分隔符和千位分隔符。但是，在一些地方并不是这样，比如在瑞士，瑞士法郎的小数点分隔符是句点。
  - 日期和时间：日期/时间的国际化，不仅涉及到地理位置（比如星期、月份等日历本地化表示），还涉及到时区（TimeZone，针对UTC/GMT的偏移量）。时区不仅是地理位置规定，更是政治规定，比如中国从地理位置上跨 5 个时区，但只使用一个统一时区。另许多国家都有“夏令时”的规定，柏林时间和北京时间的差距是会变化的。有时候是7小时（冬令时），有时候是6小时（夏令时）。
  - 数字：不同国家和地区数字表示方式也存在着区别。影响数字表现的因素包括数字字符的表示、数字符号的表示、数字的类型等。
  - 重量/长度/物理单位：因为单位的不同，同一套数据多地域版本需要做转换。
  - 业务相关的计量单位：比如不同国家产品的计费规则不同。此需要业务人员支持找出相应位置并给予换算规则。
- 句子长度：德语通常比英语长，阿拉伯语需要更多的垂直空间。
- 书写方向：许多语言是从左到右，但在希伯来语和阿拉伯语中是从右到左，在某些亚洲语言中是垂直的。
- 标点符号：例如英语中的引号 ("")、德语中的低引号 (,,") 和法语中的引号 (<<>>)。
- 换行/分词：亚洲 CJK(Chinese、Japanese、Korean) 字元集语言的规则与西方语言的规则完全不同。例如，与大多数西方书面语言不同，中文、日语、朝鲜语和泰语不一定使用空格将一个字同下一个字区分开。泰语甚至不使用标点符号。
- 大小写转换：英文中有大小写的转换，而中文没有大小写的区别。
- 法律相关：例如使用欧盟公民个人数据的 GDPR。
- 政治相关：比如本地化中涉及国旗和地图的显示，处理不好很容易造成大的事故。
- 排序方法：比如英文是按字母顺序排序的，而中文可以用拼音排序。

#### 是否需要考虑SEO

如果是 toC 的网站做本地化，需要考虑一些和搜索引擎优化 (SEO) 的事情，如这篇 [How to approach an international strategy](https://marketfinder.thinkwithgoogle.com/intl/en/guide/how-to-approach-i18n/#make-languages-easily-discoverable) 提到的一些关键点：

- 如果您以多种语言提供您的网站，请在每个页面上使用单一语言进行内容和导航，并避免并排翻译；
- 将每种语言的内容保留在单独的 URL 上，并在 URL 中标记语言。例如，URL `www.mysite.com/de/` 会告诉用户页面是德语的；
- 通过`hreflang`元标记向 Google 显示您要定位的语言。如`<link rel="alternate" href="http://example.com" hreflang="en-us" />`；
- 不要只翻译模版文本，还需要翻译模版内的内容；
- 不要完全使用自动翻译，这会影响用户体验；
- 不要使用 cookies 或脚本技术来切换语言，Google 爬虫无法正常索引这些内容。

#### 产品设计的本地化

同一内容在不同地域使用更贴合本地内容的设计会带来更好的效果，如 [产品设计的国际化与本地化](http://www.woshipm.com/pd/4404611.html) 这篇文章中提到 Spotify 的歌单封面在不同国家的差异呈现形式：

![](https://img.bmpi.dev/165e3e02-74a9-a776-cc5a-cf8ef85f8f46.png)

### 微服务下的本地化

从架构的角度看单体应用的本地化流程比较简单。但现在很多应用都是微服务架构，多个团队协作开发的模式。如果是各自团队负责各自服务的本地化，必须有统一的本地化委员会制定本地化技术标准：

- 语言标识的确定；
- 语言切换在前后端的方案设计；
- 翻译自动化流程的设计等。

或者有专门的本地化团队实施本地化，前面这些问题将由这个团队负责解决。笔者参与的项目就属于后者，我们的团队完成整个大系统近十几套微服务子系统的本地化，这十几套系统又由几个大组多个团队负责，这类跨功能需求（CFR）在多个团队中的协作流程是个复杂的工作。

#### 本地化的技术或业务标准制定

在实施本地化之前，确定相关技术或业务标准是重要的事情，一些技术或业务标准有：

- 前后端不同技术栈的国际化实现标准。由于微服务中技术栈可能有多种，每种技术栈都有其各自的国际化实现方式，制定不同技术栈的实现标准有助于在不同服务间使用同样的实现方式；
- locale 标识的确定。
  - 前端或后端静态文本抽取中，可以将和语言相关的文本存放至语言标识命名的文件中，如`en.json`存放英语的静态文本，而`en_US.json`存放和美国英语相关的文本（如计量单位、日期、数字和货币等）；
  - 在远程服务调用（前端调用后端或后端调用其他内部或外部服务）中统一采用`语言-地域`格式，如`en-US`代表获取美国地区英语语言的本地化版本。
- 日期、时间、时区、数字和货币的特定语言格式使用行业标准库。比如使用实现 [CLDR](https://en.wikipedia.org/wiki/Common_Locale_Data_Repository) 标准的库；
- 动态数据类型的识别。比如识别出哪些数据是来自内部系统（数据库或文件）；哪些来自外部系统；这些动态数据是否具备国际化能力；如何分阶段本地化这些数据；
- 文档的本地化。后台系统生成的电子文档（PDF）或电子邮件的本地化。如果这些文档是发给客户的，还需要考虑是否生成客户语言偏好的文档；
- 支持的地区与语言列表。比如出现不支持的国家或语言时进入错误页还是显示默认的地区或语言本地化版本；
- 默认地区与默认语言；
- 地区与语言是否具备绑定关系；
- 语言切换是否需保存至用户个人信息中。

#### 开发环境与业务流程

实际上我们团队在做本地化最耗费时间的就是本地环境的启动。因为涉及的服务众多，不同服务启动的方式又有着细微的差异，甚至指导文档也是错误的，需要不断的踩坑才能完成环境的搭建。最终我们的处理方式是联系各开发团队，每次在做某个服务的本地化前期，会找开发团队帮助我们设置本地的环境。

另外一个难点在于我们对业务的不了解。由于每个服务都有大量的组件和页面，包括后端服务不同来源的动态数据，光靠我们自己摸索很难搞清楚。最终我们在做这个服务的本地化前期，会找开发团队的业务分析师帮助我们介绍这个服务涉及的业务流程。

#### 静态文本的处理

- 梳理前后端的静态文本，识别哪些系统有国际化能力（初始语言版本已经抽取 locale 文件并设置好国际化库）；
- 找出日期、货币、数字格式出现的地方，并在这些地方调用本地化技术标准确定的行业标准库；
- 增量静态文本翻译流程的确定，当系统已经被本地化之后有新的文本添加时需要使用本地化的流程对其进行处理；
- 翻译平台的自动化集成，开发团队使用脚本或 CI/CD 流水线自动上传和下载原语言和翻译语言的文件。

#### 是否存储语言与地区设置

一些国际化站点的语言或地区切换设计成超链接，用户可以通过链接访问不同语言地区版本的站点，这类站点并不需要存储语言或地区的配置。

具备用户个人信息配置的站点一般会提供在个人信息设置中设置偏好语言和地区，这样用户在切换设备的时候可以同步上次设置的语言或地区。

如果你的站点用户切换设备并不频繁，简单的处理可以将这些配置存入浏览器存储中。当用户切换设备后，自动恢复默认设置。这样的设计好处在于简单，后期要过度到其他方案也会容易一些。具体选择什么设计，需要结合具体业务来选择。

#### 后端服务的本地化

后端服务的本地化涉及以下四部分：

- 静态文本。这类可以通过走读代码的方式去查找相关字符串；
- 数据库、缓存或文件。通过走读数据库初始化脚本可以查找到不满足本地化需求的初始数据，但对于动态存储的数据，还需要设计满足多语言存储的表。对于一些资源文件有必要翻译的也需要提供多语言版本并改造使用文件的代码；
- 远程调用其他内部服务（RPC）。内部服务调用的`locale`标识属于本地化技术标准制定的。比如可使用`locale = en-US`HTTP 头代表请求美国地区英语语言的页面。
- 生成文档（PDF 或 Email）。生成的文档包括模版静态文本与动态数据渲染的最终语言版本。尤其是这些文档和电子邮件需要发送给用户的时候，需要生成和用户语言相符的语言版本。

如果后端服务的技术栈不同，还需要本地化团队总结后端服务不同技术栈的国际化流程，并在组织内部同步给其他开发团队。

#### 第三方服务与资源的本地化

在后台服务远程调用中存在调用外部服务的情况，如果调用外部服务需要先确认外部服务是否支持多语言版本，如果支持的话可以按照对接文档来集成。如果不支持需要与外部服务供应商联系确定支持计划。

#### 发布流程

由于本地化的实施涉及十几个子服务的改造，可通过 [Feature Toggles](https://martinfowler.com/articles/feature-toggles.html) 控制本地化在不同环境的开启或关闭。本地化影响的测试（单元测试、集成测试与 UI 测试）也需要通过 Feature Toggles 来控制，这样可以最小影响原服务的测试套件。

一旦所有服务都完成了本地化实施，则可以打开所有服务的本地化 Feature Toggles 将最终版本上线。

关于本地化的 Feature Toggles 有两种设计可以选择：

- 集中式 Feature Toggles。比如可以搭建一个集中的特性配置服务，所有本地化相关的服务通过请求此服务来获取配置开关状态。好处在于无需重新上线即可实时开关本地化的特性。坏处在于没法灵活控制每个服务的本地化特性的开关。
- 独立 Feature Toggles。与集中式相反，每个服务设置自己的本地化 Feature Toggles 可以做到灵活解耦，坏处在于每次开关都需要重新发布上线单个服务。

### 微前端架构下的本地化

![](https://img.bmpi.dev/f57bbd44-f288-c81a-9f26-a55b767c6044.png)

如上图是一个微前端架构的网站，整个网站的界面是由 A/B/C/D/E 五个服务的页面组成的。语言切换按钮在服务 A 上，当用户切换英文到中文，其他服务 B/C/D/E 需要将各自的界面切换成中文语言版本。

一种方法是在浏览器加载页面的时候将国际化（i18n）库的实例统一由服务 A 来初始化并挂载到浏览器窗口（window）对象上，服务 B/C/D/E 使用服务 A 初始化的国际化库实例对象。当语言切换时，统一由服务 A 的国际化实例对象切换所有服务的语言。

每个服务的 locale 语言文件加载可以统一由服务 A 来加载到浏览器中，这种做法的好处在于可以知道最后一个语言文件加载完毕的时机，这意味着整个页面所有服务的本地化都初始化完毕，用户可以正常切换语言了。

### 本地化的测试

本地化测试验证应用程序或网站内容是否符合特定国家或地区的语言、文化和地域要求。

![](https://img.bmpi.dev/972d1ca1-ab5f-d0bd-25ed-bb13b11fc46d.png)

如上图是本地化测试需重点关注的点，更多详见这篇 [Localization testing: why and how to do it](https://levelup.gitconnected.com/localization-testing-9b8db20fb62f) 文章。

### 本地化平台

本地化中很重要的一块是选择合适的翻译管理平台（TMS），一般这类平台都有如下功能点：

- 术语表（Glossary）：专用品牌术语或领域术语表，可以帮助翻译人员更准确的翻译和产品或市场相关的专用语言；
- 翻译记忆库（Translation Memory）：TM 是一个数据库，用于存储之前翻译过的内容字符串。对相同或相似内容重复使用翻译。这样可保证翻译的一致性；
- 上下文编辑器（In-Context Editor）：这种编辑器可以抓取网站页面，让翻译人员了解整个页面的上下文，有助于提高翻译质量；
- 机器翻译（Machine Translations）：大多 TMS 平台都对接了一些机器翻译平台（如 Google 翻译），可以自动翻译目标语言，适合开发人员使用；
- 人工翻译（Human Translations）：可在 TMS 平台订购专业人工翻译服务。但也有如 Crowdin 提供了开源项目本地化翻译协作的功能，任何人都可以参与到这个项目上免费翻译，得票高的翻译文本将优先使用。

主要的本地化平台：

- [Crowdin](https://crowdin.com/)
- [Lokalise](https://lokalise.com/)
- [localizejs](https://localizejs.com)
- [Phrase](https://phrase.com/)

一些对国际化以及本地化的基本流程的介绍就到此为止了。本地化是个复杂的工作，最大的难点在于**对目标语言和文化的了解不足**。不过当你读完这篇文章后，我希望能给你更多的自信去做本地化相关的工作。