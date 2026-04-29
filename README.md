# ⚡ SenseNova 6.7 Flash-Lite

🌐 **English** | [中文](README_CN.md)

<p align="center">
  <a href="https://platform.sensenova.cn/"><img src="assets/logo.webp" alt="SenseNova Logo" height="120"></a>
</p>

<p align="center">
  <a href="https://platform.sensenova.cn/"><b>🌐 Official Website — platform.sensenova.cn</b></a>
</p>

> A natively multimodal LLM, better tuned for office work and more token-efficient.

**SenseNova 6.7 Flash-Lite** is SenseTime's lightweight multimodal agent model, purpose-built for real-world workflows. With a native multimodal architecture that balances quality and cost, it reliably powers complex long-horizon office tasks such as data analysis, slide deck generation, deep research reports, and infographic creation.

---

## ✨ Core Capabilities

The latest natively multimodal model from SenseTime — capable of complex office tasks including data analysis, deep research, sophisticated image understanding, and PPT generation. Token Plan is now available: faster, better, cheaper.

### 🖼️ 1. Native Multimodal Agent

Equip your agent with native vision so it can share your "field of view".

### 🔗 2. Tuned for Enterprise Office Work

Comfortably handles long-horizon, multi-step office tasks — data analysis, PPTs, deep research, and infographics are all in scope.

### 💰 3. ~60% Token Savings

Compared with text-only agents, scenarios such as information search consume around 60% fewer tokens.

> **Note:** Estimated on average across long-horizon complex tasks against leading agent models on the market; actual savings vary by task.

---

## 📑 Table of Contents

