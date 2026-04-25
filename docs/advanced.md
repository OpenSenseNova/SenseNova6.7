# 🎓 进阶指南 — Hermes Agent / OpenClaw

本文档面向进阶用户，介绍 Hermes Agent / OpenClaw 的配置文件、模型切换、Bundled Skills，以及通过 OpenClaw 官方渠道独立部署等高级用法。

---

## ⚙️ 进阶配置

> 以下内容仅供进阶用户参考，常规使用无需关心。

### 配置文件位置

| 工具 | 主配置文件 | 环境变量文件 |
| --- | --- | --- |
| Hermes | `~/.hermes/config.yaml` | `~/.hermes/.env` |
| OpenClaw | `~/.openclaw/openclaw.json` | `~/.openclaw/.env` |

### 多模型供应商切换

两个工具均支持在配置文件中同时配置 OpenRouter、OpenAI、Anthropic 等多个模型供应商，运行时可按需切换。

### 预置技能（Bundled Skills）

安装包已内置数十个开箱即用的技能模块，涵盖文档处理、数据分析、网页爬取、飞书集成等场景，工具启动时自动加载，无需额外配置。

---

## 🦞 通过 OpenClaw 官方渠道安装

如需通过 [OpenClaw](https://openclaw.ai/) 官方渠道（不经 agent_pack）独立部署，请参考 [OpenClaw 官方教程](https://openclaw.ai/)。在其引导配置中选择 **OpenAI 兼容接口**，填入 SenseNova 的 Base URL、API Key 与模型名即可接入。

> 📝 **尚未申请 SenseNova API Key？** 请前往 [SenseNova 官网](https://console.sensecore.cn) 完成注册与实名认证，并在 **管理中心 → API-Key 管理** 中创建 Key。详细步骤参见主 README 的 [🚀 快速开始 → API Key 申请](../README.md#api-key-申请) 章节。

---

如有任何问题，欢迎前往 [GitHub Issues](https://github.com/SenseTime-FVG/agent_pack/issues) 与我们交流。

[← 返回主 README](../README.md)
