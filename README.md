# ⚡ SenseNova 6.7 Flash-Lite

<p align="center">
  <img src="assets/logo.webp" alt="SenseNova Logo" height="120">
</p>

> 原生多模态的大模型，更懂办公，更省 token

**SenseNova 6.7 Flash-Lite** 是商汤日日新推出的面向真实工作流的轻量多模态智能体模型。采用原生多模态架构，兼顾效果与成本，能够稳定支撑数据分析、PPT 生成、深度调研报告生成、信息图生成等复杂长链路办公任务。

---

## ✨ 核心能力

商汤日日新原生多模态的最新模型，胜任数据分析、深度调研、复杂图片理解、PPT生成等复杂办公任务。现已推出Token Plan，更快、更好、更省。

### 🖼️ 1. 原生多模智能体

为智能体赋予原生视觉能力，让你的Agent与你共享“视界”

### 🔗 2. 更懂企业办公需求

轻松支撑长链路、多步骤的复杂办公任务，数据分析、PPT、深度调研、信息图统统不在话下

### 💰 3. Token 消耗立省 60%

相较于纯文本智能体，在信息搜索等场景下，Token节约60%

> **说明：** 基于长链路复杂任务，对比市面主流最新Agent模型，平均估算所得，根据实际执行任务不同可能存在波动

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

Agent 在真实办公任务中跑通 "读 → 想 → 做 → 交付" 的全流程，下面是三个典型案例与对应产出物。

#### 📊 数据分析 ｜ 10 份月度 Excel → 一份完整绩效分析报告

基于风电事业部 10 份月度 Excel、932 条绩效记录，Agent 自动统一表结构，完成月度趋势、等级分布、岗位对比与员工个人表现等多维分析，并自主处理字体缺失、绘图报错、变量丢失等问题。用户反馈异常图表后，Agent 回溯到数据索引层定位 MultiIndex 错误并交付最终版本，跑通 "整合 → 分析 → 生成 → 回溯 → 校验" 完整闭环。

📄 *员工绩效分析报告.docx*

---

#### 🔬 深度调研 ｜ 一次性产出投行级产业研究报告

11 章覆盖市场规模、政策、全球竞争、商业化、融资、技术、成本、供应链、路线图（2026–2028）与投资建议。重点覆盖智元、宇树、优必选、银河通用等 7 家国内玩家，国际对标 Boston Dynamics、Tesla；数据具体到亿元 / 百分比 / 同比，文末附产品参数速查表。定位投行级报告，数据驱动，不做科普。

📄 *2026 中国具身智能产业研究报告* · Research · Report

---

#### 🎨 PPT 制作 ｜ 8 页科技展览级 PPT：《生成式 AI 革命》

8 页 PPTX 围绕文本 / 图像 / 视频 / 代码生成、办公自动化、商业应用与未来趋势展开。每页聚焦一个核心主题，独立版式，吸引眼球的标题 + 简洁正文 + 1~2 张配图 + 关键数据或图表。整体色彩丰富、视觉强烈、设计高级，像科技展览与高端杂志；图片来源于搜索，不调用生图工具；HTML size 控制在 1600×900。

📄 *生成式 AI 革命.pptx* · PPT · Showcase

---

## 🚀 快速开始

### API Key 申请

