# ⚡ SenseNova 6.7 Flash

<p align="center">
  <img src="assets/logo.webp" alt="SenseNova Logo" height="120">
</p>

> 原生多模态的大模型，更懂办公，更省 token

**SenseNova 6.7 Flash** 是商汤日日新推出的面向真实工作流的轻量多模态智能体模型。采用原生多模态架构，兼顾效果与成本，能够稳定支撑数据分析、PPT 生成、深度调研报告生成、信息图生成等复杂长链路办公任务。

---

## ✨ 核心能力

### 🖼️ 1. 原生多模态，更懂企业需求

覆盖数据分析、PPT、深度调研、信息图等核心办公场景，原生多模态架构稳定支撑。

### 🔗 2. 一次输入，走得更远

支持多步骤执行与跨步骤结果整合，从理解任务到生成最终产物形成完整闭环。

### 💰 3. Token 消耗立省 60%

在复杂任务中显著降低 token 消耗，提高单位成本下的可交付产出，让长链路任务成本更可控。

> **说明：** 60% 节省数据基于典型复杂办公任务（多步骤、跨模态、长上下文）的实测对比，实际节省幅度因任务类型而异。

---

## 📊 性能评测

<p align="center">
  <img src="assets/benchmark.webp" alt="Benchmark Results" width="100%">
</p>

---

## 🔥 场景 Showcase

### 🏢 一体化智能办公闭环

以半导体存储市场行业分析为例，模型完整覆盖从数据洞察、行业研究到内容交付的全流程：

**数据洞察 → 行业研究 → 内容交付**

<p align="center">
  <img src="assets/office_workflow.webp" alt="Integrated Office Workflow" width="80%">
</p>

### 📋 覆盖核心办公场景

| 场景 | 能力 |
|------|------|
| 数据分析 | 读取表格、提炼结论、生成图表 |
| PPT 生成 | 结构化内容 → 可交付幻灯片 |
| 深度调研 | 多步信息整合 → 完整研究报告 |
| 信息图生成 | 排版丰富的像素级信息图表 |

<!-- TODO: 插入各场景演示图/GIF -->

---

## 🚀 快速开始

### API Key 申请

1. 注册并完成实名认证：[https://console.sensecore.cn](https://console.sensecore.cn)
2. 进入控制台左侧导航：**管理中心 → API-Key 管理 → 创建 API-Key**，复制并妥善保存（仅创建时展示一次）
3. 设置环境变量：
   ```bash
   export SENSENOVA_API_KEY="your_api_key_here"
   ```

### 基础调用

```bash
pip install openai
```

**Python（OpenAI SDK）**

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["SENSENOVA_API_KEY"],
    base_url="https://token.sensenova.cn/v1",
)

completion = client.chat.completions.create(
    model="SenseNova-V6.7-Flash",
    max_tokens=2000,
    messages=[{"role": "user", "content": "你好，简单介绍一下你自己"}],
)
print(completion.choices[0].message.content)
```

**curl**

```bash
curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H "Authorization: Bearer $SENSENOVA_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "SenseNova-V6.7-Flash",
    "max_tokens": 2000,
    "messages": [{"role": "user", "content": "你好，简单介绍一下你自己"}]
  }'
```

### 多模态（图片输入）

```python
completion = client.chat.completions.create(
    model="SenseNova-V6.7-Flash",
    max_tokens=2000,
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "图中是什么？"},
            {"type": "image_url", "image_url": {"url": "https://example.com/photo.jpg"}},
        ],
    }],
)
print(completion.choices[0].message.content)
```

### 流式输出

```python
stream = client.chat.completions.create(
    model="SenseNova-V6.7-Flash",
    max_tokens=2000,
    stream=True,
    messages=[{"role": "user", "content": "写一首关于春天的诗"}],
)
for chunk in stream:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

---

## 🤖 在开源 Agent 框架中使用

SenseNova 6.7 Flash 兼容 OpenAI API，可无缝接入主流开源 Agent 框架。

### 🤖 NanoBot

[NanoBot](https://github.com/HKUDS/nanobot) 是一个轻量级本地 AI Agent 框架，支持通过配置文件切换任意 OpenAI 兼容的模型接口。

```bash
pip install nanobot-ai
nanobot onboard
```

在 `~/.nanobot/config.json` 中配置 SenseNova 6.7 Flash：

```json
{
  "providers": {
    "sensenova": {
      "apiKey": "YOUR_API_KEY",
      "baseURL": "https://token.sensenova.cn/v1"
    }
  },
  "agents": {
    "defaults": {
      "provider": "sensenova",
      "model": "SenseNova-V6.7-Flash"
    }
  }
}
```

```bash
nanobot agent   # 启动 Agent 对话
```

---

### 🦞 OpenClaw

[OpenClaw](https://openclaw.ai/) 是一个本地运行的个人 AI 助理框架，可自主处理邮件、日历、浏览器自动化等真实办公任务，支持接入自定义模型。

```bash
# macOS / Linux
curl -fsSL https://openclaw.ai/install.sh | bash

# 或通过 npm
npm i -g openclaw
```

启动引导配置，选择"自定义 OpenAI 兼容接口"并填入 SenseNova 的 endpoint 与 API Key：

```bash
openclaw onboard
# 在交互式配置中选择 Custom OpenAI-compatible API
# Base URL: https://token.sensenova.cn/v1
# API Key:  your_api_key_here
# Model:    SenseNova-V6.7-Flash
```

完成配置后即可通过 OpenClaw 的技能系统调用 SenseNova 6.7 Flash 执行办公任务。

---

## 💎 Token Plan

**SenseNova Token Plan** 面向复杂知识工作与办公生产场景，不止"用得便宜"，更要"用得安心"——更高效的精选模型配合更充裕的用量保障，真正做到长任务也能放心跑。

| 特性 | 说明 |
|------|------|
| 更省 | 复杂任务 token 消耗大幅降低，提升单位成本交付产出 |
| 更强 | 完成跨步骤、跨模态、跨页面的信息处理与内容生成 |
| 更稳 | 面向 Office 场景优化，结果贴近真实工作流 |
| 更完整 | 从"理解任务"到"生成最终产物"形成闭环 |

<p align="center">
  <img src="assets/token_plan.webp" alt="Token Plan" width="100%">
</p>

> 最新 SenseNova 6.7 Flash 模型与全系 cowork-skill 均已加入**小浣熊 Pro 套餐**，提供企业级安全防护与开箱即用的丝滑体验。<!-- TODO: 插入小浣熊产品页链接 -->

<!-- TODO: 插入 Token Plan 购买/了解更多链接 -->

---

## 📚 相关链接

- 官网：<!-- TODO -->
- API 文档：<!-- TODO -->
- 技术报告：<!-- TODO -->
