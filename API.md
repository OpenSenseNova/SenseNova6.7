# SenseNova LLM API Integration Guide

🌐 **English** | [中文](API_CN.md)

This document explains how to integrate the SenseNova LLM API on SenseTime's SenseCore platform.

---

## Table of Contents

- [1. Sign up and obtain an API Key](#1-sign-up-and-obtain-an-api-key)
- [2. Using with Open-Source Agent Frameworks](#2-using-with-open-source-agent-frameworks)
- [3. Models](#3-models)
- [4. Basic invocation](#4-basic-invocation)
- [5. Recommended sampling parameters](#5-recommended-sampling-parameters)
- [6. Multi-turn dialogue](#6-multi-turn-dialogue)
- [7. Image (multimodal) input](#7-image-multimodal-input)
- [8. Streaming output](#8-streaming-output)
- [9. Calling via the OpenAI SDK](#9-calling-via-the-openai-sdk)
- [10. Error codes](#10-error-codes)

---

## 1. Sign up and obtain an API Key

### 1.1 Sign up

Visit the SenseCore portal and complete registration plus identity verification:

```
https://console.sensecore.cn
```

### 1.2 Enter AI Studio

Once logged in, browse SenseNova-family models in the AI Studio plaza:

```
https://console.sensecore.cn/cn-sh-01/aistudio/plaza
```

### 1.3 Create an API Key

In the console sidebar: **Management Center → API Key Management → Create API Key**.

After creation, **copy and store the key immediately** — the full value is shown only once. If a key leaks, delete or disable it on the same page and create a new one.

In the examples below, replace every `<YOUR_API_KEY>` with the key you created.

---

## 2. Using with Open-Source Agent Frameworks

SenseNova 6.7 Flash-Lite needs an **agent runtime** + the **official skill library** to deliver an end-to-end office-task workflow.

- **Recommended runtime**: **[OpenClaw](https://openclaw.ai/)** or **[hermes-agent](https://github.com/NousResearch/hermes-agent)**.
- **Recommended LLM**: pair it with the **[SenseNova platform API](https://platform.sensenova.cn/token-plan)** — use the API Key from Section 1 (free token plan available).
- **Install & setup**: see [SenseNova-Skills INSTALL.md](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/INSTALL.md).

### 2.1 Installing SenseNova-Skills

**Recommended: just ask the agent to install them for you.** Hand it the repo URL and let it clone and copy the contents into the right directory, e.g.:

> *"Please install https://github.com/OpenSenseNova/SenseNova-Skills into your skills directory."*

After installation you may need to **restart the agent service manually** before the new skills are picked up.

| Agent | Target directory |
|-------|------------------|
| [OpenClaw](https://openclaw.ai/) | `~/.openclaw/skills/` |
| [hermes-agent](https://github.com/NousResearch/hermes-agent) | `~/.hermes/skills/` |

<details>
<summary>Prefer to install manually?</summary>

Clone the repo, then copy (or symlink) the subdirectories under `skills/` into the target directory:

```bash
git clone https://github.com/OpenSenseNova/SenseNova-Skills.git --depth=1
mkdir -p ~/.openclaw/skills
cp -r SenseNova-Skills/skills/* ~/.openclaw/skills/
```

For Hermes, just swap the directory to `~/.hermes/skills/`.

</details>

---

## 3. Models

**SenseNova 6.7 Flash-Lite** — a lightweight multimodal agent model built for real-world workflows.

- **Lightweight & efficient**, balancing quality, cost, and deployability
- **Office-tuned**, reliably powering complex long-horizon tasks
- **Native multimodal architecture**, well suited to real office content
- **Better token efficiency**, keeping complex tasks affordable

---

## 4. Basic invocation

### 4.1 curl

```bash
curl --location 'https://token.sensenova.cn/v1/chat/completions' \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer <YOUR_API_KEY>' \
  --data '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [
      {"role": "user", "content": "Hi, please briefly introduce yourself."}
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
        "Authorization": f"Bearer {API_KEY}",   # Bearer token auth
        "Content-Type": "application/json",
    },
    json={
        "model": "sensenova-6.7-flash-lite",
        "max_tokens": 2000,                     # Max output tokens (incl. reasoning)
        "messages": [
            {"role": "user", "content": "Hi, please briefly introduce yourself."},
        ],
    },
    timeout=60,
)
resp.raise_for_status()
data = resp.json()
print(data["choices"][0]["message"])
```

### 4.3 Typical response shape

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
      "content": "Hi! I'm SenseNova...",
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

- `finish_reason`: `stop` for normal completion, `length` if `max_tokens` is hit.
- `message.content`: the final answer.
- `message.reasoning`: the chain of thought from a reasoning model.
- `total_tokens = prompt_tokens + completion_tokens`. The upper bound is the model's context window — there is no request parameter to cap it directly.

---

## 5. Recommended sampling parameters

We suggest the following parameter combinations by mode and task type:

| Mode | Task type | `temperature` | `top_p` | `top_k` | `min_p` | `presence_penalty` | `repetition_penalty` |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Thinking mode | General | 1.0 | 0.95 | 20 | 0.0 | 1.5 | 1.0 |
| Thinking mode | Precise coding (e.g. WebDev) | 0.6 | 0.95 | 20 | 0.0 | 0.0 | 1.0 |
| Instruct (non-thinking) mode | General | 0.7 | 0.8 | 20 | 0.0 | 1.5 | 1.0 |
| Instruct (non-thinking) mode | Reasoning | 1.0 | 1.0 | 40 | 0.0 | 2.0 | 1.0 |

Example — "general task + thinking mode":

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
      {"role": "user", "content": "Write a poem about spring."}
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
        {"role": "user", "content": "Write a poem about spring."},
    ],
})
print(resp.json()["choices"][0]["message"]["content"])
```

---

## 6. Multi-turn dialogue

### 6.1 curl

```bash
curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <YOUR_API_KEY>' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [
      {"role": "system",    "content": "You are a concise assistant. Reply in 20 words or fewer."},
      {"role": "user",      "content": "What is the capital of France?"},
      {"role": "assistant", "content": "Paris."},
      {"role": "user",      "content": "And Germany?"}
    ]
  }'
```

### 6.2 Python

```python
# Maintain dialogue history: append in chronological order.
history = [
    {"role": "system", "content": "You are a concise assistant. Reply in 20 words or fewer."},
]

def chat(user_msg: str) -> str:
    history.append({"role": "user", "content": user_msg})
    resp = requests.post(URL, headers=headers, timeout=60, json={
        "model": "sensenova-6.7-flash-lite",
        "max_tokens": 2000,
        "messages": history,
    })
    reply = resp.json()["choices"][0]["message"].get("content", "")
    # When echoing history back, only keep `content` — never include `reasoning`.
    history.append({"role": "assistant", "content": reply})
    return reply

print(chat("What is the capital of France?"))   # -> Paris.
print(chat("And Germany?"))                     # -> Berlin.
```

**Notes**:
- `role` may be `system`, `user`, or `assistant`.
- When echoing the previous reply, **include only `content`** — do not echo back `reasoning`.
- Multi-turn significantly increases `prompt_tokens`. For long histories, summarize or truncate.

---

## 7. Image (multimodal) input

SenseNova 6.7 Flash-Lite accepts images in the OpenAI Vision-compatible format, supporting both URL and Base64.

### 7.1 Via URL (curl)

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
        {"type": "text",  "text": "What is in the image?"},
        {"type": "image_url", "image_url": {
          "url": "https://example.com/photo.jpg"
        }}
      ]
    }]
  }'
```

> The server downloads the URL — the image must be anonymously accessible. If unreachable, you'll get an `image down failed` error; switch to Base64 or upload to an accessible object store.

### 7.2 Via URL (Python)

```python
resp = requests.post(URL, headers=headers, timeout=120, json={
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{
        "role": "user",
        "content": [
            {"type": "text",  "text": "What is in the image?"},
            {"type": "image_url", "image_url": {
                "url": "https://example.com/photo.jpg",
            }},
        ],
    }],
})
print(resp.json()["choices"][0]["message"]["content"])
```

### 7.3 Via Base64 (curl)

```bash
# Build a Data URL (Linux/macOS).
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
        {"type": "text", "text": "Describe this image."},
        {"type": "image_url", "image_url": {
          "url": "data:image/png;base64,'"${B64}"'"
        }}
      ]
    }]
  }'
```

### 7.4 Via Base64 (Python)

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
            {"type": "text",  "text": "Describe this image."},
            {"type": "image_url", "image_url": {
                "url": to_data_url("./photo.png"),
            }},
        ],
    }],
})
print(resp.json()["choices"][0]["message"]["content"])
```

### 7.5 Multi-image input (Python)

Append additional `image_url` objects within the same message's `content` array:

```python
resp = requests.post(URL, headers=headers, timeout=120, json={
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{
        "role": "user",
        "content": [
            {"type": "text",      "text": "Compare the differences between the two images."},
            {"type": "image_url", "image_url": {"url": "https://example.com/a.jpg"}},
            {"type": "image_url", "image_url": {"url": "https://example.com/b.jpg"}},
        ],
    }],
})
```

**Image guidelines**:

- Common formats are supported (PNG, JPEG, …).
- Compress to a long edge of at most 2048 px before upload to save tokens and latency.
- Prefer URL for large images; Base64 inflates the request body considerably.

---

## 8. Streaming output

Set `stream` to `true` and the server pushes incremental results via Server-Sent Events (SSE).

### 8.1 curl

```bash
curl -N 'https://token.sensenova.cn/v1/chat/completions' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <YOUR_API_KEY>' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "stream": true,
    "max_tokens": 2000,
    "messages": [{"role": "user", "content": "Write a poem about spring."}]
  }'
```

Event-stream excerpt:

```
data: {"choices":[{"delta":{"reasoning":"Thinking"},"finish_reason":""}], ...}
data: {"choices":[{"delta":{"content":"Spring"},"finish_reason":""}], ...}
data: {"choices":[{"delta":{"content":" wind"},"finish_reason":""}], ...}
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
    "messages": [{"role": "user", "content": "Write a poem about spring."}],
}) as r:
    for line in r.iter_lines(decode_unicode=True):
        if not line or not line.startswith("data:"):
            continue
        payload = line[5:].strip()
        if payload == "[DONE]":          # End-of-stream marker
            break

        chunk = json.loads(payload)

        # Before finishing, the server pushes a usage-only event (choices is empty).
        if not chunk.get("choices"):
            print("\n[usage]", chunk.get("usage"))
            continue

        delta = chunk["choices"][0].get("delta", {})

        # `delta` may carry `reasoning` (thinking) or `content` (final text).
        # Most front-ends only display `content` and ignore `reasoning`.
        if "content" in delta:
            print(delta["content"], end="", flush=True)
