# 🎓 Advanced Guide — Hermes Agent / OpenClaw

🌐 **English** | [中文](advanced_CN.md)

This guide is aimed at advanced users. It covers Hermes Agent / OpenClaw config files, multi-model switching, bundled skills, and standalone deployment via the official OpenClaw channel.

---

## ⚙️ Advanced Configuration

> The following is for advanced users only — regular usage doesn't require any of this.

### Config file locations

| Tool | Main config file | Environment file |
| --- | --- | --- |
| Hermes | `~/.hermes/config.yaml` | `~/.hermes/.env` |
| OpenClaw | `~/.openclaw/openclaw.json` | `~/.openclaw/.env` |

### Multiple model providers

Both tools let you configure several model providers (OpenRouter, OpenAI, Anthropic, …) side by side in their config files and switch between them at runtime as needed.

### Bundled Skills

The installer ships with dozens of out-of-the-box skill modules — covering document processing, data analysis, web scraping, Feishu integration, and more. They load automatically when the tool starts; no extra configuration required.

---

## 🦞 Installing via the official OpenClaw channel

If you prefer to deploy [OpenClaw](https://openclaw.ai/) standalone (without `agent_pack`), follow the [official OpenClaw guide](https://openclaw.ai/). In the setup wizard, choose the **OpenAI-compatible endpoint** option and supply SenseNova's Base URL, API Key, and model name.

> 📝 **Don't have a SenseNova API key yet?** Go to the [SenseNova portal](https://console.sensecore.cn), complete registration and identity verification, and create a key under **Management Center → API Key Management**. Detailed steps are in the main README's [🚀 Quick Start → Apply for an API Key](../README.md#apply-for-an-api-key) section.

---

If you have any questions, drop by [GitHub Issues](https://github.com/SenseTime-FVG/agent_pack/issues) and chat with us.

[← Back to main README](../README.md)
