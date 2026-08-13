---
title: "选Codex还是Claude Code？一篇讲透！从配置到适用场景，再也不纠结​"
source: "https://blog.csdn.net/CSDN_DU666666/article/details/161338596"
author:
  - "[[CSDN_DU666666]]"
published: 2026-05-24
created: 2026-08-10
description: "文章浏览阅读6.6k次，点赞16次，收藏27次。二者同为，但底层模型、设计理念、核心优势完全不同。：2026年开源的终端AI编程智能体，依托GPT系列模型，主打，适配碎片化编码、简单需求迭代，生态成熟、插件丰富。使用的核心操作是在~/.codex/config.toml中设置openai_base_url指向兼容 API 端点，替代默认的 api.openai.com，从而无需科学的上网tool。Anthropic 官方终端编程工具，依托Claude大模型，主打，擅长处理多文件、大篇幅项目，逻辑严谨、出错率低。_claude code和codex的区别"
tags:
  - "clippings"
---
## 一，code x 和 claude code区别

二者同为 **终端AI [编程](#) 智能体** ，但底层模型、设计理念、核心优势完全不同。

**OpenAI Codex CLI** ：2026年开源的终端AI编程智能体，依托 GPT 系列模型，主打 **轻量高效、兼容OpenAI生态、代码补全、脚本开发、快速改Bug** ，适配碎片化编码、简单需求迭代，生态成熟、插件丰富。使用的核心操作是在~/.codex/config.toml中设置openai\_base\_url指向g兼容 API 端点，替代默认的 api. openai.com，从而无需 科学 tool。

**Claude Code** ：Anthropic 官方终端编程工具，依托Claude 大模型 ，主打 **长文本上下文、整项目理解、大型重构、复杂业务逻辑开发、代码审查** ，擅长处理多文件、大篇幅项目，逻辑严谨、出错率低。

话不多说，一个一个来配置使用，先配置和使用code x

## 二、code x的配置和使用（claude code配置使用见第三章）

对新手来说，Codex 的准备工作：

你只需要一个GPT 账号（需要科学tool，然后登录g 00gle网页，搜索chat--gpt，如图1），哪怕是免费版也能直接上手，只是免费账号的使用额度会少一些.

![](https://i-blog.csdnimg.cn/direct/6f6fcbc2522d459a94b2b421f4b95d2f.png)

有了账号之后，不用额外配置复杂环境，直接访问官方引导页就能下载安装，链接为：

```cobol
https://chatgpt.com/zh-Hans-CN/codex/get-started/
```

![](https://i-blog.csdnimg.cn/direct/80932bb8bdc54a63bc8d4d847c4d79f4.png)

下载慢的使用下方下载好，1.2MB，百度网盘链接，后续全程需要科学工具.

```cobol
通过网盘分享的文件：Codex Installer.exe

链接: https://pan.baidu.com/s/1GbD9xcRLMbCB-XLQBcwBPw?pwd=ruan 提取码: ruan
```

![](https://i-blog.csdnimg.cn/direct/09cbae289fa44817ac51d628f00967f8.png)

登录鼓哥账号，现在codex 需要找一个其他手机号。

登录成功后提示

![](https://i-blog.csdnimg.cn/direct/4ee096499df44f26a2a55eb7d8d4f6cb.png)

返回coedx [软件](#) ，提供免费的试用额度

![](https://i-blog.csdnimg.cn/direct/74475fcd9a504351a36dd06ff8cdfcd8.png)

回复超时，可能因为网络。

![](https://i-blog.csdnimg.cn/direct/57f140c41fc34c4ab64d319bbdb3bde5.png)

后续就可以导入你自己的指定的项目文件夹，去执行任务了。（这里也推荐去使用Trae，solo模式也很好用，免费的，高峰时段，可能得等待）

第一个要了解的能力是本地文件操作，Codex 可以自主读取和操作你本地的文件，而且不限数量.

我们一点"进入项目工作"就让我们选本地的文件夹了,也可以新建文件夹，。

只要选中这个文件夹，里面的所有文件 Codex 都可以读取和操作。

![](https://i-blog.csdnimg.cn/direct/fd72823f82034bd785603b07558b94d6.png)

然后选择权限。就可以执行操作了。

![](https://i-blog.csdnimg.cn/direct/af7f08cdebde4475a1b3affd0145ef69.png)

![](https://i-blog.csdnimg.cn/direct/3cbc655590fa4401be58cfb3abf9b691.png)

可以在一个项目中开启多个会话。

![](https://i-blog.csdnimg.cn/direct/f17c487831d34f699b24743622a427dc.png)

比如在另一个对话里布置不同的任务。

查看额度用量。

![](https://i-blog.csdnimg.cn/direct/e529b89a069b430caaf52778d23c4546.png)

3.3 模型选择

模型选择。速度这里，快速相当于加急通道，会消耗更多的额度。

![](https://i-blog.csdnimg.cn/direct/0c73d4995caa4973baed7fe4cbc363b7.png)

一般中度智能就够了，我们也可以选高~

这个小麦克就是语音输入功能了。但转录速度远不如下载语音输入法，当然比手打字还是快很多，推荐用语音。

Codex的第二大能力，就是在我们授权的情况下，可以使用终端执行命令。

4.1 装环境

大家以后用各种 Agent、做项目必备的工具，比如 nodejs、git 什么的。

我们可以一句话跟 Codex 说："帮我安装nodejs最新版本"，

因为这些东西比较常见，所以在自动审核权限模式下，你看它都不向我们申请提权，就熟门熟路帮我装好了。

像龙虾，Hermes，甚至它的竞品Claude Code，都可以让Codex装，完了还能教你怎么用。

比如我们装个hermes吧，我都不需要给它hermes的官网和仓库地址，我就说最近有个叫hermes的Agent很火，你帮我安装一个吧。

它会自己搜索然后判断到底是哪个，然后根据官方的文档，帮我们装好了，还验证好了。

像 Cursor 、Antigravity 这种软件应用，平时都是我们手动网页下载的，他也可以帮你下载和卸载。

用 Codex 我也建议大家下载一个 Agent IDE。因为现在 Codex 的缺点是没法打开文件内容直接编辑，侧边栏虽然可以看到文件结构和内容，但没法编辑。

![](https://i-blog.csdnimg.cn/direct/f95ee7f052114e5dbf6be374b7528682.png)

可以让codex,帮你安装个cursor都可以。

定时任务，点击自动化，输入提示词，或者通过聊天自动创建任务 ![](https://i-blog.csdnimg.cn/direct/a150caf7d6784987abbf670e34b25c78.png)

![](https://i-blog.csdnimg.cn/direct/03843e9dc5914339a57e8f3ac99f5a3b.png)

总结：

Codex 可以随时访问本地文件，读取、写、删、移动文件。文件夹内的内容也就成了 Agent 随时可以获取的上下文。这里的"项目"也就对应着本地的一个文件夹。ts:也可以手机下载codex，侧边栏

![](https://i-blog.csdnimg.cn/direct/c70a08d5353b425bab0d9db75ec6b79d.png)

无缝衔接电脑。

![](https://i-blog.csdnimg.cn/direct/ca344eeb2969482c80cda7eddf2192d8.png)

## 三、claude code配置的使用

claude code简称 CC ， 是Anthropic 在2025年2月推出的、原本为 [编程](#) 而生、运行在终端的 agent程序。当然它现在能做的远远不止编程

![](https://i-blog.csdnimg.cn/direct/3aa6b49fdba4494285a2e0be89765875.png)

1.安装

使用 cc 严格意义上有四种方式：claude 桌面应用、网页、ide 插件、终端。最原生、且功能最全最新的是终端。

### 方案 1：用 Cursor（最推荐！零配置、不用装Claude、不用VPN）

**这是目前** **开发者 99% 都在用的方案**

- 不用装 Node
- 不用配环境变量
- 不用 API Key
- 不用命令行
- **直接下载安装即用**

先去官网下载 cursor

```cobol
https://cursor.com/cn
```

![](https://i-blog.csdnimg.cn/direct/41b8f710703e4eb284499f1335ea608f.png)

下载慢，用我下载好的网盘链接：

```cobol
3.5.33

链接: https://pan.baidu.com/s/1FyoMnQaEJRU5kAyelQvAVg?pwd=ruan 提取码: ruan
```

打开后创建一个文件夹作为第一个项目

![](https://i-blog.csdnimg.cn/direct/5e23c97e97d148819224fbdda4cb2f82.png)

![](https://i-blog.csdnimg.cn/direct/6a8e41d1df7142c0b38a2a99ad573903.png)

一行命令安装（需魔法）

在官网：

```cobol
https://code.claude.com/docs/zh-CN/overview
```

![](https://i-blog.csdnimg.cn/direct/1d11c97622554b96b769076ff8012c33.png)

拷贝命令，在 ide 终端里输入：

```cobol
irm https://claude.ai/install.ps1 | iex
```

回车，自动下载好。或者就是通过IDE的Agent帮你装.更 AI 原生，也是日后更常用的,指令：

```
帮我安装 node 并用 npm 安装好最新的 claude code
```

如果没安装上，就使用下边的方法：

```coffeescript
# 1. 确认 Node

node --version

npm --version

# 2. 结束可能占用 claude 的进程

Get-Process -Name "claude*" -ErrorAction SilentlyContinue | Stop-Process -Force

 

# 测试 npm 是否可用（可先试镜像）

npm config get registry

 

# 若官方源慢，可临时用镜像

npm config set registry https://registry.npmmirror.com

 

# 安装 Claude Code

npm install -g @anthropic-ai/claude-code@latest

# 4. 验证

claude --version

 

安装完成后，关闭并重新打开终端，然后在项目目录运行 claude 即可。
```

![](https://i-blog.csdnimg.cn/direct/d3ec6dbeaaa340569a3326c2b98ed22f.png)

安装成功！！！

但是报错：

```
Unable to connect to Anthropic services

Failed to connect to api.anthropic.com: ERR_BAD_REQUEST
```

是因为：

- **网络限制**
- **API Key配置问题**

**配置大模型**

因为cc 是 agent 程序，所以给它配什么大脑可以自己选择。

最佳的也最划算的方式是 claude 订阅会员，输入 /login 登录就好了，

用guo产大模型或其他渠道的 api。建议下载 cc switch 来管理多个模型的切换，直接按这个链接去下载

```cobol
https://github.com/farion1231/cc-switch/releases/tag/v3.14.1
```

滑到页面最下面，根据自己的系统选择不同的版本。

- 下载： **CC-Switch-v3.14.1-Windows.msi** （11MB，安装版，支持自动更新）
```cobol
通过网盘分享的文件：CC-Switch-v3.14.1-Windows.msi

链接: https://pan.baidu.com/s/1BIa3tM0l_6DPB_cstO8Isw?pwd=ruan 提取码: ruan
```
- 备选： **CC-Switch-v3.14.1-Windows-Portable.zip** （便携版，解压即用）
```cobol
通过网盘分享的文件：CC-Switch-v3.14.1-Windows-Portable.zip

链接: https://pan.baidu.com/s/1b2VO3Vb4cq1QlUU2CvdLfw?pwd=ruan 提取码: ruan
```

![](https://i-blog.csdnimg.cn/direct/0a73bad325ca426d9d30ac1a826e19e4.png)

安装，点右上角+号，添加统一供应商

![](https://i-blog.csdnimg.cn/direct/20dbab600bd746eeb7f3770488778a7e.png)

![](https://i-blog.csdnimg.cn/direct/38c9815a4c664926a38669bfb2ba4e2a.png)

填入 apikey 和 baseurl，随意起名保存就行，我用的 kimi 的api-key,不会的查看我主页内容，关于kimi的api-key文章。

然后： [在编程工具中使用 Kimi k2.5 模型 - Kimi API 开放平台](https://platform.kimi.com/docs/guide/agent-support#%E5%AE%89%E8%A3%85-claude-code "在编程工具中使用 Kimi k2.5 模型 - Kimi API 开放平台")

```coffeescript
# 打开 windows 终端中的 powershell 终端

# windows 上安装 nodejs

# 右键按 Windows 按钮，点击「终端」

 

# 然后依次执行下面的

winget install OpenJS.NodeJS

Set-ExecutionPolicy -Scope CurrentUser RemoteSigned

 

# 然后关闭终端窗口，新开一个终端窗口

 

# 安装 claude-code

npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com

 

# 初始化配置

node --eval "

    const homeDir = os.homedir();

    const filePath = path.join(homeDir, '.claude.json');

    if (fs.existsSync(filePath)) {

        const content = JSON.parse(fs.readFileSync(filePath, 'utf-8'));

        fs.writeFileSync(filePath,JSON.stringify({ ...content, hasCompletedOnboarding: true }, 2), 'utf-8');

    } else {

        fs.writeFileSync(filePath,JSON.stringify({ hasCompletedOnboarding: true }), null, 'utf-8');

    }"
```

![](https://i-blog.csdnimg.cn/direct/34a05ec8fdaf45549e96a26b22767a42.png)

![](https://i-blog.csdnimg.cn/direct/f0946f27626e43589548bb8e20fbe70d.png)

重要提示：操作cc switch一定要在打开claude **之前，** 否则它默认会让你登录。已经在 cc 里面再去切，会切不动。这一步很多新手翻车，记一下~

回到 ide，输入 claude 回车启动。一些初始设置（主题色、安全提示、推荐设置、文件夹是否可信），一般全部回车通过就好。

![](https://i-blog.csdnimg.cn/direct/59146b6228c04313a15fe8da2b76cf09.png)

建议先切换到自己的项目目录，

![](https://i-blog.csdnimg.cn/direct/6117fca9092e44bdb3205ecc637214be.png)

集合常用的命令

| 命令 | 作用 |
| --- | --- |
| `/usage` | 查看你的 API 使用额度统计 |
| `/diff` | 查看 Claude 即将修改的代码差异，确认后再应用 |
| `/help` | 查看所有可用命令和快捷键 |
| `/exit` | 退出 Claude Code 会话 |

我们可以先最简单的跟它说：「帮我做一个桌面番茄钟 [软件](#) 」。

它没有直接开始做，它发现指令模糊，主动询问技术栈。

这是 cc 的特点：上下文不清楚时它会主动提问。

在使用 AI 辅助开发进行项目开发时， **Git 是你的绝对安全网** 。

你可以把 Git 理解成 **游戏里的存档系统** ：每完成一个稳定节点，就存一个档；后续代码写崩、改错、逻辑混乱时，随时可以读档回到上一个正常版本，不会让整个项目报废。

### 为什么 AI 开发必须用 Git？

AI 生成代码具有一定的不确定性，可能会：

- 意外覆盖文件
- 批量修改导致项目无法运行
- 多文件同时改动难以手动回退

**Git 能让你永远有退路，放心大胆地让 AI 尝试各种方案。**

### 安装与使用（超简单）

- **Mac 系统自带 Git** ，开箱即用。
- **Windows 可以直接让 Claude Code 自动安装** ，无需手动配置。

建议注册一个 **GitHub 账号** ，将本地存档同步到云端：

- 换电脑可以继续开发
- 团队协作更方便
- 项目永远不会丢失

### 所有操作都能用自然语言让 CC 帮你完成

你不需要记任何 Git 命令，直接对 CC 说：

- “帮我安装 Git 并绑定我的 GitHub 账号”
- “帮我把当前项目提交一个新版本”
- “帮我把当前分支推送到远程仓库”
- “帮我回滚到上一个正常的存档版本”

CC 会自动生成命令、执行操作、引导你完成每一步。

### 最佳实践（非常重要）

**每完成一个功能、修复一个问题，就让 CC 帮你存档一次。** 有 Git 兜底，你才能真正安心地让 AI 放手创作、重构、尝试，不用担心项目崩溃。

**Git + AI 开发 = 效率拉满 + 零风险兜底**