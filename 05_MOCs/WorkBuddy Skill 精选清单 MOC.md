---
title: MOC-WorkBuddy Skill 精选清单
created: 2026-09-01
tags:
  - MOC
  - WorkBuddy
  - AI
  - 工具
---

# MOC：WorkBuddy Skill 精选清单

> 从 6 篇知乎「WorkBuddy 必装 Skill」剪藏中提炼、去重、按场景归类后的精选清单。原剪藏保留在 [[03_Resources/知乎收藏/]]，本页是可检索的导航索引。

## 一、共识先行：Skill 不是越多越好

6 篇不约而同强调同一个结论：

- **贪多反而低效**——装一堆用不到的 Skill，会增加 WorkBuddy 的选择困扰、降低准确率、白白消耗 Token。
- **价值在「串成工作流」**——单个 Skill 是节点，连起来才是自动化。典型链路：`实时搜索 → 浏览器深入 → 办公文档整理 → Obsidian 存档 → 语音转写补充`。
- **装新 Skill 先安检**——第三方 Skill 本质是可执行代码，装前先扫一遍代码与权限（见「安全」分类）。

## 二、高频共识榜（≥2 篇交叉推荐）

多篇同时点名、可信度最高的 Skill：

| 能力 | 推荐 Skill | 用途 | 出现篇数 |
|---|---|---|---|
| 浏览器操控 | agent-browser | 展开/滚动/点击/截图/填表，读取动态网页 | 3 |
| 办公四件套 | pdf · docx · xlsx · pptx | 读写改文档、表格、PDF、演示 | 4 |
| 去 AI 味 | humanizer / humanizer-zh | 消除 AI 写作痕迹，保留事实与逻辑 | 3 |
| 知识库 | obsidian | AI 直接读写本地 Obsidian 笔记 | 3 |
| 语音转文字 | local-whisper / meeting-notes | 本地离线转写会议、访谈录音 | 3 |
| 创建技能 | skill-creator | 把工作流封装成一句话可调用的 Skill | 3 |
| 记忆偏好 | mem0 / memory-setup / self-improvement | 跨会话记住习惯与偏好 | 3 |
| 技能导航 | find-skills | 按需求推荐合适的 Skill | 2 |
| 前端设计 | frontend-design | 配色/字体/版式，摆脱「AI 紫渐变风」 | 2 |
| 安装安检 | skill-scanner / Skill-vetter | 装前扫描代码与权限 | 2 |
| 总结提炼 | summarize | 长文档/网页/PDF 快速出核心结论 | 2 |

## 三、分场景精选清单

### 🕸 上网与信息获取
- [[agent-browser]] —— 操控浏览器，处理需点击/滚动/登录的内容
- Agent-Reach —— 跨平台搜索：小红书、B站、YouTube、Reddit、X、GitHub、RSS
- Dokobot —— 借本机 Chrome/Edge/Brave 读网页（登录态、动态加载）
- 实时搜索：web-search / tavily-search —— 时效性问题，务必带时间范围并要求附来源
- yt-dlp-downloader —— 下载视频/音频/字幕，转成知识卡片
- find-skills —— 技能推荐导航

### 🧠 知识库与笔记
- [[obsidian]] —— AI 读写本地笔记（先开只读模式更稳妥）
- markitdown —— 各种杂乱格式统一转 Markdown
- 记忆类：mem0 / memory-setup / self-improving-agent —— 记住偏好，越用越懂你

### 📄 办公文档与会议
- pdf · docx · xlsx · pptx —— 官方预置四件套，读写改
- document-formatter —— 文档排版格式化（写 PRD、制度文件）
- PPT 进阶：ppt-master / pptx-generator / html-ppt / guizang-ppt-skill —— 版式审美 + 可编辑 pptx
- 语音转文字：local-whisper / localwhisper / meeting-notes —— 本地转写，隐私友好
- 邮件：email-writer / IMAP·SMTP —— 读、找、整理、发送邮件

### ✍️ 写作与内容
- humanizer / humanizer-zh —— 去 AI 味，保住事实逻辑
- copywriting / social-content / content-strategy —— 文案、社媒内容、选题日历
- 深知公文写作 / 蜜度公文写作 —— 公文规范化与审校
- Prompt Engineering Expert —— 把模糊想法拆成结构化提示词
- brainstorming —— 动手前先问清需求、目标与限制

### 🎨 设计与前端
- frontend-design —— 视觉层次、配色、字体、版式
- canvas-design —— 海报、封面、视觉主图
- image-generator —— AI 生图（有 GPT image 可忽略）
- ui-ux-pro-max —— 本地 UI/UX 知识库（84 风格 / 192 配色 / 98 准则）

### 💻 开发与效率
- skill-creator —— 封装工作流为 Skill
- systematic-debugging —— 先找根因再动手，防「修一个坏三个」
- planning-with-files —— 长任务把计划/进度写进文件
- code-simplifier —— 删死代码、合并重复、统一命名
- git-master / code-review / refactor / testing-expert / security-audit —— 代码质量全家桶
- vercel-react-best-practices —— React/Next.js 性能指南

### 🔒 安全与合规
- skill-scanner / Skill-vetter —— 装第三方 Skill 前先本地扫描，不泄露数据

### 🏢 办公协作
- 企业微信 / 飞书 / 钉钉 / 金山文档套件 —— 把办公平台接进 WorkBuddy

### 📈 垂直领域（按职业取用）
- 财经：straightflush（同花顺股票分析）、kol-live-replay（财经大 V 直播回放）
- 运营/营销：seo-optimizer、competitor-analysis、data-analyst、user-research、growth-hacking、marketing-ideas
- 学术：ArXiv 论文追踪 + 论文精读、summarize
- 财税/财务：davila7/xlsx、pdf、summarize

## 四、参考原文（6 篇剪藏）

- [[WorkBuddy这10个精选的Skill揭露了一个真相！]] —— ArkAPI，10 个精选 + 工作流串联
- [[给 WorkBuddy 装上这 10 个 Skill，干活效率真的起飞了]] —— 晓来，含安装路径讲解
- [[夯爆了！WorkBuddy 必装的 10 个 Skills，建议直接收藏！]] —— 96编辑器，按「底座→长脑子→信息处理→进阶」四组
- [[workbuddy最实用的技能skill推荐]] —— 财经情报站，按职业分岗推荐
- [[Workbuddy 新手必装的 10 个高星 Skill]] —— 小郑，新手向
- [[WorkBuddy必装skills清单]] —— 周萝卜，按写作流程场景

## 相关 MOC

- [[MOC-AI学习]] —— AI 学习路线
- [[03_Resources/知乎收藏/]] —— 全部剪藏原文

## 待探索

- 从本清单中选出个人高频场景，实际安装并验证效果
- 建立自己的「典型工作流」卡片（如：选题→写稿→去 AI 味→排版→发布）