```

**Key points**:

- Each event begins with `data: ` and is separated by a blank line.
- `delta` may contain `reasoning` (incremental thinking) or `content` (incremental output). Reasoning models emit a large amount of `reasoning` before producing `content`.
- Just before completion, the server pushes a single usage-only event (`choices: []`).
- Receiving `data: [DONE]` indicates the stream is finished — clients should stop reading.

---

## 9. Calling via the OpenAI SDK

The endpoint is compatible with OpenAI's Chat Completions protocol, so you can use `openai-python` directly:

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
    messages=[{"role": "user", "content": "Hello"}],
)
print(completion.choices[0].message.content)
```

Streaming:

```python
stream = client.chat.completions.create(
    model="sensenova-6.7-flash-lite",
    max_tokens=2000,
    stream=True,
    messages=[{"role": "user", "content": "Write a poem"}],
)
for chunk in stream:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

> When using the SDK, the `reasoning` field may not be exposed on the canonical objects. To retrieve it, call the raw HTTP endpoint directly.

---

## 10. Error codes

Error response shape:

```json
{
  "error": {
    "message": "model is not found",
    "type": "not_found_error",
    "code": "5"
  }
}
```

| HTTP | error.type | Meaning | Suggested action |
| --- | --- | --- | --- |
| 400 | `invalid_request_error` | Malformed parameters, e.g. image download failure | Check parameter shape and image URL accessibility |
| 401 | `authentication_error` | API key invalid or expired | Recreate the key in the console |
| 403 | — | Lacking permission or risk-blocked | Check account permissions and content compliance |
| 404 | `not_found_error` | Model or endpoint does not exist | Double-check the spelling of `model` |
| 429 | — | Rate limited | Retry with exponential backoff |
| 5xx | — | Server-side issue | Retry later; if persistent, file a ticket |

---

For further support, sign in to the [SenseCore console](https://console.sensecore.cn) to file a ticket or browse the latest official docs.
