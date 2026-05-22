# SenseNova 6.7 Flash-Lite

**English** | [中文](README_CN.md)

<p align="center">
  <a href="https://www.sensenova.cn/"><img src="assets/logo.webp" alt="SenseNova Logo" height="120"></a>
</p>

<p align="center">
  <a href="https://www.sensenova.cn/"><img src="https://img.shields.io/badge/Website-platform-blue?logo=googlechrome&logoColor=white" alt="Website"></a>
  <a href="API.md"><img src="https://img.shields.io/badge/Model-6.7--flash--lite-orange" alt="Model"></a>
  <a href="API.md"><img src="https://img.shields.io/badge/API-Docs-green?logo=readthedocs&logoColor=white" alt="API Docs"></a>
  <a href="https://www.sensenova.cn/token-plan"><img src="https://img.shields.io/badge/Token%20Plan-Free-brightgreen?logo=gift&logoColor=white" alt="Token Plan"></a>
  <a href="https://github.com/OpenSenseNova/SenseNova-Skills"><img src="https://img.shields.io/badge/Skills-SN--Skills-black?logo=github&logoColor=white" alt="Skills"></a>
  <a href="https://office.xiaohuanxiong.com/home"><img src="https://img.shields.io/badge/Raccoon-Try%20it%20free-ff69b4" alt="Raccoon"></a>
</p>

> A lightweight multimodal agent model built for real-world workflows.

**SenseNova 6.7 Flash-Lite** is SenseTime's lightweight multimodal agent model, purpose-built for real-world workflows. With a native multimodal architecture that balances quality and cost, it reliably powers complex long-horizon office tasks such as data analysis, slide deck generation, deep research reports, and infographic creation.

---

## Core Capabilities

**SenseNova 6.7 Flash-Lite** — a lightweight multimodal agent model built for real-world workflows.

- **Lightweight & efficient**, balancing quality, cost, and deployability
- **Office-tuned**, reliably powering complex long-horizon tasks
- **Native multimodal architecture**, well suited to real office content
- **Better token efficiency**, keeping complex tasks affordable

---

## Table of Contents

