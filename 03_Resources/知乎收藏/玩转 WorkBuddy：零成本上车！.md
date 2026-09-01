---
title: "玩转 WorkBuddy：零成本上车！"
source: "https://zhuanlan.zhihu.com/p/2055060403646886815?share_code=1gC6zGbaTt7Sk&utm_psn=2064049561337869077"
author:
  - "[[吴浩亮]]"
published:
created: 2026-08-31
description: "20260825 更新： 由于 deepseek-v4-flash 涨价，opencode go 已经不在提供 deepseek-v4-flash-free 模型，但适配自定义模型的方式是类似的。 注册之后可以根据官方链接自己配置带 -free 后缀的模型 最近计划写一个…"
tags:
  - "clippings"
---
[收录于 · 全栈 101](https://www.zhihu.com/column/c_1179521903809830912)

80 人赞同了该文章

> 20260825 更新：  
> 由于 [deepseek-v4-flash](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=deepseek-v4-flash&zhida_source=entity) 涨价，opencode go 已经不在提供 deepseek-v4-flash-free 模型，但适配自定义模型的方式是类似的。  
>   
> 注册之后可以根据 [官方链接](https://link.zhihu.com/?target=https%3A//opencode.ai/docs/go/%23endpoints) 自己配置带 -free 后缀的模型

最近计划写一个系列文章，主要面向非技术工作者，分享一些 WorkBuddy 的实用技巧和使用经验，重点就是消除“信息差”，让普通人也能用好 WorkBuddy，少走弯路。

话不多说，直接开整。

第一篇先分享一下如何“ **零成本** ”上车。

[WorkBuddy](https://link.zhihu.com/?target=https%3A//www.codebuddy.cn/events/invite%3FinviteCode%3Dttq91jm6v7d) 的安装和配置非常简单，这里就不多说了，直接根据 [官方文档](https://link.zhihu.com/?target=https%3A//www.codebuddy.cn/events/invite%3FinviteCode%3Dttq91jm6v7d) 下载安装即可。

类似 WorkBuddy 这样的 AI Agent 工具，别看注册会送你一些积分，但你用它写了几篇文档、跑了几次自动化任务后，很快它就会提醒你—— **「你需要充值了」** 。

然后你一看账户，送的 500 积分，不到 2 小时就烧完了。这时候你大概会想：行吧，充钱就是了。

![](https://pic2.zhimg.com/v2-e4abd8fdff0f2482829d4989f0dfb6a1_1440w.jpg)

我本地跑了几个任务，就用掉将近十分之一

但在你掏钱之前，我先告诉你一件事： **WorkBuddy 内置模型的积分，其实完全可以不花。**

## 先花一分钟搞清楚：为什么能白嫖

首先，WorkBuddy 本质上是一个「壳」——它能操作你的文件、跑自动化任务、做各种各样的事情，但驱动这一切的是 AI 模型。

除非刻意为之，否则这类工具均是支持 BYOK(Bring Your Own Key) 的，也就是说，你可以使用你自己在其他平台申请的 API Key 来调用任何模型提供商的模型。

打个比方：内置模型是饭店招牌菜（收费），自定义模型是你自带食材让后厨加工（免费）。

原理就这么简单。

接下来的事，咱就把「食材」来源配好，端给 WorkBuddy 的「后厨」即可。

## 为 WorkBuddy 配置自定义模型

由于我日常使用 opencode 进行编码任务，所以这里以 opencode-zen 为例，其他平台大同小异。

[opencode-zen](https://link.zhihu.com/?target=https%3A//opencode.ai/go%3Fref%3DEXHCRPF6PS) 是 opencode 官方背书的模型提供商，除了收费模型外，它还提供了多个免费模型，注册后申请 API Key 就能用，不需要绑卡，也不需要付费。

值得关注的有两个：

- `deepseek-v4-flash-free` (**20260825 更新，当前已经没有了，请直接使用 [mimo-v2.5-free](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=mimo-v2.5-free&zhida_source=entity) 或者其他免费模型** ）：速度很快、支持工具调用和推理，日常办公够用
- `mimo-v2.5-free` ：除了工具调用和推理，还支持 [图片识别](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=%E5%9B%BE%E7%89%87%E8%AF%86%E5%88%AB&zhida_source=entity) ，以满足 [多模态](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=%E5%A4%9A%E6%A8%A1%E6%80%81&zhida_source=entity) 任务的需求

这两个模型基本上是常驻免费，有时 opencode-zen 还会定期免费开放开源旗舰模型，如 `minimax-m2.7-free` ，甚至闭源的 `qwen-3.7-max-free` ，非常大方。

### 第一步：注册，并创建 API Key

1. 浏览器打开 [opencode-zen](https://link.zhihu.com/?target=https%3A//opencode.ai/go%3Fref%3DEXHCRPF6PS) 页面
2. 注册账号 → 登录后进入「后台」
3. 找到 API Keys 页面，创建一个新 Key
4. 复制 Key
![](https://pica.zhimg.com/v2-89ea1526e644db793b5cfd0845a9a852_1440w.jpg)

在 opencode zen 申请并创建 API Key

### 第二步：打开 WorkBuddy 的配置页

1. 依次点击 WorkBuddy 左下角头像 →「配置」→「模型」
2. 再点击「本地 [配置文件](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6&zhida_source=entity) 」下的淡蓝色链接（models.json），之后会弹出 [操作系统](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F&zhida_source=entity) 默认的编辑器
![](https://pic3.zhimg.com/v2-84a649aa7d74f79d9e66ffd4f6c79d66_1440w.jpg)

点击这个 models.json 的淡蓝色链接

1. 在编辑器中添加以下内容，并将 `api-key` 替换为上方创建的 API Key
```
[
  {
    "id": "deepseek-v4-flash-free",
    "name": "zen:ds-v4-flash-free",
    "vendor": "Custom",
    "url": "https://opencode.ai/zen/v1",
    "apiKey": "api-key",
    "supportsToolCall": true,
    "supportsImages": false,
    "supportsReasoning": true,
    "useCustomProtocol": false
  },
  {
    "id": "mimo-v2.5-free",
    "name": "zen:mimo-v2.5-free",
    "vendor": "Custom",
    "url": "https://opencode.ai/zen/v1",
    "apiKey": "api-key",
    "supportsToolCall": true,
    "supportsImages": true,
    "supportsReasoning": true,
    "useCustomProtocol": false
  }
]
```
1. 保存，并关闭编辑器
2. 重新启动 WorkBuddy，即可看到新配置的模型出现在下拉列表中
![](https://pic2.zhimg.com/v2-6d35cfff7635fe0100fb074e79d0522d_1440w.jpg)

点击聊天窗口点击模型下拉菜单可以看到 zen 开头的两个新模型出现

### 第三步：测试并验证

之后可以新建一个对话，选择新配置的模型，验证是否生效。

![](https://pic2.zhimg.com/v2-3f92e06547cdb4105f0cc3d75a06c151_1440w.jpg)

新建对话窗口，确认模型正常工作且使用的是免费模型

## 以此类推，其他平台大同小异

学会了自己配置自定义模型后，其他的平台也按照同样的步骤操作即可，即：

1. 申请 API Key
2. 配置响应的模型信息
3. 重启、验证和测试

比如， [nousresearch.com](https://link.zhihu.com/?target=https%3A//portal.nousresearch.com/) 同样提供了免费模型接入，它也是 [Hermes](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=Hermes&zhida_source=entity) 的维护者，也是当下流行的 AI Agent 之一。

目前 [nous research](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=nous+research&zhida_source=entity) 提供了 `stepfun/step-3.7-flash:free` 模型，支持工具调用和推理，是性价比非常高的免费模型。

nous 要创建 API Key，需要先订阅一个 free 的 subscription 才行，因此你需要一张支持 [跨境支付](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=%E8%B7%A8%E5%A2%83%E6%94%AF%E4%BB%98&zhida_source=entity) 的银行卡。

创建 API Key 的步骤和 opencode-zen 类似，这里就不赘述了，也是在左侧菜单栏的 API Keys 页面创建。

当创建 API Key 后，可以使用如下的配置（记得替换 `<api-key>` ）：

```
[
  {
    "id": "stepfun/step-3.7-flash:free",
    "name": "nous:step-3.7-flash-free",
    "vendor": "Custom",
    "url": "https://inference-api.nousresearch.com/v1",
    "apiKey": "<api-key>",
    "supportsToolCall": true,
    "supportsImages": false,
    "supportsReasoning": true,
    "useCustomProtocol": false
  }
]
```

配置成功后，模型下拉菜单会出现 `nous:step-3.7-flash-free` 模型，选择它即可使用。

![](https://pic2.zhimg.com/v2-71f51b6913e7a88b8a6315abe5770ccf_1440w.jpg)

同样的，配置成功会出现 nous 开头的新模型

同样的，新建一个对话，选择新配置的模型，验证是否生效。

![](https://picx.zhimg.com/v2-fd16c865a7cfd75ccd562e421511099d_1440w.jpg)

类似地，新建对话窗口，确认模型正常工作且使用的是免费模型

以此类推，如果你未来再发现其他平台提供了免费模型，也可以按照同样的步骤配置。

当前这个时间点，国内主流的模型提供商均应算力紧缺，已经 **停止了注册即送大量免费 Token 的活动，而改为了赠送优惠券** 的方式，但优惠券的额度通常比较小，实际上并不划算。

而我这里提供的这两种方法，都是 **完全免费且常驻** 的 **高性价比** 模型，如果日常的使用场景没有太高级、太复杂的需求，是完全够用的。

## 万一出错了，别怕，这是避坑指南

### 坑一：URL 地址拼错

这些平台的接口地址末尾都是 `/v1` 。一个字不能多，也不能少，不然 404 报错（常见的错误是多一个 `/` ）。

### 坑二：模型名大小写

模型名字大小写敏感，我这里推荐的模型名字都是小写的。

### 坑三：配完没立刻生效

如果模型列表里没出现，完全退出 WorkBuddy 再打开（win 右下角任务栏右键退出，不是关窗口, [macos](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=macos&zhida_source=entity) 通过组合键 cmd + q 退出）。

## 也别忘记薅 WorkBuddy 官方的羊毛

可以直接访问 WorkBuddy 的 [成长计划](https://link.zhihu.com/?target=https%3A//www.workbuddy.cn/profile/growth-space) 页面，里面有一系列任务，完成即送 credits，虽然少，但总归是白来的。

而且这里的 **重点是，任务需要手动点击「开启」才会生效** ，不然是不会自动开始并完成的。

## 写在最后

Token 问题解决了，剩下的就是怎么把 WorkBuddy 真正用起来。

下一篇，我们聊聊 WorkBuddy 里最核心的三组概念——专家（Agent）、技能（Skill）、连接器（ [MCP](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=MCP&zhida_source=entity) ）。它们之间是什么关系、分别能干什么、以及怎么组合起来才能让你的 WorkBuddy 真正像个 AI Agent 助手而不仅仅是一个仅可以聊天的 [ChatBot](https://zhida.zhihu.com/search?content_id=277955961&content_type=Article&match_order=1&q=ChatBot&zhida_source=entity) 。

[![](https://pic1.zhimg.com/v2-ccb783f7d89083284e2a08505c515fd3.jpg?source=7e7ef6e2&needBackground=1)](https://zhuanlan.zhihu.com/p/2055586298791039959)

[![](https://picx.zhimg.com/v2-773496bfc074223d5bb8f1e3da7f53d5.jpg?source=7e7ef6e2&needBackground=1)](https://zhuanlan.zhihu.com/p/2056128137965069775)

还没有人送礼物，鼓励一下作者吧

编辑于 2026-08-25 16:22・日本[学云计算选哪个培训机构比较好?](https://www.zhihu.com/question/467047449/answer/1974162944758667060)

[你好，这里是汉码未来。首先得明确一点：我们从不说 “包就业”“毕业月薪过万” 这种空话。你有大数据大专的底子，其实对云计算里的数据分析、数据存储逻辑有基础铺垫，这是你的优势，...](https://www.zhihu.com/question/467047449/answer/1974162944758667060)

赞同 80