- [📊 Benchmarks](#-benchmarks)
- [🔥 Showcase](#-showcase)
- [🚀 Quick Start](#-quick-start)
- [🤖 Using with Open-Source Agent Frameworks](#-using-with-open-source-agent-frameworks)
- [💎 Token Plan](#-token-plan)
- [📚 Related Links](#-related-links)

---

## 📊 Benchmarks

<p align="center">
  <img src="assets/benchmark_en.jpg" alt="Benchmark Results" width="100%">
</p>

---

## 🔥 Showcase

### 🏢 Integrated Office Workflow

Taking semiconductor memory market analysis as an example, the model covers the entire pipeline from data insight to industry research to content delivery:

**Data Insight → Industry Research → Content Delivery**

<p align="center">
  <img src="assets/office_workflow.webp" alt="Integrated Office Workflow" width="80%">
</p>

The agent runs the full "read → think → do → deliver" loop on real office tasks. Below are three representative cases with their deliverables.

#### 📊 Data Analysis ｜ Cleaning and Trend Analysis on Memory Chip Quotes

> 💬 **Query**: Read `汇总.csv` and clean / analyze the recent memory chip quote data.

**🧠 Agent Conclusion**

Memory prices have been trending upward overall, with select DRAM and NAND products showing the largest gains. The inflection point appeared in late February, followed by an acceleration through March. Categories diverged sharply — server-grade products outperformed consumer ones — indicating that this rally is not uniform but is being led by key categories.

📄 [*Memory Price Analysis.pdf*](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/examples/memory-price-end2end-analysis/README.md#step-1-data-analysis)

---

#### 🔬 Deep Research ｜ Drivers Behind 2026 Memory & Flash Price Volatility

> 💬 **Query**: Based on the data analysis, investigate the main drivers of memory and flash price movements since the start of 2026.

**🧠 Agent Conclusion**

The current rally is driven by a combination of supply contraction, surging AI-server demand, and deliberate output discipline by some manufacturers. In the short term, sentiment and inventory restocking amplify volatility, but over the medium term it looks more like a structural rebalancing of supply and demand. If high-end demand persists and OEMs maintain a cautious supply stance, prices may continue to rise or stay elevated.

📄 [*Memory Price Research.pdf*](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/examples/memory-price-end2end-analysis/README.md#step-2-deep-research) · Research · Report

---

#### 🎨 PPT Creation ｜ 15–20-page Memory Price Volatility Report

> 💬 **Query**: Generate a 15–20 page Chinese PPT titled "2026 Memory Price Volatility Analysis & Market Outlook".

**🧠 Agent Conclusion**

The final deck follows a clear narrative: first prove with data that "prices are indeed rising and the gains are concentrated in key categories", then explain "why and what's driving it" via external research, and finally give a forward-looking judgement plus actionable recommendations — focus on high-momentum categories, lock in procurement timing, and continuously track OEM strategy and downstream demand.

📄 [*Semiconductor Memory Market Surge*](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/examples/memory-price-end2end-analysis/README.md#step-3-ppt-generation) · PPT · Showcase

> 💡 **Note**: The above capabilities **must be delivered by the Agent framework together with Skills** — calling the model API directly cannot reproduce the full workflow.
>
> - **Recommended path**: Use our [Agent Pack](https://github.com/SenseTime-FVG/agent_pack) one-click installer for Hermes Agent / OpenClaw — **the full Skills suite is bundled** and works out of the box (see [🤖 Using with Open-Source Agent Frameworks](#-using-with-open-source-agent-frameworks) below).
> - **Self-integration**: For other agent frameworks, grab Skills directly from [OpenSenseNova/SenseNova-Skills](https://github.com/OpenSenseNova/SenseNova-Skills) and install them yourself.

---

## 🚀 Quick Start

### Apply for an API Key

1. Register and complete identity verification at [https://console.sensecore.cn](https://console.sensecore.cn).
2. From the console sidebar, go to **Management Center → API Key Management → Create API Key**, then copy and store it safely (the full key is shown only once on creation).
3. Set the environment variable:
   ```bash
   export SENSENOVA_API_KEY="your_api_key_here"
   ```

### ⚡ Make Your First Call

**curl**

```bash
curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H "Authorization: Bearer $SENSENOVA_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{"role": "user", "content": "Hi, please briefly introduce yourself."}]
  }'
```

---

## 🤖 Using with Open-Source Agent Frameworks

SenseNova 6.7 Flash-Lite needs an **agent runtime** + the **official skill library** to deliver an end-to-end office-task workflow.

- **Recommended runtime**: **[OpenClaw](https://openclaw.ai/)** or **[hermes-agent](https://github.com/NousResearch/hermes-agent)**.
- **Recommended LLM**: pair it with the **[SenseNova platform API](https://platform.sensenova.cn/token-plan)** (free token plan available).
- **Install & setup**: see [SenseNova-Skills INSTALL.md](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/INSTALL.md).

### 🧩 Installing SenseNova-Skills

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

## 💎 Token Plan

**SenseNova Token Plan** is built for complex knowledge work and office production scenarios — not just "cheap to use" but "comfortable to use": curated, more efficient models paired with generous quota guarantees so even long-running tasks can run with confidence.

| Feature | Description |
|------|------|
| More efficient | Token consumption on complex tasks is dramatically reduced, raising delivery output per unit cost |
| More capable | Handles cross-step, cross-modal, cross-page information processing and content generation |
| More stable | Tuned for office scenarios — outputs hew closely to real workflows |
| More complete | Closes the loop from "understanding the task" to "producing the final deliverable" |

<p align="center">
  <img src="assets/token_plan.webp" alt="Token Plan" width="100%">
</p>

> The latest SenseNova 6.7 Flash-Lite model and the full cowork-skill lineup are now part of the **Little Raccoon Pro plan**, offering enterprise-grade security plus a smooth out-of-the-box experience.<!-- TODO: link to Little Raccoon product page -->

#### [🎁 Learn more & claim your Token Plan →](https://platform.sensenova.cn/token-plan)

---

## 📚 Related Links

- Website: [https://platform.sensenova.cn/](https://platform.sensenova.cn/)
- API documentation: [docs/API.md](docs/API.md)
