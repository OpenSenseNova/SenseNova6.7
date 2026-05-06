# SenseNova 大模型 API 接入文档

🌐 [English](API.md) | **中文**

本文档介绍如何接入商汤大装置（SenseCore）SenseNova 大模型 API。

---

## 目录

- [1. 注册账号与获取 API Key](#1-注册账号与获取-api-key)
- [2. 在 Agent 框架中使用](#2-在-agent-框架中使用)
- [3. 模型说明](#3-模型说明)
- [4. 基础调用](#4-基础调用)
- [5. 推荐采样参数](#5-推荐采样参数)
- [6. 多轮对话](#6-多轮对话)
- [7. 图片（多模态）输入](#7-图片多模态输入)
- [8. 流式输出](#8-流式输出)
- [9. 使用 OpenAI SDK 调用](#9-使用-openai-sdk-调用)
- [10. 错误码](#10-错误码)

---

## 1. 注册账号与获取 API Key

### 1.1 注册账号

访问大装置官网完成注册及实名认证：

```
https://console.sensecore.cn
```

### 1.2 进入 AI Studio

登录后访问 AI Studio 广场页，可浏览 SenseNova 系列模型：

```
https://console.sensecore.cn/cn-sh-01/aistudio/plaza
```

### 1.3 创建 API Key

控制台左侧导航：**管理中心 → API-Key 管理 → 创建 API-Key**

创建成功后请**立即复制并妥善保管**，API Key 仅在创建时完整显示一次。如发生泄漏请立即在同一页面删除或禁用并重建。

后续示例中所有 `<YOUR_API_KEY>` 均替换为你创建的 Key。

---

## 2. 在 Agent 框架中使用

SenseNova 6.7 Flash-Lite 需要与 **Agent 运行时** + **官方技能库** 协同工作，才能跑通完整的办公任务闭环。

- **推荐运行时**：**[OpenClaw](https://openclaw.ai/)** 或 **[hermes-agent](https://github.com/NousResearch/hermes-agent)**。
- **推荐 LLM**：配合 **[SenseNova 平台 API](https://platform.sensenova.cn/token-plan)** 使用——使用本文第 1 节获取的 API Key（提供免费 token 套餐）。
- **安装与配置**：详见 [SenseNova-Skills INSTALL_CN.md](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/INSTALL_CN.md)。

### 2.1 安装 SenseNova-Skills

**推荐做法：直接让 agent 帮你装好这些 skill。** 把仓库地址交给它，让它自己克隆并把内容拷贝到目标目录，例如：

> *“请帮我把 https://github.com/OpenSenseNova/SenseNova-Skills 安装到你的 skills 目录。”*

安装完成后，**可能需要手动重启 agent 服务**，新 skill 才会被加载。

| 智能体 | 目标目录 |
|--------|---------|
| [OpenClaw](https://openclaw.ai/) | `~/.openclaw/skills/` |
| [hermes-agent](https://github.com/NousResearch/hermes-agent) | `~/.hermes/skills/` |

<details>
<summary>想手动安装？</summary>

克隆本仓库，然后把 `skills/` 下的子目录自行复制（或软链接）到目标目录：

```bash
git clone https://github.com/OpenSenseNova/SenseNova-Skills.git --depth=1
mkdir -p ~/.openclaw/skills
cp -r SenseNova-Skills/skills/* ~/.openclaw/skills/
```

Hermes 把目录换成 `~/.hermes/skills/` 即可。

</details>

---

## 3. 模型说明

**SenseNova 6.7 Flash-Lite** —— 面向真实工作流的轻量多模态智能体模型。

- **轻量高效**，兼顾效果、成本与落地性
- **办公场景增强**，稳定支撑复杂长链路任务
- **原生多模态架构**，适合真实办公内容处理
- **Token 效率更优**，复杂任务成本更可控

---

## 4. 基础调用

### 4.1 curl

```bash
curl --location 'https://token.sensenova.cn/v1/chat/completions' \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer <YOUR_API_KEY>' \
  --data '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [
      {"role": "user", "content": "你好，简单介绍一下你自己"}
    ]
  }'
```

### 4.2 Python

```python
import os
import requests

API_KEY = os.environ["SENSENOVA_API_KEY"]
URL = "https://token.sensenova.cn/v1/chat/completions"

resp = requests.post(
    URL,
    headers={
        "Authorization": f"Bearer {API_KEY}",   # Bearer Token 鉴权
        "Content-Type": "application/json",
    },
    json={
        "model": "sensenova-6.7-flash-lite",
        "max_tokens": 2000,                     # 最大输出 tokens（含 reasoning）
        "messages": [
            {"role": "user", "content": "你好，简单介绍一下你自己"},
        ],
    },
    timeout=60,
)
resp.raise_for_status()
data = resp.json()
print(data["choices"][0]["message"])
```

### 4.3 典型响应结构

```json
{
  "id": "da48c12a-...",
  "request_id": "da48c12a-...",
  "model": "sensenova-6.7-flash-lite",
  "object": "chat.completion",
  "created": 1776952631,
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "你好！我是 SenseNova...",
      "reasoning": "Thinking Process: ..."
    },
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 37,
    "completion_tokens": 762,
    "total_tokens": 799,
    "prompt_tokens_details": {"cached_tokens": 0, "audio_tokens": 0}
  }
}
```

- `finish_reason`：`stop` 正常结束 / `length` 达到 `max_tokens` 限制
- `message.content`：最终回答正文
- `message.reasoning`：推理模型的思考过程
- `total_tokens = prompt_tokens + completion_tokens`，上限由模型上下文窗口决定，无请求参数可直接限制

---

## 5. 推荐采样参数

建议根据模式和任务类型选择以下采样参数组合：

| 模式 | 任务类型 | `temperature` | `top_p` | `top_k` | `min_p` | `presence_penalty` | `repetition_penalty` |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 思考模式 | 通用任务 | 1.0 | 0.95 | 20 | 0.0 | 1.5 | 1.0 |
| 思考模式 | 精确编码（如 WebDev） | 0.6 | 0.95 | 20 | 0.0 | 0.0 | 1.0 |
| 指令（非思考）模式 | 通用任务 | 0.7 | 0.8 | 20 | 0.0 | 1.5 | 1.0 |
| 指令（非思考）模式 | 推理任务 | 1.0 | 1.0 | 40 | 0.0 | 2.0 | 1.0 |

以“通用任务 + 思考模式”为例：

```bash
curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <YOUR_API_KEY>' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "temperature": 1.0,
    "top_p": 0.95,
    "top_k": 20,
    "min_p": 0.0,
    "presence_penalty": 1.5,
    "repetition_penalty": 1.0,
    "messages": [
      {"role": "user", "content": "写一首关于春天的诗"}
    ]
  }'
```

```python
resp = requests.post(URL, headers=headers, timeout=60, json={
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "temperature": 1.0,
    "top_p": 0.95,
    "top_k": 20,
    "min_p": 0.0,
    "presence_penalty": 1.5,
    "repetition_penalty": 1.0,
    "messages": [
        {"role": "user", "content": "写一首关于春天的诗"},
    ],
})
print(resp.json()["choices"][0]["message"]["content"])
```

---

## 6. 多轮对话

### 6.1 curl

```bash
curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <YOUR_API_KEY>' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [
      {"role": "system",    "content": "你是一个简洁的助手，回答不超过 20 个字。"},
      {"role": "user",      "content": "法国的首都是？"},
      {"role": "assistant", "content": "巴黎。"},
      {"role": "user",      "content": "那德国呢？"}
    ]
  }'
```

### 6.2 Python

```python
# 维护对话历史：按时间顺序依次 append
history = [
    {"role": "system", "content": "你是一个简洁的助手，回答不超过 20 个字。"},
]

def chat(user_msg: str) -> str:
    history.append({"role": "user", "content": user_msg})
    resp = requests.post(URL, headers=headers, timeout=60, json={
        "model": "sensenova-6.7-flash-lite",
        "max_tokens": 2000,
        "messages": history,
    })
    reply = resp.json()["choices"][0]["message"].get("content", "")
    # 回传历史时只保留 content，不要把 reasoning 放进去
    history.append({"role": "assistant", "content": reply})
    return reply

print(chat("法国的首都是？"))   # -> 巴黎。
print(chat("那德国呢？"))       # -> 柏林。
```

**注意**：
- `role` 支持 `system`、`user`、`assistant`。
- 回传上一轮回复时**只包含 `content`**，无需回传 `reasoning`。
- 多轮会显著增加 `prompt_tokens` 消耗，建议对过长历史做摘要或截断。

---

## 7. 图片（多模态）输入

SenseNova 6.7 Flash-Lite 支持 OpenAI Vision 兼容格式的图片输入，提供 URL 与 Base64 两种传入方式。

### 7.1 通过 URL（curl）

```bash
curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <YOUR_API_KEY>' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{
      "role": "user",
      "content": [
        {"type": "text",  "text": "图中是什么？"},
        {"type": "image_url", "image_url": {
          "url": "https://example.com/photo.jpg"
        }}
      ]
    }]
  }'
```

> 服务端会主动下载该 URL，请确保图片可匿名公开访问。无法访问时返回 `image down failed` 错误，此时请改用 Base64 或上传到可访问的对象存储。

### 7.2 通过 URL（Python）

```python
resp = requests.post(URL, headers=headers, timeout=120, json={
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{
        "role": "user",
        "content": [
            {"type": "text",  "text": "图中是什么？"},
            {"type": "image_url", "image_url": {
                "url": "https://example.com/photo.jpg",
            }},
        ],
    }],
})
print(resp.json()["choices"][0]["message"]["content"])
```

### 7.3 通过 Base64（curl）

```bash
# 先生成 Data URL（Linux/macOS）
B64=$(base64 -w 0 photo.png 2>/dev/null || base64 -i photo.png)

curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <YOUR_API_KEY>' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{
      "role": "user",
      "content": [
        {"type": "text", "text": "描述这张图片"},
        {"type": "image_url", "image_url": {
          "url": "data:image/png;base64,'"${B64}"'"
        }}
      ]
    }]
  }'
```

### 7.4 通过 Base64（Python）

```python
import base64
import mimetypes
import requests

def to_data_url(path: str) -> str:
    mime = mimetypes.guess_type(path)[0] or "image/png"
    with open(path, "rb") as f:
        b64 = base64.b64encode(f.read()).decode()
    return f"data:{mime};base64,{b64}"

resp = requests.post(URL, headers=headers, timeout=120, json={
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{
        "role": "user",
        "content": [
            {"type": "text",  "text": "描述这张图片"},
            {"type": "image_url", "image_url": {
                "url": to_data_url("./photo.png"),
            }},
        ],
    }],
})
print(resp.json()["choices"][0]["message"]["content"])
```

### 7.5 多图输入（Python）

在同一条消息的 `content` 数组中追加多个 `image_url` 对象即可：

```python
resp = requests.post(URL, headers=headers, timeout=120, json={
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{
        "role": "user",
        "content": [
            {"type": "text",      "text": "对比以下两张图的异同"},
            {"type": "image_url", "image_url": {"url": "https://example.com/a.jpg"}},
            {"type": "image_url", "image_url": {"url": "https://example.com/b.jpg"}},
        ],
    }],
})
```

**图片使用建议**：

- 支持常见图片格式（PNG、JPEG 等）
- 上传前建议压缩至长边不超过 2048 像素，节省 tokens 与延迟
- 大图优先使用 URL 方式；Base64 会显著增大请求体

---

## 8. 流式输出

将 `stream` 设为 `true`，服务端以 Server-Sent Events（SSE）推送增量结果。

### 8.1 curl

```bash
curl -N 'https://token.sensenova.cn/v1/chat/completions' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <YOUR_API_KEY>' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "stream": true,
    "max_tokens": 2000,
    "messages": [{"role": "user", "content": "写一首关于春天的诗"}]
  }'
```

事件流格式（节选）：

```
data: {"choices":[{"delta":{"reasoning":"Thinking"},"finish_reason":""}], ...}
data: {"choices":[{"delta":{"content":"春"},"finish_reason":""}], ...}
data: {"choices":[{"delta":{"content":"风"},"finish_reason":""}], ...}
...
data: {"choices":[{"delta":{},"finish_reason":"stop"}], ...}
data: {"choices":[], "usage":{"prompt_tokens":38,"completion_tokens":441,"total_tokens":479}}
data: [DONE]
```

### 8.2 Python

```python
import json
import requests

with requests.post(URL, headers=headers, stream=True, timeout=120, json={
    "model": "sensenova-6.7-flash-lite",
    "stream": True,
    "max_tokens": 2000,
    "messages": [{"role": "user", "content": "写一首关于春天的诗"}],
}) as r:
    for line in r.iter_lines(decode_unicode=True):
        if not line or not line.startswith("data:"):
            continue
        payload = line[5:].strip()
        if payload == "[DONE]":          # 流式结束标识
            break

        chunk = json.loads(payload)

        # 结束前会推送一条仅含 usage 的事件（choices 为空数组）
        if not chunk.get("choices"):
            print("\n[usage]", chunk.get("usage"))
            continue

        delta = chunk["choices"][0].get("delta", {})

        # delta 可能包含 reasoning（思考）或 content（正文）
        # 一般前端只展示 content，忽略 reasoning
        if "content" in delta:
            print(delta["content"], end="", flush=True)
```

**要点**：

- 每条事件以 `data: ` 开头，空行分隔
- `delta` 字段可能包含 `reasoning`（推理增量）或 `content`（正文增量），推理模型会先输出大量 `reasoning` 再输出 `content`
- 结束前会单独推送一条仅含 `usage` 的事件（`choices: []`）
- 收到 `data: [DONE]` 表示结束，客户端应停止读取

---

## 9. 使用 OpenAI SDK 调用

接口兼容 OpenAI Chat Completions 协议，可直接使用 `openai-python`：

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["SENSENOVA_API_KEY"],
    base_url="https://token.sensenova.cn/v1",
)

completion = client.chat.completions.create(
    model="sensenova-6.7-flash-lite",
    max_tokens=2000,
    temperature=0.7,
    messages=[{"role": "user", "content": "你好"}],
)
print(completion.choices[0].message.content)
```

流式：

```python
stream = client.chat.completions.create(
    model="sensenova-6.7-flash-lite",
    max_tokens=2000,
    stream=True,
    messages=[{"role": "user", "content": "写一首诗"}],
)
for chunk in stream:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

> 使用 SDK 时，`reasoning` 字段可能未在标准对象上暴露。如需获取，请直接使用原始 HTTP 接口。

---

## 10. 错误码

错误响应结构：

```json
{
  "error": {
    "message": "model is not found",
    "type": "not_found_error",
    "code": "5"
  }
}
```

| HTTP | error.type | 说明 | 处理建议 |
| --- | --- | --- | --- |
| 400 | `invalid_request_error` | 请求参数非法，例如图片下载失败 | 检查参数结构、图片 URL 可访问性 |
| 401 | `authentication_error` | API Key 无效或已失效 | 控制台重新创建 Key |
| 403 | — | 无权限或被风控 | 检查账户权限与内容合规 |
| 404 | `not_found_error` | 模型或接口不存在 | 确认 `model` 字段拼写 |
| 429 | — | 触发限流 | 指数退避重试 |
| 5xx | — | 服务端异常 | 稍后重试；如持续可提交工单 |

---

如需进一步支持，请登录 [大装置控制台](https://console.sensecore.cn) 提交工单或查阅最新官方文档。