- [Benchmarks](#benchmarks)
- [Integrated Office Workflow](#integrated-office-workflow)
- [Quick Start](#quick-start)
- [Using with Agent Frameworks](#using-with-agent-frameworks)
- [Token Plan](#token-plan)
- [Related Links](#related-links)

---

## Benchmarks

<p align="center">
  <img src="assets/benchmark_en.jpg" alt="Benchmark Results" width="100%">
</p>

SenseNova 6.7 Flash-Lite leads across multiple benchmarks, standing out among models of comparable size on long-horizon tasks, planning, and multimodal understanding.

---

## Integrated Office Workflow

Taking semiconductor memory market analysis as an example, the model covers the entire pipeline from data insight to industry research to content delivery:

**Data Insight → Industry Research → Content Delivery**

<p align="center">
  <img src="assets/showcase-hero.png" alt="Integrated Office Workflow" width="80%">
</p>

The agent runs the full "read → think → do → deliver" loop on real office tasks. Below are three representative cases with their deliverables.

#### Step 1 ｜ Data Analysis ｜ Cleaning and Trend Analysis on Memory Chip Quotes

> **Query**: Read `汇总.csv` and clean / analyze the recent memory chip quote data.

**Agent Conclusion**

Memory prices have been trending upward overall, with select DRAM and NAND products showing the largest gains. The inflection point appeared in late February, followed by an acceleration through March. Categories diverged sharply — server-grade products outperformed consumer ones — indicating that this rally is not uniform but is being led by key categories.

[*Memory Price Analysis.pdf*](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/examples/memory-price-end2end-analysis/README.md#step-1-data-analysis)

#### Step 2 ｜ Deep Research ｜ Drivers Behind 2026 Memory & Flash Price Volatility

> **Query**: Based on the data analysis, investigate the main drivers of memory and flash price movements since the start of 2026.

**Agent Conclusion**

The current rally is driven by a combination of supply contraction, surging AI-server demand, and deliberate output discipline by some manufacturers. In the short term, sentiment and inventory restocking amplify volatility, but over the medium term it looks more like a structural rebalancing of supply and demand. If high-end demand persists and OEMs maintain a cautious supply stance, prices may continue to rise or stay elevated.

[*Memory Price Research.pdf*](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/examples/memory-price-end2end-analysis/README.md#step-2-deep-research) · Research · Report

#### Step 3 ｜ PPT Creation ｜ 15–20-page Memory Price Volatility Report

> **Query**: Generate a 15–20 page Chinese PPT titled "2026 Memory Price Volatility Analysis & Market Outlook".

**Agent Conclusion**

The final deck follows a clear narrative: first prove with data that "prices are indeed rising and the gains are concentrated in key categories", then explain "why and what's driving it" via external research, and finally give a forward-looking judgement plus actionable recommendations — focus on high-momentum categories, lock in procurement timing, and continuously track OEM strategy and downstream demand.

[*Semiconductor Memory Market Surge*](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/examples/memory-price-end2end-analysis/README.md#step-3-ppt-generation) · PPT · Showcase

> **Note**: The above capabilities **must be delivered by the Agent framework together with Skills** — calling the model API directly cannot reproduce the full workflow.
>
> - **Recommended path**: Pair [OpenClaw](https://openclaw.ai/) or [hermes-agent](https://github.com/NousResearch/hermes-agent) with the official skill library from [OpenSenseNova/SenseNova-Skills](https://github.com/OpenSenseNova/SenseNova-Skills) (see [Using with Agent Frameworks](#using-with-agent-frameworks) below).
> - **Self-integration**: For other agent frameworks, grab Skills directly from [OpenSenseNova/SenseNova-Skills](https://github.com/OpenSenseNova/SenseNova-Skills) and install them yourself.

---

## Quick Start

### Apply for an API Key

1. Register and complete identity verification at [https://platform.sensenova.cn/console](https://platform.sensenova.cn/console).
2. From the console sidebar, go to **Management Center → API Key Management → Create API Key**, then copy and store it safely (the full key is shown only once on creation).
3. Set the environment variable:
   ```bash
   export SENSENOVA_API_KEY="your_api_key_here"
   ```

### Make Your First Call

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

> For the full API reference (multi-turn, multimodal input, streaming, OpenAI SDK, error codes, etc.), see [API.md](API.md).

---

## Using with Agent Frameworks

SenseNova 6.7 Flash-Lite needs an **agent runtime** + the **official skill library** to deliver an end-to-end office-task workflow.

- **Recommended runtime**: **[OpenClaw](https://openclaw.ai/)** or **[hermes-agent](https://github.com/NousResearch/hermes-agent)**.
- **Recommended LLM**: pair it with the **[SenseNova platform API](https://platform.sensenova.cn/token-plan)** (free token plan available).
- **Install & setup**: see [SenseNova-Skills INSTALL.md](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/INSTALL.md).

### Installing SenseNova-Skills

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

## 🦝 Out-of-the-Box with Raccoon

The latest model and full Cowork-Skill suite from this repo are integrated into [**Raccoon**](https://office.xiaohuanxiong.com/home), offering enterprise-grade security plus a smooth out-of-the-box experience — and it's **free to use**. If you'd rather not set up your own environment or manage API keys, you can access these capabilities directly through Raccoon.

Raccoon ships a comprehensive upgrade across product capabilities and client experience:

- **Three core office capabilities upgraded**: Powered by SenseNova 6.7 Flash and Cowork-Skill, data analysis, PPT generation, and task planning are all further strengthened — covering the full loop of complex knowledge work, from multi-file cleaning and analysis, to formal reporting decks, to industry research / competitive analysis / investment research reports.
- **New infographic generation**: Built on the SenseNova U1 model, it compresses complex data, long-form reports, and business insights into high-density, structured, visualized infographics — making complex content easier to understand and easier to share.
- **All-new client + local Agent OS**: Cloud models handle complex reasoning and multimodal understanding, while the local Agent OS centers on local files, work context, and personal habits — delivering a more personalized, localized, and secure AI-native office experience.
- **Validated at scale**: Trusted by 15 million individual users and thousands of enterprises.

> 👉 Try it now: [office.xiaohuanxiong.com/home](https://office.xiaohuanxiong.com/home)

---

## Token Plan

**More than a discount — a productivity capability pack for the office**

**SenseNova Token Plan** isn't only "cheap to use" — it's "comfortable to use". We pair curated, more efficient models with generous quota guarantees, so even long-running tasks can run with confidence — delivering a high-value, high-frequency, scalable productivity capability pack for the office.

**01 · Native multimodal agent**

A multimodal foundation that bridges understanding and generation — reads documents, images, and tables, and produces high-quality deliverables that combine text and visuals.

**02 · Tuned for enterprise office work**

Closes the loop from "understanding the task" to "producing the final deliverable" — complex office workflows run end-to-end, not just stopping at advice.

**03 · ~60% token savings**

Token consumption stays under control on complex tasks, raising deliverable output per unit cost — so long-horizon tasks can "run with confidence".

<p align="center">
  <a href="https://www.sensenova.cn/token-plan"><img src="assets/token_plan_banner_en.png" alt="Token Plan" width="80%"></a>
</p>

#### [Learn more →](https://www.sensenova.cn/token-plan)

---

## Join the Community!

Have questions about integrating SenseNova 6.7 Flash-Lite, hit a bug while using the Skills, or just want to swap notes on real-world workflows? Scan the QR code below to join our WeCom community — the team is there to provide technical support, gather your feedback, and ship improvements based on what you tell us.

<div align="center">
<table>
  <tr>
    <td align="center"><b><a href="https://discord.com/invite/BuTXPHmQub">Discord</a></b></td>
    <td align="center"><b>WeChat Group</b></td>
  </tr>
  <tr>
    <td align="center"><a href="https://discord.com/invite/BuTXPHmQub"><img src="assets/discord_qr.webp" width="160"/></a></td>
    <td align="center"><img src="assets/sensenova-skills-chatgroup.jpg" width="160"/></td>
  </tr>
</table>
</div>

---

## Related Links

- Website: [https://www.sensenova.cn/](https://www.sensenova.cn/)
- API documentation: [API.md](API.md)
- Raccoon: [office.xiaohuanxiong.com/home](https://office.xiaohuanxiong.com/home)