1. 注册并完成实名认证：[https://console.sensecore.cn](https://console.sensecore.cn)
2. 进入控制台左侧导航：**管理中心 → API-Key 管理 → 创建 API-Key**，复制并妥善保存（仅创建时展示一次）
3. 设置环境变量：
   ```bash
   export SENSENOVA_API_KEY="your_api_key_here"
   ```

### ⚡ 发起第一次调用

**curl**

```bash
curl 'https://token.sensenova.cn/v1/chat/completions' \
  -H "Authorization: Bearer $SENSENOVA_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "sensenova-6.7-flash-lite",
    "max_tokens": 2000,
    "messages": [{"role": "user", "content": "你好，简单介绍一下你自己"}]
  }'
```

---

## 🤖 在开源 Agent 框架中使用

SenseNova 6.7 Flash-Lite 兼容 OpenAI API，可无缝接入主流开源 Agent 框架。

### 🛠️ Hermes Agent 与 OpenClaw

**Hermes Agent** 与 **OpenClaw** 是面向真实办公任务的本地 Agent 框架。推荐使用官方一键安装包 [SenseTime-FVG/agent_pack](https://github.com/SenseTime-FVG/agent_pack) 完成部署，安装器会在过程中收集 LLM 凭证并自动写入配置文件（`~/.hermes/config.yaml` 与 `~/.openclaw/openclaw.json`）。前往 [Releases 页面](https://github.com/SenseTime-FVG/agent_pack/releases) 下载对应平台安装器（Windows `.exe` / macOS `.pkg` / Linux 脚本）即可开箱即用。

完整安装流程、参数说明、常见问题及高级配置请参考 [飞书文档](https://p283t9u4d9.feishu.cn/wiki/JMkCwxpnti9Xelkt05JcehlKnCb?from=from_copylink)。

#### 🪟 Windows 详细安装和使用步骤

##### 先装 WSL2（只需要一次）

WSL2 是微软给 Windows 自带的一个 "Linux 容器"，Agent Pack 底层需要它。

1. 按下 `Win` 键，输入 `cmd`
2. 在搜索结果上右键，选 **"以管理员身份运行"**（这一步很重要）
3. 在打开的黑色窗口里粘贴：

   ```powershell
   wsl --install
   ```

4. 按回车，等它装完（几分钟）

   👉 这一步会自动完成：
   - 启用 WSL 功能
   - 安装虚拟机平台
   - 安装 Linux 内核
   - 默认安装 Ubuntu

5. 重启电脑
6. 重启后 Windows 会自动弹出一个窗口让你设 Ubuntu 的用户名和密码 —— 可以不用设置

##### 安装 Agent Pack

1. 下载安装包：[AgentPack-1.0.10-windows-x64.exe](https://github.com/SenseTime-FVG/agent_pack/releases/download/v1.0.10/AgentPack-1.0.10-windows-x64.exe)
2. 双击打开安装程序
3. 选择安装 **Hermes** 或 **OpenClaw**（一次只选一个）

   <p align="center">
     <img src="assets/install_page.PNG" alt="选择安装产品" width="70%">
   </p>

4. 填入语言模型配置

   > ⚠️ `verify` 可能由于系统缺少组件导致失败，没有关系，可继续下一步。

   <p align="center">
     <img src="assets/install2.PNG" alt="填入语言模型配置" width="70%">
   </p>

5. 点击 **下一步** 之后进入安装

##### 启动安装

1. 自动弹出 `cmd` 界面，开始执行安装脚本

   <p align="center">
     <img src="assets/install3.PNG" alt="自动弹出 cmd 执行安装脚本" width="70%">
   </p>

2. 安装完成

   <p align="center">
     <img src="assets/install4.png" alt="安装完成" width="70%">
   </p>

#### 🍎 macOS 详细安装和使用步骤

##### 先装两个前置工具（只需要一次）

1. 按 `⌘ Command` + `空格`，输入 **终端**，回车打开终端
2. 粘贴这条命令，回车：

   ```bash
   xcode-select --install
   ```

3. 接着粘贴这条命令（装 Homebrew，是 macOS 的软件管理器）：

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

4. 装完后，Apple Silicon（M1/M2/M3）机器还要再跑一次：

   ```bash
   echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
   eval "$(/opt/homebrew/bin/brew shellenv)"
   ```

   > Intel Mac 跳过这一步。


#### 🚀 开始使用

##### Hermes Agent（命令行 AI 助手）

装完之后会直接打开 Hermes 对话终端；如果没有，打开任意终端（Windows 上用 `wsl`，macOS / Linux 用系统终端），直接输入：

```bash
hermes
```

就进入聊天界面了。你可以问它：

- "帮我写一个 Python 脚本，把当前目录下所有 `.jpg` 文件重命名成 `photo-1.jpg`、`photo-2.jpg` ..."
- "这段代码为什么报错？`<粘贴代码>`"
- "帮我查一下 React 18 的最新改动"

想退出，输入 `/exit` 或按 `Ctrl + C`。

##### OpenClaw（网页 UI）

安装完成后，OpenClaw 的网关（gateway）会在后台自动启动，浏览器会自动打开控制台页面，地址大概长这样：

```
http://localhost:18789/#token=xxxxx
```

如果浏览器没自动弹出，可在终端里跑：

```bash
openclaw dashboard
```

在网页上可以：
- 查看所有 AI 对话历史
- 管理 API Key
- 配置不同的模型
- 看各种指标和日志

#### ❓ 常见问题

**Q1：安装过程中窗口突然关了 / 卡住了？**

别慌，每个平台都留了完整日志：

- **Windows**：`C:\Users\<你的用户名>\AppData\Local\AgentPack\logs\`
- **macOS**：`/private/tmp/agent-pack-postinstall.log`
- **Linux**：终端里有 `Full log:` 后面带路径

把日志发给我们，95% 能定位到问题。

---

**Q2：Windows 装完后输入 `hermes` 提示 "命令未找到"？**

重开一个 PowerShell 窗口再试。安装器给系统 PATH 加了条目，但旧窗口不会自动刷新，开新的就好。

---

**Q3：国内网络很慢 / 连不上 GitHub？**

安装器会自动检测国内网络并切换到国内镜像（ghproxy、TUNA、npmmirror、阿里云 PyPI），不用手动配置。如果自动检测没成功，在运行安装器之前手动设置环境变量：

- **Windows PowerShell**：

  ```powershell
  $env:AGENTPACK_CN = "1"
  ```

  再双击安装器。

- **macOS / Linux 终端**：

  ```bash
  export AGENTPACK_CN=1
  ```

  再跑安装器。

---

**Q4：想换 API Key / 换模型怎么办？**

- **Hermes**：编辑 `~/.hermes/.env`（Windows 上是 `\\wsl$\Ubuntu\home\<用户名>\.hermes\.env`），修改其中的 `OPENROUTER_API_KEY=...`
- **OpenClaw**：在终端运行：

  ```bash
  openclaw config set agents.defaults.model openrouter/<新的模型名>
  ```

或者干脆再跑一次安装器 —— 它是幂等的，重新装一遍会覆盖旧配置，不会搞坏别的。

---

**Q5：想彻底卸载？**

- **Windows**：控制面板 → 程序 → 找到 "Agent Pack" → 卸载
- **macOS / Linux**：

  ```bash
  rm -rf ~/.agent-pack ~/.hermes ~/.openclaw
  # 如果想保留会话历史，把这三个目录先备份一下再删
  ```

---

**Q6：OpenClaw 网页打不开 / 端口被占了？**

OpenClaw 默认用端口 `18789`。如果被其他程序占了，网关会自动选另一个端口 —— 看 OpenClaw 启动时打印的 `Then open:` 后面那个地址，别硬记 `18789`。

手动停 / 启网关：

```bash
openclaw gateway stop
openclaw gateway
```

---

**Q7：我没 API Key 能先试试吗？**

可以，安装时 API Key 那一栏留空就行。装完后再去注册 / 申请，然后编辑 `~/.hermes/.env`（或 `~/.openclaw/.env`）补上即可。
#### 📮 反馈与支持

遇到问题或希望提交建议，可通过以下渠道与我们联系：

- **GitHub Issues**：[SenseTime-FVG/agent_pack](https://github.com/SenseTime-FVG/agent_pack/issues) —— 用于报告 Bug 与提交功能建议
- **Hermes 官方文档**：<https://github.com/NousResearch/hermes-agent>
- **OpenClaw 官方文档**：<https://docs.openclaw.ai>

为便于我们快速定位问题，提交 Issue 时请附上以下信息：

1. **操作系统版本**：例如 Windows 11、macOS 14、Ubuntu 22.04
2. **复现步骤**：在哪一步出现问题
3. **完整日志内容**：日志路径参见上方[常见问题 Q1](#-常见问题)

---

#### 🛠️ 进阶配置

> 以下内容仅供进阶用户参考，常规使用无需关心。

**配置文件位置**

| 工具 | 主配置文件 | 环境变量文件 |
| --- | --- | --- |
| Hermes | `~/.hermes/config.yaml` | `~/.hermes/.env` |
| OpenClaw | `~/.openclaw/openclaw.json` | `~/.openclaw/.env` |

**多模型供应商切换**

两个工具均支持在配置文件中同时配置 OpenRouter、OpenAI、Anthropic 等多个模型供应商，运行时可按需切换。

**预置技能（Bundled Skills）**

安装包已内置数十个开箱即用的技能模块，涵盖文档处理、数据分析、网页爬取、飞书集成等场景，工具启动时自动加载，无需额外配置。

---
如有任何问题，欢迎前往 [GitHub Issues](https://github.com/SenseTime-FVG/agent_pack/issues) 与我们交流。



#### 🦞 通过 OpenClaw 官方渠道安装

如需通过 [OpenClaw](https://openclaw.ai/) 官方渠道（不经 agent_pack）独立部署，请参考 [OpenClaw 官方教程](https://openclaw.ai/)。在其引导配置中选择 **OpenAI 兼容接口**，填入 SenseNova 的 Base URL、API Key 与模型名即可接入。

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

> 最新 SenseNova 6.7 Flash-Lite 模型与全系 cowork-skill 均已加入**小浣熊 Pro 套餐**，提供企业级安全防护与开箱即用的丝滑体验。<!-- TODO: 插入小浣熊产品页链接 -->

<!-- TODO: 插入 Token Plan 购买/了解更多链接 -->

---

## 📚 相关链接

- 官网：[https://platform.sensenova.cn/](https://platform.sensenova.cn/)
- API 文档：[docs/API.md](docs/API.md)
