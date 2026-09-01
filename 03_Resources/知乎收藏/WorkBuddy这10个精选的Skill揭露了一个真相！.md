---
title: "WorkBuddy这10个精选的Skill揭露了一个真相！"
source: "https://zhuanlan.zhihu.com/p/2061121685168960244"
author:
  - "[[ArkAPI]]"
published:
created: 2026-08-31
description: "当你还在手动复制粘贴网页内容、逐字打会议纪要、一个PPT改八遍的时候，隔壁同事已经用 Claude自动跑完了一套工作流。怎么做到的？答案只有一个—— 装Skill。 Skill是什么？你可以理解为给AI助手安装的技能，让它…"
tags:
  - "clippings"
---
21 人赞同了该文章

当你还在手动复制粘贴网页内容、逐字打 [会议纪要](https://zhida.zhihu.com/search?content_id=279172868&content_type=Article&match_order=1&q=%E4%BC%9A%E8%AE%AE%E7%BA%AA%E8%A6%81&zhida_source=entity) 、一个PPT改八遍的时候，隔壁同事已经用 [Claude](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=Claude&zhida_source=entity) 自动跑完了一套工作流。

怎么做到的？答案只有一个—— **装Skill** 。  
  
Skill是什么？你可以理解为给AI助手安装的技能，让它不仅能聊天，还能操作浏览器、处理文档、下载视频、管理笔记。WorkBuddy的 [Skill生态](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=Skill%E7%94%9F%E6%80%81&zhida_source=entity) 现在越来越丰富，但真正的门槛不是安装，而是 **选哪些、怎么用** 。  
  
我翻了官方文档，把10个零成本精选Skill逐个试了一遍。  
  
这篇文章，就是一份使用地图。

## 一、agent-browser：让AI真正上网

这是我最先装的一个Skill，也是使用频率最高的。  
  
以前让AI读网页，它只能拿到静态 [HTML](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=HTML&zhida_source=entity) ，遇到需要点击"展开全文"、滚动加载、登录后才能看的内容，直接抓瞎。Agent Browser解决的就是这个问题，它让WorkBuddy像一个真人一样操作浏览器：打开页面、滚动、点击、截图、读取内容。  
我实际用过的场景：

- 某竞品官网的产品介绍需要点三级菜单才能看到，我让WorkBuddy自动展开并整理成对比表格
- 一篇公众号长文被折叠了，直接让它展开后提取全文摘要
- 需要给领导截某个后台数据页面的图，描述清楚位置和操作步骤，它自动搞定

你可以直接对话说"打开这个网页，完整阅读需要展开和滚动后才能看到的内容，整理成 [结构化](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E7%BB%93%E6%9E%84%E5%8C%96&zhida_source=entity) 摘要，并为关键页面截图留档。"

![](https://pic2.zhimg.com/v2-eccc90f34347d84dcf53e4c2153952d5_1440w.jpg)

## 二、办公文档四件套：PDF/DOCX/PPTX/XLSX全搞定

这四个Skill我打包说，因为它们是 **打工人减负的核心武器** 。

- pdf：抽正文、抽表格、 [OCR](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=OCR&zhida_source=entity) 识别、拆分合并。我上次收到一份扫描版合同，直接让它OCR提取文字+整理关键条款，以前1小时的活现在10分钟搞定。
- docx：生成Word文档、补目录、补页码、处理批注与修订。最实用的是整理结构，你把一堆零散内容丢给它，它能自动排成有层级的正式文档。
- pptx：读取现有幻灯片、总结每页内容、压缩结构、重做演示文稿。老板给了一份50页的PPT让精简到20页，以前手动删，现在直接让它分析每页核心信息再重组。
- xlsx：清洗表格、补公式、整理列、输出汇总。做 [数据分析](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90&zhida_source=entity) 的朋友会爱死这个，尤其是脏 [数据清洗](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E6%95%B0%E6%8D%AE%E6%B8%85%E6%B4%97&zhida_source=entity) ，去重、补全、格式统一，一句话的事。
![](https://pic1.zhimg.com/v2-3260aa01962fde3eee6781d41f787b28_1440w.jpg)

## 三、local-whisper：会议纪要从2小时变成5分钟

这个Skill解决的是一个特别具体的痛点： **[语音转文字](https://zhida.zhihu.com/search?content_id=279172868&content_type=Article&match_order=1&q=%E8%AF%AD%E9%9F%B3%E8%BD%AC%E6%96%87%E5%AD%97&zhida_source=entity) ，而且完全本地离线** 。  
模型下载完成后，不需要联网就能跑。这意味着什么？内部会议、敏感访谈、客户电话，这些不能上传云端的内容，本地转写就派上大用场了。  
我的工作流程是：

1. 会议录音 → [Local Whisper](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=Local+Whisper&zhida_source=entity) 转文字
2. 让WorkBuddy基于文字整理会议纪要（主要结论、行动项、负责人）
3. 直接输出成Markdown或同步到 [Obsidian](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=Obsidian&zhida_source=entity)
![](https://pic3.zhimg.com/v2-af2c407532d49acf3c171be00f64161e_1440w.jpg)

## 四、yt-dlp-downloader：视频内容纳入你的知识库

[B站](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=B%E7%AB%99&zhida_source=entity) 学习视频、YouTube教程、在线讲座，这些内容看完就忘，因为没存进你的 [知识体系](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB&zhida_source=entity) 中。  
这个Skill能下载视频、提取音频、下载字幕，然后把内容交给AI做摘要、整理、二次处理。  
我一般是这么使用的：

- 看到一个好的B站教程 → 下载音频+字幕 → AI提取重点 → 整理成知识卡片存进Obsidian
- 客户发了一个竞品宣传视频 → 下载后让AI分析文案结构和卖点
- 线上讲座来不及听 → 先下载音频，后面让AI总结核心观点
![](https://picx.zhimg.com/v2-2759ddddd0fac37582764ea65c95243f_1440w.jpg)

## 五、web-search：AI的实时大脑

大模型的知识有截止日期，遇到"最近""最新""现在"这类 [时效性](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E6%97%B6%E6%95%88%E6%80%A7&zhida_source=entity) 问题，它就开始瞎编。Web Search就是给AI装了一个实时搜索引擎。  
  
什么时候必须用呢：

- 查竞品最新动态（"最近一个月XX公司有什么新动作"）
- 整理行业资讯（"今年以来AI办公领域的重要变化"）
- 核实某个信息是否过时
- 做调研前先让AI搜一轮，再归纳整理

最高效的用法是： 一定要带时间范围（"最近1周""今年以来"），要求附带来源链接。这样输出的内容可以直接放进报告里，不用二次核实。

![](https://pic2.zhimg.com/v2-10766125a42b6d452932eaedb7a023b3_1440w.jpg)

## 六、obsidian：AI直接操作你的第二大脑

如果你已经在用Obsidian管理知识库，这个Skill是 **必装** 的。  
  
它让WorkBuddy能直接读取、写入、搜索、整理你的本地笔记。不用复制粘贴，不用手动搬运，AI直接在你的知识库里干活。  
  
**我现在的日常：**

- 每天工作对话结束 → "把今天的对话整理成日报，保存到Obsidian的日报目录"
- 看到一篇好文章 → 让AI提取要点，写成知识卡片存进对应主题文件夹
- 项目复盘 → 搜索Vault里相关笔记，让AI [关联分析](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E5%85%B3%E8%81%94%E5%88%86%E6%9E%90&zhida_source=entity)

**建议：** 提前规划好你的知识目录结构（日报/项目/灵感/会议纪要），不然后面文件多了会乱。

![](https://pic1.zhimg.com/v2-d22b69faf04580ad36a7ef3b9c7477fc_1440w.jpg)

![](https://pic1.zhimg.com/v2-bafeebd9d703594e88adb88d38e3fc00_1440w.jpg)

## 七、skill-scanner：装skill前先进行安检

第三方Skill本质上是可执行代码，来源不明的有风险。 [Skill Scanner](https://zhida.zhihu.com/search?content_id=279172868&content_type=Article&match_order=1&q=Skill+Scanner&zhida_source=entity) 的作用是在安装前做 **安全审查** ：发现可疑依赖、硬编码配置、潜在风险。  
  
安装任何非官方来源的Skill之前，先跑一遍扫描。尤其是从 [GitHub](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=GitHub&zhida_source=entity) 上找的第三方Skill，别盲目信任。  
  
**它不能替代你的判断，但能帮你避开明显的坑。**

**![](https://pica.zhimg.com/v2-4244cd50591a586f30e9c00b49258f60_1440w.jpg)

**

## 八、self-improvement：AI越用越懂你

这个Skill没有炫酷的即时效果，但 **长期使用价值最大** 。  
  
它的作用是记录你的纠正、偏好、习惯，让WorkBuddy在后续任务中越来越贴合你。比如你说过"我来自 [湖北](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E6%B9%96%E5%8C%97&zhida_source=entity) 但不爱吃辣"，下次它提到饮食推荐时就会避开辣菜。  
  
特别适合长期使用者。刚装上没什么感觉，用了一个月后你会发现它怎么会这么懂我。

![](https://pic3.zhimg.com/v2-f8e2a2b6bd2222030963064e4a6738bc_1440w.jpg)

## 九、find-skills：不知道装啥？先问它

Skill越来越多，选择困难症犯了。Find Skills的作用就是 **根据你的需求推荐合适的Skill** ，并解释差异。  
  
比如："我想让WorkBuddy帮我处理 [网页采集](https://zhida.zhihu.com/search?content_id=279172868&content_type=Article&match_order=1&q=%E7%BD%91%E9%A1%B5%E9%87%87%E9%9B%86&zhida_source=entity) 、截图和页面交互，请先帮我找出合适的Skill，并说明它们之间的区别。"  
  
特别适合刚入门不知道怎么选的新手，或者面对新需求不确定有没有现成方案的时候。

![](https://picx.zhimg.com/v2-2683b4a742dd7db0abd943a36f29ea65_1440w.jpg)

## 十、frontend-design：让AI生成的页面像人做的

很多AI生成的页面能用，但界面很丑。 [Frontend Design](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=Frontend+Design&zhida_source=entity) 让WorkBuddy在输出代码的同时考虑视觉层次、配色和整体质感，适合做产品落地页、活动报名页、演示Demo。  
  
具体怎么使用呢：  
  
举个例子：  
  
"帮我设计一个课程活动报名页，风格现代专业，突出报名入口和课程亮点，要能直接预览。"

![](https://pic1.zhimg.com/v2-e82fb7d3d128b91f8e308a2357eefd78_1440w.jpg)

Skill不是越多越好  
  
试完这10个Skill，我最大的感受是： **真正提升效率的，不是装了多少Skill，而是能不能把它们串成工作流。**  
  
比如我现在的一套典型流程：  
  
Web Search搜最新资讯 → Agent Browser深入竞品页面 → 办公文档Skill整理成报告 → Obsidian存档 → Local Whisper处理会议录音补充进去  
  
每个Skill都是一个节点，节点连起来，才是 [自动化](https://zhida.zhihu.com/search?content_id=276019139&content_type=Article&match_order=1&q=%E8%87%AA%E5%8A%A8%E5%8C%96&zhida_source=entity) 。  
**别追风口，找到你高频重复的工作，给它装上对应的手和脚。**

**如果你还不会部署WorkBuddy，搜索 [ArkAPI](https://zhida.zhihu.com/search?content_id=279172868&content_type=Article&match_order=1&q=ArkAPI&zhida_source=entity) ，点进主页观看小白完整详细教程！**

![](https://pic2.zhimg.com/v2-3b83e13320f1aa9f4dd3dfb111c2c57b_1440w.jpg)

编辑于 2026-07-16 16:30・浙江[科大讯飞AI学习机T30 Pro实际用起来怎么样？双11优惠应该怎么买？](https://zhuanlan.zhihu.com/p/1971590041492001185)

[

作为一个五年级男孩的爸爸，真的越辅导作业越让我真切的体会到了什么叫父爱如山体滑坡！ 儿子上学期期末考试成绩单一出来，我和老婆都傻眼了，语数外三门功课，只有语文成绩还说的过去...

](https://zhuanlan.zhihu.com/p/1971590041492001185)

赞同 21