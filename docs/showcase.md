# 🎬 Showcase

围绕 **SenseNova 6.7 Flash-Lite** 的真实办公任务示例，覆盖数据分析、深度调研等核心场景。每个 case 提供任务描述、模型结论与可在线浏览的 HTML 报告。

> 复现以上样例需配合 OpenClaw / Hermes Agent 框架并启用 [SenseNova-Skills](https://github.com/OpenSenseNova/SenseNova-Skills/)。

---

## 🏢 半导体存储市场分析（数据分析 + 深度调研）

围绕近期 DRAM / NAND 价格走势完成"数据洞察 → 行业研究"的两段式分析。

### 1. 数据分析

**Prompt**

> 请读取「芯片价格汇总.csv」，对近期的存储芯片报价数据进行清洗和分析。

**结论**：近期存储价格整体呈上行趋势，部分 DRAM 与 NAND 产品涨幅最明显；2 月下旬开始出现拐点，3 月后进入加速阶段；服务器相关产品表现强于消费类，本轮上涨由重点品类率先带动。

👉 [查看在线报告：半导体存储市场价格汇总分析](https://opensensenova.github.io/SenseNova6.7/showcase/semiconductor-analysis.html)

### 2. 深度调研

**Prompt**

> 基于数据分析结果，调研 2026 年以来内存和闪存价格波动的主要原因。

**结论**：本轮价格上涨主要由供给收缩、AI 服务器需求增强以及部分厂商主动控产共同推动；短期看存在情绪和备货带来的波动放大，中期更像是供需重新平衡下的结构性修复；后续若高端需求持续、原厂延续谨慎供给策略，价格仍有继续上行或高位震荡的可能。

👉 [查看在线报告：半导体存储价格暴涨原因与趋势预测](https://opensensenova.github.io/SenseNova6.7/showcase/semiconductor-research.html)

---

## 👥 风电事业部员工绩效分析（数据分析）

**Prompt**

> 请读取风电事业部 10 个月度绩效考核表（2024 年 12 月至 2025 年 9 月），生成一份报告，覆盖绩效总体情况、月度趋势、岗位对比、个人表现以及结论与改进建议；要求每部分都有深度分析，并使用图表更美观地展示数据。

**结论**：模型完成 10 个月的绩效数据汇总与分析，输出整体情况、月度趋势、岗位对比、TOP/BOTTOM 员工识别及面向管理者的改进建议，最终以图文结合的报告形式呈现。

👉 [查看在线报告：风电事业部 2024-2025 年度员工绩效分析](https://opensensenova.github.io/SenseNova6.7/showcase/wind-energy-performance.html)

---

## 🤖 具身智能行业调研（深度调研）

**Prompt**

> 帮我对具身智能行业做个调研，生成一份专业的行业调研报告。

**结论**：模型基于公开信息完成具身智能行业的现状梳理，覆盖技术路线、代表玩家、产业链、典型应用场景与未来趋势判断，最终输出一份结构完整的行业调研报告。

👉 [查看在线报告：具身智能行业现状调研](https://opensensenova.github.io/SenseNova6.7/showcase/embodied-ai-research.html)

---

## 📦 完整产物下载

所有 HTML 报告均可在 [docs/showcase/](./showcase/) 目录中获取；附带的原始数据 / zip / pptx 等完整产物未随仓库分发，如需获取请联系产品团队。
